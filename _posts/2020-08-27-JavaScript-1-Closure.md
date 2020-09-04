---
title: JavaScript 1.클로저(Clousure)
tags: JavaScript
article_header:
  type: cover
  image:
---
자신을 포함하고 있는 외부함수보다 내부함수가 더 오래 유지되는 경우, 외부 함수 밖에서 내부함수가 호출되더라도 외부함수의 지역 변수에 접근할 수 있는데 이러한 함수를 클로저(Closure)라고 부른다.
클로저는 반환된 내부함수가 자신이 선언됐을 때의 환경인 스코프를 기억하여 자신이 선언됐을 때의 환경 밖에서 호출되어도 그 환경(스코프)에 접근할 수 있는 함수이다.

자바스크립트 고유의 개념이 아니라 함수를 일급 객체로 취급하는 함수형 프로그래밍 언어에서 사용되는 중요한 특성이다.

클로저에 의해 참조되는 외부함수의 변수를 자유변수(Free Variable) 라고 부른다. 클로저라는 이름은 자유변수에 엮여있는 함수라는 뜻이다.

외부함수가 이미 반환되었어도 외부함수 내의 변수는 이를 필요로 하는 내부함수가 하나 이상 존재하는 경우 계속 유지된다. 이 때 내부함수가 외부함수에 있는 변수의 복사본이 아니라 실제 변수에 접근한다는 것에 주의하여야 한다.

1. 클로저의 활용

  클로저는 자신이 생성될 때의 환경(스코프) 을 기억해야 하므로 메모리 차원에서 손해를 볼 수 있다.

  (1) 상태 유지
  클로저가 가장 유용하게 사용되는 상황은 현재 상태를 기억하고 변경된 최신 상태를 유지하는 것.
	
``` javascript
  let increaseBtn = document.querySelector("#increase");
  let count = document.querySelector("#count");

  let increase = (function () {
  let counter = 0;

  return function(){
    return ++counter;
  };
  }());

  increaseBtn.addEventListener("click", function(){
    count.innerHTML = increase();
  })
```
  
  (2) 전역 변수의 사용 억제
  
	






