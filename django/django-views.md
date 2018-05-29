put template in app/templates/app/page.html, address it by 'app/page.html'
> Here: variable inside {{ var }}

in views: render the template and return HttpResponse, pass parameters to template using context
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

raise a 404 error:
```
from django.http import Http404

def detail(request, question_id): #question_id comes from url
    try:
        question = Question.objects.get(pk=question_id)
    except Question.DoesNotExist:
        raise Http404("Question does not exist")
    return render(request, 'polls/detail.html', {'question': question})
```
