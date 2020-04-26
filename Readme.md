# Django 개발

## 순서

### 1. Local 환경에 설치, project 구성

### 2. Project, App 구성 둘러보기

### 3. URL 설정

### 4. View에서 하는 일(1)

### 5. Variable Routing

### 6. Django Templates Language

### 7. View에서 하는 일(2)

### 8. Model 기초



---

### 1. Local 환경에 설치, project 구성

#### 장고 설치

`pip install django==2.1.5`

#### 프로젝트 생성

`django-admin startproject <your_project_name>`

#### 프로젝트 폴더로 이동

`cd <your_project_name>`

#### 앱 생성

`python manage.py startapp <your_app_name>`

#### 서버 실행

`python manage.py runserver <your_port_number>`

#### 참고 자료

공식문서

> https://docs.djangoproject.com/en/3.0/intro/tutorial01/

### 2. Project 구성 둘러보기

프로젝트는 앱들로 구성되어 있다. 하나의 앱은 하나의 기능으로 분리되어 있다고 볼 수 있다. **앱의 이름은 복수형**으로 짓는다. 앱을 추가하고 나면 settings.py에 앱을 반드시 등록해준다.

#### 2.1 `setting.py`

언어, 정적 파일 탐색 위치 설정, app 추가 등이 가능하다.

```python
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'articles'
]

# 서버 언어 및 시간대 설정
LANGUAGE_CODE = 'ko-KR'

TIME_ZONE = 'Asia/Seuol'

USE_I18N = True

USE_L10N = True

USE_TZ = True

STATIC_URL = '/static/'
```

- manage.py

명령어와 관련된 코드가 들어 있다.

- wsgi.py

web server gateway interface의 약자이다. 배포와 관련한 코드가 들어 있다.

- \__init\_\_.py

안에는 비어 있지만 묘듈로 사용하기 위해 삭제하면 안 된다.

### 3. URL 설정

프로젝트 이름의 디렉토리에서 한번 분기하고 각 앱 안에서 한번 더 분기한다. path의 첫번째 인자에 url 문자열이 들어오는데 반드시 `/`로 끝내준다.

```python
# mysite/urls.py
from django.urls import path, include

urlpatterns = [
    path('articles/', include('articles.urls'))
]
```

```python
# articles/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index)
]
```



### 4. View에서 하는 일

```python
from django.shortcuts import render

def index(request):
    context = {
        'info' : info,
    }
    render render(request, 'index.html', context)
```

함수이름을 정의하고 첫번째 인자로 'request'를 받는다. 반환할 때는 첫번째 인자로 request, 두번째 인자로 반환하고자 하는 페이지, 세번째로는 전달하려는 데이터를 딕셔너리 형태로 보내준다. 함수 안에서 데이터를 처리해서 처리가 끝난 데이터를 context에 dictionary 형태로 전달한다.

### 5. Variable Routing

> url의 특정 위치의 값을 변수로 활용

#### 5.1 `urls.py`

```python
urlpatterns = [
	path('user/<str:name>/', views.index),
]
```

#### 5.2 `views.py`

```python
def index(request, name):
    context = {
        name : 'name',
    }
    render(request, 'index.html', context)
```

함수의 첫번째 인자인 request는 바꿔도 된다. 반환할 때 같은 이름으로 바꿔주면 동작은 한다. 하지만 컨벤션을 절대 바꾸면 안된다. urls.py에서 넘어오는 변수 이름은 views.py에서 두번째 인자 이름과 같아야 한다. 

### 6. DTL

#### 6.1 기본 문법

- {{}}
- 변수의 길이 : {{ variable|length }}
- 출력되는 길이의 제한 : {{ variable|truncatechars:10 }}
- |는 `필터`라고 부른다. 

#### 6.2 문법

- {% %}

- for

  - 기본 for 문

  ```django
  {% for article in articles %}
  <!-- do something -->
  {% endfor %}
  ```

  - {% empty %}를 쓰면 views.py에서 전달 받은 배열이 비었는데 순회할 경우 실행할 부분을 정의해 줄 수 있음

  ```django
  {% for article in articles %}
  <!-- do something -->
  {% empty %}
  <!-- do something when articles is empty -->
  {% endfor %}
  ```

- if

  ```django
  {% if user == 'admin' %}
  	<!-- 사용자가 admin일 경우 -->
  {% else %}
  	<!-- 사용자가 admin이 아닌 경우 -->
  {% endif%}
  ```

#### 6.3 주석

```django
<!-- {# 주석 #} -->
```

위와 같이 주석 처리를 해야 클라이언트(브라우저 소스보기)에서 완전히 주석처리가 된 것을 알 수 있다.

#### 6.4 Template 상속

반복적인 코드는 줄여나가고 활용할 수 있는 부분은 활용할 수 있게 나누어서 설정할 수 있다.

##### 부모 template file

```django
<!-- codes -->
{% block css%}
{% endblock %}
<!-- codes -->
{% block content %}
{% endblock%}
<!-- codes -->
```

##### 자식 template file

```django
{% extends 'parent.html' %}
{% block css %}
<style>
	<!-- css code -->
</style>
{% endblock %}

{% block content %}
	<!-- block codes-->
{% endblock %}
```

#### 6.5 forloop

```html
{{ forloop.counter }}
{{ forloop.counter0 }}
```

#### 6.6 built-in tag, filter

```html
{{ contene|legnth }}
{{ contene|truncatechars:10 }}
```

#### 6.7 settings.py

`APP_DIRS:True`: installed_apps에 등록된 app의 모든 templates폴더아래 파일들을 사용한다.

`'DIRS':[os.path.join(BASE_DIR, 'your_project_name', 'templates')]`

- 각 앱 아래에 `templates`디렉토리 아래에 `your_app_name`디렉토리를 만들고 그 아래에 html 파일을 저장한다.

### 7. View에서 하는 일(2)

```python
def create(request):
    # request.GET.get('keyword(input name)')
    # request.method
    # request.path
    # 등등 request에 많은 정보가 담겨있다.
```

`request`는 `HTTPresponse`객체이다.  

[공식문서]: https://docs.djangoproject.com/en/3.0/ref/request-response/



### 8. Model 기초

#### 8.1 스키마

각각의 열들이 어떤 정보를 어떤 형태로 저장할지 지정할 수 있다.

#### 8.2 테이블

행과 열로 구성되어 있다.

- 행: row, record
- 열: column, 속성, 필드

#### 8.4 DB조작언어

 `Object Relational Mapping`: 객체 조작 언어

`pip install django-extensions ipython`

- INSTALLED_APPS에 `django-extensions`추가

---

django-CRUD에서 이어서 할 예정