# Django/Celery/Redis

## Celery using Django

1. poetry add celery

Celery has great docs on integrating with Django.

[First Steps](https://docs.celeryproject.org/en/stable/getting-started/first-steps-with-celery.html#first-steps-with-celery)

[Django First Steps](https://docs.celeryproject.org/en/stable/django/first-steps-with-django.html#index-0)

## Redis

1. brew install redis (is you don't already have it installed)
2. poetry add redis
3. brew services start redis
4. redis-cli to check connection
5. celery -A 'project_name' worker -l info

[Celery/Redis Documentation](https://docs.celeryproject.org/en/stable/getting-started/backends-and-brokers/redis.html)