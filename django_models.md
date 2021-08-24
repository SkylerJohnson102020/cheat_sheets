# Models

1. Go to models.py at app level.
2. Create your model. example below:

        from django.db import models
        from django.contrib.auth import get_user_model

        class Hike(models.Model):
            author = models.ForeignKey(get_user_model(), on_delete=models.CASCADE)
            trail_name = models.CharField(max_length=100)
            description = models.TextField(max_length=500)
            created_at = models.DateTimeField(auto_now_add=True)
            updated_at = models.DateTimeField(auto_now=True)

            def __str__(self):
                return self.trail_name #(example: return self.name[:50] for first 50 chars.)

3. python manage.py makemigrations
4. python manage.py migrate
5. Go to admin.py to register model:

        from .models import 'Model name'
        from django.contrib import admin

        admin.site.register(Model name)

6. python manage.py createsuperuser and follow the steps
7. python manage.py runserver and go to /admin to go into admin panel