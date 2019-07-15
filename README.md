# Django project 'koala' v3

## references    

* https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django
* https://github.com/mdn/django-locallibrary-tutorial

* From koala v2
- https://docs.docker.com/compose/django/
- https://blog.devartis.com/django-development-with-docker-a-completed-development-cycle-7322ad8ba508

## start the project

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



## git housekeeping

### create new repo in github and extract ssh_url

```
feng@ubuntu:~/docker-django-v2$ curl -u 'fen9li' https://api.github.com/user/repos -d '{"name":"docker-django-v2"}' | jq '.ssh_url'
Enter host password for user 'fen9li': <your github user password>
"git@github.com:fen9li/docker-django-v2.git"
feng@ubuntu:~/docker-django-v2$ 
```

### git commands cheatsheet
```
git init
git remote add origin git@github.com:fen9li/docker-django-v2.git

feng@ubuntu:~/docker-django-v2$ git remote -v
origin  git@github.com:fen9li/docker-django-v2.git (fetch)
origin  git@github.com:fen9li/docker-django-v2.git (push)
feng@ubuntu:~/docker-django-v2$ 

git add Dockerfile README.md requirements.txt docker-compose.yml 
git commit -am "Initial commit"

feng@ubuntu:~/docker-django-v2$ git push --set-upstream origin master
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 8 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (6/6), 1.97 KiB | 1007.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To github.com:fen9li/docker-django-v2.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
feng@ubuntu:~/docker-django-v2$
```

## create django project 'koala'

```
feng@ubuntu:~/docker-django-v2$ sudo docker-compose run web django-admin startproject koala .
Creating network "docker-django-v2_default" with the default driver
Creating docker-django-v2_db_1 ... done
feng@ubuntu:~/docker-django-v2$ 
feng@ubuntu:~/docker-django-v2$ sudo chown -R $USER:$USER .
feng@ubuntu:~/docker-django-v2$ ls -l
total 24WARNING: Image for service web was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.1.png)

-rw-r--r-- 1 feng feng  210 Jul 13 20:56 docker-compose.yml
-rw-r--r-- 1 feng feng  150 Jul 13 20:54 Dockerfile
drwxr-xr-x 2 feng feng 4096 Jul 13 22:21 koala
-rwxr-xr-x 1 feng feng  625 Jul 13 22:21 manage.py
-rw-r--r-- 1 feng feng 1482 Jul 13 22:19 README.md
-rw-r--r-- 1 feng feng   36 Jul 13 20:55 requirements.txt
feng@ubuntu:~/docker-django-v2$ 

feng@ubuntu:~/docker-django-v2$ docker-compose down
Stopping docker-django-v2_db_1 ... done
Removing docker-django-v2_web_run_76556932ebea ... done
Removing docker-django-v2_db_1                 ... done
Removing network docker-django-v2_default
feng@ubuntu:~/docker-django-v2$

feng@ubuntu:~/docker-django-v2$ docker-compose up -d
Creating network "docker-django-v2_default" with the default driver
Creating docker-django-v2_db_1 ... done
Creating docker-django-v2_web_1 ... done
feng@ubuntu:~/docker-django-v2$ 

feng@ubuntu:~/docker-django-v2$ echo */__pycache__ >> .gitignore
feng@ubuntu:~/docker-django-v2$ 
```

## test the project

enter url 'http://localhost:8000' in browser and you will see

![koala_start_project_01](images/koala_start_project_01.png)
