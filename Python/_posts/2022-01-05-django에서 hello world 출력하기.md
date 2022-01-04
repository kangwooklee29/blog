#### 1. django를 설치한다.

```
pip install django
```

#### 2. 새 프로젝트를 만든다.

```
django-admin startproject project1
```

\- 이때 새 프로젝트를 구성하는 소스파일들이 project1 폴더에 담기게 된다. 프로젝트를 구성하는 각 소스파일들에는 다음과 같은 내용들이 담겨있다.

- settings.py: 그 프로젝트의 기본 설정에 관한 변수들이 저장돼 있다. 그 프로젝트를 서버로 실행했을 때 그 서버에 접속을 허용할 호스트(ALLOWED_HOST), 그 프로젝트에 설치된 앱(INSTALLED_APPS), URL 관리 모듈 이름(ROOT_URLCONF), css, js, 이미지 등의 정적 파일이 담길 폴더명(STATIC_URL) 등의 변수에 값을 지정하여 이들 설정을 변경할 수 있다.

- **urls.py**: urlpatterns라는 리스트의 값을 지정할 수 있다. 이 리스트는 그 서버의 특정 URL로 요청이 들어올 때 프로젝트 내 어떤 함수를 실행시킬지를 지정한다. 보통 프로젝트 안에 포함된 앱의 views.py 소스파일에 들어있는 함수를 호출하게 한다.

- manage.py: 서버를 실행하는 소스코드가 들어있다. 파이썬으로 여러 옵션과 함께 실행하면 다양한 기능을 사용할 수 있다.

  - runserver: 프로젝트를 완성한 후 runserver 옵션을 붙여 실행하면 그 프로젝트가 올라가 있는 서버가 실행된다.

  - migrate: 옵션에 지정된 앱의 models.py 변경사항을 DB에 migrate한다. 옵션에 앱을 지정하지 않으면 전체 앱에 대하여 migration을 진행한다.

  - createsuperuser: 관리자 계정을 새로 만든다. 프로젝트를 처음 만든 후 아무 migration도 하지 않았다면 관리자 계정이 만들어지지 않음을 주의해야 한다.


#### 3. project1 폴더로 이동 후 새 앱을 만든다.

(1) 앱을 만드는 명령어를 실행한다.

```
django-admin startapp app1
```

\- 그 프로젝트에 새로운 앱이 추가되며, 그 앱을 구성하는 소스파일들이 app1 폴더에 담기게 된다. django는 보통 여러 개의 앱이 모여 하나의 프로젝트를 구성하는데, 새로 프로젝트를 만든 직후에는 어떤 앱도 포함하고 있지 않기 때문에 새로 앱을 만들어서 추가해야 한다.

\- django에서는 하나의 앱이 model - view - template 세 개의 모듈로 이루어진다. 이러한 구조는 디자인패턴 중 MVC 패턴의 일종으로, 특정 클라이언트에서 요청이 들어오면 그 요청을 view에서 처리하여 (1)DB에서 데이터를 가져올 일이 있으면 model로 요청을 보내고 (2)DB에서 가져온 데이터를 처리하여 클라이언트로 웹페이지를 보내야 할 때는 데이터를 template으로 보내 이를 처리하여 웹페이지를 보내게 한다.

\- 앱을 구성하는 각 소스파일들에는 다음과 같은 내용들이 담겨있다.

- models.py: 그 앱의 model의 소스파일. 보통 그 앱에서 사용할 DB의 스키마가 클래스 형태로 작성된다. django에서는 이 소스파일의 내용을 변경했다면 그 변경사항을 DB에 적용시키는 명령을 실행해야 비로소 변경사항이 DB에 적용되며, 이를 migration이라 한다. (DB에 대한 git의 commit과 비슷한 개념이라고 생각할 수 있다.)

- views.py: 그 앱의 view의 소스파일. urls.py에서 호출할 함수의 소스코드는 보통 이 소스파일 안에 작성한다.


(2) settings.py의 INSTALLED_APPS라는 리스트형 변수에 'app1'이라는 문자열을 원소로 추가한다.


#### 4. views.py에 Hello world! 메시지를 리턴하는 함수를 작성한다.

(1) views.py의 소스파일에 다음 코드를 추가한다.

```python
def helloworld(req): #이때 쓰는 함수는 반드시 파라미터 하나를 꼭 써줘야 한다. 
    return HttpResponse("<p>Hello world!</p>") #HttpResponse는 django.shorcuts에서 import할 수 있다.
```


(2) urls.py의 urlpatterns라는 리스트형 변수에 다음 함수 호출을 원소로 추가한다.

```python
path('helloworld/', helloworld) 
```

\- 다만, 위와 같이 쓰기에 앞서 소스파일 상단에 다음과 같이 views.py로부터 helloworld를 import하는 코드를 작성해야 한다.

```python
from app1.views import helloworld
```



#### 5. 파이썬으로 manage.py를 실행한다.

```
python manage.py runserver
```

\- 그 다음에 브라우저로 로컬호스트에 접속해 보면 Hello world! 메시지가 출력되는 것을 확인할 수 있다.





### * .html template을 이용해 Hello world! 메시지 출력하기


#### 1. .html template 파일을 만든다.

{% raw %}
```HTML
{{str1}}
```

\- 위 코드는 이 .html 파일이 template으로서 str1이라는 변수와 함께 호출됐을 때 django가 {{str1}} 자리를 str1 변수의 내용으로 치환해서 그 내용을 리턴한다는 것을 뜻한다.
{% endraw %}


#### 2. settings.py의 TEMPLATES 변수에 위에서 만든 .html template 파일이 위치한 경로를 문자열로 넣는다.

```python
'DIRS':[],
```

\- 여기서 'DIRS'라는 키의 밸류 리스트 안에 경로 문자열을 넣는다. BASE_DIR을 반드시 포함해야 한다.

\- os.path.join() 함수를 사용하여 간결하게 경로 문자열을 만들 수 있다. 다음 방식으로 사용한다.

```python
os.path.join(BASE_DIR, "templates", "helloworld")
```


#### 3. views.py에 앞서 만든 .html template 파일을 호출하는 소스코드를 쓴다.

```python
def helloworld(req): 
    return render(req, "helloworld.html", {"str1":"<p>Hello world!</p>"}) 
```

\- 위 코드는 helloworld.html이라는 template 파일을 str1이라는 변수와 함께 호출하되 str1 변수의 내용으로는 "\<p\>Hello world!\</p\>"을 대입한다는 뜻의 함수 호출이 담겨 있다.
