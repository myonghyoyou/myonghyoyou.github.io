---
title: JavaScript (9) 코딩 스타일
tags: JavaScript
article_header:
  type: cover
  image:
---

### 개요

개발자는 가능한 한 간결하고 읽기 쉽게 코드를 작성해야 한다.

복잡한 문제를 간결하고 가독성이 좋은 코드로 작성해 해결하는 것은 좋은 개발자가 되기 위해 필수적인 능력이다.

---

### 중괄호

중괄호는 ‘이집션(Egyptian)’ 스타일을 따라 새로운 줄이 아닌 상응하는 키워드와 같은 줄에 작성하며 여는 중괄호 앞엔 공백이 하나 있어야 한다.

예시:
``` javascript
if (condition) {
  // 코드 1
  // 코드 2
  // ...코드 n...
}
```

코드가 짧다면 중괄호 없이 한 줄에 쓰는 것도 좋은 방법이다.
예시:
``` javascript
if (n < 0) alert(`Power ${n} is not supported`);
```

가장 추천하는 방법은 다음과 같다.
예시:
``` javascript
if (n < 0) {
  alert(`Power ${n} is not supported`);
}
```

---

### 가로 길이

코드의 가로 길이가 길어진다면 여러 줄로 나눠서 작성한다.

``` javascript
// 백틱(`)을 사용하면 문자열을 여러 줄로 쉽게 나눌 수 있습니다.
let str = `
  ECMA International's TC39 is a group of JavaScript developers,
  implementers, academics, and more, collaborating with the community
  to maintain and evolve the definition of JavaScript.
`;
```

`if`문의 조건문이 길어진다면 아래와 같이 작성할 수 있다.
``` javascript
if (
  id === 123 &&
  moonPhase === 'Waning Gibbous' &&
  zodiacSign === 'Libra'
) {
  letTheSorceryBegin();
}
```

---

참조 : <br>
* https://ko.javascript.info/switch
