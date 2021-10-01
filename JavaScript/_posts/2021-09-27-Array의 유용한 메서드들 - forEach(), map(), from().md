---
layout: post
title: Array의 유용한 메서드들 - forEach(), map(), from(), call()
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

\- 콜백함수에는 최대 3개의 인자가 전달되며, 이는 각각 (그 차례의 Array의 원소, 그때의 인덱스값, Array 객체 내용 전체) 이다. 파라미터가 3개보다 적은 콜백함수를 forEach()의 인자로 넣는 것도 가능하다. _(아예 인자가 없는 콜백함수를 forEach()의 인자로 넣는 것도 허용된다.)_ 

\- forEach()를 호출하는 객체는 반드시 변수명에 저장된 객체여야 하는 것은 아니며 대괄호 기호 바로 뒤에 메서드로 호출하는 것도 가능하다. 예를 들어 다음과 같은 표현이 허용된다.

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

### 3\. 배열 아닌 이터러블 객체에 대한 forEach, map 사용

#### 1) Array.from()

\- forEach()나 map()은 Array형 객체에 대해서만 지원되는 메서드이므로, 설사 iterable한 객체라 하더라도 배열이 아닌 한 이러한 메서드를 직접 사용하는 것은 불가능하다. 그러나 Array.from()이라는 메서드는 인자로 전달된 iterable한 객체를 Array 객체로 변환해 리턴해주므로 이를 이용하여 Array가 아닌 객체들에 대해 간편하게 forEach() 등의 메서드를 사용할 수 있다.

```javascript
const map1 = new Map([["엄마", "사과"], ["아빠", "배"], ["나", "포도"]]);
const array1 = Array.from(map1);
```

\- 위 코드의 경우 array1 배열에 Array 객체인 \[\["엄마", "사과"\], \["아빠", "배"\], \["나", "포도"\]\] 배열이 저장된다.

\- 이러한 특성을 이용하여, Array.from() 바로 뒤에 forEach(), map() 메서드를 붙여 쓸 수도 있다. 예를 들면, 위 코드의 뒤에 다음과 같은 코드를 쓸 수 있다.

```javascript
Array.from(map1).forEach(elem=>console.log(`${elem[0]}는 ${elem[1]}를 좋아해.`));
```

\- 이 코드는 다음 결과를 낸다.

```HTML
엄마는 사과를 좋아해.
아빠는 배를 좋아해.
나는 포도를 좋아해.
```

#### 2) forEach.call() 메서드 이용

```javascript
const map1 = new Map([["엄마", "사과"], ["아빠", "배"], ["철수", "포도"]]);
["장미", "코스모스", "프리지아"].forEach.call(map1, 
    (elem, i, arr) => 
        console.log(`{$elem[0]}가 좋아하는 과일은 {$elem[1]}이고 좋아하는 꽃은 {$arr[i]}이다.`));
```
\- forEach에는 call()이라는 메서드가 있는데, 인자로 이터러블 객체와 콜백함수를 가져 배열 아닌 이터러블 객체에 대해서도 forEach 메서드를 사용할 수 있게 해준다. 이 경우 콜백함수의 인자는 최대 3개인 것이 기존 forEach()의 인자인 콜백함수와 같으며, 그 인자는 각각  (그 차례의 이터러블 객체의 원소, 그때의 인덱스값, forEach 앞 배열 내용 전체)이다. 위 코드는 다음 결과를 낸다.

```HTML
엄마가 좋아하는 과일은 사과이고 좋아하는 꽃은 장미이다.
아빠가 좋아하는 과일은 배이고 좋아하는 꽃은 코스모스이다.
철수가 좋아하는 과일은 포도이고 좋아하는 꽃은 프리지아이다.
```
