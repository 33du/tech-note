1. Include django.contrib.staticfiles in INSTALLED_APPS

2. STATIC_URL = '/static/' -> store static files in /static/ -> polls/static/polls/style.css

3. In templates, use the static template tag to build the URL

in index.html:
```
{% load static %}
<link rel="stylesheet" type="text/css" href="{% static 'polls/style.css' %}" />
```
insert **image**: create polls/static/polls/images/background.gif
```
body {
    background: white url("images/background.gif") no-repeat;
}
```

### Deploying static files
1. change STATIC_ROOT to static file server
```
STATIC_ROOT = "/var/www/example.com/static/"
```
2. run the collectstatic command when static files change
```
python manage.py collectstatic
```
