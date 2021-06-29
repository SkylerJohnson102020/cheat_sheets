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

4. poetry export -o requirements.txt -without-hashes
(if that doesn't work, poetry export -o requirements.txt) Code should like like this:

        asgiref==3.4.0; python_version >= "3.6"
        django==3.2.4; python_version >= "3.6"
        djangorestframework==3.12.4; python_version >= "3.5"
        pytz==2021.1; python_version >= "3.6"
        sqlparse==0.4.1; python_version >= "3.6"

5. make docker-compose.yml at root tevel:

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

8. docker-compose up --build, if you need to rebuild.