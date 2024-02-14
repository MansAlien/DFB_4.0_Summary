<h1 align='center'> :fire: CH 7 :fire: <br> 👉 User Accounts 👈</h1>

## 1. Log In
1. :link: update the `project/urls.py`: 
   ```
   from django.contrib import admin
   from django.urls import path, include

   urlpatterns = [
      path("admin/", admin.site.urls),
      path("accounts/", include("django.contrib.auth.urls")),
      path("", include("blog.urls")),
   ] 
   ```
2. :page_facing_up: create the `tempaltes/registeration` folder and create the `login.html` file
3. add the following code to the `login.html` file:
   
   ```
   <!-- templates/registration/login.html -->
   {% extends "base.html" %}
   {% block content %}
      <h2>Log In</h2>
      <form method="post">{% csrf_token %}
         {{ form.as_p }}
         <button type="submit">Log In</button>
      </form>
   {% endblock content %}
   ```
4. ⚙️ The final step is we need to specify where to redirect the user upon a successful log in. update the `project/settings.py` file:
   
   ```
   # project/settings.py
   LOGIN_REDIRECT_URL = "home"
   ```

## 2. Update Homepage :page_facing_up:
```
<!-- templates/base.html -->
{% load static %}
<html>
   <head>
      <title>Django blog</title>
      <link href="https://fonts.googleapis.com/css?family=Source+Sans+Pro:400"
         rel="stylesheet">
      <link href="{% static 'css/base.css' %}" rel="stylesheet"s>
   </head>
   <body>
      <div>
         <header>
            <div class="nav-left">
               <h1><a href="{% url 'home' %}">Django blog</a></h1>
            </div>
            <div class="nav-right">
               <a href="{% url 'post_new' %}">+ New Blog Post</a>
            </div>
         </header>
         {% if user.is_authenticated %}
            <p>Hi {{ user.username }}!</p>
         {% else %}
            <p>You are not logged in.</p>
            <a href="{% url 'login' %}">Log In</a>
         {% endif %}
         {% block content %}
         {% endblock content %}
      </div>
   </body>
</html>
```
## 3. Log Out
1. :page_facing_up: update the `templates/base.html` file:
   ```
   <!-- templates/base.html-->
   ...
   {% if user.is_authenticated %}
      <p>Hi {{ user.username }}!</p>
      <p><a href="{% url 'logout' %}">Log out</a></p>
   {% else %}
   ...
   ```
   ### :pushpin: note:
   - In `django 4.0` : this code will work correctly
   - In `django 5.0` : The "logout url will make an error :x: so to solve this problem we need to repace the url of the logout `<a>...</a>` with the following code:
      ```
      <form method="post" action="{% url 'logout' %}">
         {% csrf_token %}
         <button type="submit" class="dropdown-item">Log Out</button>
      </form>
      ```

2. ⚙️ The final step is we need to specify where to redirect the user upon a successful log out. update the `project/settings.py` file:
   
   ```
   # project/settings.py
   LOGIN_REDIRECT_URL = "home"
   LOGOUT_REDIRECT_URL= "home"
   ```

## 4. Sign Up
1. start the accounts app: 
   - create the accounts app: `python manage.py startapp accounts`
   - add it to the `project/settings.py`: `accounts.apps.AccountsConfig`
2. :link: update the `project/urls.py`:
3. 
   ```
   # project/urls.py
   from django.contrib import admin
   from django.urls import path, include
   urlpatterns = [
      path("admin/", admin.site.urls),
      path("accounts/", include("django.contrib.auth.urls")),
      path("accounts/", include("accounts.urls")), 
      path("", include("blog.urls")),
   ]
   ```

4. :link: accounts app `urls` :
   - create the `accounts/urls.py` file
   - add the following code:
   ```
   # accounts/urls.py
   from django.urls import path
   from .views import SignUpView

   urlpatterns = [
      path("signup/", SignUpView.as_view(), name="signup"),
   ]
   ```
5. :eyes: singup views:
   - create the `accounts/views.py` file
   - add the following code:
   ```
   # accounts/views.py
   from django.contrib.auth.forms import UserCreationForm
   from django.urls import reverse_lazy
   from django.views.generic import CreateView

   class SignUpView(CreateView):
      form_class = UserCreationForm
      success_url = reverse_lazy("login")
      template_name = "registration/signup.html"
   ```

6. :page_facing_up: signup templates:
   - create the `templates/registration/signup.html` file
   - add the following code:
   ```
   <!-- templates/registration/signup.html -->
   {% extends "base.html" %}
   {% block content %}
      <h2>Sign Up</h2>
      <form method="post">
         {% csrf_token %}
         {{ form.as_p }}
         <button type="submit">Sign Up</button>
      </form>
   {% endblock content %}
   ```
7. :page_facing_up:update the `templates/base.html`:
   ```
   <!-- templates/base.html-->
   ...
   <p>You are not logged in.</p>
   <a href="{% url 'login' %}">Log In</a> |
   <a href="{% url 'signup' %}">Sign Up</a>
   ...
   ```

## 5. Static Files
1. ⚙️ update the settings file:
   ```
   # project/settingts.py
   STATIC_URL = "/static/"
   STATICFILES_DIRS = [BASE_DIR / "static"]
   STATIC_ROOT = BASE_DIR / "staticfiles"
   STATICFILES_STORAGE = "django.contrib.staticfiles.storage.StaticFilesStorage"
   ```
2. run this command : `python manage.py collectstatic`

## 6. Git & GitHub
same as before

## 7. Deployment
- i will use vercel.
- *William S. Vincent* used Heroku, but Heroku needes a credit card and a lot of headaches.
- what is the advantage of vercel:
    - it's free :smile:
- [*how to use vercel?*](https://github.com/MansAlien/DFB_Revision/blob/main/important/vercel.md)


<br>
🔥 [Next -> Chapter_8](https://github.com/MansAlien/DFB_4.0_Summary/blob/main/Ch_8-Custom_user.md) 🔥
<br>
<br>
<h2 align="center"> :smile: Done :smile: </h2>