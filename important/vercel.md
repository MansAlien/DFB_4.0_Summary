<h1 align='center'>👉 Custom User 👈</h1>

## 1. create the requirements file: 
- `pip freeze > requirements.txt`
- vercel use django 4.0 and django 4.2: `Django==4.0`
## 2. add the following code the end of `project/wsgi.py` file :
- `app = application`
## 3. update the settings file:
```
import os
...
DEBUG = False
ALLOWED_HOSTS = ['.vercel.app','now.sh','127.0.0.1','localhost']
...
STATIC_URL = 'static/'
STATICFILES_DIRS = os.path.join(BASE_DIR, 'static'),
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles', 'static')
```
## 4. add the following code to the end of the `project/urls.py`file :
```
from django.conf import settings
from django.conf.urls.static import static

urlpatterns += static(settings.MEDIA_URL, document_root = settings.MEDIA_ROOT)
urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```
## 5. build file: 
- create buid file: `touch build_file.sh`
- add the following code :
    ```
    pip install -r requirements.txt
    python3.9 manage.py collectstatic --noinput
    ```
## 7. vercel file:
- create vercel file: `touch vercel.json`
- add the following code :
    ```
    {
    "version": 2,
    "builds": [
        {
        "src": "project/wsgi.py",
        "use": "@vercel/python",
        "config": { "maxLambdaSize": "15mb", "runtime": "python3.9" }
        },
        {
        "src": "build_files.sh",
        "use": "@vercel/static-build",
        "config": {
            "distDir": "staticfiles"
        }
        }
    ],
    "routes": [
        {
        "src": "/static/(.*)",
        "dest": "/static/$1"
        },
        {
        "src": "/(.*)",
        "dest": "project/wsgi.py"
        }
    ]
    }
    ```
## 8. Postgresql database:
1. open [railway](https://railway.app/)
2. choose Provision PostgreSQL
3. go to the postgresql variables
4. change the database settings code with it:
    ```
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql',
            'NAME': 'railway',
            'USER': 'pg_username',
            'PASSWORD': 'pg_password',
            'HOST': 'pg_host',
            'PORT': 'pg_host',
        }
    }
    ```
5. create the migration file: `python manage.py makemigrations`
6. migrate it : `python manage.py migrate`

## 9. Git & GitHub:
- `git add .`
- `git commit -m "updates for vercel deployment"`
- `git push origin main`
<br>
<br>
<h2 align="center"> :smile: Done :smile: </h2>