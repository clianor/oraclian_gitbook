# Django - Base views

장고에는 Class Based View라는 클래스 방식의 View 작성법이 있습니다.  
장고의 CBV 방식에서는 다양한 구현체 API가 존재하는데 이번글은 그중 가장 기본적인 Base views에 대한 내용입니다.

먼저 장고의 Base views API에는 다음의 세 가지의 View가 존재합니다.   
1. View  
2. TemplateView  
3. RedirectView

순서대로 장고 공식 문서에 있는 내용대로 정리해보겠습니다.  
\( 번역글에 가깝습니다 \)  
먼저 View에 대해서입니다.

### **1. View**

View 클래스는 장고내의 모든 CBV가 상속받는 클래스입니다.  
이 클래스는 아래와 같이 사용할 수 있습니다.

```python
# views.py
from django.http import HttpResponse
from django.views import View

class MyView(View):

    def get(self, request, *args, **kwargs):
        return HttpResponse('Hello, World!')
        
        
# urls.py
from myapp.views import MyView

urlpatterns = [
    path('mine/', MyView.as_view(), name='my-view'),
]
```

여기서 MyView의 get 메소드는 mine/으로 get 요청이 들어오면 매치될 메소드입니다.  
위의 예제에서는 get 요청이 들어올시 'Hello, World!' 라는 응답을 주게 됩니다.  
  
**\* 속성 \***  
  
**http\_method\_names**  
- View에서 허용할 HTTP 메소드 리스트  
- \(default\) `['get', 'post', 'put', 'patch', 'delete', 'head', 'options', 'trace']`  
  
  
**\* 메소드 \***  
  
\(classmethod\) **as\_view \(**_**\*\*initkwargs**_**\)**  
- 요청을 받고 응답을 반환하는 호출 가능한 뷰를 리턴합니다.  
- `response = MyView.as_view()(request)`  
- 반환된 뷰에는 view\_class 및 view\_initkwargs 속성이 있습니다.  
  
**setup** **\(**_**request**_**,** _**\*args**_**,** _**\*\*kwargs**_**\)**  
- dispatch\(\) 이전에 self.request, self.args, self.kwargs와 같은 뷰 인스턴스 속성을 초기화 합니다.  
- 이 메소드를 재정의할 때는 super\(\)를 호출해야합니다.  
- 이 메소드를 재정의하면 믹스인이 자식 클래스에서 재사용할 인스턴스 속성을 설정할 수 있습니다.  
  
 **dispatch** **\(**_**request**_**,** _**\*args**_**,** _**\*\*kwargs**_**\)**  
- 요청과 인자들을 받 HTTP 응답을 반환하는 메소드.  
- HTTP 메소드를 검사하고 HTTP 메소드와 일치하는 메소드에 위임 하는 역할을 담당.  
- GET method -&gt; get, POST method -&gt; post  
  
**http\_method\_not\_allowed \(request,** _**args, \***_**kwargs\)**  
- View가 지원되지 않는 메소드로 호출이 되었을때 이 메소드가 호출됩니다.  
- HttpResponseNotAllowed를 반환합니다.  
  
**options \(request,** _**args, \***_**kwargs\)**  
- HTTP 메소드 중 OPTIONS와 같은 역할입니다.  
- 응답으로 Allow 헤더에 허용되는 메소드 목록이 있는 응답을 반환힙니다.

### 2. TemplateView

이 뷰는 주어진 템플릿을 렌더링합니다.  
또한 이 뷰는 아래의 세 뷰와 믹스인을 상속합니다.  
`django.views.generic.base.TemplateResponseMixin  
django.views.generic.base.ContextMixin  
django.views.generic.base.View`  
  
이 클래스는 아래와 같이 사용할 수 있습니다.

```python
# views.py 
from django.views.generic.base import TemplateView

from articles.models import Article

class HomePageView(TemplateView):

    template_name = "home.html"

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['latest_articles'] = Article.objects.all()[:5]
        return context
        
# urls.py
from django.urls import path

from myapp.views import HomePageView

urlpatterns = [
    path('', HomePageView.as_view(), name='home'),
]
```

Context 관련 내용이 있었으나 장고 3.1부터 deprecated되었으므로 생략하겠습니다.

### 3. RedirectView

주어진 URL로 리다이렉트합니다.  
주어진 URL은 dictionary 형태의 문자열 형식을 포함할 수 있습니다.  
URL의 % 문자는 %%로 작성되어야 파이썬으로 출력시 단일 퍼센트 기호로 변환됩니다.  
또한 이 뷰는 아래의 뷰를 상속합니다.  
`django.views.generic.base.View`  
  
주어진 URL이 없으면 Django는 HttpResponseGone\(410\)을 반환합니다.  
  
**\* 참고 \***  
http method 410: Gone  
- 요청한 콘텐츠가 서버에서 영구적으로 삭제되었으며 전달해 줄 수 있는 주소 역시 존재하지 않을때 보냄.  
- 클라이언트가 캐시와 리소스에 대한 링크를 지우길 원할때 사용한다.  
이 클래스는 아래와 같이 사용할 수 있습니다.

```python
# views.py 
from django.views.generic.base import RedirectView

from articles.models import Article

class ArticleCounterRedirectView(RedirectView):

    permanent = False
    query_string = True
    pattern_name = 'article-detail'

    def get_redirect_url(self, *args, **kwargs):
        article = get_object_or_404(Article, pk=kwargs['pk'])
        article.update_counter()
        return super().get_redirect_url(*args, **kwargs)
        
# urls.py
from django.views.generic.base import RedirectView

from article.views import ArticleCounterRedirectView, ArticleDetail

urlpatterns = [
    path('counter/<int:pk>/', ArticleCounterRedirectView.as_view(), name='article-counter'),
    path('details/<int:pk>/', ArticleDetail.as_view(), name='article-detail'),
    path('go-to-django/', RedirectView.as_view(url='https://djangoproject.com'), name='go-to-django'),
]
```

**\* 속성 \*  
  
url**  
- 리다이렉트할 URL입니다.  
- 없을경우 410\(Gone\) HTTP 에러를 발생시킵니다.  
  
**pattern\_name**  
- 리다이렉트할 URL 패턴의 이름입니다.  
- 뷰에 전달된 것과 동일한 args 및 kwargs를 사용하 수행됩니다.  
  
**permanent \(default = False\)**  
- 리다이렉트가 영구적이어야하는지 여부.  
- permanent가 True인 경우 상태코드는 301이 반환되고 False인경우 302로 반환이됩니다.  
  
**query\_string \(default = False\)**  
- GET 쿼리 문자열을 새로운 위치로 전달할지 여부.  
- True인 경우 새로운 주소에서도 쿼리 문자열이 URL에 추가되고 False인 경우 쿼리 문자열이 삭제됩니다.  
  
**\* 메소드 \*  
  
get\_redirect\_url \(**_**args, \***_**kwargs\)**  
  


