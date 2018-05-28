```django-admin startproject mysite```

in mysite: 
```python manage.py runserver [+port]  # http://localhost:8000/
```

```python manage.py startapp polls #creates app```

####MVC in django
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
    path('', views.index, name='index'),
]
```
mysite/urls.py:
```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```

polls/views.py:
```
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```
