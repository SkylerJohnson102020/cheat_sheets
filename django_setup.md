# Django setup steps

1. mkdir 'name'
2. cd into new folder
3. poetry init -n
4. poetry add django 
5. poetry add --dev black flake8
6. poetry shell

7. django-admin startproject 'project name' .
8. python manage.py migrate
9. python manage.py startapp 'name'
10. python manage.py runserver
11. code .

12. Go to settings.py add app name to INSTALLED_APPS

#### Optional commands
django-admin
tree