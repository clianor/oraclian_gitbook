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

### 2. CreateView

특정한 객체의 저장과 관련된 뷰로써 데이터의 유효성 검사 수행과 유효하지 않은 데이터일 경우 다시 그 뷰를 보여주고 오류를 표현해주기 위한 뷰입니다.  
이 뷰는 아래의 내용을 상속받습니다.  
`django.views.generic.detail.SingleObjectTemplateResponseMixin   
django.views.generic.base.TemplateResponseMixin  
django.views.generic.edit.BaseCreateView   
django.views.generic.edit.ModelFormMixin   
django.views.generic.edit.FormMixin   
django.views.generic.detail.SingleObjectMixin   
django.views.generic.edit.ProcessFormView   
django.views.generic.base.View`  
  
**\* 속성 \***  
**template\_name\_suffix**  
CreateView에서 초기화된 Model\_template\_name\_suffix.html와 같은 방식으로 template의 이름을 설정합니다.  
기본 값은 \_form 입니다.  
\(template\_name와 template\_name\_suffix 중에서 선택하여 사용하면 됩니다\)  
  
**object**  
객체가 생성된 경우 self.object로 접근이 가능하고 아직 생성되지 않은 경우에는 self.object는 None이 됩니다.  
그런데 form으로 유효성 검사를 하고나면 바로 save를 할텐데 CreateView에서 object에 접근할일이 어떤 상황이며 어떤때 접근하는지는 모르겠습니다.  
\(객체를 생성하는 단계에서 self.object 속성을 제공한다는게 잘 이해되지 않네요\)

```python
# views.py
from django.views.generic.edit import CreateView
from myapp.models import Author

class AuthorCreate(CreateView):
    model = Author
    fields = ['name']
    
# author_form.html
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit" value="Save">
</form>
```

FormView에서와 같이 form과 관련된 템플릿 기능을 사용할 수 있습니다.

### 3. UpdateView

CreateView와 유사하지만 특징은 특정 객체의 생성이 아닌 기존 내용을 편집하기 위한 뷰입니다.  
이 뷰는 아래의 내용을 상속받습니다.  
`django.views.generic.detail.SingleObjectTemplateResponseMixin   
django.views.generic.base.TemplateResponseMixin  
django.views.generic.edit.BaseUpdateView   
django.views.generic.edit.ModelFormMixin   
django.views.generic.edit.FormMixin   
django.views.generic.detail.SingleObjectMixin   
django.views.generic.edit.ProcessFormView   
django.views.generic.base.View`

**\* 속성 \***  
**template\_name\_suffix**  
CreateView의 template\_name\_suffix와 동일하며 기본값이 \_update\_form입니다.  
  
**object**  
수정하려고 하는 객체에 대해 self.object로 접근할 수 있습니다.

```python
# views.py
from django.views.generic.edit import UpdateView
from myapp.models import Author

class AuthorUpdate(UpdateView):
    model = Author
    fields = ['name']
    template_name_suffix = '_update_form'
    
# author_update_form.html
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit" value="Update">
</form>
```

FormView에서와 같이 form과 관련된 템플릿 기능을 사용할 수 있습니다.



