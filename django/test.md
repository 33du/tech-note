polls/tests.py: inherit TestCase and implement test method beginned with "test"

### tests for models
```
import datetime

from django.test import TestCase
from django.utils import timezone

from .models import Question


class QuestionModelTests(TestCase):

    def test_was_published_recently_with_future_question(self):
        """
        was_published_recently() returns False for questions whose pub_date
        is in the future.
        """
        time = timezone.now() + datetime.timedelta(days=30)
        future_question = Question(pub_date=time)
        self.assertIs(future_question.was_published_recently(), False)
```

then:
```
python manage.py test polls
```
### tests for views
test **Client**: simulates a user interacting with the code at the view level
#### in shell:
```
from django.test.utils import setup_test_environment
setup_test_environment()

from django.test import Client

client = Client()
response = client.get('/polls/')
response.status_code
```
 
#### in tests.py: TestCase has own client and admin input (create_question)
```
def create_question(question_text, days):
    time = timezone.now() + datetime.timedelta(days=days)
    return Question.objects.create(question_text=question_text, pub_date=time)
    

class QuestionIndexViewTests(TestCase):
    def test_no_questions(self):
        """
        If no questions exist, an appropriate message is displayed.
        """
        response = self.client.get(reverse('polls:index'))
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, "No polls are available.")
        self.assertQuerysetEqual(response.context['latest_question_list'], [])
        
    def test_past_question(self):
        """
        Questions with a pub_date in the past are displayed on the
        index page.
        """
        create_question(question_text="Past question.", days=-30)
        response = self.client.get(reverse('polls:index'))
        self.assertQuerysetEqual(
            response.context['latest_question_list'],
            ['<Question: Past question.>']
        )
```

### Further
use **Selenium** or django built-in **LiveServerTestCase** to test behaviors of html

### Code coverage
use **coverage.py**:
```
coverage run --source='.' manage.py test myapp
coverage report
```
for more detailed report:
```
coverage html #generates htmlcov/index.html
```
