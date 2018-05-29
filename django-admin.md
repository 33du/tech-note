#### For site managers
http://127.0.0.1:8000/admin/

create super user:
```
python manage.py createsuperuser
```

register model in polls/admin.py:
```
from django.contrib import admin
from .models import Question

admin.site.register(Question)
```
