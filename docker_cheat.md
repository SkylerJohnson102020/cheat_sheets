# Docker setup

1. From command line, touch Dockerfile at top level
2. code .
3. Go to Dockerfile and enter:

        FROM python:3
        ENV PYTHONDONTWRITEBYTECODE 1
        ENV PYTHONUNBUFFERED 1
        RUN mkdir /code
        WORKDIR /code
        COPY requirements.txt /code/
        RUN pip install -r requirements.txt
        COPY . /code

4. poetry export -o requirements.txt --without-hashes
(if that doesn't work, poetry export -h for help) Code should like:

        asgiref==3.4.0; python_version >= "3.6"
        django==3.2.4; python_version >= "3.6"
        djangorestframework==3.12.4; python_version >= "3.5"
        pytz==2021.1; python_version >= "3.6"
        sqlparse==0.4.1; python_version >= "3.6"

5. touch docker-compose.yml at root tevel:

        version: '3'
        services:
            web:
                build: .
                command: python manage.py runserver 0.0.0.0:8000
                volumes: 
                    - .:/code
                ports:
                    - "8000:8000"

6. on command line, exit

7. docker-compose up

8. docker-compose up --build, if you need to rebuild. Docker containers are ephemeral.

### Additional commands

9. docker-compose down
10. add -h flag for help
11. docker-compose up --detach - runs containers in background, doesn't take up an entire server
12. docker-compose run web python manage.py makemigrations
13. docker-compose run web python manage.py migrate
14. docker-compose run web python manage.py createsuperuser
15. docker-compose run web python manage.py collectstatic 
