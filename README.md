# Django Sign Up and login with confirmation Email | Python
Django by default provides an authentication system configuration. User objects are the core of the authentication system. Today we will implement Django’s authentication system. 

Modules required: Django install, crispy_forms
Django Sign Up and Login with Confirmation Email

## To install crispy_forms you can use the terminal command:

```
pip install --upgrade django-crispy-forms
```

### Start a project with the following command –

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

Open the project folder using a text editor. The directory structure should look like this :

![image](https://github.com/jayanildodia/signup_django/assets/32728591/40ca736f-ae55-4333-87d3-b2fb8d987b63)

Now add the “user” app and “crispy_form” in your todo_site in settings.py, and add 

```
CRISPY_TEMPLATE_PACK = 'bootstrap4'
```

at last of settings.py 

![image](https://github.com/jayanildodia/signup_django/assets/32728591/0bf07834-8835-46bb-8d51-28a7264cf01b)

configure email settings in setting.py:

![image](https://github.com/jayanildodia/signup_django/assets/32728591/32c1eddd-6cda-483e-a32a-1a6037c66ad7)

place your email and password here.

Edit urls.py file in project
In this file we provide the path for the login,logout, register page and include the user.urls to the main project URL file:

```
from django.contrib import admin
from django.urls import path, include
from user import views as user_view
from django.contrib.auth import views as auth

urlpatterns = [

	path('admin/', admin.site.urls),

	##### user related path########################## 
	path('', include('user.urls')),
	path('login/', user_view.Login, name ='login'),
	path('logout/', auth.LogoutView.as_view(template_name ='user/index.html'), name ='logout'),
	path('register/', user_view.register, name ='register'),

]

```

Edit urls.py in user

Here we provide the URL path for index view and these views are connected to the main project URL file.
