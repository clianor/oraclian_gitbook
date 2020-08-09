# Django - Generic editing views

이번 글에서는 네 가지 클래스 기반 뷰에 대한 내용입니다.  
이 네 클래스는 컨텐츠 편집과 관련된 기능을 제공합니다.  
  
\* [메세지 프레임워크](https://docs.djangoproject.com/en/3.0/ref/contrib/messages/)를 이용하여 성공적인 양식 제출에 대한 메시지를 쉽게 표시할 수 있는 SuccessMessageMixin이 포함되어 있습니다.  
  
\* 아래의 내용들에서는 아래의 모델이 정의되어 있다고 가정합니다.

```python
# models.py
from django.db import models
from django.urls import reverse

class Author(models.Model):
    name = models.CharField(max_length=200)

    def get_absolute_url(self):
        return reverse('author-detail', kwargs={'pk': self.pk})

```

### 1. FormView

폼 형식을 표시해주는 뷰입니다.  
오류 발생시 유효성 검사 오류가 있는 양식을 다시 표시합니다.  
성공하면 새 URL로 리다이렉트합니다.  
이 뷰는 아래의 내용을 상속받습니다.  
`django.views.generic.base.TemplateResponseMixin  
django.views.generic.edit.BaseFormView   
django.views.generic.edit.FormMixin   
django.views.generic.edit.ProcessFormView   
django.views.generic.base.View`

이 클래스는 아래와 같이 사용할 수 있습니다.

```python
# forms.py
from django import forms

class ContactForm(forms.Form):
    name = forms.CharField()
    message = forms.CharField(widget=forms.Textarea)

    def send_email(self):
        # send email using the self.cleaned_data dictionary
        pass
        
# views.py
from myapp.forms import ContactForm
from django.views.generic.edit import FormView

class ContactView(FormView):
    template_name = 'contact.html'
    form_class = ContactForm
    success_url = '/thanks/'

    def form_valid(self, form):
        # This method is called when valid form data has been POSTed.
        # It should return an HttpResponse.
        form.send_email()
        return super().form_valid(form)
        
# contact.html
<form method="post">{% csrf_token %}
    {{ form.as_p }}
    <input type="submit" value="Send message">
</form>
```

장고의 Form을 이용하여 값의 유효성 검사를 수행할 수 있는 FormView입니다.  
Form은 Generic Editing Views에서 전반적으로 사용할 수 있습니다.  
  
이 유효성 검사를 할 수 있는 폼으로 생성된 폼은 아래와 같이 사용하여 화면에 렌더링해줄 수 있습니다.  
`as_table(), as_p(), as_ul()` 을 이용하여 기본적인 입력 폼을 생성해줄 수 있습니다.  
또한 폼은 `cleaned_data` 를 통해 검증에 통과한 값을 사전 타입으로 제공해줍니다.  
  
또한 폼은 as\_table과 같은 방식으로 사용하지 않고 아래와 같이도 사용할 수 있습니다.  
`{% for field in form %}  
{{ field.errors }}  
{{ field.label_tag }} : {{ field }}  
{% endfor %}`  




