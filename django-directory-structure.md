# Lets understand the django-dir-strcture along with create a path to understand each file importance. 

## Django Commands --------
 
 ```python
        pip install django
        pip install django-admin
        conda install django-admin
        conda install django version=0.0.0 if need

```

## Run Commands

All commands superdo is manage.py file the entry point of django application through manage.py the server file.

```python 
    python manage.py <command>

    manage.py -> settings.py -> Installed Apps -> Execute Command

    python manage.py help // to get all the details associated with it

    python manage.py help runserver //gives all the details of runserver command okay 

    python manage.py runserver <port> //use to run the server of django 

    python manage.py runserver <ip+port> //same with ip config allows access from other devices on same network 

    It auto reload so for this we have flag to stop auto reloading.
    python manage.py  runserver --noreload

    If you have frontend on same django project need to stop that static serving 
    python manage.py runserver --nostatic

    Need to migrate the models into db so for this we have 
    python manage.py makemigrations //To apply
    python manage.py migrate //To migrate changes

    Need to migrate the specific app so use this
    python manage.py makemigrations <appname>

    If you need to show changes by not creating migration files
    python manage.py makemigrations --dry-run

    If you need to migrate sql file then 
    python manage.py sqlmigrate <appname> file_id

    If you need to migrate specfic file or app then 
    python manage.py migrate <appname> <file_id>

    Fake migration for testing than 
    python manage.py migrate --fake

    If need to see applied migrations than 
    python manage.py showmigrations    // show all aplied and unapplied migration okay 

```

## Create SuperUser in Django

```python

    If you need to create super user for admin panel
    python manage.py cratesuperuser  //then access /admin

    If you need to change password
    python manage.py   changepassword admin

```

## Interactive Django Shell

```python 

    python manage.py shell

    shell_plus -> django extensions
    pip install django-extensions

    then:
    python manage.py shell_plus

    db_shell
    python manage.py dbshell   //in this you can write SQL queries
```

## Project Checks

```python 
    
    python manage.py check  //this will check issues in project

    python manage.py check --deploy    //check for deployment like DEBUG=True, HTTP error, Missing URL's  Production Command

    python manage.py collectstatic   //Collect all static files and copy into STATIC_ROOT understand

```

## Run tests

```python

    python manage.py test  //this will run all testing files for our project

    python manage.py test <appname>

    python manage.py test <appname>.<filename>

```

## Delete all data

```python 

    python manage.py flush  //delete everything only left with schema

    python manage.py reset_db     //going to drop database

    python manage.py clearsessions     //This will removed expired sessions
```


## Export Data

```python 

    python manage.py dumpdata //this is used to dumpdata to export like in the form of json 

    python manage.py dumpdata <appname>

```

## LoadData 

```python 

    python manage.py loaddata <filename.json>

```

## Generate Models from existing DB

```python

    python manage.py inspectdb   //this will generate models from existing db man used for legacy dbs

```

## Start Specific App

```python 
    
    python manage.py startapp <appname>

```

## Start Project Usually run only once1

```python 

    django-admin startproject <projectname>

```

## Squash migrations

```python 

    python manage.py squashmigrations <appname> <migration file id>   //usefull in large project

```

## Show setting changed from Django default

```python 

    python manage.py diffsettings     //very usefull for debugging

```


# Django extensions commands

```python

    python manage.py show_urls  //this will going to show the urls present in the project each 

    python manage.py graph_models <appname>    //this will going to show the ER digrams of the models of each app !

    python manage.py runscript  <script_name>    //this will going to run the script using this command


```

If i create django-drf deployment i always include :

         - django-extensions
         - drf-spectacular
         - celery
         - redis 


# Django directory Strcture

## Django Core

```dir

DJANGO CORE
├── manage.py
├── settings.py
├── urls.py
├── views.py
├── models.py
├── admin.py
├── apps.py
├── migrations/
├── templates/
├── static/
├── forms.py
├── tests.py
├── middleware.py
├── signals.py
├── validators.py
├── managers.py
├── exceptions.py
├── wsgi.py
└── asgi.py

```

## Django DRF

```dir

DJANGO REST FRAMEWORK (DRF)
├── serializers.py
├── ViewSets
├── APIView
├── GenericAPIView
├── Mixins
├── Routers
├── Permissions
├── Authentication
├── Pagination
├── Throttling
├── Filtering
├── Parsers
├── Renderers
└── Versioning

```

## Django api

```dir

api/
├── serializers.py
├── permissions.py
├── pagination.py
├── filters.py
├── throttles.py
└── authentication.py

```

## Websockets

```dir

DJANGO CHANNELS (WebSockets)
├── consumers.py
├── routing.py
├── asgi.py
├── channel layers
├── websocket routes
└── groups

```

## Celery 

```dir 

CELERY (Background Tasks)
├── tasks.py
├── celery.py
├── beat_schedule.py
└── workers

```

## Django debuggin 

```dir 

DJANGO DEBUG TOOLBAR
├── debug panels
├── sql inspection
├── cache inspection
└── request profiling

```


## Jwt Auth

```dir 

JWT AUTHENTICATION
(SimpleJWT)
├── access tokens
├── refresh tokens
├── token blacklist
└── authentication classes

```


## Redis 

```dir 

REDIS
├── caching
├── sessions
├── channels backend
└── celery broker
```


## Django Swagger 

```dir 

DRF SPECTACULAR / SWAGGER
├── schema generation
├── OpenAPI docs
├── Swagger UI
└── ReDoc
```

## Django FilterSet

```dir 

DJANGO FILTER
├── FilterSet
├── Search Filters
└── Ordering Filters

```

## Django Cache 

```dir 

DJANGO CACHING
├── cache.py
├── Redis Cache
├── Memcached
└── Local Memory Cache

```


