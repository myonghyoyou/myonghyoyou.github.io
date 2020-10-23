---
title: HTTP (8) HTTP Redirection
tags: HTTP
article_header:
  type: cover
  image: 
      src: 
---

### 개요

리다이렉트란 말 그대로 re(다시) + 지시하다(direct) 다시 지시하는 것이다.

예를 들어 브라우저가 `www.test.com/page1` URL을 웹 서버에 요청했다. 
서버는 HTTP 응답 메시지를 통해 `www.test.com/page2`에 대한 Redirect Message를 보내며 브라우저에게 다른 URL(길, 방향) 을 지시한다.

---

### 원리

HTTP Redirection은 서버가 응답으로 리다이렉트 관련 요소를 보내며 이루어진다. `Redirect`가 포함된 응답은 `3`으로 시작하는 상태코드를 가지며,
리다이렉트할 URL을 담은 `Location` 헤더를 포함한다.

리다이렉트 응답을 받은 브라우저는 `Location` 헤더에 명시된 URL을 즉시 불러온다. 리다이렉트가 이뤄지는 데는 사용자가 알아차리기 힘들 정도로 자원 소모가 적다.

리다이렉트에는 아래와 같이 3가지 종류가 존재한다.
1. 영구적 리다이렉션
2. 일시적 리다이렉션
3. 특수 리다이렉션

#### `영구적 리다이렉션`

영구적으로 지속되는 리다이렉션을 의미한다. 기존의 URL이 사라지고 리다이렉트된 새로운 URL로 교체된다. 

검색 엔진, RSS 리더 및 다른 크롤러들은 기존의 URL을 리소스로 업데이트한다.

|코드|텍스트|메서드|주로 사용되는 곳|
|---|---|---|---|
|301|영구적으로 이동함|`GET` 메서드는 변경되지 않는다. 다른 메서드들은 `GET`으로 변할 수도 있다.|웹 사이트의 재구성|
|308|영구적 리다이렉트|메소드와 body는 변하지 않음.|GET이 아닌 링크나 동작을 지난 웹 사이트의 재편성|


---

### Redirection을 할 수 있는 그 외 방법들

---

### 유스 케이스



---

### 서버에서 Redirection 설정



---

### Redirection 루프


---

참조 : <br> 
* https://developer.mozilla.org/ko/docs/Web/HTTP/Range_requests
* https://derveljunit.tistory.com/311
