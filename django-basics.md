```
django-admin startproject mysite
```

in mysite: 
```
python manage.py runserver [+port]  # http://localhost:8000/

python manage.py startapp polls #creates app
```

#### MVC in django
model: manages the data, logic and rules of the application
view: output representation of information -> **templates** in django
controller: accepts input and converts it to commands for the model or view -> **view** in django, a Python callback function for a particular URL, describes which data is presented

**urls.py** in project and app folder:
matches a url to a function in view

polls/urls.py:
```
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'), # name can be used in reverse()
]
```
mysite/urls.py:
```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')), # ref to polls/urls.py
    path('admin/', admin.site.urls), # the only ref that doesn't need an include
]
```

polls/views.py:
```
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

#### Database (mysql)
**migration:** propagates changes made to models (adding a field, deleting a model, etc.) into database schema
```
manage.py makemigrations <app_name> 
manage.py migrate
```

settings.py:
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql', 
        'NAME': 'DB_NAME',
        'USER': 'DB_USER',
        'PASSWORD': 'DB_PASSWORD',
        'HOST': 'localhost',   # Or an IP Address that your DB is hosted on
        'PORT': '3306',
    }
}
```

or:
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'OPTIONS': {
            'read_default_file': '/path/to/my.cnf',
        },
    }
}
```

/path/to/my.cnf:
```
[client]
database = DB_NAME
host = localhost
user = DB_USER
password = DB_PASSWORD
default-character-set = utf8
```