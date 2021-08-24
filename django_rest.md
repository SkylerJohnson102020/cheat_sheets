# Django REST

1. poetry add djangorestframework
2. add 'rest_framework' to INSTALLED_APPS in settings.py
3. add the following to the bottom of settings.py:

        REST_FRAMEWORK = {
            'DEFAULT_PERMISSION_CLASSES': [
                'rest_framework.permissions.AllowAny'   #<------ (or .IsAuthenticated, IsAuthenticatedOrReadOnly)
            ]
        }

4. Go to project level urls.py and add path. Follow the last instruction in doc string.

        path("api/v1/appname/", include('appname.urls'))
        path("api-auth/", include("rest_framework.urls"))


5. Create urls.py at app level

        from django.urls import path
        from .views import HikeList, HikeDetail

        urlpatterns = [
            path("", HikeList.as_view(), name='hike_list'),
            path("<int:pk>/", HikeDetail.as_view(), name='hike_detail'),
        ]


6. Go to views.py

        from rest_framework.generics import ListCreateAPIView, RetrieveUpdateDestroyAPIView
        from .serializers import HikeSerializer
        from .models import Hike

        class HikeList(ListCreateAPIView):
            queryset = Hike.objects.all()
            serializer_class = HikeSerializer

        class HikeDetail(RetrieveUpdateDestroyAPIView):
            queryset = Hike.objects.all()
            serializer_class = HikeSerializer

7. Create serializers.py at app level

        from rest_framework.serializers import ModelSerializer
        from .models import Hike

        class HikeSerializer(ModelSerializer):
            class Meta:
                fields = ('id', 'author', 'trail_name', 'description', 'created_at') #<----(put `__all__` if all fields desired.)
                model = Hike


### Permissions

1. Go to app level and create permissions.py

        from rest_framework.permissions import BasePermission, SAFE_METHODS

        class IsAuthorOrReadOnly(BasePermission):
            def has_object_permission(self, request, view, obj):

                if request.method in SAFE_METHODS:
                    return True

                return obj.author == request.user

2. Go to views.py at app level and update with the following:

        from rest_framework.generics import ListCreateAPIView, RetrieveUpdateDestroyAPIView
        from .serializers import HikeSerializer
        from .models import Hike
        from .permissions import IsAuthorOrReadOnly #<----- UPDATE

        class HikeList(ListCreateAPIView):
            queryset = Hike.objects.all()
            serializer_class = HikeSerializer

        class HikeDetail(RetrieveUpdateDestroyAPIView):
            permission_classes = (IsAuthorOrReadOnly,) #<---- UPDATE. NOTE: the comma used to set tuple.
            queryset = Hike.objects.all()
            serializer_class = HikeSerializer

### JSON Web Tokens

1. poetry add djangorestframework-simplejwt
2. poetry export -o requirements.txt --without-hashes
3. Go to settings.py and update REST_FRAMEWORK:

        REST_FRAMEWORK = {
            'DEFAULT_PERMISSION_CLASSES': [
                'rest_framework.permissions.IsAuthenticated'
            ],
            "DEFAULT_AUTHENTICATION_CLASSES": [
                'rest_framework_simplejwt.authentication.JWTAuthentication',
                'rest_framework.authentication.SessionAuthentication',
                'rest_framework.authentication.BasicAuthentication',
            ]
        }

4. Go to project level urls.py and add:

        from rest_framework_simplejwt import views as jwt_views

        path("api/token/", jwt_views.TokenObtainPairView.as_view(), name='token_obtain_pair'),
        path("api/token/refresh/", jwt_views.TokenRefreshView.as_view(), name='token_refresh'),

### Gunicorn

1. poetry add gunicorn
2. poetry export -o requirements.txt --without-hashes  
3. update docker-compose file

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
                # command: python manage.py runserver 0.0.0.0:8000
                command: gunicorn nameof_project.wsgi:application --bind 0.0.0.0:8000 --workers 4
                volumes:
                    - .:/code
                ports:
                    - "8000:8000"
                depends_on:
                    - db
                    
        volumes:
            postgres_data:

4. refresh browser
5. poetry add whitenoise
6. Go to settings.py and add:

        MIDDLEWARE = [
            'django.middleware.security.SecurityMiddleware',
            'whitenoise.middleware.WhiteNoiseMiddleware', #<-------- Make sure this appear on 2ND LINE!!
            ........

7. Still in settings.py, add this underneath STATIC_URL near bottom:

        STATIC_ROOT = BASE_DIR / 'staticfiles'

8. docker-compose run web python manage.py collectstatic
9. `python -c “import secrets; print(secrets.token_urlsafe())"` to generate secret key.

