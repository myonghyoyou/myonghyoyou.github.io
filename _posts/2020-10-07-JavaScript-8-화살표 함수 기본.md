---
title: JavaScript (8) 화살표 함수 기본
tags: JavaScript
article_header:
  type: cover
  image:
---

### 개요

화살표 함수를 사용하면 함수 표현식보다 단순하고 간결한 문법으로 함수를 만들 수 있다.

``` javascript
let func = (arg1, arg2, ...argN) => expression
```

위와 같이 코드를 작성하면 인자 `arg1...argN`을 받는 함수 `func`이 만들어진다. 함수 `func`은 화살표(`=>`) 우측의 `표현식(Expression`)을 평가하고, 평가 결과를 반환한다.

좀 더 구체적인 예시를 살펴보자.

``` javascript
let sum = (a,b) => a+b;

alert( sum(1,2) ); //3
```

위의 예시에서 `(a,b) => a + b`는 실행되는 순간 표현식 `a + b`를 평가하고 그 결과를 반환한다.

* 인수가 하나밖에 없다면 인수를 감싸는 괄호를 생략할 수 있다.

``` javascript
let double = n => n * 2;

alert(double(3)); // 6
```

* 인수가 하나도 없을 땐 괄호를 비워놓으면 되지만, 생략해서는 안 된다.

``` javascript
let sayHello = () => alert("Hello");

sayHello();
```

화살표 함수는 함수 표현식과 같은 방법으로 사용할 수 있다.

``` javascript
let age = prompt("나이를 알려주세요", 18);

let welcome = (age < 18) ?
  () => alert("안녕") :
  () => alert("안녕하세요");
  
welcome();
```

---

### 본문이 여러줄인 화살표 함수

평가해야 할 표현식이나 구문이 여러 개인 경우에도 화살표 함수 문법을 사용해 함수를 만들 수 있다. 이 땐 중괄호 `{}` 안에 평가해야 할 코드를 넣어주면 된다. \
그리고 `reuturn` 지시자를 활용해 명시적으로 결과값을 반환해주어야 한다.

``` javascript
let sum = (a,b) => {
  let reulst = a + b;
  return result;
}

alert( sum(1,2) ); //3
```

---

참조 : <br>
* https://ko.javascript.info/switch
