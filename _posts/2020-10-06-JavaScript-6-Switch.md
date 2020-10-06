---
title: JavaScript (6) Switch
tags: JavaScript
article_header:
  type: cover
  image:
---

### 개요

복수의 `if` 조건문은 `switch`문으로 바꿀 수 있다. `switch`문을 활용하면 코드 자체가 비교 상황을 잘 설명한다. 

---

### 문법

`switch`문은 하나 이상의 `case` 문으로 구성된다. `default`도 있지만, 필수는 아니다.

예시 :

``` javascript
switch(x) {

  case 'value1' : // if(x==='value1')
    alert('this is value1');
    break;
    
  case 'value2 : // if(x==='value2')
    alert('this is value2');
    break;
    
  default: 
    alert('this is default');
    break;
}
```

* `case`문에서 변수 `x`의 값과 일치하는 값을 찾으면 해당 `case`문에 정의된 코드가 실행된다. 이 때, `break`문을 만나거나 `switch`문이 끝나면 코드의 실행이 멈춘다.

---

### 예시

아래 예시에선 case 19가 실행되는 것을 확인할 수 있다.

``` javascript
let age = 19;

switch(age){
  case 17:
    alert("고등학교 1학년생입니다");
    break;
  
  case 18:
    alert("고등학교 2학년생입니다");
    break;
  
  case 19:
    alert("고등학교 3학년생입니다");
    break;
  
  case 20:
    alert("다 컸습니다");
    break;
  
  default:
    alert("모르겠습니다");
    break;
}
```

위의 `switch`문은 age의 값과 각 `case`문의 값을 비교하며 내려온다.

세 번째 `case`문의 값인 19와 age가 일치하고, 따라서 `break`문을 만날 때까지 `case 19` 아래의 코드가 실행된다. 

**만일 `case`문 안에 `break`문이 없다면 조건에 부합하는지 여부를 따지지 않고 이어지는 `case`문을 실행한다.**

아래 예시를 살펴보자.

``` javascript
let age = 19;

switch(age){    
  case 19:
    alert("고등학교 3학년생입니다");
  
  case 20:
    alert("다 컸습니다");
  
  default:
    alert("모르겠습니다");
}
```

위의 예시를 실행하면 아래 3개의 `alert`문이 실행된다.

``` javascript
alert("고등학교 3학년생입니다");
alert("다 컸습니다");
alert("모르겠습니다");
```
---

### 여러 개의 "case"문 묶기

코드가 같은 `case`문은 한데 묶을 수 있다.

두 개의 `case`문에서 실행하려는 코드가 같은 경우에 대한 예시를 아래에서 보자.

```javascript
  let a = 3;
  
  switch(a){
    case 4 :
      alert("정답입니다!");
      break;
      
    case 3: // 2개의 case문을 묶음
    case 5:
      alert("아쉽게 틀리셨네요");
      break;
      
    default:
      alert("계산 결과가 이상하네요?");
      break;
  }
```

위의 예시에서 `case 3`과 `case 5`는 동일한 메시지를 보여준다.

---

### 자료형의 중요성

`switch`문은 일치 비교로 조건을 확인한다. 확인하려는 값과 `case`문의 값의 형과 값이 같아야 해당 `case`문이 실행된다.

``` javascript
let num = "3";

switch(num){
  case "1" : 
    alert("2가 부족합니다");
    break;
    
  case "2" : 
    alert("1이 부족합니다");
    break;
    
  case 3 :
    alert("정답입니다만 이 코드는 절대 실행될 수 없습니다");
    break;
  
  default : 
    alert("다시 시도해보세요");
    break;
}
```

위의 예시에서 변수 `num`에 들어간 "3"은 세 번째 `case`문에 들어간 값 3과는 형식이 다르므로 (string과 number) `case 3`은 절대 실행될 수 없다.

---

참조 : <br>
* https://ko.javascript.info/switch
