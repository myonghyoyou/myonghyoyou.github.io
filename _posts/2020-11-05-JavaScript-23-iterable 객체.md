---
title: JavaScript (23) iterable 객체
tags: JavaScript
article_header:
  type: cover
  image:




---

### 개요

> iterable 객체 : 배열을 일반화한 객체

iterable 이라는 개념을 사용하면 어떤 객체에든 `for..of` 반복문을 적용할 수 있다.

대표적인 iterable로는 배열과 문자열이 있다. 

---

### Symbol.iterator

아래 예시의 객체 `range`는 숫자 간격을 나타낸다.

``` javascript
let range = {
    from : 1,
    to: 5
}
```

`range`를 iterable 객체로 만들기 위해선 (`for...of`가 동작하도록 하려면) 객체에 `Symbol.iterator` 라는 메서드를 추가해야 한다.

1. `for..of`가 시작되자마자 `Symbol.iterator`를 호출. `Symbol.iterator`는 반드시 *이터레이터* (iterator, 메서드 `next`가 있는 객체)를 반환해야 한다.
2. 이후 `for..of`는 *반환된 객체 (이터레이터)*만을 대상으로 동작한다.
3. `for..of`에 다음 값이 필요하면, `for..of`는 이터레이터의 `next()` 메서드를 호출한다.
4. `next()`의 반환 값은 `{done: Boolean, value: any}`와 같은 형태여야 한다. `done=true`는 반복이 종료되었음을 의미한다. `done=false`일 땐 `value`에 다음 값이 저장된다.

아래 예시는 `range`를 반복 가능한 객체로 만들어준다.

``` javascript
let range = {
    from: 1,
    to: 5
}

// 1. for..of 최초 호출 시, Symbol.iterator가 호출된다.
range[Symbol.iterator] = function() {
    
    // Symbol.iterator는 이터레이터 객체를 반환한다.
    // 2. for..of는 이터레이터 객체만을 대상으로 동작하며, 이때 다음 값도 정해진다.
    return {
        current: this.from,
        last: this.to,
        
        next() {
            if (this.current <= this.last) {
                return {done: false, value: this.current ++};
            } else {
                return {done: true};
            }
        }
    }
}

for (let num of range) {
    alert(num);
}
```

이터러블 객체의 핵심은 '관심사의 분리(Seperation of concern, SoC)' 이다. 

* `range`엔 메서드 `next()`가 없다.
* 대신 `range[Symbol.iterator]()`를 호출해서 만든 '이터레이터' 객체와 이 객체의 메서드 `next()`에서 반복에 사용될 값을 만든다.

위와 같이 하면 이터레이터 객체와 반복 대상 간의 분리를 이뤄낼 수 있다.

이터레이터 객체와 반복 대상 객체를 합치면 `range` 자체를 이터레이터로 만들 수 있다.

``` javascript
let range = {
    from: 1,
    to: 5,
    
    [Symbol.iterator] () {
        this.current = this.from;
        return this;
    },
    
    next() {
        if (this.current <= this.to) {
            return { done: false, value: this.current++ };
        } else {
            return { done: true };
        }
    }
};

for (let num of range) {
    alert(num);
}
```

위 예시에서 `range[Symbol.iterator]()`는 객체 `range`를 반환한다. 반환된 객체에는 필수 메서드인 `next()`가 있고 `this.current`에 반복이 얼마나 진행되었는지 나타내는 값도 저장된다. 

---

### 문자열은 이터러블이다.

배열과 문자열은 대표적인 내장 이터러블이다.

`for..of`를 통해 문자열의 각 글자를 순회할 수 있다.

``` javascript
for (let char of "test") {
    alert ( char ); // t, e, s, t 출력
}
```

서로게이트 쌍(surrogate pair)에서도 동작한다.

* surrogate pair: UTF-16으로 표현할 수 없는 값들을 두 개의 16비트 문자로 변환되어 한 쌍(32비트)이 해당 문자를 표현하는 것

``` javascript
let str = '𝒳😂';
for (let char of str) {
    alert( char ); // 𝒳와 😂가 차례대로 출력됨
}
```

---

### 이터레이터 명시적으로 호출하기

이터레이터를 수동으로 호출하는 것도 가능하다.

``` javascript
let str = "Hello";

let iterator = str[Symbol.iterator]();

while (true) {
    let result = iterator.next();
    if (result.done) break;
    alert (result.value);
}
```

위의 방법은 자주 사용하지는 않으나, `for..of`를 사용하는 것보다 반복 과정을 더 쉽게 통제할 수 있다.

---

### 이터러블과 유사 배열

* 이터러블(iterable) : 메서드 `Symbol.iterator` 가 구현된 객체 
* 유사 배열(array-like) : 인덱스와 `length` 프로퍼티가 존재하여 배열처럼 보이는 객체

문자열은 이터러블 객체이면서 유사배열 객체이기도 하다. 

이터러블 객체라고 해서 유사 배열 객체는 아니고, 유사 배열 객체라고 해서 이터러블 객체인 것도 아니다.

---

### Array.from

이터러블과 유사 배열은 배열이 아니기 때문에 `push`, `pop` 등의 메서드를 지원하지 않는다.

이러한 불편을 해소하기 위해 `Array.from`을 사용한다. 

`Array.from`은 객체를 받아 이터러블이나 유사 배열인지 조사한다. 넘겨 받은 인수가 둘 중 하나일 경우, 새로운 배열을 만들고 객체의 모든 요소를 새롭게 만든 배열로 복사한다.

예시:

``` javascript
let arrayLike = {
    0: "Hello",
    1: "World",
    length :2
};

let arr = Array.from(arrayLike);
alert(arr.pop()); // World 
```

아래는 이터러블을 사용한 예시이다.

``` javascript
let range = {
    from: 1,
    to: 5,
    
    [Symbol.iterator] () {
        this.current = this.from;
        return this;
    },
    
    next() {
        if (this.current <= this.to) {
            return { done: false, value: this.current++ };
        } else {
            return { done: true };
        }
    }
};

let arr = Array.from(range);
alert(arr);
```

`Array.from`엔 '매핑(mapping)' 함수를 옵션으로 넘겨줄 수 있다.

``` javascript
Array.from(obj[, mapFn, thisArg])
```

`mapFn`을 두 번째 인수로 넘겨주면 새로운 배열에 `obj`의 요소를 추가하기 전에 각 요소를 대상으로 `mapFn`을 적용할 수 있다. 새로운 배열엔 `mapFn`을 적용하고 반환된 값이 추가된다. `thisArg`는 각 요소의 `this`를 지정하게 해준다.

예시:

``` javascript
let range = {
    from: 1,
    to: 5,
    
    [Symbol.iterator] () {
        this.current = this.from;
        return this;
    },
    
    next() {
        if (this.current <= this.to) {
            return { done: false, value: this.current++ };
        } else {
            return { done: true };
        }
    }
};

let arr = Array.from(range, num => num *num);

alert(arr);
```

`Array.from`을 통해 문자열을 배열로 만들 수 있다. (문자열은 이터러블 객체이자 유사 배열 객체이기 때문)

``` javascript
let str = '𝒳😂';

let chars = Array.from(str);

for (let char of chars){
    console.log(char);
}

console.log(chars.length);
```


---

참조 : <br>

* https://ko.javascript.info/iterable
