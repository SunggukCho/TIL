# django 바닥부터 시작하기

## 0. 장고 구조 

django는 MVC 모델 그러나 이름이 조금 다름

- M - Model (다른거랑 똑같음) : 데이터 저장

- T - Template (다른 프레임워크의 View역할) : 사용자에게 전달되는 모습
- V - View (다른 프레임워크의 Controller역할) :  Model, Templates 제어



![django 03. 첫번째 장고앱 7 - MTV 연동하기 · 초보몽키의 개발공부로그](https://wayhome25.github.io/assets/post-img/django/mtv.png)

![Image Pasted at 2020-8-14 13-13](C:\Users\sung kook\Desktop\TIL\markdown.assets\Image Pasted at 2020-8-14 13-13.png)



## 1. 시작하기

1. 설치 

   ```bash
   #최신 버전 설치
   $pip install django
   
   #특정 버전 설치
   $pip install django==2.17
   ```

2. CLI 명령어로 프로젝트 생성/서버 ON

   ```bash
   #프로젝트 생성
   $django-admin startproject {프로젝트명}
   
   #프로젝트 내부로 한 칸 이동
   $cd {프로젝트명}
   
   #서버 ON
   $python manage.py runserver
   [----공통부분-----][--개별부분--]
   
   ```

   <http://127.0.0.1:8000/>

   ![20200814_101751](C:\Users\sung kook\Desktop\TIL\markdown.assets\20200814_101751.png)

위 화면이 나오면 서버 잘 켜진 상태이다

3. Project에 App 만들기

   ```bash
   $ python manage.py startapp {app이름}
   # 대표적으로 articles를 가장 많이 쓴다
   # App의 이름은 "복수형"으로 쓴다.
   ```

   

## 2. 각 파일 살펴보기

#### 1. Settings.py

```python
# Application definition

INSTALLED_APPS = [
    #1. local apps
    'articles',
    #2. 3rd party apps
    #3. django apps
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

- django 기본 권장사항은 local apps를 위에서부터 추가해준다.
- 3rd party app을 사용하면 local apps 다음에 추가해준다. 마지막에 django apps들을 위치시킨다.
- 마지막 요소에 콤마(,), 트레일링 콤마라 부르는 것이 항상 따라온다.


# Internationalization
# https://docs.djangoproject.com/en/3.1/topics/i18n/

##언어설정
LANGUAGE_CODE = 'en-us' -> 한글은 'ko-kr'

TIME_ZONE = 'UTC'

USE_I18N = True

USE_L10N = True

USE_TZ = True
```



#### 2. url 설정

```python
from django.contrib import admin
from django.urls import path
from articles import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('index/', views.index),
    path('dinner/', views.dinner),
]

#path 함수 사용
#path(1번 url 2번 템플릿 대응)
```



#### 3. views.py

```python
import random
from django.shortcuts import render

# Create your views here.
def index(request):							
    return render(request, 'index.html')


def dinner(request):
    menus = ['족발', ' 햄버거','치킨','피자']
    pick = random.choice(menus)

    return render(request, 'dinner.html', {'pick': pick,}).


#템플릿 이름으로 views에서는 함수를 만든다고 생각하면 좋다. (이 함수는 동일 이름의 template을 제어하는 것이다!)
#return 할 때는 render()함수를 써주고, (request받은 것, 보낼 곳, 보내는 것)으로 작성한다.
```



#### 4. Template( dinner.html )

```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1> 오늘 저녁은 {{ pick }} 입니다.</h1>
</body>
</html>

dinner views에서 보내주는 pick을 html에서 보여주려면
{{}} 중괄호 두 개로 감싸주면 된다.
```



## 4. Convention

#### 1. django imports style guide

1. standard library
2. 3rd party
3. Django
4. local django (다른 모듈 혹은 내가 만든 것)

```python
import random						#1. Standard library
from django.shortcuts import render #3. Django
```



#### 2. trailing comma :

- 목록을 작성할 때 맨 마지막 요소의 뒤에도 쉼표를 붙여, 다음 번에 요소를 추가하기 쉽도록 하는 컨벤션이 있다!



#### 3. view 목록 작성 `views.py`

- 각 def(함수) 간에는 두 줄 띄어쓰기





## 5. Django Template Language(DTL)

- django template system에서 사용하는 built-in template system이다
- 조건, 반복, 치환, 필터, 변수 등의 기능을 제공한다.
- 프로그래밍적 로직이 아니라 `프레젠테이션(presentation)`을 표현하기 위한 것
  - 프로그래밍 로직은 `VIEW`에서 한다.
- python 처럼 if, for 등의 로직을 사용할 수는 있으나 단순히 python code로 실행되는 것이 아님.



#### syntax

- `variable` {{}}
- `variable with filter` {{variable|filter}}
- `tag`: {% tag %}



### 템플릿 시스템 설계 철학

- 장고는 템플릿 시스템이 표현을 제어하는 도구이자 표현에 관련된 로직일뿐이라 생각한다.
- 템플릿 시스템에서는 이런 기본목표를 넘어서는 기능을 지원해서는 안된다.
- 