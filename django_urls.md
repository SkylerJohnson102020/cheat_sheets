# URLS

1. Go to project level urls.py and add the path to urlpatterns. Follow the last instruction in doc string:
       
        path("", include('appname.urls'))

2. Go to app folder and create urls.py and add boilerplate code:

        from django.urls import path
        from .views import HikeList, HikeDetail

        urlpatterns = [
        path("", HikeList.as_view(), name='hike_list'),
        path("<int:pk>/", HikeDetail.as_view(), name='hike_detail'),
        ]