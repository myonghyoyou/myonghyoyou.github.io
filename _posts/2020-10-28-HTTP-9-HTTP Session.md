---
title: HTTP (9) HTTP Session
tags: HTTP
article_header:
  type: cover
  image: 
      src: 

---

### 개요

HTTP 와 같은 서버-클라이언트 프로토콜에서 세션은 아래와 같은 과정을 거쳐 만들어진다.

1. 클라이언트는 TCP/IP 연결을 구성한다. (전송 계층이 TCP가 아니라면 상황에 맞는 연결을 구성한다.) -> 웹 브라우저 프로그램 내의 쓰레드가 소켓을 생성하여 서버 소켓에 연결 수행
2. 그 후 요청을 전송하고 응답을 기다린다.
3. 서버가 요청을 처리한 뒤 상태 코드와 각종 정보를 담은 응답을 클라이언트에 전송한다.

HTTP/1.0 프로토콜에서는 2번과 3번 과정이 수행되고 난 이후에 클라이언트 소켓이 닫히기 때문에 다음 요청을 처리하기 위해서는 새로은 쓰레드를 이용해서 소켓을 생성하고 다시 연결 작업을 수행해야 했다. 

HTTP/1.1 프로토콜에서는 위와 같은 비효율적 상황을 타개하기 위해 소켓을 재사용할 수 있게 하였다. 세션이 성립하면 서버가 클라이언트 쪽으로 세션 ID를 HTTP Cookie를 통해서 전송하고 클라이언트 PC에 저장되기 때문에 가능한 일이다. Cookie에 들어있는 Session ID를 클라이언트가 요청과 같이 보내기 때문에 서버는 같은 세션에서 온 요청이라는 것을 알게 된다.

---

### 연결 구성

클라이언트-서버 프로토콜에서 연결을 구성하는 건 클라이언트측이다. HTTP에서 연결을 구성한다는 것은 주로 TCP/IP를 이용하여 연결을 구성한다는 것을 의미한다.

TCP/IP를 통해 연결을 구성하게 되면, 대개 기본 포트는 80으로 설정되어 있다. 그러나 8000, 8080 등의 다른 포트들도 사용할 수 있다. 요청할 때 필요한 페이지 URL 에는 도메인 이름과 포트 번호가 둘 다 들어가는데 포트 번호가 80일 경우에는 포트 번호를 생략할 수 있다.

---

### 클라이언트 요청 전송

세션과 소켓이 한 번 열리고 나면, 사용자-에이전트(대개 브라우저, crawler 등 무엇이든 될 수 있긴 하다)는 서버에 요청을 보낼 수 있다. 클라이언트 요청은 세 가지 블록으로 나누어진, CRLF로 구분된 텍스트 지시자들로 구성된다.

> CRLF?  새로운 줄로 바꾸는 방식을 의미. CRLF는 윈도우의 default 줄바꿈 방식이다.

1. 첫 번째 줄에는 요청에 쓰인 메서드와 함께 문서의 경로, 사용 중인 HTTP 버젼이 명시된다.
2. 바로 다음 줄부터는 언어가 무엇인지, MIME타입은 무엇인지, 이미 캐시되어 있는 경우 응답을 전송하지 않게끔 한다든지 등 데이터의 종류와 서버의 동작을 수정하는 헤더 등을 포함시켜 서버에 전송한다. 모든 내용이 서술되고 나면 비어있는 한 줄을 만든다.
3. 마지막 블록은 부가적인 데이터 블록으로, 주로 POST 메서드에 의해 전송되는 부가적인 데이터가 명시된다.

#### 요청 예시

아래 예시는 `http://developer.mozilla.org/`의 최상위 페이지를 가져오도록 요청한다. 추가적으로 서버에 프랑스어로 된 페이지를 원한다고 명시했다.

``` http
GET / HTTP/1.1
Host: developer.mozilla.org
Accept-Language: fr

```

마지막의 빈 줄은 헤더 블록과 데이터 블록을 구분하는 역할을 한다. 헤더 중에 `Content-Length:` 헤더가 없으므로 데이터 블록이 비어있음을 알 수 있고 따라서 서버는 헤더의 마지막을 나타내는 빈 줄을 받는 즉시 요청을 처리하게 된다.

아래 예시는 form의 결과를 전송하는 모습이다.

``` http
POST /contact_form.php HTTP/1.1
Host: developer.mozilla.org
Content-Length: 64
Content-Type: application/x-www-form-urlencoded

name=Joe%20User&request=Send%20me%20one%20of%20your%20catalogue
```

#### 요청 메소드

