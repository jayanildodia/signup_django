# Django Sign Up and login with confirmation Email | Python
Django by default provides an authentication system configuration. User objects are the core of the authentication system. Today we will implement Django’s authentication system. 

Modules required: Django install, crispy_forms
Django Sign Up and Login with Confirmation Email

To install crispy_forms you can use the terminal command:

```
pip install --upgrade django-crispy-forms
```

Start a project with the following command –

```
django-admin startproject project
```

Change directory to project –

```
cd project
```

Start the server- Start the server by typing the following command in the terminal –

```
python manage.py runserver
```

To check whether the server is running or not go to a web browser and enter http://127.0.0.1:8000/ as URL. and to stop the server press keys

```
ctrl+c
```

Let’s create an app now called the “user”. 

```
python manage.py startapp user
```

Goto user/ folder by doing: cd user and create a folder templates with files index.html, login.html, Email.html, register.html files.

![Screenshot-from-2019-07-29-10-03-46](https://github.com/jayanildodia/signup_django/assets/32728591/50250d78-2dea-4ecf-b3f2-9f526266b98f)


