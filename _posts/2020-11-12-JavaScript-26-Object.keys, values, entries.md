---
title: JavaScript (26) Object.keys, values, entries
tags: JavaScript
article_header:
  type: cover
  image:



---

### 개요

`keys()`, `values()`, `entries()` 는 아래의 자료구조에 적용할 수 있다.

* `Map`
* `Set`
* `Array`

일반 객체에도 유사한 메서들을 적용시킬 수 있지만, `Map`, `Set`, `Array`에 적용하는 메서드와는 문법이 다르다.

---

### Object.keys, values, entries

일반 객체엔 다음과 같은 메서드를 사용할 수 있다.

* Object.keys() : 키가 담긴 배열을 반환한다.
* Object.values() : 값이 담긴 배열을 반환한다. 
* Object.entries() : `[key, value]` 쌍이 담긴 배열을 반환한다.

`Map`, `Set`, `Array` 에 적용하는 메서드와 객체에 적용하는 메서드의 차이는 다음과 같다.

|           | 맵            | 객체             |
| --------- | ------------- | ---------------- |
| 호출 문법 | map.keys()    | Object.keys(obj) |
| 반환 값   | iterable 객체 | '진짜' 배열      |

첫 번째 차이 : obj.keys()가 아닌 Object.keys(obj)를 호출한다.

이렇게 문법에 차이가 있는 이유는 유연성 때문이다. 자바스크립트에선 복잡한 자료구조 전체 (배열 등) 가 객체에 기반한다. 그러다 보니 객체 `data`에 자체적으로 메서드 `data.values()`를 구현해 사용하는 경우가 있을 수 있다. 이렇게 커스텀 메서드를 구현한 상태라도 `Object.values(data)`같이 다른 형태로 메서드를 호출할 수 있으면 커스텀 메서드와 내장 메서드 둘 다 사용할 수 있다.

두 번째 차이는 메서드 `Object.*`를 호출하면 iterable 객체가 아닌 아닌 배열을 반환한다는 점이다. 배열을 반환하는 이유는 하위 호환성 때문이다.

예시:

````javascript
let user = {
    name: "John",
    age: 30
};
````

* Object.keys(user) = ["name", "age"];
* Object.values(user) = ["John", 30];
* Object.entries(user) = [["name", "John"], ["age", 30]];

아래 예시는 Object.values를 이용하여 프로퍼티 값을 대상으로 원하는 작업을 하는 모습이다.

``` javascript
let user = {
    name: "John",
    age: 30
};

// 값을 순회한다.
for (let value of Object.values(user)) {
    alert(value); // "John", 30
}
```

---

### 객체 변환하기

객체엔 `map`, `filter` 같은 배열 전용 메서드를 사용할 수 없다.

하지만 `Object.entries`와 `Object.fromEntries`를 순차적으로 적용하면 객체에도 배열 전용 메서드를 사용할 수 있다.

1. `Object.entries(obj)`를 이용해 객체의 키-쌍 배열을 요소로 가지는 배열을 생성한다.
2. *1*에서 만든 배열에 `map` 과 같은 배열 전용 메서드를 사용한다.
3. *2*에서 반환된 배열에 `Object.fromEntries(array)`를 적용시켜 배열을 다시 객체로 만든다.

예시:

```` javascript
let prices = {
    banana: 1,
    orange: 2,
    apple: 4
}

let doublePrices = Object.fromEntries(
	Object.entries(prices).map(([key,value]) => [key, value * 2])
);

alert(doublePrices.apple); // 8
````

---

참조 : <br>

* https://ko.javascript.info/keys-values-entries