클라이언트가 웹 서버에게 사용자 요청의 목적/종류를 알리는 수단이다.

우리가 HTTP 요청을 보낼 때 쓰는 메서드는 대개 2가지로 `GET`과 `POST` 방식이다.

* `GET` : URI 형식으로 서버측 데이터를 요청한다. URI 뒤쪽에 파라미터를 넘겨서 해당하는 데이터를 받는다.
* `POST` : 내용 전송. 클라이언트에서 서버로 전달하고자 하는 정보를 서버로 보낸다.

---

### 서버 응답의 구조

클라이언트가 서버에 요청을 보내면, 서버는 요청을 처리한 뒤 클라이언트에 응답을 전송한다. 클라이언트 요청과 마찬가지로, 서버 응답 또한 세 개의 블록으로 나누어진, CRLF로 구분된 응답 메시지를 만들어낸다.

1. 사용하는 HTTP 버젼, 상태 코드를 명시한다.
2. 두번째 블록에서는 여러 HTTP 헤더를 명시한다. 전송되는 데이터에 관한 정보 (데이터 타입, 데이터 크기, 사용된 압축 알고리즘, 캐시에 대한 힌트 등)를 클라이언트에게 제공한다. 클라리언트의 요청에 대한 HTTP 헤더 블록과 마찬가지로 비어있는 한 줄을 끝에 추가한다.
3. 데이터가 존재할 경우 마지막 블록에 데이터를 명시한다.

#### 응답 예시

아래 예시는 요청에 대한 응답이 성공적으로 전송된 모습을 보여준다.

``` http
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
Content-Length: 55743
Connection: keep-alive
Cache-Control: s-maxage=300, public, max-age=0
Content-Language: en-US
Date: Thu, 06 Dec 2018 17:37:18 GMT
ETag: "2e77ad1dc6ab0b53a2996dfd4653c1c3"
Server: meinheld/0.6.1
Strict-Transport-Security: max-age=63072000
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Vary: Accept-Encoding,Cookie
Age: 7


<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>A simple webpage</title>
</head>
<body>
  <h1>Simple HTML5 webpage</h1>
  <p>Hello, world!</p>
</body>
</html>
```

 요청한 자료가 옮겨졌을 경우엔 아래와 같은 모습을 보인다.

``` http
HTTP/1.1 301 Moved Permanently
Server: Apache/2.4.37 (Red Hat)
Content-Type: text/html; charset=utf-8
Date: Thu, 06 Dec 2018 17:33:08 GMT
Location: https://developer.mozilla.org/ (요청한 자료에 대한 새로운 주소. 클라이언트가 가져갈 것을 예상한다.)
Keep-Alive: timeout=15, max=98
Accept-Ranges: bytes
Via: Moz-Cache-zlb05
Connection: Keep-Alive
Content-Length: 325 (브라우저가 Location에 명시된 링크를 타고 가지 않으면 띄울 컨텐츠의 길이이다.)


<!DOCTYPE html... (사용자가 자료를 찾는 데 필요한 내용을 담은 페이지를 구성한다.)
```

요청한 자료가 존재하지 않을 때의 예시이다.

``` http
HTTP/1.1 404 Not Found
Content-Type: text/html; charset=utf-8
Content-Length: 38217
Connection: keep-alive
Cache-Control: no-cache, no-store, must-revalidate, max-age=0
Content-Language: en-US
Date: Thu, 06 Dec 2018 17:35:13 GMT
Expires: Thu, 06 Dec 2018 17:35:13 GMT
Server: meinheld/0.6.1
Strict-Transport-Security: max-age=63072000
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Vary: Accept-Encoding,Cookie
X-Cache: Error from cloudfront


<!DOCTYPE html... (사용자가 사라진 자료를 찾는 데 필요한 내용을 담은 페이지를 구성한다.)
```

#### 응답 상태 코드

HTTP 응답은 5개의 그룹으로 나누어진다. 아래 3개의 응답 코드는 여러 응답 코드들 중 가장 많이 볼 수 있는 응답 코드이다.

* `200 OK` : 성공적으로 처리했을 때 쓰인다. 가장 일반적으로 볼 수 있는 HTTP 상태. 
* `301 Moved Permanently` : 영구적으로 컨텐츠가 이동했을 때 사용된다.
* `404 Not Found` : 찾는 리소스가 없다는 뜻

---

참조 : <br> 

* https://developer.mozilla.org/en-US/docs/Web/HTTP/Redirections
* https://zorba91.tistory.com/136
* https://technote.kr/300
* https://tobewiseys.tistory.com/72
