# HTTP는무엇일까?

# HTTP란?

 하이퍼텍스트 전송 프로토콜(HTTP)은 HTML과 같은 하이퍼미디어 문서를 전송하기 위한 애플리케이션 레이어 프로토콜입니다. 최근에는 HTML 뿐만 아니라 Plain Text, JSON, XML 등 다양한 형태의 정보도 전송합니다.

 원래는 웹 브라우저와 웹 서버간의 커뮤니케이션을 위해 디자인되었지만, 다른 목적으로도 사용될 수 있습니다. 대표적으로 모바일 애플리케이션 및 IoT 등과의 커뮤니케이션이 있죠.

 HTTP는 클라이언트가 요청을 생성하기 위한 연결을 연 다음 응답을 받을 때까지 대기하는 전통적인 클라이언트-서버 모델을 따릅니다. HTTP는 무상태 프로토콜이며, 이는 서버가 두 요청간에 어떠한 데이터(상태)도 유지하지 않음을 의미합니다. (상태를 유지하기 위해서 사용하는 것이 Cookie와 Session입니다.)

 일반적으로 안정적인 TCP/IP 레이어를 기반으로 사용하는 응용 프로토콜입니다.

# HTTP의 동작 원리

![https://mdn.mozillademos.org/files/13677/Fetching_a_page.png](https://mdn.mozillademos.org/files/13677/Fetching_a_page.png)

 HTTP는 클라이언트-서버 구조를 가진다고 앞서 언급했습니다. 클라이언트(웹 브라우저, 모바일 등)가 특정 서비스를 URI를 통해 서버에 요청(`Request`)하면 서버에서는 해당 요청에 대한 결과를 응답(`Response`)하는 방식으로 동작합니다.

# HTTP 요청 메서드

 클라이언트가 서버에게 특정 리소스에 대해 수행하길 원하는 행동을 요청 메서드라고 합니다. 간단하게 말하면 해당 리소스를 가지고 어떤 작업을 하고 싶은지를 서버에게 알려주는 것이죠.

## GET

 `GET` 메서드는 특정 리소스의 표시를 요청합니다. `GET`을 사용하는 요청은 오직 데이터를 받기만 합니다.

## POST

 `POST` 메서드는 특정 리소스에 엔티티를 제출할 때 쓰입니다. 즉, 리소스를 신규 생성하거나 컨트롤러를 실행하는 용도이죠. 그러나 때때로 서버 상태의 변화 및 부작용을 일으킬 수도 있습니다.

## PUT

 `PUT` 메서드는 변경 가능한 리소스를 업데이트 할 때 사용됩니다. 따라서 `PUT`을 사용하는 요청은 리소스를 구분할 수 있는 식별 정보를 포함해야 합니다.

## DELETE

 `DELETE` 메서드는 특정 리소스를 삭제할 때 사용합니다.

# HTTP 요청

## 요청

```bash
GET / HTTP/1.1
Host: developer.mozilla.org
Accept-Language: fr
```

![https://mdn.mozillademos.org/files/13687/HTTP_Request.png](https://mdn.mozillademos.org/files/13687/HTTP_Request.png)

- 1번 줄
 HTTP 메서드와 경로(Path), 그리고 프로토콜의 종류와 버전을 적습니다.
- 2번 줄
 리소스를 요청하는 경로인 서버의 도메인을 적습니다. 위 예시에서는 HTTP의 기본 포트(80번 포트)를 사용하기 때문에 포트 번호가 생략되었습니다.
- 2번 줄 이후
 HTTP Request Headers 부분입니다.

## 응답

```bash
HTTP/1.1 200 OK
Date: Sat, 09 Oct 2010 14:28:02 GMT
Server: Apache
Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
ETag: "51142bc1-7449-479b075b2891b"
Accept-Ranges: bytes
Content-Length: 29769
Content-Type: text/html

<!DOCTYPE html... (here comes the 29769 bytes of the requested web page)
```

![https://mdn.mozillademos.org/files/13691/HTTP_Response.png](https://mdn.mozillademos.org/files/13691/HTTP_Response.png)

- 1번 줄
 프로토콜의 종류와 버전, HTTP 상태 코드, HTTP 상태 메시지를 적습니다.
- 2번 줄 이후
 HTTP Response Headers 부분입니다.

# 참고자료

[[Network] HTTP란 무엇인가?](https://surprisecomputer.tistory.com/54)

[HTTP | MDN](https://developer.mozilla.org/ko/docs/Web/HTTP)

[HTTP 요청 메서드 - HTTP | MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)

[HTTP 개요 - HTTP | MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview)