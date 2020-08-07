# Django - Generic display views

이번 글에서는 두 가지 클래스 기반 뷰에 대한 내용입니다.  
이 두 클래스는 데이터를 표시하도록 설계가 되었습니다.  
많은 프로젝트에서 가장 일반적으로 사용되는 뷰입니.

### 1. DetailView

이 뷰가 실행되는 동안 self.object는 뷰가 작동하는 객체를 포함합니다.  
HTTP 메소드는 get만 사용할 수 있습니다.  
이 뷰는 아래의 내용을 상속받습니다.  
`django.views.generic.detail.SingleObjectTemplateResponseMixin  
django.views.generic.base.TemplateResponseMixin  
django.views.generic.detail.BaseDetailView  
django.views.generic.detail.SingleObjectMixin  
django.views.generic.base.View`

이 클래스는 아래와 같이 사용할 수 있습니다.

```python
# views.py
from django.views.generic.detail import DetailView

from articles.models import Article

class ArticleDetailView(DetailView):

    model = Article

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['now'] = timezone.now()
        return context

# urls.py
from django.urls import path

from article.views import ArticleDetailView

urlpatterns = [
    path('<slug:slug>/', ArticleDetailView.as_view(), name='article-detail'),
]

# article_detail.html
<h1>{{ object.headline }}</h1>
<p>{{ object.content }}</p>
<p>Reporter: {{ object.reporter }}</p>
<p>Published: {{ object.pub_date|date }}</p>
<p>Date: {{ now|date }}</p>
```

### 2. ListView

개체의 목록을 나타내는 뷰로서 이 뷰가 실행되는 동안 self.objects\_list를 포함합니다.  
\(self.object\_list는 쿼리 셋일 필요는 없습니다.\)  
이 뷰는 아래의 내용을 상속받습니다.

`django.views.generic.detail.MultipleObjectTemplateResponseMixin  
django.views.generic.base.TemplateResponseMixin  
django.views.generic.detail.BaseListView  
django.views.generic.detail.MultipleObjectsMixin  
django.views.generic.base.View`

이 클래스는 아래와 같이 사용할 수 있습니다.

```python
# views.py
from django.utils import timezone
from django.views.generic.list import ListView

from articles.models import Article

class ArticleListView(ListView):

    model = Article
    paginate_by = 100  # if pagination is desired

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['now'] = timezone.now()
        return context
        
# urls.py
from django.urls import path

from article.views import ArticleListView

urlpatterns = [
    path('', ArticleListView.as_view(), name='article-list'),
]

# article_list.html
<h1>Articles</h1>
<ul>
{% for article in object_list %}
    <li>{{ article.pub_date|date }} - {{ article.headline }}</li>
{% empty %}
    <li>No articles yet.</li>
{% endfor %}
</ul>
```

* 페이지네이션을 사용하는 경우에는 템플릿에서 object\_list에서 page\_obj로 변경해야합니다.

  
**\* 메소드 \*  
get \(request,** _**args, \***_**kwargs\)**  
- 컨텍스트에 object\_list를 추가합니다.  
- allow\_empty가 True일때 빈 목록을 표시하고 allow\_empty가 False라면 404 에러를 발생시킵니다.

