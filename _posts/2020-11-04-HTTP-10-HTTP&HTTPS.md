---
title: HTTP (10) HTTP, HTTPS 차이
tags: HTTP
article_header:
  type: cover
  image: 
      src: 


---

### HTTP (Hyper Text Transfering Protocol)

>  HTTP : 서버와 브라우저 사이에 문서(Hyper Text)를 전송(Transfer)하기 위한 통신 규약(Protocol)

HTTP 서버는 기본 포트인 80번에서 서비스 대기한다. 클라이언트가 TCP 80 포트를 사용해 연결하면 서버는 요청에 응답하면서 요청된 자료를 전송한다.

HTTP는 정보를 텍스트로 주고 받는다. 단순 텍스트를 주고 받기 때문에 네트워크에서 전송 신호를 인터셉트하는 경우 데이터 유출이 발생할 수 있다.

HTTP의 보안 취약점을 해결하기 위한 프로토콜이 **HTTPS**이다.

---

### HTTPS

> HTTPS : HTTP + S(Secure Socekt)

기본 골격이나 사용 목적은 HTTP와 동일하나, 데이터를 주고 받는 과정에 보안 요소가 추가된다는 점에서 큰 차이가 있다.

HTTPS를 사용하게 되면 서버와 클라이언트 사이의 모든 통신 내용이 암호화된다.

HTTPS는 서버에 접속하는 사용자마자 각자 다른 암호를 제공한다.

---

### SSL (Secure Socket Layer)

> SSL : 클라이언트와 서버 간의 통신을 제3자가 보증해주는 전자화된 문서

클라이언트가 서버에 접속한 직후에 서버는 클라이언트에게 SSL 인증서 정보를 전달한다. 클라이언트는 이 인증서 정보가 신뢰할 수 있는 것인지를 검증한 후에 그 다음 절차를 수행한다. 

SSL의 핵심은 암호화로 보안과 성능상의 이유로 두가지 암호화 기법을 혼용해서 사용한다.

#### 대칭키

동일한 키로 암호화와 복호화를 같이 할 수 있는 방식. 암호화를 할 때 1234라는 값을 이용했다면 복호화를 할 때도 1234라는 값을 입력해야 한다.

#### 공개키

대칭키 방식은 암호를 주고 받는 과정에서 대칭키를 전달하는 것이 어렵다. 키를 주고 받는 과정에서 키가 유출되면 공격자가 암호의 내용을 복호화할 수 있기 때문이다. 

이런 배경에서 공개키 방식이 나오게 된다.

공개키는 두 개의 키를 가지게 되는데 2개의 키는 서로 암호화 / 복호화할 수 있다는 룰이 존재한다. A키로 암호화하면 반드시 B키로 복호화 가능하고 B키로 암호화하면 반드시 A키로 복호화 가능하다. 

하나의 키는 공개키 저장소에 등록하여 사용하고 남은 하나는 개인키로 이용하여 통신한다.

클라이언트는 공개키를 얻어 데이터를 암호화해서 전송하고, 서버는 개인키를 이용하여 정보를 보호한다. 반대로 서버가 정보를 제공할 때는 개인키로 암호화된 정보를 전송하기 때문에 공개키가 있는 사용자는 누구나 정보를 알 수 있다.

<img class="image image--xl" src="https://user-images.githubusercontent.com/70210573/98087181-1386f600-1ec3-11eb-84c3-c861e0842510.png"/>

<img class="image image--xl" src="https://user-images.githubusercontent.com/70210573/98087101-ff42f900-1ec2-11eb-87e9-5f81f4809dd5.png"/>

---

### HTTPS 대신 HTTP 사용 시 단점

간단한 웹 서핑의 경우 HTTP를 사용해도 무방하나, 클라이언트와 서버 간 자료를 주고 받는 경우에는 개인 정보 유출 가능성이 있다. 

거기에 더해 네이버, 다음, 구글 등의 검색 엔진에서 검색 엔진 최적화 관련 내용 (SEO : Search Engine Organization)을 웹 사이트에 적용시키는데 키워드 검색 시 상위 노출되는 기준 중 하나가 보안 요소이기에, HTTP 사이트보다 HTTPS 사이트가 우선 검색된다.

---

참조 : <br>

* https://post.naver.com/viewer/postView.nhn?volumeNo=16561296&memberNo=1834
* http://blog.wishket.com/http-vs-https-%EC%B0%A8%EC%9D%B4-%EC%95%8C%EB%A9%B4-%EC%82%AC%EC%9D%B4%ED%8A%B8%EC%9D%98-%EB%A0%88%EB%B2%A8%EC%9D%B4-%EB%B3%B4%EC%9D%B8%EB%8B%A4/
* https://jeong-pro.tistory.com/89
