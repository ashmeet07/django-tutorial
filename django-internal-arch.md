# Manage.py (Entry Point)

This is the server file used for serving the django applications inside a project.

` manage.py -> settings.py -> installed_apps -> url config -> wsgi_asgi server `'

# Settings.py (Control Center)

This files having all the app configuration and access management to the backend through allowed host.

Contain :  INSTALLED_APPS, DATABASES, MIDDLEWARE, CACHES, LOGGING, REST_FRAMEWORK, DATABASES, STATIC_URL, MEDIA_URL, AUTH_USER_MODEL, Business Logic, Queries, API Code


# Urls.py

This file is both in app/project in app for app configuration and in project is for all apps configuration like centralized command cell routing.

`map request to view` MVT(Model View Template)

# Database Layer

## Models.py

Importing models where needed from models.py and as per model creation this models going to act as ORM/OEM layer Object Entity Mapping/Object Relational Mapper.

- Here we define database table schema contain each thing required like domain, constraint, indexes, relationships, 

## Managers.py 

Custom databsae queries having some custom workflow functions inside classes.


# Migrations/ 

Basically generate through makemigrations and applied through migrate generally production teams commit this folder onto github.


# Api Layer

## Serializers.py  

This is used to convert [ JSON -> Ntive object ]  [ Native object -> JSON ]


## Views.py 

Request handling/Business Logic.

Request -> Validate -> Call Services -> Response

Business Pattern : Views.py -> Services.py -> models.py 


# Permissions.py 

It is used for authorization providing permission to user to different resources.

permission_classes

Its main motive is can user access ? 

# Authentication.py 

This file basically covers authentication for the user (identity verification).

Example : JWT, Token, Oauth, Session. 

# Pagination.py 

This is where large dataset handling happen. 
 
- Instead of 100000000 records gives 100 per page records.

# Filters.py 

This is used for searching and filtering through urls.


# Request Lifecycle.

## middleware.py 

Runs before and after every request. [Cors, Logging, Security, Rate Limiting, Tracking]

## Signals.py 

This is basically event driven actions -> User Created -> Signal -> Create Profile. 

[Notifications, Audit Logs, Analytics]

# Real Time Layer

## Consumers.py 

- This file is used to equivalent to the websockets (Views.py)

HTTP: Views.py

WEBSOCKETS: Consumers.py (logic file)

## Routing.py 

This files is equivalet to the urls.py for websockets.

## Realtime.py/Service.py 

This is often created by the production teams. 

Contain: 
    - Notification Logic
    - Broadcast Logic
    - Chat Logic
    //keep consumers thin

# Background Processing

## Tasks.py 

This file is used by celery for performing task like: 
- Email 
- Reports
- Notifications 
- Data Sync 
//these are the task which never done in the production views okay. 

# Admin Layer

## admin.py 

Here you register models for admin dashboard this file generally configure the models on UI. 

```python 
    admin.site.register(Model_Name)

```

## Validation Layer

## validators.py 

This file is used for custom_validation. 

## tests.py 

In this file we write test cases for our application like :

- Model Tests 
- API Tests
- Permission Tests
- Integration Tests

# Startup Files

These files are required to run the application/Executes it. 

## apps.py 

Executed when app loads. 

## wsgi.py

Used for traditional deployment. [Gunicorn, uWSGI]

## asgi.py

This is modern deployment. [Websockets, Async, Channels]

# Files Senior Developer Must ADD: 

```files
services.py
repositories.py
constants.py
enums.py
utils.py
exceptions.py
selectors.py
```