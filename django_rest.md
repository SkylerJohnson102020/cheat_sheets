# Django REST

1. poetry add djangorestframework
2. add 'rest_framework' to INSTALLED_APPS in settings.py
3. add the following to the bottom of settings.py:

        REST_FRAMEWORK = {
            'DEFAULT_PERMISSION_CLASSES': [
                'rest_framework.permissions.AllowAny'
            ]
        }

4. Go to project level urls.py and add path. Follow the last instruction in doc string.

        path("api/v1/appname", include('appname.urls'))


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

        from rest_framework import serializers
        from .models import Hike

        class HikeSerializer(serializers.ModelSerializer):
            class Meta:
                fields = ('id', 'author', 'trail_name', 'description', 'created_at')
                model = Hike
