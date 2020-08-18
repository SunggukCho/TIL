# 0818 TIL - django url dealing

## 1. url name 사용하기

```html
기존 방식
<form action="/catch" method="GET">
  <label for="title">제목을 입력해 주세요</label>
  <input type="text" id="title" name="title">
  <input type="submit">
</form>

바뀐 방식
<form action="{% url 'catch' %}" method="GET">
  <label for="title">제목을 입력해 주세요</label>
  <input type="text" id="title" name="title">
  <input type="submit">
</form>
```

- 왜 이렇게 하는가?
  - 이렇게 되면 url의 주소값이 바뀌어도 `이름`을 따라가기 때문에 이름값이 변하지 않는한 자동으로 url link를 따라간다.
- 이렇게 됐을 때 url 설정은 어떻게 변하는가?
  - 각 페이지별로 name="값" 만 추가해주면 된다.

```python
기존 방식
from . import views

urlpatterns = [
    path('catch/', views.catch),   
]

바뀐 방식
from . import views

urlpatterns = [
    path('catch/', views.catch, name="catch"),   
]
```





