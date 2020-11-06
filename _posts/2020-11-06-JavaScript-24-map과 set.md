---
title: JavaScript (24) map과 set
tags: JavaScript
article_header:
  type: cover
  image:

---

### 개요

객체와 배열만으로는 현실 세계를 반영하기에 부족한 감이 있어서 `맵(Map)`과 `셋(Set)`이 등장하게 되었다.

---

### 맵 (Map)

맵은 키가 있는 데이터를 저장한다는 점에서 `객체`와 유사하다. 그러나 `맵`은 키에 다양한 자료형을 허용한다.

아래는 맵의 메서드와 프로퍼티이다.

* new Map() : 맵을 만든다.
* map.set(key, value) : `key`를 이용해 `value`를 저장한다.
* map.get(key, value) : `key`에 해당하는 값을 반환한다. `key`가 없으면 `undefined`를 반환한다.
* map.has(key) : `key`가 존재하면 `true`, 존재하지 않으면 `false`를 반환한다.
* map.delete(key) : `key`에 해당하는 값을 삭제한다.
* map.clear() : 맵 안의 모든 요소를 제거한다.
* map.size : 요소의 개수를 반환한다.

예시:

```` javascript
let map = new Map();

map.set('1', 'str1'); // 문자형 키
map.set(1, 'num1'); // 숫자형 키
map.set(true, 'bool1'); // 불린형 키

// 맵은 키의 타입을 변환시키지 않고 그대로 유지한다.
alert(map.get('1')); // 'str1'
alert(map.get(1)); // 'num1'

alert(map.size) // 3
````

map[key]를 통해 map에 값을 지정할 수는 있지만 이렇게 되면 객체를 사용하는 것과 다를 바 없다.

따라서 map에 값을 지정할 때는 map.set을 사용해준다.

#### map은 키로 객체를 허용한다.

예시:

```` javascript
let john = { name: "john" };

// 고객의 가게 방문 횟수
let visitsCountMap = new Map();

// john을 맵의 키로 사용
visitsCountMap.set(john, 123);

alert(visitsCountMap.get(john)); // 123
````

객체르 키로 사용할 수 있다는 것은 `맵`의 가장 중요한 기능 중 하나이다.

---

### 맵의 요소에 반복 작업하기

아래의 3가지 요소를 이용해 `맵`의 각 요소에 반복 작업이 가능하다.

* map.keys() : 각 요소의 키를 모은 반복 가능한(iterable, 이터러블) 객체 반환
* map.values() : 각 요소의 값을 모은 이터러블 객체 반환
* map.entries() : 요소의 `[키, 값]`을 한 쌍으로 하는 이터러블 객체를 반환. 이 이터러블 객체가 `for..of` 반목문의 기초로 쓰인다.

예시:

```` javascript
let recipeMap = new Map([
    ['cucumber', 500],
    ['tomatoes', 350],
    ['onion', 50]
]);

// 키(vegetable)을 대상으로 순회
for (let vegetable of recipeMap.keys()) {
    alert(vegetable); // cucmber, totmatoes, onion
}

// 값(amount)를 대상으로 순회
for (let amount of recipeMap.values()) {
    alert(amount); // 500, 350, 50
}

// [키, 값] 쌍을 대상으로 순회
for (let entry of recipeMap.entries()) {
    alert(entry); // cucmber,500 ...
}
````

`맵`은 객체와는 달리 값이 삽입된 순서대로 순회를 한다. 

`맵`은 배열과 유사하게 `forEach`도 지원한다.

```` javascript
// 각 [키, 값] 쌍을 대상으로 함수를 실행
recipMap.forEach( (value, key, map) => {
    alert(`${key}: ${value}`); // cucumber: 500 ...
})
````

---

### Object.entries: 객체를 맵으로 바꾸기

 각 요소가 키-값 쌍인 배열이나 이터러블 객체를 초기화 용도로 `맵`에 전달해 새로운 `맵`을 만들 수 있다.

``` javascript
// 각 요소가 [키, 값] 쌍인 배열
let map = new Map([
    ['1', 'str1'],
    [1, 'num1'],
    [true, 'bool1']
]);
alert(map.get('1')); // str1
```

평범한 객체를 `맵`으로 만들고 싶다면 Object.entries(obj)를 활용해야 한다. 해당 메서드는 개체의 키-값 쌍을 요소 (`[key,value]`)로 가지는 배열을 반환한다.

예시:

```` javascript
let obj = {
    name: "John",
    age: 30
};

let map = new Map(Object.entries(obj));

