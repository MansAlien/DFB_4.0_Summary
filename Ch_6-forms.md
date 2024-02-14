<h1 align='center'> :fire: CH 6 :fire: <br> 👉 Forms 👈</h1>

- Writing this code by hand would be time-consuming and difficult so Django comes with powerful
built-in Forms89 that abstract away much of the difficulty for us. Django also comes with generic
editing views90 for common tasks like displaying, creating, updating, or deleting a form.

## 1. CreateView
1. :page_facing_up: update the `templates/base.html` file (adding new post link):
   ```
    <!-- templates/base.html -->
    {% load static %}
    <html>
        <head>
            <title>Django blog</title>
            <link href="https://fonts.googleapis.com/css?family=\
                Source+Sans+Pro:400" rel="stylesheet">
            <link href="{% static 'css/base.css' %}" rel="stylesheet">
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
                {% block content %}
                {% endblock content %}
            </div>
        </body>
    </html>
   ```
2. :link: create a `url` for it:
   ```
   # blog/urls.py
    from django.urls import path
    from .views import BlogListView, BlogDetailView, BlogCreateView

    urlpatterns = [
        path("post/new/", BlogCreateView.as_view(), name="post_new"),
        path("post/<int:pk>/", BlogDetailView.as_view(), name="post_detail"),
        path("", BlogListView.as_view(), name="home"),
    ]
   ```
3. :eyes: add the `BlogCreateView` of the `view`:
   ```
   # blog/views.py
    from django.views.generic import ListView, DetailView
    from django.views.generic.edit import CreateView
    from .models import Post

    class BlogListView(ListView):
        model = Post
        template_name = "home.html"

    class BlogDetailView(DetailView):
        model = Post
        template_name = "post_detail.html"

    class BlogCreateView(CreateView):
        model = Post
        template_name = "Post_detail.html"
        fields = ['title','body','author']
   ```

4. :page_facing_up: create the `template/post_new.html` file:
    ```
    <!-- templates/post_new.html -->
    {% extends "base.html" %}
    {% block content %}
        <h1>New post</h1>
        <form action="" method="post">
            {% csrf_token %}
            {{ form.as_p }}
            <input type="submit" value="Save">
        </form>
    {% endblock content %}
    ```
5. check if everything is going ***ok***:
   - go to this link: [http://127.0.0.1:8000/post/new/](http://127.0.0.1:8000/post/new/)
   - create a post with this form to check if it works ok
  
### :pushpin: *note*:
- If you are thinking about it doesn't make sense to have an `author` field in the `BlogCreateView`, you are right but you will get the answer of your quistion at newspapper chapters.

## 2. UpdateView
1. :page_facing_up: update the `templates/post_detail.html` file (adding a link for the ***edit form***):
   ```
    <!-- templates/post_detail.html -->
    {% extends "base.html" %}
    {% block content %}
        <div class="post-entry">
            <h2>{{ post.title }}</h2>
            <p>{{ post.body }}</p>
        </div>
        <a href="{% url 'post_edit' post.pk %}">+ Edit Blog Post</a>
    {% endblock content %}
   ```
2. :page_facing_up: create the `templates/post_edit.html` file and add the following code:
   ```
    <!-- templates/post_edit.html -->
    {% extends "base.html" %}
    {% block content %}
        <h1>Edit post</h1>
        <form action="" method="post">
            {% csrf_token %}
            {{ form.as_p }}
            <input type="submit" value="Update">
        </form>
    {% endblock content %}
   ```
3. :eyes: add the `BlogUpdateView` form to the `view`:
    ```
    from django.views.genenric import ListView, DetailView
    from django.views.generic.edit import CreateView, UpdateView
    from .models import Post

    class BlogListView(ListView):
        model = Post
        template_name = "home.html"

    class BlogDetailView(DetailView):
        model = Post
        template_name = "post_detail.html"

    class BlogCreateView(CreateView):
        model = Post
        template_name = "post_new.html"
        fields = ['title','body','author']

    class BlogUpdateView(UpdateView):
        model = Post
        template_name = "post_edit.html"
        fields = ['title', 'body']
    ```

4. :link: create the `url` for it:
    ```
    # blog/urls.py
    from django.urls import path
    from .views import BlogListView, BlogDetailView, BlogCreateView, BlogUpdateView

    urlpatterns = [
        path("post/new/", BlogCreateView.as_view(), name="post_new"),
        path("post/<int:pk>/edit", BlogUpdateView.as_view(), name="post_edit"),
        path("post/<int:pk>/", BlogDetailView.as_view(), name="post_detail"),
        path("", BlogListView.as_view(), name="home"),
    ]
    ```
## 3. DeleteView
1. :page_facing_up: update the `templates/post_detail.html` file (adding a link for the ***delete form***):
    ```
    <!-- templates/post_detail.html -->
    {% extends "base.html" %}
    {% block content %}
        <div class="post-entry">
            <h2>{{ post.title }}</h2>
            <p>{{ post.body }}</p>
        </div>
        <p><a href="{% url 'post_edit' post.pk %}">+ Edit Blog Post</a></p>
        <p><a href="{% url 'post_delete' post.pk %}">+ Delete Blog Post</a></p>
    {% endblock content %}
    ```
2. :page_facing_up: create the `templates/post_delete.html` file and add the following code:
   ```
    <!-- templates/post_delete.html -->
    {% extends "base.html" %}
    {% block content %}
        <h1>Delete post</h1>
        <form action="" method="post">{% csrf_token %}
            <p>Are you sure you want to delete "{{ post.title }}"?</p>
            <input type="submit" value="Confirm">
        </form>
    {% endblock content %}
   ```
3. :eyes: add the `BlogDeleteView` form to the `view`:
    ```
    from django.view.generic import BlogListView, BlogDetailView
    from django.views.generic.edit import BlogCreateView,BlogUpdateView,BlogDeleteView
    from django.urls import reverse_lazy
    from .models import Post

    ....
    class BlogDeleteView(BlogDeleteView):
        model = Post
        template_name = "post_delete.html"
        success_url = reverse_lazy("home")
    ```
4. :link: create the `url` for it:
    ```
    # blog/urls.py
    from django.urls import path
    from .views import BlogListView, BlogDetailView, BlogCreateView, BlogUpdateView, BlogDeleteView
    urlpatterns = [
        ....
        path("post/<int:pk>/delete/", BlogDeleteView.as_view(), name="post_delete"),
    ]
    ```

## Tests
1. create the tests:
    ```
    # blog/tests.py
    ....
    def test_post_createview(self):
        response = self.client.post(reverse("post_new"), 
            {
                "title": "title",
                "body": "body",
                "author": "self.user.id",
            },)
        self.assertEqual(response.status_code, 302)
        self.assertEqual(Post.objects.last().title, "title")
        self.assertEqual(Post.objects.last().body, "body")

    def test_post_updateview(self):
        response = self.client.post(reverse("post_edit", args="1"), 
            {
                "title":"update title",
                "body":"update body",
            },)
        self.assertEqual(response.status_code, 302)
        self.assertEqual(Post.objects.last().title, "update title")
        self.assertEqual(Post.objects.last().body, "update body")

    def test_post_deleteview(self):
        response = self.client.post(reverse("post_delete", args="1"))
        self.assertEqual(response.status_code, 302)
    ```


<br>
🔥 [Next -> Chapter_7](https://github.com/MansAlien/DFB_4.0_Summary/blob/main/Ch_7-user_accounts.md) 🔥
<br>
<br>
<h2 align="center"> :smile: Done :smile: </h2>