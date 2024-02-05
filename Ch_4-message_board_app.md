<h1 align='center'> :fire: CH 4 :fire: <br> 👉 Message Board App 👈</h1>

## 1. Initial Set Up
- create the folder of the project: `mkdir message-board `
- go to the folder of the project: `cd message-board`
- create a virtual environment: `python -m venv .venv`
- activate the virtual environment: `source .venv/bin/activate`
- update pip: `python -m pip install --upgrade pip`
- install django: `python -m pip install Django==4.0`
- start django project: `django-admin startproject project .`
- start django app called "posts": `python manage.py startapp posts`
- add the app to the settings file 
- create the initial database: `python manage.py migrate`
- check everything is ok: `python manage.py runserver`

## 2. Create a Database Model
1. create the model itself: 
    ```
    # posts/models.py
    from django.db import models
    class Post(models.Model):
        text = models.TextField()
    ```
2. create a migrations file: `python manage.py makemigrations posts`
3. build the database: `python manage.py migrate`

## 3. Django Admin
1. create a super user: `python manage.py createsuperuser --username admin --email admin@gmail.com`
2. enter the password of the user and confirm it
3. run the server: `python manage.py runserver`
4. go to this link [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/)
5. login with the superuser that we just created.

- note that the posts app is not there so we need to add it to the admin first
6. update the `posts/admin.py` file:
    ```
    #posts/admin.py
    from django.contrib import admin
    from .models import Post
    
    admin.site.register(Post)
    ```
7. run the server again: `python manage.py runserver`
8. click on the "+ Add" next to the posts app and add some posts to it
9. change the title of the post in the admin panle to be the text that we gived to the model: 
    ```
    #posts/models.py
    from django.db import models

    class Post(models.Model):
        text = models.TextField()

        def __str__(self):
            return self.text[:50]
    ```
## 4. Views/Templates/URLs
- reade more about the ListView:
    - [django 4.0](https://docs.djangoproject.com/en/4.0/ref/class-based-views/generic-display/#listview)
    - [django 5.0](https://docs.djangoproject.com/en/5.0/ref/class-based-views/generic-display/#listview)

1. create the post views:
    ```
    #posts/views.py
    from django.views.generic import ListView
    from .models import Post

    class HomePageView(ListView):
        model = Post
        template_name = 'home.html'
    ```

2. create templates directory: `mkdir templates`
3. update the settings file :
    ```
    # django_project/settings.py
    TEMPLATES = [
        {
            ...
            "DIRS": [BASE_DIR / "templates"], # new
            ...
        },
    ]
    ```
4. home page:
    - create it: `touch templates/home.html`
    - add the it's content: 
        ```
        <!-- templates/home.html -->
        <h1>Message board homepage</h1>
        <ul>
            {% for post in post_list %}
                <li>{{ post.text }}</li>
            {% endfor %}
        </ul>
        ```
5. add the urls of the posts to the urls of the project: 
    ```
    # project/urls.py
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path("admin/", admin.site.urls),
        path("", include("posts.urls")),
    ]
    ```
6. adding the posts urls:
    ```
    # posts/urls.py
    from django.urls import path
    from .views import HomePageView

    urlpatterns = [
        path("", HomePageView.as_view(), name="home"),
    ]
    ```
## 5. Tests
- note that in this project we are using a database so we need to user `TestCase` instead of `SimpleTestCase` read more about it here:
    - [django 4.0](https://docs.djangoproject.com/en/4.0/topics/testing/tools/#django.test.TestCase)
    - [django 5.0](https://docs.djangoproject.com/en/5.0/topics/testing/tools/#django.test.TestCase)

1. create the tests:
    ```
    posts/test.py
    from django.test import TestCase
    from django.urls import reverse
    from .models import Post

    class PostTests(TestCase):
        @classmethod
        def setUpTestData(cls):
            cls.post = Post.objects.create(text="this is a test")
        
        def test_model_content(self):
            self.assertEqual(self.post.text, "this is a test")

        def test_url_exists_at_correct_location(self):
            response = self.client.get("/")
            self.assertEqual(response.status_code, 200)

        def test_homepage(self):
            response = self.client.get(reverse("home"))
            self.assertEqual(response.status_code, 200)
            self.assertTemplateUsed(response, "home.html")
            self.assertTemplateContains(response, "this is a test")
    ```
2. run the test: `python manage.py test`
## 6. Git & GitHub
same as the previous

## 7. Deployment
- i will use vercel.
- *William S. Vincent* used Heroku, but Heroku needes a credit card and a lot of headaches.
- what is the advantage of vercel:
    - it's free :smile:
- [*how to use vercel?*](https://github.com/MansAlien/DFB_Revision/blob/main/important/vercel.md)



<br>

🔥 [Next -> Chapter_5](https://github.com/MansAlien/DFB_Revision/blob/main/Ch_3-pages_app.md) 🔥

<br>
<br>

<h2 align="center"> :smile: Done :smile: </h2>

