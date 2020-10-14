---
title: HTTP (7) HTTP Range 요청
tags: HTTP
article_header:
  type: cover
  image: 
      src: 
---

### 개요

HTTP 범위(Range) 요청이란 HTTP를 통해 일정한 부분을 서버에서 클라이언트로 보내는 것을 Accept하고 보내는 방법이다.

미디어 파일 등을 다운로드 하는 중에 일시정지와 다시 시작 기능이 가능하다는 점을 이용하여 
클라이언트에서 미디어 파일을 '재생', '일시정지', '다시 시작'이 가능하게 만들 수 있다.

예를 들어 4.5MB 짜리 파일을 다운로드 한다고 하면, 서버는 전체 범위와 일정 범위에 해당하는 파일을 bytes로 내려보내준다.

---

### 서버가 부분 요청을 지원하는지 확인

서버가 범위(Range) 요청을 지원하면, HTTP 응답(Response)에 `Accept-Ranges` (그리고 그 값이 "`none`"이 아님) 가 존재한다.

```
curl -I http://i.imgur.com/z4d4kWk.jpg

HTTP/1.1 200 OK
...
Accept-Ranges: bytes
Content-Length: 146515
```

위 예시에서, `Accept-Ranges: bytes`는 범위 요청 시 바이트가 최소 단위로 사용될 수 있음을 명시한다. \
`Content-Length` 헤더를 통해 이미지 파일의 크기를 알 수 있다.

만약 응답에 `Accept-Ranges` 헤더가 포함되어 있지 않다면, 범위 요청이 지원되지 않는 것이다. \
일부 HTTP 응답에는 "`none`"을 값으로 보내 범위 요청을 지원하지 않음을 명확하게 알려준다.

```
curl -I https://www.youtube.com/watch?v=EwTZ2xpQwpA

HTTP/1.1 200 OK
...
Accept-Ranges: none
```

---

### 서버에 특정 범위를 요청

서버가 범위 요청을 지원한다면, `Range` 헤더를 사용하여 요청할 수 있다. \
`Range` 헤더를 사용하면 서버에서 요청한 자료의 일부만 받아올 수 있다.

#### `단일 부분 범위`

사용자는 리소스의 단일 범위에 대해 요청을 보낼 수 있다. cURL을 사용하여 요청을 테스트할 수 있다.
"`-H`"는 HTTP 요청의 헤더에 추가하는 옵션이며, 아래 예시는 `Range` 헤더를 통해 첫 1024 바이트를 요청하는 모습이다.

```
curl http://i.imgur.com/z4d4kWk.jpg -i -H "Range: bytes=0-1023"
```

요청은 HTTP 요청에서 다음과 같이 보여진다.

```
GET /z4d4kWk.jpg HTTP/1.1
Host: i.imgur.com
Range: bytes=0-1023
```

서버는 `206` `Partial Content`로 응답한다.

```
HTTP/1.1 206 Partial Content
Content-Range: bytes 0-1023/146515
Content-Length: 1024
...
(binary content)
```

위 예시를 보면 `Content-Length` 헤더는 이제 요청한 범위의 크기를 알려주는 것을 알 수 있다.
`Content-Range` 헤더는 리소스의 전체 크기와 요청한 부분이 어디에 속하는지 알려준다.

#### `다중 부분 범위`

`Range` 헤더는 문서의 여러 부분을 다중 범위 요청을 통해 한번에 가져올 수 있다. 범위는 콤마로 나눈다.

```
curl http://www.example.com -i -H "Range: bytes=0-50, 100-150"
```

서버는 `206` `Partial Content`로 응답하며 `Content-Type : multipart/byteranges;` `boundary=3d6b6a416f9b5`는 다중 요청 부분의 바이트 범위를 알려준다. \

각 부분은 고유의 `Content-Type`과 `Content-Range`를 가지며, 경계를 나누는 문자열인 경계 파라미터를 필요로 한다.

```
HTTP/1.1 206 Partial Content
Content-Type: multipart/byteranges; boundary=3d6b6a416f9b5
Content-Length: 282

--3d6b6a416f9b5
Content-Type: text/html
Content-Range: bytes 0-50/1270

<!doctype html>
<html>
<head>
    <title>Example Do
--3d6b6a416f9b5
Content-Type: text/html
Content-Range: bytes 100-150/1270

eta http-equiv="Content-type" content="text/html; c
--3d6b6a416f9b5--
```

#### `조건 분할 요청`

리소스에 대해 추가 요청을 보내기에 앞서, 마지막으로 분할 데이터를 받은 이후에 요청한 리소스가 수정되지 않았는지 확인해야 한다.

`If-Range` 요청 헤더는 범위 요청에 조건을 만든다. 만약 조건이 만족하면, 분할 요청에 대해 `206` `Partial Content` 응답이 돌아온다.

하지만 조건이 충족되지 않으면, 전체 리소스가 `200` `OK` 응답으로 전달된다. `If-Range` 헤더는 `Last-Modified`나 `ETag` 확인자와 함께 사용될 수 있으나, 한꺼번에 2개를 사용하는 것은 불가능하다.

```
If-Range: Wed, 21 Oct 2015 07:28:00 GMT 
```

---

### 분할 요청 응답

범위 요청에 대한 응답은 아래의 3가지 방식으로 전송된다.

* 요청이 성공적으로 이뤄진 경우, `206` `Partial Content` 응답이 클라이언트로 전송된다.
* 범위 요청 값이 리소스 크기를 벗어났을 때는 `416` `Requested Range Not Satisfiable` 상태로 응답이 전송된다.
* 범위 요청을 지원하지 않는 경우, 서버는 `200` `OK` 상태와 함께 전체 리소스를 응답으로 전송한다.

---

### `Chunked Transfer-Encoding`과 비교

`Transfer-Encoding` 헤더는 `chunked encoding` 또한 지원하며, 이는 대용량 데이터를 클라이언트에 보내거나 요청이 모두 처리되기 전까지 리소스의 전체 크기를 알 수 없을 때 유용하다.

서버는 데이터를 클라이언트에 버퍼링 없이 즉시 응답을 보내거나, 정확한 길이를 측정하여 지연 시간을 최대한 축소시킨다.

---

참조 : <br> 
* https://developer.mozilla.org/ko/docs/Web/HTTP/Range_requests
* https://derveljunit.tistory.com/311
