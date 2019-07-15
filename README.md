# Django project 'koala' v3

## references    

* https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django
* https://github.com/mdn/django-locallibrary-tutorial

> From koala v2
> https://docs.docker.com/compose/django/
> https://blog.devartis.com/django-development-with-docker-a-completed-development-cycle-7322ad8ba508

## start the project locallibrary and app catalog

* Create GIT_BASE directory 'docker-django-v3'

```
feng@ubuntu:~$ mkdir docker-django-v3
feng@ubuntu:~$ cd docker-django-v3
feng@ubuntu:~/docker-django-v3$ 
```

* Create below files in GIT_BASE directory

```
feng@ubuntu:~/docker-django-v3$ tree
.
├── docker-compose.yml
├── Dockerfile
└── requirements.txt

0 directories, 3 files
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
├── locallibrary
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── manage.py
└── requirements.txt

1 directory, 8 files
feng@ubuntu:~/docker-django-v3$ cat .gitignore 
*/__pycache__
*/*/__pycache__/*.pyc
HOT_TIPS.md
feng@ubuntu:~/docker-django-v3$ sudo chown -R $USER:$USER .
feng@ubuntu:~/docker-django-v3$ vim README.md
feng@ubuntu:~/docker-django-v3$ cat README.md
......
feng@ubuntu:~/docker-django-v3$ 
```


* create new repo in github and get ssh_url

```
feng@ubuntu:~/docker-django-v3$ curl -u 'fen9li' https://api.github.com/user/repos -d '{"name":"docker-django-v3"}' | jq '.ssh_url'
Enter host password for user 'fen9li': <your github user password>
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  5590  100  5563  100    27   3779     18  0:00:01  0:00:01 --:--:--  3797
"git@github.com:fen9li/docker-django-v3.git"
feng@ubuntu:~/docker-django-v3$ 
```

* git commands cheatsheet

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

* hot commands used in this project
```
docker-compose down
docker-compose up -d
docker-compose up -d --build

sudo docker-compose run web django-admin startproject locallibrary .
sudo docker-compose run web python manage.py startapp catalog

sudo docker-compose run web python manage.py migrate
sudo docker-compose run web python manage.py createsuperuser
sudo docker-compose run web python manage.py makemigrations catalog
sudo docker-compose run web python manage.py migrate catalog

sudo docker-compose run web python manage.py test 
```

* use below method to clean stale docker images
> we will solve this later

