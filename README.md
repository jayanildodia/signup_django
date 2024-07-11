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

## Edit urls.py file in project
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

## Edit urls.py in user

Here we provide the URL path for index view and these views are connected to the main project URL file.

```
from django.urls import path, include
from django.conf import settings
from . import views
from django.conf.urls.static import static

urlpatterns = [
		path('', views.index, name ='index'),
]
```

## Edit views.py in user

Now we will provide the logic and code for the Email system in the views of user app

```
from django.shortcuts import render, redirect
from django.contrib import messages
from django.contrib.auth import authenticate, login
from django.contrib.auth.decorators import login_required
from django.contrib.auth.forms import AuthenticationForm
from .forms import UserRegisterForm
from django.core.mail import send_mail
from django.core.mail import EmailMultiAlternatives
from django.template.loader import get_template
from django.template import Context


#################### index####################################### 
def index(request):
	return render(request, 'user/index.html', {'title':'index'})

########### register here ##################################### 
def register(request):
	if request.method == 'POST':
		form = UserRegisterForm(request.POST)
		if form.is_valid():
			form.save()
			username = form.cleaned_data.get('username')
			email = form.cleaned_data.get('email')
			######################### mail system #################################### 
			htmly = get_template('user/Email.html')
			d = { 'username': username }
			subject, from_email, to = 'welcome', 'your_email@gmail.com', email
			html_content = htmly.render(d)
			msg = EmailMultiAlternatives(subject, html_content, from_email, [to])
			msg.attach_alternative(html_content, "text/html")
			msg.send()
			################################################################## 
			messages.success(request, f'Your account has been created ! You are now able to log in')
			return redirect('login')
	else:
		form = UserRegisterForm()
	return render(request, 'user/register.html', {'form': form, 'title':'register here'})

################ login forms################################################### 
def Login(request):
	if request.method == 'POST':

		# AuthenticationForm_can_also_be_used__

		username = request.POST['username']
		password = request.POST['password']
		user = authenticate(request, username = username, password = password)
		if user is not None:
			form = login(request, user)
			messages.success(request, f' welcome {username} !!')
			return redirect('index')
		else:
			messages.info(request, f'account done not exit plz sign in')
	form = AuthenticationForm()
	return render(request, 'user/login.html', {'form':form, 'title':'log in'})
```

Configure your email here.

## Now create a forms.py in user
Now with help of django form we will create a Registration page for the new user to register and this will mail to registering gmail from the gmail we mention in the settings.py file of the project.

```
from django import forms
from django.contrib.auth.models import User
from django.contrib.auth.forms import UserCreationForm

class UserRegisterForm(UserCreationForm):
	email = forms.EmailField()
	phone_no = forms.CharField(max_length = 20)
	first_name = forms.CharField(max_length = 20)
	last_name = forms.CharField(max_length = 20)
	class Meta:
		model = User
		fields = ['username', 'email', 'phone_no', 'password1', 'password2']
```

## Navigate to templates/user/ and edit files : 

## Index.html file
This file includes metadata, loads external CSS and JavaScript files (Bootstrap and Font Awesome), and uses Django template tags to handle dynamic content. The template features a navigation bar, displays alert messages, and adjusts the page content based on user authentication, showing a personalized welcome message or a login prompt. This code is designed for building user-friendly web interfaces within a Django project.

```
{% load static %}
{% load crispy_forms_tags %}
<!DOCTYPE html>
<html lang="en">

<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="title" content="project">
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<meta name="language" content="English">
<meta name="author" content="jay dodia">

<title>{{title}}</title>


<!-- bootstrap file -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" />
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
<!-- bootstrap file-->

<!-- jQuery -->
<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">


<!-- main css -->
<link rel="stylesheet" type="text/css" href="{% static "index.css" %}" />


<!-- message here -->

{% if messages %}
{% for message in messages %}

<script>
	alert("{{ message }}");
</script>

{% endfor %}
{% endif %}

<!--_______________________________________________-->



</head>

<body class="container-fluid">


<header class="row">

	<!-- navbar-->
	<nav class="navbar navbar-inverse navbar-fixed-top">
	<div class="container-fluid">
		<div class="navbar-header">
		<button class="navbar-toggle" data-toggle="collapse" data-target="#mainNavBar">
			<span class="icon-bar"></span>
			<span class="icon-bar"></span>
			<span class="icon-bar"></span>
			<span class="icon-bar"></span>
		</button>
		<a class="navbar-brand" class="styleheader" href="{% url "index" %}">project</a>
		</div>
		<div class="collapse navbar-collapse" id="mainNavBar">
		<ul class="nav navbar-nav navbar-right">
			<li><a href="{% url "index" %}">Home</a></li>

			{% if user.is_authenticated %}
			<li><a href="{% url "logout" %}"><span class="glyphicon glyphicon-log-out"></span>  Logout</a></li>
			{% else %}
			<li><a href="{% url "register" %}"><span class="glyphicon glyphicon-user"></span>  Sign up</a></li>
			<li><a href="{% url "login" %}"><span class="glyphicon glyphicon-log-in"></span>  Log in</a></li>
			{% endif %}

		</ul>
		</div>
	</div>
	</nav>
</header>
<br/>
<br>
<br>
<div class="row">
	{% block start %}
	{% if user.is_authenticated %}
	<center><h1>welcome back {{user.username}}!</h1></center>
	{% else %}
	<center><h1>log in, plz . . .</h1></center>
	{% endif %}
	{% endblock %}
</div>
</body>

</html>
```

