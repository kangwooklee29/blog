---
layout: post
---
### 1\. forEach()

```javascript
const array1 = ["사과", "배", "포도"];
const obj = {key1: "과일 이름"};

const cb_func = function(elem, idx, arr)
{
   console.log(`배열 [${arr}]의 ${idx}번 인덱스의 ${this.key1}: ${elem}`);
};

array1.forEach(cb_func, obj);
```

\- forEach()는 Array()형으로 선언된 객체에서 사용 가능한 메서드로, 파라미터로는 콜백함수와 (콜백함수 내에서 this로 호출 가능한) 객체를 갖는다(객체 파라미터는 생략 가능). forEach() 메서드를 인자들과 함께 호출하면, 해당 메서드를 호출한 Array의 각 원소를 인자로 해서 **콜백함수가 Array의 원소 개수만큼 실행**된다. 예를 들어, 위 코드는 다음과 같은 결과를 출력한다.

```HTML
배열 [사과, 배, 포도]의 0번 인덱스의 과일 이름: 사과
배열 [사과, 배, 포도]의 1번 인덱스의 과일 이름: 배
배열 [사과, 배, 포도]의 2번 인덱스의 과일 이름: 포도
```

\- 콜백함수에는 최대 3개의 인자가 전달되며, 이는 각각 (그 차례의 Array의 원소, 그때의 인덱스값, Array 객체 내용 전체) 이다. 파라미터가 3개보다 적은 콜백함수를 forEach()의 인자로 넣는 것도 가능하다. _(아예 인자가 없는 콜백함수를 forEach()의 인자로 넣는 것도 허용된다.)_ \- forEach()를 호출하는 객체는 반드시 변수명에 저장된 객체여야 하는 것은 아니며 대괄호 기호 바로 뒤에 메서드로 호출하는 것도 가능하다. 예를 들어 다음과 같은 표현이 허용된다.

```HTML
[1, 2, 3, 4, 5].forEach(_=>console.log(`ㅋㅋ`));
```

### 2\. map()

```javascript
const array1 = ["사과", "배", "포도"];
const obj = {key1: "과일 이름"};

const cb_func = function(elem, idx, arr)
{
   return `배열 [${arr}]의 ${idx}번 인덱스의 ${this.key1}: ${elem}`;
};

const array2 = array1.map(cb_func, obj);
for (let elem of array2)
    console.log(elem);
```

\- map() 또한 Array()형으로 선언된 객체에서 사용 가능한 메서드로, forEach()와 마찬가지로 파라미터로 콜백함수와 객체(생략 가능)를 갖는다. forEach()와 마찬가지로 map() 메서드를 호출하면 Array의 각 원소를 인자로 해서 콜백함수가 Array의 원소 개수만큼 실행되는데, map() 메서드는 이때 콜백함수가 리턴한 리턴값을 원소로 한 배열을 만들어 리턴한다. 예를 들어 위 코드는 다음과 같은 결과를 출력한다.

```HTML
배열 [사과, 배, 포도]의 0번 인덱스의 과일 이름: 사과
배열 [사과, 배, 포도]의 1번 인덱스의 과일 이름: 배
배열 [사과, 배, 포도]의 2번 인덱스의 과일 이름: 포도
```

### 3\. Array.from()

\- foreach()나 map()은 Array형 객체에 대해서만 지원되는 메서드이므로, 설사 iterable한 객체라 하더라도 이러한 메서드를 직접 사용하는 것은 불가능하다. 그러나 Array.from()이라는 메서드는 인자로 전달된 iterable한 객체를 Array 객체로 변환해 리턴해주므로 이를 이용하여 Array가 아닌 객체들에 대해 간편하게 forEach() 등의 메서드를 사용할 수 있다.

```javascript
const map1 = new Map([["엄마", "사과"], ["아빠", "배"], ["나", "포도"]]);
const array1 = Array.from(map1);
```

\- 위 코드의 경우 array1 배열에 Array 객체인 \[\["엄마", "사과"\], \["아빠", "배"\], \["나", "포도"\]\] 배열이 저장된다.