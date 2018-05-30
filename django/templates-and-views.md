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

HttpRequest -> view -> HttpResponse
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
```
<form ...> 
    {% csrf_token %} 
    ... 
</form>
```
Then: always return an **HttpResponseRedirect** after successfully dealing with POST data
```
return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))
```

### generic views - a shortcut
polls/urls.py:
```
from django.urls import path

from . import views

app_name = 'polls'
urlpatterns = [
    path('', views.IndexView.as_view(), name='index'),
    path('<int:pk>/', views.DetailView.as_view(), name='detail'),
    path('<int:pk>/results/', views.ResultsView.as_view(), name='results'),
    path('<int:question_id>/vote/', views.vote, name='vote'),
]
```

polls/views.py:
```
from django.http import HttpResponseRedirect
from django.shortcuts import get_object_or_404, render
from django.urls import reverse
from django.views import generic

from .models import Choice, Question


class IndexView(generic.ListView):
    template_name = 'polls/index.html'
    context_object_name = 'latest_question_list'

    def get_queryset(self):
        """Return the last five published questions."""
        return Question.objects.order_by('-pub_date')[:5]


class DetailView(generic.DetailView): #DetailView expects the primary key value captured from the URL to be called "pk"
    model = Question
    template_name = 'polls/detail.html'


class ResultsView(generic.DetailView):
    model = Question
    template_name = 'polls/results.html'
```
