put template in app/templates/app/page.html, address it by 'app/page.html'
> Here: variable inside {{ var }}

in views: render the template and return HttpResponse, pass parameters to template using context
```
from django.http import HttpResponse
from django.template import loader

from .models import Question

def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    template = loader.get_template('polls/index.html')
    context = {
        'latest_question_list': latest_question_list,
    }
    return HttpResponse(template.render(context, request))
```