```
feng@ubuntu:~/docker-django-v3$ docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
docker-django-v3_web   latest              981f6ccbf6cd        37 minutes ago      969MB
<none>                 <none>              4faec5240e76        About an hour ago   971MB
docker-django-v2_web   latest              671085042898        3 hours ago         972MB
<none>                 <none>              1ed54f112f1f        3 hours ago         972MB
<none>                 <none>              c42e9be5a301        3 hours ago         972MB
<none>                 <none>              d05ddff287b9        11 hours ago        972MB
<none>                 <none>              e61f3a3d267e        14 hours ago        972MB
<none>                 <none>              13c6d16530ed        14 hours ago        972MB
<none>                 <none>              3f0a0210e2c3        14 hours ago        972MB
<none>                 <none>              685b6597da6e        14 hours ago        972MB
<none>                 <none>              bea35ce767ab        14 hours ago        972MB
<none>                 <none>              cb61de1037c8        14 hours ago        972MB
<none>                 <none>              975749595579        23 hours ago        972MB
docker-django_web      latest              c599dd4e726a        2 days ago          302MB
fen9li/testapp         v0.0.1              bf92679505ed        3 days ago          131MB
testhello              latest              bf92679505ed        3 days ago          131MB
mariadb                10.3                b882b2f24e58        11 days ago         344MB
python                 3.7.3-alpine3.10    2caaa0e9feab        2 weeks ago         87.2MB
postgres               latest              79db2bf18b4a        3 weeks ago         312MB
mariadb                latest              f1e4084965e5        3 weeks ago         356MB
mariadb                10.2                883bdccb9930        3 weeks ago         341MB
python                 2.7-slim            ca96bab3e2aa        4 weeks ago         120MB
python                 3.7.3               34a518642c76        4 weeks ago         929MB
hello-world            latest              fce289e99eb9        6 months ago        1.84kB
python                 3.6.7-alpine        cb04a359db13        6 months ago        74.3MB
feng@ubuntu:~/docker-django-v3$ docker rmi `docker images | grep "none" | awk '{print $3}'`
Deleted: sha256:4faec5240e768fb8e72c98f308082fb0cf3247cdb329c094dcee8c9b53ef24c6
Deleted: sha256:b90c7d9622302fbb4cebddfe48ccc7a7c6ba16f62a2aafe95f6a7281327966aa
Deleted: sha256:1ed54f112f1f6a9e5101a9372f19d02297ed9eaf77c958f9962871e8a2736dd8
Deleted: sha256:f1aea12e1551a8efb8de49b5b33faca3c99f1cfe0f0bec0b628876552aed641c
Deleted: sha256:c42e9be5a301ff14c88784a1fc90906c484cae768fc56f09621c8da130c29fde
Deleted: sha256:8c2fecd2b139b58c33098f01de1c63cfee10f806097030a7badff72b71dda093
Deleted: sha256:d05ddff287b9f897c9d37c197724462c28ce8b2c65a675a34d576a83356fc45d
Deleted: sha256:4d1884cd8a6f9ed56dffa3d07e7f15215ce91c60652d5812b064f4d8eec9b477
Deleted: sha256:e61f3a3d267ebd143dea71f26970cee46e0ad0a5734b4f4776789a972694329b
Deleted: sha256:976a3b3c413d5202c6ccfb56e02136dcc14d6cb389798bbf3e262aed23fa8797
Deleted: sha256:13c6d16530edc7afb9668f54531c49483cdfb6ca30ba419bef6755e913a6527a
Deleted: sha256:51d06117e6151aad25376bb35feb5ce23e314bcd8022f4e215516054a886f0be
Deleted: sha256:3f0a0210e2c33a8ac6b70cb24e6e5fa3c6bd1e6a5c18118a1e6962a7a78ce327
Deleted: sha256:6acef09830456696502de8b0f08f25fbc365cc430201429469ddbd44f845ebd4
Deleted: sha256:685b6597da6e4fc97c74929a3d400623163c31ba15aaf2da021c993744bcae3c
Deleted: sha256:22d42affa928da951fc581d37d3077ee8c6ed0bc7ece3e54d66e0af74b2352b3
Deleted: sha256:bea35ce767abf0b388531c4ec88b92c089aa9132a6f1203b6816b1de2dd59e10
Deleted: sha256:77caaa7c5dcd0100823822dbcaeea53bc3a9c026d2340ada66f302700a0f14e3
Deleted: sha256:cb61de1037c8e12353a81466d808c4b979fa25cf7e1ada8d3fdb1f83f29ef083
Deleted: sha256:3d07a8b131537f09d4fa01ab8ba721a2d7d88396d60ea03569c74e087cd1a1bf
Deleted: sha256:975749595579eac9036754c6a9c583d03804e2c7562a0f3650e90908409f1183
Deleted: sha256:38b6ca2fbd433c9d466ded58dd3cf7c46cec0853a55cf70f490555a77794cdde
feng@ubuntu:~/docker-django-v3$ docker images
REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
docker-django-v3_web   latest              981f6ccbf6cd        37 minutes ago      969MB
docker-django-v2_web   latest              671085042898        3 hours ago         972MB
docker-django_web      latest              c599dd4e726a        2 days ago          302MB
fen9li/testapp         v0.0.1              bf92679505ed        3 days ago          131MB
testhello              latest              bf92679505ed        3 days ago          131MB
mariadb                10.3                b882b2f24e58        11 days ago         344MB
python                 3.7.3-alpine3.10    2caaa0e9feab        2 weeks ago         87.2MB
postgres               latest              79db2bf18b4a        3 weeks ago         312MB
mariadb                latest              f1e4084965e5        3 weeks ago         356MB
mariadb                10.2                883bdccb9930        3 weeks ago         341MB
python                 2.7-slim            ca96bab3e2aa        4 weeks ago         120MB
python                 3.7.3               34a518642c76        4 weeks ago         929MB
hello-world            latest              fce289e99eb9        6 months ago        1.84kB
python                 3.6.7-alpine        cb04a359db13        6 months ago        74.3MB
feng@ubuntu:~/docker-django-v3$ 
```

## check changelogs for following README.
