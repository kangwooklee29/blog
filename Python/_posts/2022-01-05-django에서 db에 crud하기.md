#### 1. DB 스키마 적기


```python
class Table1(models.Model): #models.Model이라는 클래스를 상속하는 Table1이라는 클래스를 만든다.
    def __str__(self): #관리자 페이지에서 보여질 튜플의 이름을 리턴하는 메서드 함수 
        return str(self.field1)
    field1 = models.IntegerField(default=0, null=False)
    field2 = models.CharField(default="", max_length=30)
    field3 = models.BooleanField(default=False)
```

\- 위 코드를 models.py 소스파일에 추가한 후 migration을 만든 다음 DB에 migrate를 한다.


#### 2. 새로 만든 테이블을 관리자 페이지에서 직접 CRUD 할 수 있도록 관리자 페이지에 등록하기

```python
admin.site.register(Table1)
```

\- 위 코드를 admin.py 소스파일에 추가한다. 단, admin.py 파일의 상단에 Table1을 model.py에서 import 하는 코드를 먼저 추가해야 한다.


#### 3. DB에서 튜플 가져와 template 파일에 전달하기


```python
table1 = Table1.objects.all()
```

\- 위 코드와 같이 함수를 호출하면 Table1에 담긴 모든 튜플이 table1에 저장된다. table1의 값은 render 함수로 template 파일에 전달할 수 있으며, template 파일 내 template language에서는 각 튜플의 속성값을 클래스의 멤버 변수 호출하듯 호출할 수 있다. 다음은 template language로 작성한 table1의 모든 튜플을 출력하는 코드이다.

{% raw %}
```html
{% for tuple in table1 %}
<p> {{ tuple.field1 }}, {{ tuple.field2 }}, {{ tuple.field3 }}</p>
{% endfor %}
```
{% endraw %}


#### 4. django의 forms 모듈로 웹페이지에서 DB에 직접 CRUD 할 수 있는 form 만들기

(1) forms.ModelForm을 상속하는 클래스 만들기

```python
class Table1InsertForm(forms.ModelForm): #이를 위해서는 django에서 forms 모듈을 import 해야 한다.
    class Meta:
        model = Table1 #입력 form을 만들고자 하는 테이블의 이름을 적는다.
        fields = ('field1', 'field2', 'field3') #입력 형식을 만들고자 하는 속성들의 속성명을 문자열에 담아 튜플로 적는다. 
```

(2) 그 클래스형 객체 만들어 template 파일에 전달하기


```python
table1_if = Table1InsertForm()
```

\- 위와 같이 만든 입력 form 변수를 render 함수로 template 파일에 전달할 수 있다. template 파일에서는 다음과 같이 써서 브라우저에 입력 form을 출력하게 할 수 있다.

{% raw %}
```html
<form method="POST">
    {% csrf_token %}
    {{ table1_if.as_p }}
    <button type="submit">SAVE</button>
</form>
```
{% endraw %}



(3) 입력 form으로 데이터가 입력되었을 때 이를 DB에 등록하는 코드 만들기

```python
if req.method == "POST":
    table1_if = Table1InsertForm(req.POST) #입력 form에서 입력된 데이터를 초기값으로 하는 Table1InsertForm형 객체를 만든다.
    if table1_if.is_valid(): #Table1의 스키마를 쓸 때 정했던 조건을 입력 데이터가 충족하면 True. 아니면 False.
        table1_if.save() #입력 데이터를 Table1에 추가한다.
```