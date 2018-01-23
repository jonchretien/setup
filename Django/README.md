# [Django](https://www.djangoproject.com/)

Quick install:
```
cd projects
mkdir mydjangoproject && cd "$_"
virtualenv venv
source venv/bin/activate
pip3 install Django==2.X.X
```

Setup a new project (add the new app to the `settings.py` file before proceeding):
```
python manage.py migrate
django-admin.py startproject mydjangoproject .
django-admin.py startapp mydjangoapp
python manage.py runserver
```
