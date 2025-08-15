# django-starter
A starting point for a django application.  This starter uses docker to build a container that has python3 and django in it.

To use this, first decide if you want to name your application.   If so, you might want to replace the word `app` with your application name in the `docker-compose.yml` file.

1. First build the docker image that has python, django, and postgres client in it, and any other dependencies you list in the `requirements.txt` file.

    docker compose build backend

2. You can now start a Django project by running a command to have Django start the project.  In the example below, the project is named `myapp`.

    docker compose run backend django-admin startproject myapp

3. In the `myapp/myapp/settings.py` file, replace or modify the `DATABASES` section to connect to postgres (or mysql if you prefer).

    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql',
            'NAME': 'postgres',
            'USER': 'postgres',
            'HOST': 'db',
            'PASSWORD': 'password',
            'PORT': 5432
        }
    }


4. Creating a database, user and replacing those in the settings file is left as an exercise for the reader.

5. Create the database schema and a superuser.

    docker compose run backend wait-for-it.sh db:5432 -- python /app/myapp/manage.py migrate --noinput
    docker compose run backend wait-for-it.sh db:5432 -- python /app/myapp/manage.py createsuperuser

6. Now you can create Django apps

    docker compose run backend wait-for-it.sh db:5432 -- python /app/myapp/manage.py startapp search

7. To run the application and test it locally

    docker compose up

8. The backend will be listening to port 8000. You can test by hitting http://localhost:8000 or hit the admin at http://localhost:8000/admin/ and login with the superuser you created above.


This project uses `wait-for-it` from https://github.com/vishnubob/wait-for-it
