<h1 align='center'> :fire: CH 9 :fire: <br> 👉 User Authentication 👈</h1>

## Templates :page_facing_up:
1. :file_folder: create the `templates` directory
2. :file_folder: create the `templates/registraion` directory
3. ⚙️ adding the path of the templates to the `project/settings.py` file
4. ⚙️ adding the (login, logout) redirection urls to the `project/settings.py` file:
   ```
   LOGIN_REDIRECT_URL = 'home'
   LOGOUT_REDIRECT_URL = 'home'
   ```
5. :page_facing_up: create the templates:
   - `templates/base.html`
        ```
        <!-- templates/base.html -->
        <!DOCTYPE html>
        <html>
            <head>
                <meta charset="utf-8">
                <title>{% block title %}Newspaper App{% endblock title %}</title>
            </head>
            <body>
                <main>
                    {% block content %}
                    {% endblock content %}
                </main>
            </body>
        </html>
        ```
   - `templates/home.html`
        ```
        <!-- templates/home.html -->
        {% extends "base.html" %}
        {% block title %}Home{% endblock title %}
        {% block content %}
            {% if user.is_authenticated %}
                Hi {{ user.username }}!
                <p><a href="{% url 'logout' %}">Log Out</a></p>
            {% else %}
                <p>You are not logged in</p>
                <a href="{% url 'login' %}">Log In</a> |
                <a href="{% url 'signup' %}">Sign Up</a>
            {% endif %}
        {% endblock content %}
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
   - `templates/registration/login.html`
        ```
        <!-- templates/registration/login.html -->
        {% extends "base.html" %}
        {% block title %}Log In{% endblock title %}
        {% block content %}
            <h2>Log In</h2>
            <form method="post">{% csrf_token %}
                {{ form.as_p }}
                <button type="submit">Log In</button>
            </form>
        {% endblock content %}
        ```
   - `templates/registration/singup.html`
        ```
        <!-- templates/registration/signup.html -->
        {% extends "base.html" %}
        {% block title %}Sign Up{% endblock title %}
        {% block content %}
            <h2>Sign Up</h2>
            <form method="post">{% csrf_token %}
                {{ form.as_p }}
                <button type="submit">Sign Up</button>
            </form>
        {% endblock content %}
        ```
## URLs :link:
1. :link: adding the urls of the `project/urls.py` file:
    ```
    # django_project/urls.py
    from django.contrib import admin
    from django.urls import path, include 
    from django.views.generic.base import TemplateView 

    urlpatterns = [
        path("admin/", admin.site.urls),
        path("accounts/", include("accounts.urls")), 
        path("accounts/", include("django.contrib.auth.urls")), 
        path("", TemplateView.as_view(template_name="home.html"), name="home"), 
    ]
    ```

2. :link: adding the url of the singup to the `accounts/urls.py` file:
   ```
   from django.urls import path
   from django.views import SignUpView

   urlpatterns = [
        path("singup/", SingUpView.as_view(), name="singup")
   ]
   ```
3. :eyes: create the view of the singup in `accounts/views.py` file:
   ```
   # accounts/views.py
   from django.urls import reverse_lazy
   from django.views.generic import CreateView
   from .forms import CustomUserCreationForm

   class SingUpView(CreateView):
        form_class= CustomUserCreationForm
        success_url = reverse_lazy("login")
        template_name = "registration/signup.html"


   ```
4. update the `home.html` file:
    ```
    <!-- templates/home.html -->
    {% extends "base.html" %}
    {% block title %}Home{% endblock title %}
    {% block content %}
        {% if user.is_authenticated %}
            Hi {{ user.username }}! You are {{ user.age }} years old.
            <p><a href="{% url 'logout' %}">Log Out</a></p>
        {% else %}
            <p>You are not logged in</p>
            <a href="{% url 'login' %}">Log In</a> |
            <a href="{% url 'signup' %}">Sign Up</a>
        {% endif %}
    {% endblock content %}
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

## Admin
1. :page_facing_up: update the `accounts/forms.py` file:
   ```
   # accounts/forms.py
   from django.contrib.auth.forms import UserCreationForm, UserChangeForm
   from .models import CustomUser

   class CustomUserCreationForm(UserCreationForm):
        class Meta(UserCreationForm):
            model = CustomUser
            fields = (
                "username",
                "email",
                "age",
            )

    class CustomUserCreationForm(UserCreationForm):
        class Meta:
            model = CustomUser
            fields = (
                "username",
                "email",
                "age",
            )

   ```

## Tests :mag_right:
```
from django.contrib.auth import get_user_model
from django.test import TestCase
from django.urls import reverse

class SignupPageTests(TestCase):
    def test_url_exists_at_correct_location_signupview(self):
        response = self.client.get("/accounts/signup/")
        self.assertEqual(response.status_code, 200)

    def test_signup_view_name(self):
        response = self.client.get(reverse("signup"))
        self.assertEqual(response.status_code, 200)
        self.assertTemplateUsed(response, "registration/signup.html")

    def test_signup_form(self):
        response = self.client.post(
        reverse("signup"),
        {
        "username": "testuser",
        "email": "testuser@email.com",
        "password1": "testpass123",
        "password2": "testpass123",
        },
        )
        self.assertEqual(response.status_code, 302)
        self.assertEqual(get_user_model().objects.all().count(), 1)
        self.assertEqual(get_user_model().objects.all()[0].username, "testuser")
        self.assertEqual(get_user_model().objects.all()[0].email, "testuser@email.com")
```

<br>

🔥 [Next -> Chapter_10]() 🔥

<br>
<br>
<h2 align="center"> :smile: Done :smile: </h2>