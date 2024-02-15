<h1 align='center'> :fire: CH 10 :fire: <br> 👉 Bootstrap 👈</h1>

## 1. Pages App
1. create the app:
   - start the pages app: `python manage.py startapp pages`
   - add it to the `project/settings.py` -> `pages.apps.PagesConfig`
2. :link: update the urls of the `project/urls.py`:
   ```
   # project/urls.py
   from djnago.urls import path, include
   from dajngo.contrib import admin

   urlpatterns = [
        path("admin/", admin.site.urls),
        path("accounts/", include("accounts.urls")),
        path("accounts/", include("django.contrib.auth.urls")),
        path("", include("pages.urls")),
   ]
   ```
3. :link: create the `pages/urls.py`:
   ```
   # pages/urls.py
   from django.urls import path
   from .views import HomePageView

   urlpatterns = [
        path("", HomePageview.as_view(), name="home")
   ]
   ```
   
4. :eyes: add the views:
   ```
   # pages/views.py
   from django.views.generic import TemplateView

   class HomePageview(TemplateView):
        template_name = "templates/home.html"
   ```
## Tests :mag_right:
```
# pages/tests.py
from django.test import SimpleTestCase
from django.urls import reverse

class HomePageTests(SimpleTestCase):
    def test_url_exists_at_correct_location_homepageview(self):
        response = self.client.get("/")
        self.assertEqual(response.status_code, 200)

    def test_homepage_view(self):
        response = self.client.get(reverse("home"))
        self.assertEqual(response.status_code, 200)
        self.assertTemplateUsed(response, "home.html")
        self.assertContains(response, "Home")
```
## Bootstrap
1. There are two ways to install bootstrap in django:
    1. use it online: go to the bootstrap website and look at the steps
    2. install it locally (offline):
        - follow the steps in this tutorial that i made for you
        - [*Install Bootstrap*](https://github.com/MansAlien/DFB_Revision/blob/main/important/bootstrap.md)
    3. the finall resulte of `templates/base.html` file:

        #### :pushpin: note: We are using bootstrap locally 
        ```
        <!-- templates/base.html -->
        <!DOCTYPE html>
        <html>
            <head>
                <meta charset="utf-8">
                <title>{% block title %}Newspaper App{% endblock title %}</title>
                <meta name="viewport" content="width=device-width,
                initial-scale=1, shrink-to-fit=no">
                <!-- Bootstrap Framework -->
                <link rel="stylesheet" href="{% static 'css/bootstrap.min.css' %}" />
            </head>
            <body>
                <main>
                    {% block content %}
                    {% endblock content %}
                </main>
                <!-- Bootstrap JavaScript Bundle -->
                <script src="{% static 'js/bootstrap.bundle.min.js' %}"></script>
            </body>
        </html>
        ```
    4. update the `templates/base.html` to be like this:
        #### :pushpin: note: i updated this code.
        ```
        <!-- templates/base.html -->
        ...
        <body>
            <div class="container">
                <header class="p-3 mb-3 border-bottom">
                    <div class="d-flex flex-wrap align-items-center justify-content-center
                        justify-content-lg-start">
                        <a class="navbar-brand" href="{% url 'home' %}">Newspaper</a>
                        <ul class="nav col-12 col-lg-auto me-lg-auto mb-2 justify-content-center
                            mb-md-0">
                            {% if user.is_authenticated %}
                            <li><a href="#" class="nav-link px-2 link-dark">+ New</a></li>
                        </ul>
                        <div class="dropdown text-end">
                            <a href="#" class="d-block link-dark text-decoration-none dropdown-toggle" id="dropdownUser1"
                                data-bs-toggle="dropdown" aria-expanded="false">
                                {{ user.username }}
                            </a>
                            <ul class="dropdown-menu text-small" aria-labelledby="dropdownUser1">
                                <li><a class="dropdown-item" href="{% url 'password_change' %}">
                                        Change password</a></li>
                                <li><a class="dropdown-item" href="{% url 'logout' %}">Log Out</a></li>
                            </ul>
                        </div>
                        {% else %}
                        </ul>
                        <div class="text-end">
                            <a href="{% url 'login' %}" class="btn btn-outline-primary me-2">
                                Log In</a>
                            <a href="{% url 'signup' %}" class="btn btn-primary">Sign Up</a>
                        </div>
                        {% endif %}
                    </div>
                </header>
                <main>
                    {% block content %}
                    {% endblock content %}
                </main>
            </div>
            ...
        ```
    5. update the `templates/registration/singup.html` to be like this:
        ```
        <!-- templates/registration/signup.html -->
        {% extends "base.html" %}
        {% load crispy_forms_tags %}
        {% block title %}Sign Up{% endblock title%}
        {% block content %}
            <h2>Sign Up</h2>
            <form method="post">
                {% csrf_token %}
                {{ form|crispy }}
                <button class="btn btn-success" type="submit">Sign Up</button>
            </form>
        {% endblock content %}
        ```
    6. update the `templates/registration/login.html` to be like this:
        ```
        <!-- templates/registration/login.html -->
        {% extends "base.html" %}
        {% load crispy_forms_tags %}
        {% block title %}Log In{% endblock title %}
        {% block content %}
            <h2>Log In</h2>
            <form method="post">
                {% csrf_token %}
                {{ form|crispy }}
                <button class="btn btn-success ml-2" type="submit">Log In</button>
            </form>
        {% endblock content %}
        ```

<br>

🔥 [Next -> Chapter_11]() 🔥

<br>
<br>
<h2 align="center"> :smile: Done :smile: </h2>