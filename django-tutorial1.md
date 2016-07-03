## Django Tutorial
  - https://docs.djangoproject.com/en/1.9/intro/tutorial01/

-----

### 설치

1. python 설치
  - https://www.python.org/

2. Django 설치
  - `pip install Django`
  - 설치 확인은 `python -m django --version`

-----

### 프로젝트 생성

1. 커맨드로 기본 프로젝트 구조 생성
  - `django-admin startproject mysite`

2. 생성 파일 확인
  ```
  mysite/
      manage.py
      mysite/
          __init__.py
          settings.py
          urls.py
          wsgi.py
  ```
  - 바깥쪽 `mysite/` 폴더는 프로젝트를 감싸는 역할을 하고, 이름은 Django 에게 중요하지 않아서 원하면 바꿔도 된다.
  - `manage.py` : Django 프로젝트와 상호작용 할 수 있도록 해주는 커맨드라인 유틸리티이다. 
  더 상세한 내용은 [django-admin and manage.py](https://docs.djangoproject.com/en/1.9/ref/django-admin/) 에서 확인 가능하다.
  - 안쪽 `mysite/` 폴더는 프로젝트를 위한 실제 Python 패키지이다. 
  이 폴더명은 Python 패키지 이름이 되며 안에 있는 것을 import 하려면 이 이름을 사용해야 한다. (예: mysite.urls).
  - `mysite/__init__.py` : Python에게 이 디렉토리가 Python 패키지라고 간주하도록 하는 빈 파일이다.
  Python 초심자라면 공식 Python 문서인 [more about packages](https://docs.python.org/3/tutorial/modules.html#tut-packages)를 읽어라.
  - `mysite/settings.py` 이 Django 프로젝트의 설정이다. 
  [Django settings](https://docs.djangoproject.com/en/1.9/topics/settings/)에서 설정하는 방법에 대해 알려준다.
  - `mysite/urls.py` : 이 Django 프로젝트의 URL 선언이다; Django로 생성된 사이트의 목차이다. 
  (URL dispatcher)[https://docs.djangoproject.com/en/1.9/topics/http/urls/]에서 URL에 대해 더 읽을 수 있다.
  - `mysite/wsgi` : 프로젝트를 WSGI 호환 웹 서버로 제공하기 위한 엔트리 포인트이다.
  (How to deploy with WSGI)[https://docs.djangoproject.com/en/1.9/howto/deployment/wsgi/]에서 WSGI과 함께 배포하는 방법에 대한 자세한 내용을 참조할 수 있다.
  
-----

### 개발 서버 실행
1. 실행 
  - 바깥쪽 `mysite` 폴더로 이동한 뒤 커맨드를 이용해 실행한다. `python manage.py runserver`
  - 실행시 나오는 데이터베이스 관련 경고는 곧 다룰 것이므로 지금은 무시한다.
  - Django 에 경량 웹서버를 포함해두어서 배포 전 까지 Apache 등의 서버 설정을 고려하지 않고 빠르게 개발할 수 있다.
  - 개발 서버는 실제 환경에서 사용하지 말고 개발 중에만 사용하도록 한다.

2. 접속 확인
  - http://127.0.0.1:8000/
  
  




  
  
  
  
  
