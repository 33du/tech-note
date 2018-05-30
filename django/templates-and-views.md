put **template** in app/templates/app/page.html, address it by 'app/page.html'
> Here: variable inside {{ var }}

polls/templates/polls/detail.html:
```
<h1>{{ question.question_text }}</h1>
<ul>
{% for choice in question.choice_set.all %}
    <li>{{ choice.choice_text }}</li>
{% endfor %}
</ul>
```
use a url:
```
<a href="{% url 'polls:detail' question.id %}"> #'detail' is defined in urls.py as name, question.id is the variable
```
> in polls/urls.py:
```
app_name = 'polls'
urlpatterns = [
    path('<int:question_id>/', views.detail, name='detail')
]
```


**in views:** render the template and return HttpResponse, pass parameters to template using context
```
from django.shortcuts import render

from .models import Question

def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    context = {
        'latest_question_list': latest_question_list,
    }
    return render(request, 'polls/index.html', context)
```

**raise a 404 error:**
```
from django.http import Http404

def detail(request, question_id): #question_id comes from url
    try:
        question = Question.objects.get(pk=question_id)
    except Question.DoesNotExist:
        raise Http404("Question does not exist")
    return render(request, 'polls/detail.html', {'question': question})
```
shortcut:
```
from django.shortcuts import get_object_or_404

def detail(request, question_id):
    #also use get_list_or_404()
    question = get_object_or_404(Question, pk=question_id) #can have many keyword argument
    return render(request, 'polls/detail.html', {'question': question})
```

To prevent **Cross Site Request Forgeries**: 
all POST forms that are targeted at internal URLs should use the {% csrf_token %} template tag
> <form ...> {% csrf_token %} ... </form>
