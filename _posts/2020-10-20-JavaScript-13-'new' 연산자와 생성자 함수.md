---
title: JavaScript (13) 'new' 연산자와 생성자 함수
tags: JavaScript
article_header:
  type: cover
  image:
---

### 개요

개발을 하다 보면 유사한 객체를 여러 개 만들어야 할 때가 생긴다. 복수의 사용자, 메뉴 내 다양한 아이템을 객체로 표현하려고 하는 경우가 발생한다.

"new" 연산자와 생성자 함수를 사용하면 유사한 객체 여러 개를 쉽게 만들 수 있다.

---

### 생성자 함수

생성자 함수는 아래 두 관례를 따른다.

* 함수 이름의 첫 글자는 대문자로 시작.
* 반드시 "new" 연산자를 붙인다.

예시:
``` javascript
function User(name) {
  this.name = name;
  this.isAdmin = false;
}

let user = new User("Jack");

alert(user.name); // Jack
alert(user.isAdmin); // false
```

`new User(...)`을 써서 함수를 실행하면 아래와 같은 알고리즘이 실행된다.

* 빈 객체를 만들어 this에 할당.
* 함수 본문을 실행. this에 새로운 프로퍼티를 추가해 this를 수정.
* this를 반환.

아래 예시는 `new User(...)`가 실행되면 어떻게 되는지 보여준다.

``` javascript
function User(name) {
  // this = {};  (빈 객체가 암시적으로 만들어짐)

  // 새로운 프로퍼티를 this에 추가
  this.name = name;
  this.isAdmin = false;

  // return this;  (this를 암시적으로 반환)
}
```

`let user = new User("Jack")`는 아래 예시와 동일하게 동작한다.

``` javascript
let user = {
  name: "Jack",
  isAdmin: false
};
```

생성자의 의의는 재사용할 수 있는 객체 생성 코드를 구현하는 것이다.

모든 함수는 생성자 함수가 될 수 있다. `new`를 붙여 실행하면 어떤 함수라도 위에 언급된 알고리즘이 실행된다. 
이름 "첫 글자가 대문자"인 함수는 `new`를 붙여 실행해야 한다.

---

### 생성자와 return문

생성자 함수엔 보통 `return`문이 없지만 `return`문을 넣게 되면 아래와 같은 일이 발생한다.

* 객체를 return 할 시, this 대신 객체를 반환.
* 원시형을 return 할 시, return문 무시.

return 뒤에 객체가 오면 생성자 함수는 해당 객체를 반환하며, 이 외의 경우는 this가 반환된다.

아래 예시에선 `this`를 무시하고 객체를 `return`한다.

``` javascript
function BigUser() {

  this.name = "John";

  return { name: "Godzilla" };  // <-- this가 아닌 새로운 객체를 반환
}

alert( new BigUser().name );  // Godzilla
```

이와 달리, 아래 예시에선 `this`를 반환한다.

``` javascript
function SmallUser() {

  this.name = "John";

  return; // <-- this를 반환
}

alert( new SmallUser().name );  // John
```

`return`문이 있는 생성자 함수는 거의 없다.

---

### 생성자 내 메서드

생성자 함수를 사용하면 매개변수를 이용해 객체 내부를 자유롭게 구성할 수 있다.

아래 예시에서 new User(name)는 프로퍼티 name과 메서드 sayHi를 가진 객체를 만들어줍니다.

``` javascript
function User(name) {
  this.name = name;

  this.sayHi = function() {
    alert( "My name is: " + this.name );
  };
}

let john = new User("John");

john.sayHi(); // My name is: John

/*
john = {
   name: "John",
   sayHi: function() { ... }
}
*/
```

---

참조 : <br>
* https://ko.javascript.info/constructor-new
