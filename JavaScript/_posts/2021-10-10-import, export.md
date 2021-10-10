### 1. 개요

서로 다른 .js 파일들을 import, export 예약어를 사용하여 마치 하나의 소스파일을 작성한 것처럼 묶어 사용할 수 있다. 이처럼 모듈 단위로 개발한 수개의 .js 파일을 import, export 예약어로 서로 묶는 식으로 개발하면 다음 이점을 얻을 수 있다.
 
 (1) \<script> 태그로 .js를 여러 개 로드할 시 로드하는 .js 파일의 순서에 따라 미처 선언되지 않은 함수/변수를 호출하는 에러가 발생할 수 있는데, import, export 예약어를 사용하면 이러한 문제를 극복할 수 있다.
 
 (2) \<script> 태그로 .js를 여러 개 로드할 시 한 파일에서 사용한 변수명이 다른 파일에서도 사용되고 있어 오작동이 일어나는 변수 스코프 문제가 발생할 수 있는데, import, export 예약어를 사용하면 선택된 변수명만이 다른 모듈에서 참조될 수 있어 이러한 문제를 극복할 수 있다.


### 2. 사용 방법

#### 1) 간단한 원리

 \- .js 파일을 로드하는 페이지에서는 단 하나의 .js 파일만을 로드한다. 이때 로드되는 .js 파일에서는 다른 .js 파일들을 import 예약어를 통해 프로젝트에 포함시킨다.

 \- import 예약어를 사용할 때에는, 다른 .js 파일의 함수 또는 변수 중 이 import 예약어를 사용한 .js 파일에서 사용할 함수 또는 변수의 이름을 import 예약어를 쓸 때 적어주어야 한다. 이때, 이처럼 import로 지정된 함수 또는 변수는 그 내용이 작성된 .js 파일에서는 export 예약어를 통해 '이 함수/변수는 다른 .js 파일에서 import 될 수 있는 상태임' 표시를 해주어야 한다.

#### 2) 구체적인 사용 방법

 \- import, export 예약어가 사용된 .js 파일을 로드하는 페이지에서는 이 .js파일을 로드할 때 다음과 같이 type 속성을 module로 지정해서 로드해야 한다.

```HTML
<script type="module" src="source1.js"></script>
```


### 3. 문법


#### 1) export


```javascript
export 
{
    num1 as NUM1,
    num2,
    func1 as FUNC1,
    func2 as default,
    func3
}

const num1, num2;
function func1() { ... }
function func2() { ... }
function func3() { ... }
```

 \- export 예약어를 통해 표시된 함수/변수만이 다른 .js 파일에서 import로 호출될 수 있다. 구체적으로, 위 코드와 같이 export 예약어 뒤에 중괄호쌍({}) 사이에 함수/변수의 이름을 쓰면 그 변수/함수들이 다른 .js 파일에서 import로 호출될 수 있다.

 \- export로 함수/변수를 내보낼 때 이들을 import하는 .js 파일에서 사용될 이름을 as 예약어를 사용해 표시할 수 있다. 예를 들면 위 코드의 num1 변수는 이를 import하는 다른 .js 파일에서는 NUM1라는 이름을 가진 것으로 여겨진다.

 \- **export하는 함수 중 적어도 하나는 반드시 default 표시**가 되어 있어야 하며, export 표시를 했다고 하더라도 default 표시가 단 하나도 없다면 에러가 발생한다. default 표시는 위 코드의 func2 함수처럼 as 예약어를 사용해 할 수 있다.
 
\- export 표시를 위 코드처럼 변수/함수 선언과 분리해서 적을 수도 있지만 아래 코드처럼 변수/함수 선언과 함께 적을 수도 있다. _(단, 이 경우 as 예약어를 사용할 수는 없다.)_ 

```javascript
export const num1, num2;
export function func1() { ... }
export default function func2() { ... }
```

\- export 예약어를 선언식 앞에 쓰는 것은 변수선언 또는 함수선언식의 경우에만 가능하며, 함수표현식으로 함수를 선언하며 export를 붙이는 경우에는 에러가 발생한다. 예를 들어, 다음 코드는 에러가 발생한다.

```javascript
export const func1 = _=> { ... };
```

\- 다만, export default 예약어를 사용하는 경우에는 익명함수를 default 함수로 export 할 수 있다. 예를 들어 다음과 같은 코드가 허용된다.

```javascript
export default _=> { ... }
```

#### 2) import

```javascript
import
{
    NUM1 as num1,
    num2
} from './source1.js';
```
\- 다른 .js 파일에서 export로 내보내진 함수/변수를 import로 가져와 사용할 수 있다. 구체적으로, 위 코드와 같이 import 예약어 뒤에 중괄호쌍({}) 사이에 함수/변수의 이름을 쓰면 코드에서 그 변수/함수들을 자유롭게 호출할 수 있다.

\- import 중괄호쌍 뒤에는 from 예약어와 함께 변수/함수들을 가져오는 .js 파일의 경로를 적어야 한다. 이때, 파일명 앞에 반드시 슬래시(/)를 적어줘야 하며 슬래시가 없으면 에러가 발생한다. 같은 폴더에 있는 .js 파일을 로드하고자 한다면 './파일명.js' 형태로 경로를 쓰면 된다.

\- import로 함수/변수를 가져올 때 as 예약어를 사용하여 export 될 때 이름과 다른 이름으로 코드에서 그 함수/변수를 사용할 수 있다. 예를 들어 위 코드의 NUM1는 위 코드에서 num1이라는 이름으로 사용된다.

```javascript
import
{
    FUNC1 as func1,
    func3
} as SOURCE1 from './source1.js';

import DFunc1 from './source1.js';
```

\- 중괄호 뒤에 as 예약어를 사용하는 경우 중괄호 안의 함수/변수들을 마치 클래스의 멤버처럼 호출할 수 있다. 예를 들어 위 코드의 경우 source1.js에서 export된 func1 함수는 이 코드에서 SOURCE1.func1이라는 이름으로 호출할 수 있다.

\- 어떤 .js 파일의 default 함수를 import할 때에는 위 코드의 마지막줄처럼 import와 from 사이에 **중괄호 없이 특정한 함수명을 적는 식으로 import한다.** 이때 적는 함수명은 그 .js파일 내에서 쓰인 default 함수의 함수명과 **달라도 무방**하다. _(그렇기에 default 함수는 익명함수여도 되는 것이다.)_ 예를 들어 위 코드의 source1.js 파일 내에 DFunc1이라는 이름을 가진 함수가 전혀 없다 하더라도, DFunc1을 호출하면 source1.js 파일의 default 함수가 호출된다.

\- default 함수 이름을 중괄호쌍 사이에 적어놓았다면, 아무리 default 함수의 이름을 정확히 적었다고 하더라도 그 함수명 호출 시 **default 함수가 아닌 다른 함수를 호출한 것으로 취급**되어 제대로 작동하지 않는다.

