<h1 align='center'> :fire: CH 5 :fire: <br> 👉 Blog App 👈</h1>

## 1. Initial Set Up
- :file_folder: make a new directory called blog
- :arrow_down: install djnago in a new virtual environment called .venv
- :pencil2: create a new django project called project
- :pencil2: create a new app blog
- ⚙️ perform the migration to setup the database
- ⚙️ update project/settings.py file (add the new app)

## 2. Database Models

1. create the models:
   ```
   # blog/models.py
    from django.db import models
    from django.urls import reverse

    class Post(models.Model):
        title = models.CharField(max_length=200)
        author= models.ForeignKey(auth.User, on_delete=models.CASCADE)
        body = models.TextField()

        def __str__(self):
            return self.title

        def get_absolute_url(self):
            return reverse("post_detail", kwargs={"pk":self.pk})
   ```
2. create a migrations file for the blog app: `python manage.py makemigrations blog`
3. build the model in the database: `python manage.py migrate`
   
## 3. Admin
1. create a superuser:
   - `python manage.py createsuperuser --username admin --email admin@gmail.com`
   - enter the password two times
2. add the post model to the admin site:
   ```
   # blog/admin.py
   from django.contrib import admin
   from .models import Post

   admin.site.register(Post)
   ```
3. run the server and go to this link [127.0.0.1:8000/admin/](127.0.0.1:8000/admin/)
4. create some posts in the admin panel

## 4. URLs
1. :link: create the urls of the blog:
   ```
   # blog/urls.py
    from django.urls import path
    from .views import BlogListView

    urlpatterns = [
        path("", BlogListView.as_view(), name="home")
    ]
   ```
2. :link: add the urls of the blog to the project:
   ```
   # project/urls.py
   from django.contrib import admin
   from django.urls import path, include

   urlpatterns=[
    path("admin/", admin.site.urls),
    path("", include("blog.urls")),
   ]
   ```

## 5. Views :eyes:
```
# blog/views.py
from django.views.generic import ListView
from .models import Post

class BlogListView(ListView):
    model = Post
    template_name = "home.html"
```
## 6. Templates
1. :file_folder: create the directory of it : `mkdir templates`
2. ⚙️ update the settings file (adding the templates path to it)
3. :page_facing_up: create the templates/base.html file and add the following code:
   ```
   <!-- templates/base.html -->
    <html>
        <head>
            <title>Django blog</title>
        </head>
        <body>
            <header>
                <h1><a href="{% url 'home' %}">Django blog</a></h1>
            </header>
            <div>
                {% block content %}
                {% endblock content %}
            </div>
        </body>
    </html>
   ```
4. :page_facing_up: create the templates/home.html file and add the following code: 
   ```
   <!-- templates/home.html -->
    {% extends "base.html" %}
    {% block content %}
    {% for post in post_list %}
        <div class="post-entry">
            <h2><a href="">{{ post.title }}</a></h2>
            <p>{{ post.body }}</p>
        </div>
    {% endfor %}
    {% endblock content %}
   ```

## Static Files
1. :file_folder: create the static folder: `mkdir static`
2. ⚙️ update the project/settings.py:
   ```
   # project/settings.py
    STATIC_URL = "/static/"
    STATICFILES_DIRS = [BASE_DIR / "static"]
   ```
3. :file_folder: create the static/css folder: `mkdir static/css`
4. :page_facing_up: create the static/css/base.css file: `touch static/css/base.css`
5. :page_facing_up: update the base.html file:
   ```
   <!-- templates/base.html -->
    {% load static %}
    <html>
        <head>
            <title>Django blog</title>
            <link rel="stylesheet" href="{% static 'css/base.css' %}">
        </head>
        ...
   ```
6. add your css code to the base.css file

## Individual Blog pages
1. :eyes: create the BlogDetailsView:
    ```
    # blog/views.py
    from django.views.generic import ListView, DetailView
    from .models import Post

    class BlogListView(ListView):
        model = Post
        template_name = "home.html"
    
    class BlogDetailsView(DetailView):
        model = Post
        template_name = "post_detail.html"
    ```
2. :page_facing_up: create the templates/post_detail.html template and add the following code:
   ```
    <!-- templates/post_detail.html -->
    {% extends "base.html" %}
    {% block content %}
        <div class="post-entry">
            <h2>{{ post.title }}</h2>
            <p>{{ post.body }}</p>
        </div>
    {% endblock content %}
   ```
3. :link: add a url for the blog details page:
   ```
   # blog/urls.py
   from django.urls import path
   from .views import BlogListView, BlogDetailsView

   urlpatterns = [
    path("post/<int:pk>/", BlogDetailsView.as_view(), name="post_detail"),
    path("", BlogListView.as_view(), name="home"),
   ]
   ```
4. :page_facing_up: add the urls of the details page to the base.html:
   ```
   <!-- templates/home.html -->
    {% extends "base.html" %}
    {% block content %}
    {% for post in post_list %}
        <div class="post-entry">
            <h2><a href="{% url 'post_detail' post.pk %}">{{ post.title }}</a></h2>
            <p>{{ post.body }}</p>
        </div>
    {% endfor %}
    {% endblock content %}
   ```
## Tests
1. read about the get_user_model():
   - [django 4.0](https://docs.djangoproject.com/en/4.0/topics/auth/customizing/#django.contrib.auth.get_user_model)
   - [django 5.0](https://docs.djangoproject.com/en/5.0/topics/auth/customizing/#django.contrib.auth.get_user_model)
  
2. create the test:
    ```
    from djangol.contrib.auth import get_user_model
    from django.test import TestCase
    from django.urls import reverse
    from .models import Post

    class BlogTests(TestCase):
        @classmethod
        def setUpTestData(cls):
            cls.user = get_user_model().objects.create_user(
                username="testuser", email="testuser@gmail.com", password="password"
            )

            cls.post = Post.objects.create(
                title="test", body="test body", author="cls.user"
            )
        def test_post_model(self):
            self.assertEqual(self.post.title, "test")
            self.assertEqual(self.post.body, "test body")
            self.assertEqual(self.post.author.username, "testuser")
            self.assertEqual(str(self.post), "test")
            self.assertEqual(self.post.get_absolute_url(), "/post/1/")

        def test_url_exists_at_correct_location_listview(self):
            response = self.client.get("/")
            self.assertEqual(response.status_code, 200)

        def test_url_exists_at_correct_location_detailview(self):
            response = self.client.get("/post/1/")
            self.assertEqual(response.status_code, 200)

        def test_post_listview(self):
            response = self.client.get(reverse("home"))
            self.assertEqual(response.status_code, 200)
            self.assertContains(response, "test body")
            self.assertTemplateUsed(response, "home.html")

        def test_post_detailview(self):
            response = self.client.get(reverse("post_detail", 
                kwargs={"pk": self.post.pk}"))
            self.assertEqual(response.status_code, 200)
            self.assertEqual(no_response.status_code, 404)
            self.assertContains(response, "test")
            self.assertTemplateUsed(response, "post_detail.html")
    ```

## Git & GitHub
- same as before :smile:

<br>

🔥 [Next -> Chapter_6](https://github.com/MansAlien/DFB_4.0_Summary/blob/main/Ch_06-forms.md) 🔥

<br>
<br>

<h2 align="center"> :smile: Done :smile: </h2>