#### 1. .html template 파일에 다음 코드를 넣는다.
{% raw %}

(1) 파일 상단에 {% load static %} 추가하기

```
{% load static %}
```

(2) 정적 template 파일 경로를 다음과 같은 형식으로 쓰기

```html
<img src="{% static 'img1.jpg' %}">
```
{% endraw %}

#### 2. settings.py 소스파일에서 STATICFILES_DIRS 변수를 정의한다.

```python
STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]
```

\- 'static'은 새로 추가한 정적 template 파일이 저장된 폴더 이름이다.