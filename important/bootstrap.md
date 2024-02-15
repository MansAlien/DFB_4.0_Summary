<h1 align='center'>👉 Install Bootstrap In Django locally 👈</h1>

## 1. Download Bootstrap
1. go to Bootstrap website: [Bootstrap](https://getbootstrap.com/docs/5.3/getting-started/download/)
2. download it and unzip it

## 2. Set up django to use the static files
1. :file_folder: create `static` folder in the main folder of your project.
2. add the following code to the `project/settings.py` file:
   ```
    STATIC_URL = 'static/'
    STATICFILES_DIRS = os.path.join(BASE_DIR, 'static'),
    STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles', 'static')
   ```

## 3. Add Bootstrap to Django
1. :file_folder: create `/static/css` and `/static/js` folders.
2. :clipboard: copy only these files from the `bootstrap-5.3.2-dist/css` folder to the `/static/css` folder:
   - `bootstrap.min.css`
   - `bootstrap.min.css.map`
3. :clipboard: copy only these files from the `bootstrap-5.3.2-dist/js` folder to the `/static/js`:
   - `bootstrap.bundle.min.js`
   - `bootstrap.bundle.min.js.map`

## using Bootstrap in Django
1. open the html file that you want to user bootsrap on it for example `templates/base.html`
2. add the following code to the `<head>` tag:
   ```
    <!-- Bootstrap Framework -->
    <link rel="stylesheet" href="{% static 'css/bootstrap.min.css' %}" />
   ```
3. add the following code to the bottom of the `<body>` tag:
   ```
   <script src="{% static 'js/bootstrap.bundle.min.js' %}"></script>
   ```

## Now Bootstrap is installed in Django and you can use it in your project.

<br>
<br>

<h2 align="center"> :smile: Done :smile: </h2>
