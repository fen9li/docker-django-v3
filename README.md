# Django project 'koala' v3

## references    

* https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django
* https://github.com/mdn/django-locallibrary-tutorial

* From koala v2
* https://docs.docker.com/compose/django/
* https://blog.devartis.com/django-development-with-docker-a-completed-development-cycle-7322ad8ba508

## start the project locallibrary and app catalog

* Create GIT_BASE directory 'docker-django-v3'

```
feng@ubuntu:~$ mkdir docker-django-v3
feng@ubuntu:~$ cd docker-django-v3
feng@ubuntu:~/docker-django-v3$ tree
.
├── docker-compose.yml
├── Dockerfile
├── HOT_TIPS.md
├── README.md
└── requirements.txt

0 directories, 5 files
feng@ubuntu:~/docker-django-v3$ 
```

* Create below files in GIT_BASE directory

```
feng@ubuntu:~/docker-django-v3$ cat requirements.txt 
Django>=2.0,<3.0
mysqlclient
feng@ubuntu:~/docker-django-v3$ cat Dockerfile 
FROM python:3.7.3
ENV PYTHONUNBUFFERED 1
RUN mkdir /code
WORKDIR /code
COPY requirements.txt /code/
RUN pip install -r requirements.txt
COPY . /code/
feng@ubuntu:~/docker-django-v3$ cat docker-compose.yml 
version: '3'

services:
  db:
    image: mariadb:10.3
    restart: on-failure
    volumes:
      - koala:/var/lib/mysql
    ports:
      - 3306:3306
    expose:
      - 3306
    environment:
      MYSQL_DATABASE: koala
      MYSQL_USER: koalauser
      MYSQL_PASSWORD: password
      MYSQL_ROOT_PASSWORD: password
    networks:
      - backend
  web:
    build: .
    restart: on-failure
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - 8000:8000
    networks:
      - backend
    depends_on:
      - db
    
volumes:
  koala:
networks:
  backend:
feng@ubuntu:~/docker-django-v3$ 
```

* Run below command in GIT_BASE directory

```
sudo docker-compose run web django-admin startproject locallibrary .
```

* Check new files created in GIT_BASE directory and change user priviliege if applicable

```
feng@ubuntu:~/docker-django-v3$ tree
.
├── docker-compose.yml
├── Dockerfile
├── HOT_TIPS.md
├── locallibrary
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── manage.py
├── README.md
└── requirements.txt

1 directory, 10 files
feng@ubuntu:~/docker-django-v3$ cat .gitignore 
*/__pycache__
*/*/__pycache__/*.pyc
HOT_TIPS.md
feng@ubuntu:~/docker-django-v3$  

sudo chown -R $USER:$USER .

feng@ubuntu:~/docker-django-v3$ 
```

## git housekeeping @ GIT_BASE directory

### create new repo in github and get ssh_url

```
feng@ubuntu:~/docker-django-v3$ curl -u 'fen9li' https://api.github.com/user/repos -d '{"name":"docker-django-v3"}' | jq '.ssh_url'
Enter host password for user 'fen9li': <your github user password>
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  5590  100  5563  100    27   3779     18  0:00:01  0:00:01 --:--:--  3797
"git@github.com:fen9li/docker-django-v3.git"
feng@ubuntu:~/docker-django-v3$ 
```

### git commands cheatsheet

```
git init
git remote add origin git@github.com:fen9li/docker-django-v3.git

feng@ubuntu:~/docker-django-v3$ git remote -v
origin  git@github.com:fen9li/docker-django-v3.git (fetch)
origin  git@github.com:fen9li/docker-django-v3.git (push)
feng@ubuntu:~/docker-django-v3$  

git add .
git commit -am "initial commit"
git push --set-upstream origin master

feng@ubuntu:~/docker-django-v3$
```

## docker-compose commands to manipulate development environment
```
docker-compose down
docker-compose up -d
docker-compose up -d --build
```

## check changelogs for following README.
