# Django 개발

## 순서

### 1. Local 환경에 설치, project 구성

### 2. Project, App 구성 둘러보기

### 3. URL 설정

### 4. View에서 하는 일 (1)

### 5. Django Templates Language

### 6. Model 기초



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



