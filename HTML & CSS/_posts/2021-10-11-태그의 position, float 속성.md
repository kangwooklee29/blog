### 1. 개요

\- position 속성은 그 엘리먼트를 노드를 어떤 위치에 배치해 화면에 보여줄지를 결정하는 속성이다. 기본 속성은 static(=문서 흐름에 맞게 배치)이지만, relative, absolute, fixed, sticky 같은 속성을 주어서 그 엘리먼트가 화면에 배치되는 위치를 바꿀 수 있다.

\- **position의 속성을 특별히 지정하지 않는다면, 그 엘리먼트에 top, bottom, left, right 속성을 주더라도 효과가 나타나지 않는다.** 그러나 position에 static 외 속성을 지정 후 이러한 top, bottom, left, right 속성을 준다면 그 효과가 나타나게 된다.


### 2. 속성 별 특성

#### 1) relative

\- **'static일 때 그 엘리먼트가 있었을 위치'를 기준으로 top, bottom, left, right를 적용**하는 속성이다. top, bottom, left, right 속성을 특별히 지정하지 않는다면 적어도 그 엘리먼트에 한해서는 static과 완벽히 동일한 특성을 보인다.

\- relative로 지정된 엘리먼트는 **그 자손 엘리먼트 중 absolute로 지정된 엘리먼트의 top, bottom, left, right 속성으로 이동하는 위치의 기준점**이 된다.

#### 2) absolute

\- **조상노드 중 relative로 지정된 노드를 기준으로 top, bottom, left, right를 적용**하는 속성이다.

#### 3) fixed

\- 문서의 흐름을 따르지 않는다는 점에서 absolute와 유사하나, absolute와 달리 **브라우저 왼쪽 위 꼭지점을 기준**으로 top, bottom, left, right를 적용한다는 점이 다른 속성.

#### 4) sticky

\- sticky 속성은 일단 조상노드 중 화면 overflow가 있어 스크롤바가 있을 때 의미가 있는 속성으로, 스크롤바가 없다면 static과 다를 바 없는 속성이다. sticky 속성으로 지정된 엘리먼트는 다음과 같이 행동한다.

(1) 다음 상황에서는 static처럼 행동한다.

- 스크롤바가 아직 '이 엘리먼트가 static일 때 있을 위치'까지 오지 않은 경우

- 이 엘리먼트가 화면에 나타나게 되었더라도, '이 엘리먼트가 static일 때 있을 위치'가 아직 화면에 보이는 경우

_(-> 정리하면, 브라우저 왼쪽 위 꼭지점을 기준으로 그 엘리먼트의 static 좌표가 지정된 top, bottom, left, right 좌표보다 더 큰 경우.)_

(2) 다음 상황에서는 fixed처럼 행동한다.

- 스크롤바를 더 내려서 '이 엘리먼트가 static일 때 있을 위치'가 화면에서 사라진 경우

_(-> 정리하면, 브라우저 왼쪽 위 꼭지점을 기준으로 그 엘리먼트의 static 좌표가 지정된 top, bottom, left, right 좌표보다 더 작아진 경우. 더 작아지지 않게 fixed처럼 행동하는 것.)_



### 3. float 속성

\- img 태그의 경우 inline 엘리먼트이기 때문에, 특별한 처리 없이 크기가 큰 img 태그 바로 옆에 문자열을 배치할 경우 img 태그 옆에 문자열 위로 빈 공간이 넓게 자리하게 된다. 이때 img 태그에 float:left 속성을 설정하면 화면 왼쪽에 img 태그가 배치되고 그 오른쪽으로 문자열이 자연스럽게 배치된다. float 속성은 이처럼 **근처의 inline 엘리먼트를 옆으로 밀어내고 그 자신을 그보다 우선하여 왼쪽/오른쪽에 배치하는 속성**으로서, 기본적으로 position 속성과는 작동방식이 다르다.

\- float 속성은 구체적으로 다음과 같은 특성을 갖는다.

(1) position을 absolute, fixed로 설정하지 않았더라도 부모노드 내부 영역의 가장 왼쪽 또는 오른쪽에 자기 자신을 배치시킨다.

(2) inline 엘리먼트였더라도 block 엘리먼트와 유사한 속성을 갖게 된다.

(3) 주변에 있는 inline 엘리먼트를 그 자신의 옆으로 밀어낸다. 이때 주변에 있는 inline 엘리먼트는 float 엘리먼트의 형제가 아니라 해도 float 엘리먼트에 의해 밀려난다. (반대로 주변에 있는 block 엘리먼트는 이 float 엘리먼트 밑에 깔린다. float 엘리먼트가 마치 position:absolute 또는 position:fixed 엘리먼트처럼 행동하기 때문.)

(4) 부모가 같고 float 속성값이 같은 엘리먼트끼리는 서로 inline-block 엘리먼트들이 갖는 것과 같은 유사한 관계를 갖는다. (즉, 부모가 같은 float:left 엘리먼트끼리는 왼쪽부터 순서대로 차곡차곡 쌓인다.)

(5) position이 absolute나 fixed로 설정되면 float를 따로 설정했다 하더라도 무시된다.
