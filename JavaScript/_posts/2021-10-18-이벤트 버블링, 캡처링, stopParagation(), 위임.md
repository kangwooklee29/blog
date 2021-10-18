### 1. 이벤트 버블링, 캡처링

\- 웹페이지에 클릭, 이벤트 등 어떠한 이벤트가 발생했을 때, 자바스크립트로 이를 감지하고 그에 따라 특정한 자바스크립트 함수를 실행시키게 할 수 있다. 이처럼 이벤트를 감지하여 특정 함수를 실행시키게 하는 것을 '이벤트 핸들러'라 하며, 여러 방법이 있는데 addEventListener() 메서드를 이용하면 이벤트 버블링/캡처링 등을 쉽게 제어할 수 있다는 이점이 있다.

\- DOM 트리는 엘리먼트 노드들이 서로 부모자식 관계를 이루고 있다. 이때, 한 노드에 추가한 이벤트 리스너의 발동 조건이 **그 조상노드 중 어느 하나에 추가한 이벤트 리스너의 발동 조건과 동일**한 경우를 생각할 수 있다. 이때 두 노드 중 **어느 노드에 등록된 콜백함수를 먼저 실행시킬지**가 문제가 된다. 가장 자식 노드에 등록된 콜백함수가 먼저 실행되고 그 다음으로 그 부모 노드에 등록된 콜백함수가 차례대로 실행되는 것을 **이벤트 버블링** 이라 하며, 반대로 가장 조상 노드에 등록된 콜백함수가 먼저 실행되고 그 다음으로 그 자식 노드에 등록된 콜백함수가 차례대로 실행되는 것을 **이벤트 캡처링**이라 한다. 

\- 어떤 콜백함수가 이벤트 버블링/캡처링에 따라 실행될 것인지는 이벤트 리스너 등록 때 설정할 수 있다. 이벤트가 발생하면, 일단 **DOM 트리의 루트노드에서 이벤트가 발생한 해당 노드로 차례대로 내려가되 이벤트 캡처링에 따라 실행되기로 한 콜백함수를 만나면 그 함수가 먼저 실행**된다. 그리고 이벤트가 발생한 해당 노드에서부터 **다시 차례대로 상위 노드로 올라오면서 이벤트 버블링에 따라 실행되기로 한 콜백함수들이 차례대로 실행**된다.

```HTML
node1.addEventListner("click", func1);
node1.addEventListner("click", func1, false);
node1.addEventListner("click", func1, {capture: false});
```
위와 같이 쓰면 콜백함수가 이벤트 버블링에 따라 실행된다.

```HTML
node1.addEventListner("click", func1, true);
node1.addEventListner("click", func1, {capture: true});
```
위와 같이 쓰면 콜백함수가 이벤트 캡처링에 따라 실행된다.



### 2. stopPropagation() 메서드

```HTML
<div id="div1">
    <div id="div2">
        <div id="div3">
            여기를 클릭!
        </div>
    </div>
</div>

<script>
    document.querySelector("#div1").addEventListener("click", e=>{console.log("div1");e.stopPropagation();}, true);
    document.querySelector("#div2").addEventListener("click", _=>console.log("div2"));
    document.querySelector("#div3").addEventListener("click", _=>console.log("div3"));
</script>
```

\- 콜백함수의 인자로 전달되는 객체에 stopPropagation() 메서드가 있는데, 콜백함수 내에서 이 메서드를 호출할 경우 이벤트 캡처링-버블링이 일어나다 그 메서드 호출과 함께 거기서 이벤트 캡처링-버블링이 중단된다. 예를 들어, 위 코드의 경우 콘솔에 'div1'만 출력되고 div2, div3 메시지는 출력되지 않는다.



### 3. 이벤트 위임

```HTML
<ul id="ul1">
    <li id="li1">사과</li>
    <li id="li2">배</li>
    <li id="li3">포도</li>
</ul>

<script>
    document.querySelector("#ul1").addEventListener("click", e =>
    {
		console.log(e.target.innerHTML);
    });
</script>
```

\- 수개의 엘리먼트 노드에 각각 이벤트 리스너를 추가하는 대신, 그 노드들의 **공통 조상 노드에 이벤트 리스너를 추가**하고 그 콜백함수에 전달되는 객체를 사용하여 이벤트가 발생한 노드별로 수행될 함수 내용을 통제할 수 있다. 이처럼 수개의 엘리먼트 노드에 각각 이벤트 리스너를 추가하는 대신 그 공통 조상노드에 이벤트 리스너를 추가하는 것을 이벤트 위임(event delegation)이라 한다.

\- 개별 엘리먼트 노드에 각각 이벤트 리스너를 추가하는 것에 비해 이벤트 위임은 (1)관리가 쉽고 메모리 누수 위험이 적다는 점, (2)동적으로 엘리먼트를 추가하는 경우 추가되는 개별 엘리먼트마다 일일이 이벤트 리스너를 새로 추가할 필요가 없다는 점 등에서 많은 이점이 있다.









