<h1 align='center'>🔥 CH 3 🔥 <br> 👉 Pages App 👈</h1>

## 1. Initial Set Up
- all like the previous app
- just don't forget to add the app to the settings: `settings.py`

## 2. Templates
1. create templates file: `mkdir templates`
2. update the settings file: 
    ```
    # project/settings.py
    TEMPLATES= [
        {
            ...
            "DIRS": [BASE_DIR / "templates"],
            ...
        },
    ]
    ```
3. create the template file: `touch templates/home.html`
4. add the content of the home.html file:
    ```
    <h1>Homepage</h1>
    ```
## 3. Class_Based Views
- The TemplateView already contains all the logic needed
to display our template, we just need to specify the template’s name.
- if you need more about class based views: 
    - [django 4.0](https://docs.djangoproject.com/en/4.0/topics/class-based-views/intro/)
    - [django 5.0](https://docs.djangoproject.com/en/5.0/topics/class-based-views/intro/)
- if you need more about TemplateView: 
    - [django 4.0](https://docs.djangoproject.com/en/4.0/ref/class-based-views/base/#django.views.generic.base.TemplateView)
    - [django 5.0](https://docs.djangoproject.com/en/5.0/ref/class-based-views/base/#django.views.generic.base.TemplateView)

## 4. URLs
1. adding the app urls to the project urls: 
    ```
    # project/urls.py
    from django.contrib import admin
    from django.urls import path, include # new
    urlpatterns = [
        path("admin/", admin.site.urls),
        path("", include("pages.urls")), # new
    ]
    ```
2. adding the app urls:
    ```
    # pages/urls.py
    from django.urls import path
    from .views import HomePageView
    urlpatterns = [
        path("", HomePageView.as_view(), name="home"),
    ]
    ```
## 5. AboutPage
- just like what we did with the homepage.
- don't be lazy and do it by yourself. :muscle: :fire:

## 6. Extending Templates
- first of all, have a look at the built-in url template tag:
    - [django 4.0](https://docs.djangoproject.com/en/4.0/ref/templates/builtins/#url)
    - [django 5.0](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#url)
- The real power of templates is their ability to be extended. If you think about most websites, there is content that is repeated on every page (header, footer, etc). Wouldn’t it be nice if we, as
developers, could have one canonical place for our header code that would be inherited by all other templates?
1. create the template base file: `touch templates/base.html`
2. add the content of it :
    ```
    <!-- templates/base.html -->
    <header>
        <a href="{% url 'home' %}">Home</a> |
        <a href="{% url 'about' %}">About</a>
    </header>
    {% block content %} {% endblock content %}
    ```
3. make the home and about templates inherit from the base template:
    ```
    <!-- templates/home.html -->
    {% extends "base.html" %}
    {% block content %}
    <h1>Homepage</h1>
    {% endblock content %}
    ```
    ```
    <!-- templates/about.html -->
    {% extends "base.html" %}
    {% block content %}
    <h1>AboutPage</h1>
    {% endblock content %}
    ```
## 7. Tests 
#### Jacob Kaplan-Moss: *"Code without tests is broken as designed."* :fire:
- read more about the tests because it's important
- we are gonna use the `SimpleTestCase` instead of the `TestCase` class because we don't want to test models yet.
- create the tests file: 
    ```
    from django.test import SimpleTestCase
    from django.urls import reverse
    class HomePageTests(SimpleTestCase):
        def test_exists_at_correct_location(self):
            response = self.client.get("/")
            self.assertEqual(response.status_code, 200)

        def test_url_available_by_name(self):
            respnse = self.client.get(reverse("home"))
            self.assertEqual(response.status_code, 200)

        def test_template_name_correct(self):
            response = self.client.get(reverse("home"))
            self.assertTempalteUsed(response, "home.html")

        def test_template_content(self):
            respnse = self.client.get(reverse("home"))
            self.assertTemplateContains(response, "<h1>Homepage</h1>")

    class AboutPageTests(SimpleTestCase):
        def test_exists_at_correct_location(self):
            response = self.client.get("/about/")
            self.assertEqual(response.status_code, 200)

        def test_url_available_by_name(self):
            respnse = self.client.get(reverse("about"))
            self.assertEqual(response.status_code, 200)

        def test_template_name_correct(self):
            response = self.client.get(reverse("home"))
            self.assertTempalteUsed(response, "about.html")

        def test_template_content(self):
            respnse = self.client.get(reverse("about"))
            self.assertTemplateContains(response, "<h1>AboutPage</h1>")
    ```

## 8. Git & GitHub
- same as the previous app

## Deployment
- i will use vercel.
- *William S. Vincent* used Heroku, but Heroku needes a credit card and a lot of headaches.
- what is the advantage of vercel:
    - it's free :smile:
- [*how to use vercel?*](https://github.com/MansAlien/DFB_4.0_Summary/blob/main/important/vercel.md)



<br>

🔥 [Next -> Chapter_4](https://github.com/MansAlien/DFB_4.0_Summary/blob/main/Ch_04-message_board_app.md) 🔥

<br>
<br>

<h2 align="center"> :smile: Done :smile: </h2>
