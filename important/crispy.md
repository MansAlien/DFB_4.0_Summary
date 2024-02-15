<h1 align='center'>👉 Install Bootstrap In Django locally 👈</h1>

## Download Crispy
1. open your terminal
2. type this code 
   - `pip install django-crispy-forms`
   - `pip install crispy-bootstrap5`

## Set Up django-crispy-forms in your project
1. add it to the `INSTALLED_APPS` in `project/settings.py`:
   ```
    # django_project/settings.py
    INSTALLED_APPS = [
        "django.contrib.admin",
        "django.contrib.auth",
        "django.contrib.contenttypes",
        "django.contrib.sessions",
        "django.contrib.messages",
        "django.contrib.staticfiles",
        # 3rd Party
        "crispy_forms", # new
        "crispy_bootstrap5", # new
    ]
   ```

2. add these two variables to the end of `project/settings.py` file:
   - `CRISPY_ALLOWED_TEMPLATE_PACKS = "bootstrap5"` 
   - `CRISPY_TEMPLATE_PACK = "bootstrap5"`


## Now Crispy is installed in Django and you can use it in your project.

<br>
<br>

<h2 align="center"> :smile: Done :smile: </h2>