## Email.html
The provided HTML code is an email template for a registration confirmation message. It uses the Roboto font, has a centered thank-you message with user-specific content (username), and a horizontal line for separation. This template is designed to deliver a visually pleasing and informative confirmation email to users.

```
<!DOCTYPE html>
<html lang="en" dir="ltr">
	<head>
		<meta charset="utf-8">
		<title></title>
		<style>
			@import url('https://fonts.googleapis.com/css?family=Roboto:400,100,300,500,700,900');
		</style>
	</head>
	<body style="background: #f5f8fa;font-family: 'Roboto', sans-serif;">
		<div style="width: 90%;max-width:600px;margin: 20px auto;background: #ffffff;">
			<section style="margin: 0 15px;color:#425b76;">
				<h2 style="margin: 40px 0 27px 0;text-align: center;">Thank you to registration</h2>
				<hr style="border:0;border-top: 1px solid rgba(66,91,118,0.3);max-width: 50%">
				<p style="font-size:15.5px;font-weight: bold;margin:40px 20px 15px 20px;">Hi {{username}}, we have received your details and will process soon.</p>
			</section>
		</div>
	</body>
</html>
```

## Login.html
Inside this block, it creates a centered login form with specific styling, including a black border, padding, and a rounded border. The form includes a CSRF token for security and uses the crispy filter to render form fields with enhanced formatting, along with a login button and a link to the registration page.

```
{% extends "user/index.html" %}
{% load crispy_forms_tags %}
{% block start %}

<div class="content-section col-md-8 col-md-offset-2">
<center>
<form method="POST" style="border: 1px solid black; margin: 4%; padding:10%; border-radius:1%;">
	{% csrf_token %}
	<fieldset class="form-group">
	{{ form|crispy}}
	</fieldset>
<center>
	<button style="background: black; font-size: 2rem; padding:1%;" class="btn btn-outline-info" type="submit"><span class="glyphicon glyphicon-log-in"></span>  login</button>
</center>
<br/>
<sub style="text-align: left;"><a href="{% url 'register' %}" style="text-decoration: none; color: blue; padding:2%; cursor:pointer; margin-right:2%;">don't have account,sign up</a></sub>
</form>
</center>
</div>
{% endblock start %}
```

## Register.html
This file creates a centered sign-up form with specific styling, including a black border, padding, and rounded corners. The form includes a CSRF token for security and uses the crispy filter for enhanced form field rendering, along with a sign-up button and a link to the login page for users with existing accounts.

```
{% extends "user/index.html" %}
{% load crispy_forms_tags %}
{% block start %}

<div class="content-section col-md-8 col-md-offset-2">
<form method="POST" style="border: 1px solid black; margin: 4%; padding:10%; border-radius:1%;">
	{% csrf_token %}
	<fieldset class="form-group">
	{{ form|crispy}}
	</fieldset>
	<center>
	<button style="background: black; padding:2%; font-size: 2rem; color:white;" class="btn btn-outline-info" type="submit"><span class="glyphicon glyphicon-check"></span>  sign up</button>
	</center>
	<br />
	<sub><a href="{% url "login" %}" style="text-decoration: none; color: blue; padding:3%; cursor:pointer;">Already have an account ?</a></sub>
</form>
</div>
{% endblock start %}
```

Make migrations and migrate them. 

```
python manage.py makemigrations
```
```
python manage.py migrate
```

Now you can run the server to see your app.

```
python manage.py runserver
```

And Voila, your login, signup page is ready!
![image](https://github.com/user-attachments/assets/f9a9f59d-2ea6-49f9-9ec1-3c63dabf33e2)
