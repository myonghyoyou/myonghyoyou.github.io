---
title: JavaScript 4. 형 변환 
tags: JavaScript
article_header:
  type: cover
  image:
---

### 개요

함수와 연산자에 전달되는 값은 대부분 적절한 자료형으로 자동 변환된다. 이를 '형 변환'(type conversion) 이라고 한다. 이 외에, 전달 받은 값을 의도적으로 원하는 타입으로
변환해주는 경우도 형 변환이라고 할 수 있다.

---

### 문자형으로 변환

문자형으로의 형 변환은 문자형의 값이 필요할 때 발생한다. \
`alert` 메서드는 매개 변수로 문자형을 받기 때문에, `alert(value)` 에서 value는 문자형이어야 한다. 만약, 다른 형의 값을 전달 받으면 자동으로 문자형으로 변환된다.

`String(value)` 함수를 호출해 전달받은 값을 문자열로 변환하는 것도 가능하다.

``` javascript
let value = true;
alert(typeof value); // boolean

value = String(value); // 변수 value엔 문자열 "true"가 저장된다.
alert(typeof value); // String
```

`false`는 문자열 `"false"`로, `null`은 문자열 `"null"`로 변환되는 것과 같이, 문자형으로의 변환은 대부분 예측 가능한 방식으로 일어난다.

---

### 숫자형으로 변환

숫자형으로의 변환은 수학과 관련된 함수와 표현식에서 자동으로 일어난다. 

``` javascript
alert("6"/"2"); // 3, 문자열이 숫자형으로 자동 변환된 후에 연산이 진행된다.
```

`Number(value)` 함수를 사용하면 주어진 값(`value`)를 숫자형으로 명시해서 변환할 수 있다.

``` javascript
let str = "123";
alert(typeof str); // String

let num = Number(str); // 문자열 "123"이 숫자 123으로 변환
alert(typeof str); // number
```

숫자형 값을 사용해 무언가를 하려고 하는데 그 값을 문자 기반 폼(form)을 통해 입력받을 때는, 이런 명시적 형변환이 필수이다.

한편, 숫자 이외의 글자가 들어가 있는 문자열을 숫자형으로 변환하려고 하면, 그 결과는 `NaN`이 된다. 

``` javascript
let age = Number("임의의 문자열 123");

alert(age); // NaN, 형 변환이 실패
```

아래는 숫자형으로 변환 시 적용되는 규칙이다.

|전달받은 값|형 변환 후|
|---|---|
|`undefined`|`NaN`|
|`null`|`0`|
|`true and false`|`1`과 `0`|
|`string`|문자열의 처음과 끝 공백이 제거된다. 공백 제거 후 남아있는 문자열이 없다면 0, 그렇지 않다면 문자열에서 숫자를 읽는다. 변환에 실패하면 `NaN`이 된다.|

예시 :

``` javascript
alert( Number("   123   ")); // 123
alert( Number("123z")); // NaN (z의 형 변환 실패)
alert( Number(true)); // 1
alert( Number(false)); // 0
```

`null`은 형 변환 시 `0`이 되고 `undefined`는 `NaN`이 되는 것을 유의해야 한다.

---

### 불린형으로 변환

불린형으로의 변환은 아주 간단하다.

주로 논리 연산을 수행할 때 발생하고 `Boolean(value)`을 호출하면 명시적으로 Boolean으로의 형 변환이 가능하다.

Boolean형으로 변환 시 적용되는 규칙은 다음과 같다.

* 숫자 `0`, 빈 문자열, `null`, `undefined`, `NaN`과 같이 직관적으로도 "비어있다고" 판단되는 값들은 `false`가 된다.
* 그 외의 값은 `true`로 반환된다.

예시 : 

``` javascript
alert( Boolean(1)); // true
alert( Boolean(0)); // false

alert( Boolean("Hello")); // true
alert( Boolean("")); // false, 빈 문자열
```

>주의 : 문자열 `"0"`은 `true`이다.
>>PHP 등의 일부 언어에선 문자열 `"0"`을 `false`로 취급한다. 그러나 자바스크립트에선 비어있지 않은 문자열은 모두 `true`이다.

---

### 요약
문자, 숫자, 논리형으로의 형 변환은 자주 발생하는 형 변환이다.

**`문자형으로의 변환`** \
무언가를 출력할 때 자주 발생. `String(value)`를 사용하여 문자형으로 명시적 변환이 가능하다.

**`숫자형으로의 변환`** \
수학 관련 연산 시 자주 발생. `Number(value)`를 사용하여 숫자형으로 명시적 변환이 가능하다.

숫자형으로의 변환은 아래와 같은 규칙을 따른다.

|전달받은 값|형 변환 후|
|---|---|
|`undefined`|`NaN`|
|`null`|`0`|
|`true and false`|`1`과 `0`|
|`string`|문자열의 처음과 끝 공백이 제거된다. 공백 제거 후 남아있는 문자열이 없다면 0, 그렇지 않다면 문자열에서 숫자를 읽는다. 변환에 실패하면 `NaN`이 된다.|


**`Boolean형으로 변환`** \
논리 연산 시 발생. `Boolean(value)`를 사용하여 Boolean형으로 명시적 변환이 가능하다.

Boolean형의로의 변환은 아래와 같은 규칙을 따른다.

|전달받은 값|형 변환 후|
|---|---|
|`0`,`null`,`undefined`,`NaN`,`""`|`false`|
|그 외의 값|`true`|

---

참조 : <br>
* https://ko.javascript.info/type-conversions
