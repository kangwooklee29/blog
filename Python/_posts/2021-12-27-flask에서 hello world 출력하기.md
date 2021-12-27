#### 1. 빈 폴더로 이동한 후 그 폴더에 가상환경을 생성한다.

```
python -m venv .
```

\- '-m venv .'는 venv라는 모듈을 파이썬으로 실행해 현재 위치의 폴더에 가상환경을 생성하도록 하는 옵션이다. venv는 파이썬에 내장된 가상환경 생성 모듈로서, 지정된 경로에 가상환경을 생성한다.

#### 2. 명령어를 입력하여 새로 생성한 가상환경을 실행시킨다.

```
scripts\activate
```

\- 가상환경을 생성한 폴더에서 위 명령어를 입력하면 새로 생성한 가상환경이 실행된다. 정상적으로 가상환경이 실행되었다면 cmd의 경로 맨 앞에 (\<현재 폴더명\>) 표시가 붙는다.

\- 가상환경 종료는 다음 명령어를 입력하면 된다.

```
deactivate
```


#### 3. 그 가상환경에 flask를 설치한다.


```
pip install flask
```

#### 4. 그 폴더에 다음과 같은 코드를 내용으로 하는 .py 파일을 추가한다.

```python
from flask import Flask

obj1 = Flask(__name__)

@obj1.route("/") 
def hello_world():
    return "<p>Hello world!</p>"

if __name__ == "__main__": #서버의 중복 실행을 방지하는 조건문. 이 소스파일을 직접 파이썬으로 실행했을 때만 서버를 실행하게 한다.
    obj1.run(debug=True) #debug 모드로 서버를 실행한다.
```


### 5. 파이썬으로 위 .py 파일을 실행한다.

```
python <파일명>.py
```

\- cmd에서 위 명령어를 입력하면 flask 서버가 실행된다. 브라우저 주소창에서 https://localhost:5000 을 입력하면 브라우저 창에 "Hello world!" 메시지가 뜨는 것을 확인할 수 있다.



#### * route() 메서드

```python
@obj1.route("/<user_id>", methods=['GET', 'POST'])
def func1(user_id):
    if obj1.method == 'GET':
        return user_id

    rq_data = request.get_json() #요청으로 함께 들어온 메시지를 json 형식의 메시지로 보고 이를 list 또는 dictionary형으로 변형해서 rq_data 변수에 담는다.
    new_data = {"user_id": user_id}
    return jsonify(new_data) #new_data를 json 형식으로 변형해 담은 Response형 변수를 리턴한다. 
``` 
 
\- route() 메서드는 Flask 클래스의 메서드로서, 인자로 전달된 HTTP URI에 지정된 HTTP method로 요청이 들어올 경우 이 메서드 바로 아랫줄의 함수의 리턴값을 클라이언트에 대한 응답 메시지로 전송한다. HTTP method를 지정하지 않고 route() 메서드를 호출할 수도 있으며, 이 경우 GET method를 지정해 route() 메서드를 호출한 것으로 취급한다.

\- route() 메서드의 인자로 전달된 HTTP URI에 해당하는 문자열에 \<\>로 둘러싸인 부분이 있을 경우, 클라이언트가 \<\>로 둘러싸인 부분 앞까지 주소가 동일하고 그 이후 부분에 어떤 것을 써서 그 URL로 접속을 했다면 그 '어떤 것'이 문자열로서 route() 메서드 아랫줄 함수의 인자로 전달된다.