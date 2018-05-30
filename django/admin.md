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

class QuestionAdmin(admin.ModelAdmin):
    fields = ['pub_date', 'question_text'] #reorder the fields

admin.site.register(Question, QuestionAdmin)
```
or group the fields:
```
fieldsets = [
        (None,               {'fields': ['question_text']}),
        ('Date information', {'fields': ['pub_date']}),
    ]
```
edit choices in question admin page with place for 3 choices by default:
```
class ChoiceInline(admin.StackedInline): #TabularInline for more compact view
    model = Choice
    extra = 3

class QuestionAdmin(admin.ModelAdmin):
    inlines = [ChoiceInline]
    list_display = ('question_text', 'pub_date', 'was_published_recently') #showed on all-questions page
    list_filter = ['pub_date'] #add a filter
    search_fields = ['question_text'] #add a search box at the top of the change list
    
admin.site.register(Question, QuestionAdmin)
```

in models: give was_published_recently attributes for list dispaly
```
class Question(models.Model):
    # ...
    def was_published_recently(self):
        now = timezone.now()
        return now - datetime.timedelta(days=1) <= self.pub_date <= now
    was_published_recently.admin_order_field = 'pub_date'
    was_published_recently.boolean = True
    was_published_recently.short_description = 'Published recently?'
```

### Customize look
creating a /templates folder in root for the project's template, then add to settings:
```
TEMPLATES = [
  {
    'DIRS': [os.path.join(BASE_DIR, 'templates')]
  }
]
```
-> put templates for admin in templates/admin/

-> Original place for admin templates: django/contrib/admin/templates, files there can be copied and overriden (to find django source files:)
```
python -c "import django; print(django.__path__)"
```

-> Or modify AdminSite object
