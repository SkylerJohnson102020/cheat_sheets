# Postgres

Make sure Docker is set up in project. It is a good rule of thumb to get everything working on sqlite3 before switching to postgres.

1. docker-compose.yml should look like:

        version: '3'
        services:
            db:
                image: postgres:11
                environment:
                    - POSTGRES_DB=postgres
                    - POSTGRES_USER=postgres
                    - POSTGRES_PASSWORD=postgres
                volumes:
                    - postgres_data:/var/lib/postgresql/data/
            web:
                build: .
                command: python manage.py runserver 0.0.0.0:8000
                volumes:
                    - .:/code
                ports:
                    - "8000:8000"
                depends_on:
                    - db
        volumes:
            postgres_data:

2. Go to settings.py and go to DATABASES.
3. Update with code below:

        DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.postgresql',
                'NAME': 'postgres',
                'USER': 'postgres',
                'PASSWORD': 'postgres',
                'HOST': 'db',
                'PORT': 5432,
            }
        }

4. docker-compose run web
5. docker-compose run web python manage.py createsuperuser (if needed)
6. Update settings.py:

        ALLOWED_HOSTS = ['0.0.0.0','localhost','127.0.0.1']

7. Update pyproject.toml, go to tool.poetry.dependencies and add:

        psycopg2-binary = "^2.8.5"

8. poetry export -o requirements.txt --without-hashes
9. requirements.txt should look like:

        asgiref==3.4.0; python_version >= "3.6"
        django==3.2.4; python_version >= "3.6"
        djangorestframework==3.12.4; python_version >= "3.5"
        psycopg2-binary==2.9.1; python_version >= "3.6"
        pytz==2021.1; python_version >= "3.6"
        sqlparse==0.4.1; python_version >= "3.6"