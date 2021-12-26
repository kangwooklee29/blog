#### 1. 빈 폴더로 이동한 후 그 폴더에 가상환경을 생성한다.

```
python -m venv .
```

#### 2. 명령어를 입력하여 새로 생성한 가상환경을 실행시킨다.

```
scripts\activate
```

\- 가상환경을 생성한 폴더에서 위 명령어를 입력하면 새로 생성한 가상환경이 실행된다. 정상적으로 가상환경이 실행되었다면 cmd의 경로 맨 앞에 (\<현재 폴더명\>) 표시가 붙는다.

\- 가상환경 종료는 다음 명령어를 입력하면 된다.

```
scripts\deactivate
```


#### 3. 그 가상환경에 flask를 설치한다.


```
pip install flask
```

#### 4. 그 폴더에 다음과 같은 코드를 내용으로 하고 이름이 app.py인 파일을 추가한다.

```python
from flask import Flask

obj1 = Flask(__name__)

@obj1.route("/") #인자로 전달된 "/"라는 URL(=서버)로 접속되면 바로 이 아랫줄의 함수의 리턴값을 클라이언트의 브라우저로 전송한다. 
def hello_world():
    return "<body>Hello world!</body>"
```


### 5. flask를 실행한다.

```
flask run
```

\- cmd에서 위 명령어를 입력하면 flask 서버가 실행된다. 브라우저 주소창에서 https://localhost:5000 을 입력하면 브라우저 창에 "Hello world!" 메시지가 뜨는 것을 확인할 수 있다.