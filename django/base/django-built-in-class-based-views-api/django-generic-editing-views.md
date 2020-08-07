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