alert( map.get('name') ); // John
````

---

### Object.fromEntries: 맵을 객체로 바꾸기

`Object.fromEntries`를 사용하면 맵을 객체로도 바꿀 수 있다.  해당 메서드는 각 요소가 `[키, 값]` 쌍인 배열을 객체로 바꿔준다.

```` javascript
let prices = Object.fromEntries([
    ['banana', 1],
    ['orange', 2],
    ['meat', 4]
]);

// prices = { banana: 1, orange: 2, meat: 4};

alert(prices.orange); // 2
````

아래 예시는 `맵`을 객체로 바꾸는 모습이다.

```` javascript
let map = new Map();
map.set('banana', 1);
map.set('orange', 2);
map.set('meat', 4);

let obj = Object.fromEntries(map.entries(0); // 맵을 일반 객체로 전환한다.
                             
// obj = {banana: 1, orange: 2, meat: 4};

alert(obj.orange) // 2                             
````

map.entries()를 활용해 `[키, 값]`을 가지는 이터러블을 생성한 후에 `Object.fromEntries`의 인수로 넣어서 일반 객체로 변환시키는 것을 볼 수 있다.

`.entries()`를 생략하는것도 가능하다.

```` javascript
let obj = Object.fromEntries(map);
````

`map`에서의 일반적인 반복이 `map.entries()`를 사용했을 때처럼 키-쌍 값을 반환하기 때문에 꼭 `.entries()`를 써서 배열을 전달할 필요는 없다.

---

### 셋 (Set)

> 셋(Set) : 중복을 허용하지 않는 값을 모아놓은 컬렉션. 키가 없는 값이 저장된다.

셋의 주요 메서드는 다음과 같다.

* new Set(iterable) : 셋을 만든다. `이터러블` 객체를 전달받으면 그 안의 값을 복사해 셋에 넣는다.
* set.add(value) : 값을 추가하고 셋 자신을 반환한다.
* set.delte(value) : 값을 제거한다. 호출 시점에 셋 내에 값이 있어서 제거에 성공하면 `true`, 아니면 `false`를 반환한다.
* set.has(value) : 셋 내에 값이 존재하면 `true`, 아니면 `false`를 반환한다.
* set.clear() : 셋을 비운다.
* set.size : 셋에 몇 개의 값이 있는지 카운트한다.

셋 내부에 동일한 value가 있다면 `set.add(value)`를 아무리 많이 호출해도 값이 추가되지 않는다. 셋에는 중복을 허용하지 않기 때문이다.

방문자 방명록은 한 방문자가 단 한번만 기록되어야 한다. 이럴 때 셋을 사용한다.

```` javascript
let set = new Set();
let john = { name: 'John' };
let pete = { name: 'Pete' };
let mary = { name: 'Mary' };

// 고객이 여러번 방문해도 한 번만 기록된다.
set.add(john);
set.add(pete);
set.add(mary);
set.add(john);
set.add(mary);

// 셋에는 유일무이한 값만 저장된다.
alert( set.size ); // 3

for (let user of set) {
    alert(user.name); // 삽입된 순서에 따라 John, Pete, Mary 순으로 출력된다. 
}
````


---

### 셋의 값에 반복 작업하기

`for..of`나 `forEach`를 사용하면 셋의 값을 대상으로 반복 작업을 수행할 수 잇다.

``` javascript
let set = new Set(["oranges", "apples", "bananas"]);

for (let value of set) alert(value);

// forEach에도 동일하게 작동한다.

set.forEach( (value, valueAgain, set) => {
    alert(value);
});
```

`forEach` 에 사용되는 콜백함수는 세 개의 인수를 받는다. 첫 번째는 `값`, 두 번째도 똑같이 `valueAgain`을 받는다. 세 번째는 목표하는 객체(셋)이다. 

이렇게 구현된 이유는 `맵`과의 호환성 때문이다. `맵`의 `forEach`에 쓰인 콜백인 세 개의 인수를 받을 때를 위해서이다.

`셋`에도 반복 작업을 위한 메서드가 존재한다.

* set.keys() : 셋 내의 모든 값을 포함하는 이터러블 객체를 반환한다.
* set.values() : `set.keys`와 동일한 작업을 한다. `맵`과의 호환성을 위해 존재하는 메서드이다.
* set.entries() : 셋 내의 각 값을 이용해 만든 `[value, value]` 배열을 포함하는 이터러블 객체를 반환한다. `맵`과의 호환성을 위해 존재한다.

---

참조 : <br>

* https://ko.javascript.info/map-set
