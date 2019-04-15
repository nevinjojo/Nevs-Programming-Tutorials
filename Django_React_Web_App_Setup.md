# Django & React Web App Setup Instructions

This document describes how to build a basic web application with Django and React.

## Tools
Some tools that you'll be using during this tutorial include:

* React, a JavaScript framework that allows developers to build web and native frontends for their REST API backends.
* Django, a free and open-source Python web framework that follows the model view controller (MVC) software architectural pattern.
* Django REST framework, a powerful and flexible toolkit for building REST APIs in Django.

## Step 1 — Creating a Python Virtual Environment and Installing Dependencies

#### Create Virtual Environment
```
python3 -m venv ./env
source env/bin/activate

```

#### Install Django
```
pip install django djangorestframework django-cors-headers
```

## Step 2 - Create a Django Project
#### Create a project named cesp

`django-admin startproject myProject`

#### tree
Check if the project was set up properly using 'tree'

```
sudo apt-get install tree
cd myProject
tree
```
The output will be the following

```
Output
├── myProject
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py
```

#### Configure packages installed
`vim myProject/myProject/settings.py`

* Add the following inside the `INSTALLED_APPS` block:

* ```
'rest_framework',
'corsheaders'
```

* Add the following inside the `MIDDLEWARE` block:

* ```
'corsheaders.middleware.CorsMiddleware'
```

* Add the following anywhere in the settings.py file:

```
CORS_ORIGIN_ALLOW_ALL = False
CORS_ORIGIN_WHITELIST = (
       'localhost:3000',
)
```

Navigate to root folder and make a Django application

`python manage.py startapp myApp`

This will contain the models and views for managing customers.

#### More configurations
Now navigate back to `cesp/cesp` and open `settings.py`

Add the following to the bottom of `INSTALLED_APPS` block and save it:

`'myApp'`

Navigate back to the root directory and run the following command to migrate the local database:
`python manage.py migrate`

Now run the local development server:
`python manage.py runserver`

Congrats! Your web application will be running from [http://127.0.0.1:8000](http://127.0.0.1:8000). Check it out!
