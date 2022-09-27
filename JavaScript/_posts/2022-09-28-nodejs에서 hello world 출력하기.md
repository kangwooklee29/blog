---
title: Node.js의 기본 개념
---

### 1. Node.js의 개념 

\- 웹 브라우저는 컴퓨터 내외부 서버에 HTTP/HTTPS 접속을 해서 HTML, XML, JavaScript 등의 코드와 CSS 등의 서식정보, 그리고 그외 음악, 사진, 동영상 등 여러 미디어 파일을 얻어온 후 코드를 해석하고 화면에 표현할 각종 요소를 렌더링하여 최종적으로 사용자에게 웹 페이지를 보여준다. 이처럼 여러 형식의 코드와 파일을 처리하기 때문에 웹 브라우저는 각 기능을 담당하는 여러 요소로 구성돼 있는데, 그 중에 JavaScript를 처리하는 요소를 JavaScript 인터프리터 또는 JavaScript 엔진이라 한다. 웹 브라우저마다 각각 다른 JavaScript 엔진을 두고 있는데, 대표적으로 과거 넷스케이프에서 쓰였고 현재도 파이어폭스에서 쓰고 있는 SpiderMonkey, 사파리에서 쓰고 있는 WebKit, 크롬에서 쓰고 있는 V8 등이 있다.

\- Node.js는 크롬의 V8 엔진만을 떼온 후 여기에 OS 커널과의 소통을 위한 C 라이브러리인 libuv를 결합해, 브라우저 없이도 JavaScript 코드를 실행할 수 있도록 한 플랫폼이다. 따라서 Node.js를 이용하면 브라우저 없이 단독으로 JavaScript 코드를 실행시킬 수 있다. 

\- 다만 크롬에서 작동하는 JavaScript 코드라 하더라도 다 Node.js에서 실행 가능한 것은 아닌데, 예를 들어 크롬에서 지원되는 브라우저 관련 API는 V8에 직접 포함된 것이 아니라 이러한 API 호출이 있는 코드는 Node.js에서는 에러가 발생할 수 있다. (document 엘리먼트가 이러한 API 호출에 의한 엘리먼트이며, 시스템 함수로 생각하기 쉬운 fetch() 또한 마찬가지다. fetch()를 사용하고자 한다면 이를 지원하는 패키지를 내 앱에 추가해야 한다.) 모듈 관련해서도 크롬과 Node.js가 채택한 문법이 달라(Node.js는 모듈 관련 문법으로 CommonJS를 허용하는 반면 크롬은 ES6만을 따른다) 모듈을 불러올 때 문법에 주의해야 한다. 그 외에도 몇가지 문법적 차이가 있으나(예를 들어 Node.js에서는 var, const, let으로 변수 선언 않을 시 에러가 발생할 수 있다), 그럼에도 거의 대부분의 영역에서 문법이 일치한다.


### 2. Hello world 출력하기

index.js에 다음 내용이 있다 하자.

```html
console.log("Hello world");
```


OS에 Node.js를 설치 후 터미널에 다음 명령어를 입력하면 화면에 Hello world가 출력된다.


```html
node index.js
```



### 3. Node.js에서 모듈화 관련 문법

#### 1) CommonJS 방식

\- ES6와 달리, CommonJS는 (1)그 파일의 객체 또는 함수 중 다른 파일에서 호출할 수 있게 할 객체 또는 함수들을 exports라는 이름의 객체의 멤버로 할당하면 (2)그 파일의 경로를 인자로 하는 require()라는 시스템 함수를 호출하여 그 exports 객체를 리턴받아 그 멤버들로서 사용할 수 있다. 

예를 들어 module1.js의 코드 내용이 다음과 같다 하자.
```javascript
exports.fruit1 = "사과";
exports.fruit2 = "포도";
```
index.js의 코드 내용은 다음과 같다 하자.
```javascript
const my_fruits = require("./module1.js");
console.log(my_fruits.fruit1);
```
이 경우 `사과`가 출력된다.

#### 2) ES6 방식

\- Node.js에서도 ES6의 문법 그대로 모듈화 코드를 구현할 수 있다. 단, 이 경우 반드시 package.json에서 type 속성을 module로 지정해야 한다. 



### 4. Node.js의 패키지의 개념과 package.json

#### 1) 패키지

\- Node.js는 JavaScript 소스파일과 모듈 소스파일들을 하나의 패키지로 묶어 관리하며, 내가 만든 패키지를 온라인 패키지 저장소에 배포할 수도 있고 온라인에 배포돼 있는 패키지를 내가 만든 패키지 안에 포함시킬 수도 있다. Node.js로 실행시킬 JavaScript 소스파일이 있는 폴더에 package.json이라는 파일을 두어 내 앱이 모듈을 포함하는 패키지인지, 온라인에 배포돼 있는 패키지 중 내 패키지에 추가한 패키지가 있는지 등을 기록할 수 있다. (파이썬의 경우 기본적으로 온라인에 배포돼 있는 패키지를 설치하면 이후 생성하는 모든 파이썬 소스코드에서 그 패키지를 사용할 수 있으나, Node.js의 경우 온라인에 배포돼 있는 패키지를 설치하는 명령어를 실행하더라도 그 명령어를 실행한 폴더에 있는 패키지에 한해서만 그 패키지가 설치되는 게 기본 설정이다.)

#### 2) package.json

\- package.json은 기본적으로 내 패키지의 이름, 버전, 설명 등의 명세를 담는 파일인데, 이러한 내용이 꼭 있어야만 Node.js로 JS 소스파일을 실행시킬 수 있는 것은 아니다. 단 모듈 관련해 ES6 문법을 사용했거나 온라인에 배포돼 있는 모듈을 사용했다면 반드시 그에 관한 내용을 이 안에 기록해야 한다.

\- npm의 다음 명령어로 간단히 package.json의 초기 포맷을 생성할 수 있다. 

```html
npm init
```

이 명령어로 package.json를 생성 시 그 파일에 패키지의 이름, 버전, 설명 등의 명세에 관한 포맷이 JSON 형식으로 초기화돼 기록된다.


#### 3) package.json의 scripts 속성

\- package.json의 scripts 속성은 특정 단축명에 터미널 내에서 실행시키고자 하는 터미널 명령어 스크립트를 지정하는 속성으로, 단축명을 지정해 두면 npm, yarn 같은 패키지 매니저를 통해 굉장히 긴 스크립트도 굉장히 간결하게 실행시킬 수 있다. 

예를 들어 package.json의 내용이 다음과 같다 하자.
```json
{
  "name": "test1",
  "version": "1.0.0",
  "description": "test",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "lkwks",
  "license": "ISC"
}
```
이때 다음 명령어를 실행해보자.
```html
npm run start
```
그러면 `node index.js` 라는 스크립트가 실행된다.

