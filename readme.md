## To Get started with creating restful APIs with Django

1. first, Install/Configure Django Rest Framework (DRF), helps build the RESTful API:

        pip install django djangorestframework
        django-admin startproject <project_name>
        cd <project_name>
        django-admin startapp <app_name>

2. Add rest_framework & '<app_name>' & 'rest_framework.authtoken'  to INSTALLED_APPS:

        INSTALLED_APPS = [
            ...others
            'rest_framework',
            'myapp',
            'rest_framework.authtoken',
        ]

3. Database Configuration
We configured the project to use PostgreSQL instead of the default SQLite:
Install 

        pip install psycopg2-binary
        pip install psycopg

NB:You can choose between psycopg2-binary or psycopg.

Update DATABASES settings in  <project_name>/settings.py:

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql',
            'NAME': 'your_db_name',
            'USER': 'your_db_user',
            'PASSWORD': 'your_db_password',
            'HOST': 'localhost',
            'PORT': '5432',
        }
    }

## Important: If you don't have your python env 
If you are not already using a virtual environment, it is a good practice to create one to manage dependencies:

        python -m venv <myenv>
        source <myenv>/bin/activate

4. Run Migrations
We ran the migrations to create the necessary database tables:

        Generate migration files: 
            python manage.py makemigrations
        Apply migrations: 
            python manage.py migrate

5. Create Serializers
We created serializers to convert Django model instances to JSON and validate data:

        Check the serializers.py in <app_name> 

6. Create Views
We created views to handle user registration and login:

        Check the views.py in <app_name>

7. Set Up URLs
We defined URL patterns to route requests to the appropriate views:

        Check the urls.py in <app_name>

## Include <app_name> URLs in the main urls.py in <project_name>:
        Check the urls.py in <project_name>

8. Handle Root URL: To handle the root URL and prevent a 404 error:

        Create a simple root view in <app_name>/views.py:
        Check the views.py in <app_name>

        Update <project_name>/urls.py to include the root URL:
        Check the urls.py in <project_name>:


curl -X POST http://127.0.0.1:8000/api/register/ -H "Content-Type: application/json" -d '{"username": "testuser", "email": "test@example.com", "password": "password123"}'

        {
            "username": 
            "testuser", 
            "email": "test@example.com", 
            "password": "password123"
        }


curl -X POST http://127.0.0.1:8000/api/login/ -H "Content-Type: application/json" -d '{"username": "testuser", "password": "password123"}'

        {
            "username": "testuser", 
            "password": "password123"
        }


if you clone my project, you need to run the
        pip install -r requirements.txt
to install the dependencies needed for this app

A common practice is to list all your project dependencies 
in a requirements.txt file. You can create this file by running:
        pip freeze > requirements.txt



Now to host your django project
Install Gunicorn:

Gunicorn is a Python WSGI HTTP Server for UNIX. It will serve your Django application.
pip install gunicorn

A Procfile is a text file in the root directory of your application that tells Render how to run your application.

web: gunicorn myproject.wsgi


4. Create a runtime.txt:

Specify the Python version you want to use by creating a runtime.txt file in the root directory of your project.


5.Configure settings.py:

Update your settings.py to support Render's environment:

Allow all hosts:
ALLOWED_HOSTS = ['*']

Configure static files:
import os

STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

Use dj-database-url to parse the database URL from environment variables:
pip install dj-database-url

import dj_database_url

DATABASES['default'] = dj_database_url.config(conn_max_age=600, ssl_require=True)




Set DEBUG to False for production:
DEBUG = False


Install whitenoise:

Whitenoise helps with serving static files.

pip install whitenoise


Update MIDDLEWARE in settings.py:
MIDDLEWARE = [
    'whitenoise.middleware.WhiteNoiseMiddleware',
    # ... other middleware
]

Update STATICFILES_STORAGE:
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'