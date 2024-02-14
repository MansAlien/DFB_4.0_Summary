<h1 align='center'>👉 Custom User 👈</h1>

## 1. Initial Set Up

1. :file_folder: create a virtual environment : `python -m venv .venv`
2.  Activate the virtual environment : `source .venv/bin/activate`
3. :arrow_down: Install django : `pip install django`
4. :pencil2: Start a new project : `django-admin startproject project .`
5. :pencil2: Start a new app : `python manage.py startapp accounts`
6. ⚙️ Adding the new app to the settings file : `accounts.apps.AccountsConfig`


## 2. Create User Model

1. ⚙️ Update the settings file  : `AUTH_USER_MODEL = "accounts.CustomUser"`
2. Now update accounts/models.py with a new User model called CustomUser that extends the existing AbstractUser.
    ```
    # accounts/models.py
    from django.contrib.auth.models import AbstractUser
    from django.db import models

    class CustomUser(AbstractUser):
        age = models.PositiveIntegerField(null=True, blank=True)
    ```
    **:pushpin: note:** if you need a very high level of control and cutomization user `AbstractBaseUser` instead of `AbstractUser`

## 3. Create User Form
### 1. There are two ways to interact with the new CustomUser model :

1. User Signup (Registration): `UserCreationForm`.
2. Admin App (Superuser Modification): `UserChangeForm`.

### 2. create the form :
    # accounts/forms.py
    from django.contrib.auth.forms import UserCreationForm, UserChangeForm
    from .models import CustomUser

    class CustomUserCreationForm(UserCreationForm):
        class Meta(UserCreationForm):
            model = CustomUser
            fields = UserCreationForm.Meta.fields + ("age",)
            
    class CustomUserChangeForm(UserChangeForm):
        class Meta:
            model = CustomUser
            fields = UserChangeForm.Meta.fields

### 3. update the admin.py file :

    # accounts/admin.py
    from django.contrib import admin
    from django.contrib.auth.admin import UserAdmin
    from .forms import CustomUserCreationForm, CustomUserChangeForm
    from .models import CustomUser

    class CustomUserAdmin(UserAdmin):
        add_form = CustomUserCreationForm
        form = CustomUserChangeForm
        model = CustomUser
        list_display = [
            "email",
            "username",
            "age",
            "is_staff",
        ]
        fieldsets = UserAdmin.fieldsets + ((None, {"fields": ("age",)}),)
        add_fieldsets = UserAdmin.add_fieldsets + ((None, {"fields": ("age",)}),)
    admin.site.register(CustomUser, CustomUserAdmin)

## 4. Final Steps
1. create migrations file for it : `python manage.py makemigrations accounts`
2. migrate the changes : `python manage.py migrate`
3. create a super user : `python manage.py createsuperuser`
4. run the server : `python manage.py runserver`
5. go to the admin panel site : [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin)

<br>
<br>

<h2 align="center"> :smile: Done :smile: </h2>
