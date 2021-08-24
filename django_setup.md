# Django setup steps

1. mkdir 'name'
2. cd into new folder
3. poetry init -n
4. poetry add django 
5. poetry add --dev black flake8
6. poetry shell

7. django-admin startproject 'project name' .
8. python manage.py startapp 'name'
9. python manage.py runserver
10. python manage.py migrate
11. code .

12. Go to settings.py add app name to INSTALLED_APPS

13. Models
14. Views
15. Urls
16. Templates if needed
17. REST

18. python manage.py createsuperuser and follow steps (in [django_models](django_models.md))

#### Additional commands
django-admin
tree
python manage.py test