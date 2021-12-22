---
layout: post
---
### 1\. for in

```javascript
const obj = { key1: 'value1', key2: 'value2', key3: 'value3' };

for (let key in obj)
{
    console.log(`속성명 ${key}의 속성값 ${obj[key]}`);
}
```

for in 반복문은 수개의 속성으로 이루어진 객체의 각 **속성명**을 매 루프마다 지정한 변수명(위 코드의 key)에 전달하는 반복문이다. 위 코드의 경우 다음과 같은 결과를 얻는다.

```HTML
속성명 key1의 속성값 value1
속성명 key2의 속성값 value2
속성명 key3의 속성값 value3
```

for in 반복문은 객체뿐 아니라 배열, 문자열에 대해서도 사용 가능하다. 이 경우 배열/문자열의 **인덱스값**이 0에서부터 배열 마지막 인덱스까지 지정된 변수명에 전달된다. _(for(let i=0; i<arr.length; i++)를 for(let i in arr)로 줄여 쓸 수 있다는 장점이 있다.)_

### 2\. for of

```javascript
const map_obj = new Map([["엄마", "사과"],["아빠", "배"],["나", "포도"]]); 

for(let value of map_obj)
{
    console.log(`과일 이름: ${value}`);
}
```

for of 반복문은 자바스크립트가 지원하는 컬렉션 중 각 원소에 차례로 하나씩 접근 가능(이를 iterable이라 한다)한 객체(Array, String, Map, Set 등)에 대해 사용 가능한 반복문이다. 이러한 객체에 대하여 for of 반복문을 사용하면, 매 루프마다 객체의 원소값이 지정된 변수명으로 전달된다. 위 코드는 다음과 같은 결과를 얻는다.

```HTML
과일 이름: 사과
과일 이름: 배
과일 이름: 포도
```