---
title: JavaScript (7) 함수 표현식
tags: JavaScript
article_header:
  type: cover
  image:
---

### 개요

JavaScript는 함수를 특별한 종류의 값으로 취급한다. 다른 언어처럼 '특별한 동작을 하는 구조'로 취급하지 않는다. 

함수는 아래와 같이 `함수 선언`을 통해 만들어질 수 있다.

``` javascript
function sayHello(){
  alert("Hello");
}
```

그 외에도 `함수 표현식`을 사용해서 함수를 만들 수 있다.

``` javascript
let sayHello = function(){
  alert("Hello");
}
```

함수가 어떤 방식으로 만들어졌는지에 관계없이 함수는 값이고, 따라서 변수에 할당할 수 있다. 

위의 예시에선 함수가 변수 `sayHello`에 저장된 값이다.

함수는 값이기 때문에 `alert`을 이용하여 함수 코드를 출력할 수 있다.

``` javascript
function sayHello(){
  alert("Hello");
}

alert(sayHello);
```

마지막 줄에서 sayHello 옆에는 괄호가 없기 때문에 함수가 실행되지 않는다. 자바스크립트는 괄호가 있어야만 함수가 호출된다.

자바스크립트에서 함수는 값이다. 그러므로 함수를 값처럼 취급할 수 있다. 위의 코드에선 함수 소스 코드가 문자형으로 바뀌어 출력됐다.

함수는 `sayHello()`처럼 호출할 수 있다는 점 때문에 다른 값들과는 다른 특별한 종류의 값이다.

그러나 그 본질은 값이기 때문에 다른 값에 할 수 있는 일을 함수에도 할 수 있다.

아래와 같이 변수를 복사해 다른 변수에 할당할 수 있다.

``` javascript
function sayHello() {
  alert("Hello");
}

let func = sayHello;

sayHello(); // Hello
func(); // Hello, sayHello가 복사되서 실행됐기에 같은 결과값이 나온다.
```

---

### 콜백 함수

매개변수가 3개 있는 함수, ask(qeustion, yes, no)를 작성해보자. 각 매개변수에 대한 설명은 다음과 같다.

`qeustion` = 질문

`yes` = "예" 라고 대답한 경우 실행되는 함수

`no` = "아니오" 라고 대답한 경우 실행되는 함수

함수는 반드시 `question(질문)` 해야 하고, 사용자의 답변에 따라 `yes()`나 `no()`를 호출한다.

``` javascript
function ask(question, yes, no) {
  if (confirm(question)) yes()
  else no();
}

function showOk() {
  alert("동의하셨습니다.");
};

function showCancel(){
  alert("아니오 버튼을 누르셨습니다");
}

ask("동의하십니까", showOk, showCancel);
```

**함수 `ask`의 인수, `showOk`와 `showCancel`은 콜백 함수 또는 콜백이라고 불린다.**

함수를 함수의 인수로 전달하고, 필요하면 인수로 전달한 그 함수를 "나중에 호출(called back)"하는 것이 콜백함수의 개념이다. \
위 예시에선 사용자가 "yes"라고 대답한 경우 `showOk`가 콜백이 되고, "no"라고 대답한 경우 `showCancel`이 콜백이 된다.

아래와 같이 함수 표현식을 사용하면 코드 길이가 짧아진다.

``` javascript
function ask(question, yes, no) {
  if (confirm(question)) yes()
  else no();
}
ask(
  "동의하십니까",
  function () {alert("동의하셨습니다.");},
  function (){alert("아니오 버튼을 누르셨습니다");}
);
```

`ask` 안에 들어간 이름 없이 선언한 함수는 익명 함수라고 부른다. 익명 함수는 변수에 할당된 것이 아니기에 `ask`함수 바깥에서는 접근할 수 없다.

---

### 함수 표현식 vs 함수 선언문

첫 번째는, 문법적으로 다르다.

*함수 선언문 : 함수는 주요 코드 흐름 중간에 독자적인 구문 형태로 존재한다.

``` javascript
// 함수 선언문
function sum(a,b){
  return a + b;
}
```

* 함수 표현식 : 함수는 표현식이나 구문 구성(syntax construct) 내부에 생성된다. 아래 예시에선 함수가 할당 연산자 `=`를 이용해 만든 "할당 표현식" 우측에 생성되었다.

``` javascript
// 함수 표현식
let sum = function(a,b){
  return a + b;
}
```

두 번째는 자바스크립트 엔진이 언제 함수를 생성하냐에 있어 차이를 보인다.

함수 표현식은 실제 실행 흐름이 해당 함수에 도달했을 때 함수를 생성한다. 따라서 실행 흐름이 함수에 도달했을 때부터 해당 함수를 사용할 수 있다.

하지만 함수 선언문은 함수 선언문이 정의되기 전에도 호출할 수 있다.

세 번째 차이점은, 스코프이다.

엄격 모드에서 함수 선언문이 코드 볼록 내에 위치하면 해당 함수는 블록 내 어디서든 접근할 수 있지만 블록 밖에서는 접근하지 못한다.

>함수 선언식과 함수 표현식 중 어떤 것을 써야하는가?
>> 함수 선언문의 이용을 이용해 함수를 생성하는 것을 먼저 고려해야한다. 그러나 조건에 따라 함수를 선언해야한다면 함수 표현식을 사용한다.
---

참조 : <br>
* https://ko.javascript.info/function-expressions
