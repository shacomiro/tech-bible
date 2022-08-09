# 교차 출처 리소스 공유(CORS)

1. [CORS에 대한 기본적인 내용](#cors에-대한-기본적인-내용)
   1. [출처(Origin)](#출처origin)
   2. [SOP(Same-Origin Policy)](#sopsame-origin-policy)
   3. [같은 출처와 다른 출처의 구분](#같은-출처와-다른-출처의-구분)
2. [CORS의 작동 원리](#cors의-작동-원리)
   1. [Preflight Request](#preflight-request)
   2. [Simple Request](#preflight-request)
   3. [Credentialed Request](#credentialed-request)
3. [CORS를 해결할 수 있는 방법](#cors를-해결할-수-있는-방법)
   1. [Access-Control-Allow-Origin 설정하기](#access-control-allow-origin-설정하기)
   2. [Webpack Dev Server로 리버스 프록싱하기](#webpack-dev-server로-리버스-프록싱하기)
   3. [요청을 img 태그에 담기?](#요청을-img-태그에-담기)
4. [참고 자료](#참고-자료)

> 문서 내의 예제 코드는 Evan Moon님의 [티스토리 블로그](https://evanmoon.tistory.com), [깃허브 블로그](https://evan-moon.github.io) URL을 활용했습니다.

## CORS에 대한 기본적인 내용

웹 개발 시, 특히나 API 서버와의 통신을 거치는 경우 CORS 관련 문제를 한 번쯤 겪기 마련이다. 이러한 COR 관련 이슈는 모두 CORS 정책을 위반했기 떄문에 발생하는 것이다. 개발자로서는 이 정책으로 인해 신경 써야 할 점이 늘어나서 번거로울 수도 있으나, 사실 CORS라는 방어막이 있는 덕분에 온갖 장소에서 가져오는 리소스가 안전하다는 최소한의 보장을 받을 수 있는 것이다.

CORS는 **Cross-Origin Resource Sharing**의 약자로, 한국어로 직역하면 교차 출처 리소스 공유라고 해석할 수 있다. 여기서 "교차 출처"란 "다른 출처"를 의미한다고 볼 수 있다.

### 출처(Origin)

서버의 위치를 의미하는 `https://www.google.com`과 같은 URL들은 마치 하나의 문자열처럼 보이지만, 사실은 여러 개의 구성 요소로 이루어져 있다.

![CORS-01](./images/CORS-01.png)

이때 출처는 `Protocol`과 `Host`, 그리고 위 그림에는 없지만 `:80`, `:443`과 같은 포트 번호까지 모두 합친 것을 의미한다. 즉, 서버의 위치를 찾아가는 데 필요한 가장 기본적인 요소들을 합쳐놓은 것이다.

또한 출처 내의 포트 번호는 생략할 수 있는데, 이는 각 웹에서 사용하는 HTTP, HTTPS 프로토콜의 기본 포트 번호가 정해져 있기 때문이다. HTTP가 정의된 [RFC 2616](https://datatracker.ietf.org/doc/html/rfc2616#section-3.2.2) 문서에서는 다음과 같이 기본 포트 번호를 함께 정의하고 있다.

> 3.2.2 http URL
>
> (전략)
>
> http_URL = "http:" "//" host [ ":" port ] [ abs_path [ "?" query ]]
>
> **If the port is empty or not given, port 80 is assumed.** The semantics
> are that the identified resource is located at the server listening
> for TCP connections on that port of that host, and the Request-URI
> for the resource is abs_path (section 5.1.2).
>
> (후략)

그런데 만약 `https://google.com:443`과 같이 출처에 포트 번호가 명시적으로 포함되어 있다면 이 포트 번호까지 모두 일치해야 같은 출처라고 인정된다. 하지만 이 경우에 대한 명확한 정의가 표준으로 정해진 것은 아니기 때문에, 더 정확히 이야기하면 어떤 경우에는 같은 출처, 또 다른 경우에는 다른 출처로 판단될 수도 있다.

브라우저의 개발자 도구의 콘솔에서 `Location` 객체가 가지고 있는 `origin` 프로퍼티에 접근함으로써 손쉽게 애플리케이션이 실행되고 있는 출처를 알아낼 수도 있다.

```javascript
console.log(location.origin);
```

```
"https://shacomiro.github.com"
```

### SOP(Same-Origin Policy)

웹 생태계에는 다른 출처로의 리소스 요청을 제한하는 것과 관련된 두 가지 정책이 존재한다. 한 가지는 이 문서의 주제인 CORS이고, 다른 한 가지는 <b>SOP(Same-Origin Policy)</b>이다.

SOP는 지난 2011년, [RFC 6454](https://datatracker.ietf.org/doc/html/rfc6454#page-5)에서 처음 등장한 보안 정책으로 말 그대로 <b>"같은 출처에서만 리소스를 공유할 수 있다."</b>라는 규칙을 가진 정책이다.

그러나 웹이라는 오픈 스페이스 환경에서 다른 출처에 있는 리소스를 가져와서 사용하는 일은 굉장히 흔한 일이기에 무작정 막을 수도 없는 노릇이다. 이에 몇 가지 예외 조항을 두고 이 조항에 해당하는 리소스 요청은 출처가 다르더라도 허용하기로 했는데, 그중 하나가 <b>"CORS 정책을 지킨 리소스 요청"</b>이다.

> Access to network resources varies depending on whether the resources are in the same origin as the > >content attempting to access them.
>
> Generally, reading information from another origin is forbidden. However, an origin is permitted to use >some kinds of resources retrieved from other origins. For example, an origin is permitted to execute >script, render images, and apply style sheets from any origin. Likewise, an origin can display content >from another origin, such as an HTML document in an HTML frame. **Network resources can also opt into >letting other origins read their information, for example, using Cross-Origin Resource Sharing.**
>
> [RFC 6454 - 3.4.2 Network Access](https://datatracker.ietf.org/doc/html/rfc6454#section-3.4.2)

다른 출처로 리소스를 요청한다면 SOP 정책을 위반한 것이 되고, 거기에다 SOP의 예외 조항인 CORS 정책까지 지키지 않는다면 아예 다른 출처의 리소스를 사용할 수 없게 된다.

즉, 이렇게 다른 출처의 리소스를 사용하는 것을 제한하는 행위는 하나의 정책만으로 결정된 사항이 아니라는 의미가 된다. SOP에서 정의된 예외 조항과 CORS를 사용할 수 있는 경우들이 맞물리지 않을 때는 아예 리소스 요청을 할 수 없는 경우도 존재할 수 있다.

개발자는 정해진 서버와의 통신을 수행하도록 개발할 뿐인데, 이처럼 번거롭고 귀찮은 정책을 만든 이유는 **출처가 서로 다른 두 개의 애플리케이션이 마음대로 소통할 수 있는 환경은 상당히 위험한 환경**이기 때문이다.

근본적으로 클라이언트 애플리케이션, 특히나 웹에서 작동하는 클라이언트 애플리케이션은 사용자의 공격에 너무나도 취약하다. 당장 브라우저의 개발자 도구만 열어도 DOM이 어떻게 작성되었는지, 어떤 서버와 통신하는지, 리소스의 출처는 어디인지와 같은 각종 정보를 아무런 제재 없이 열람할 수 있다.

최근에는 자바스크립트 소스 코드를 난독화하여 읽기 어렵다고는 해도, 난독화는 암호화가 아니므로 사람이 충분히 이해할 수 있다. 무엇보다도 소스 코드를 직접 볼 수 있다는 것 자체가 보안 측면에서 상당히 취약한 부분이다. 일부 사이트들은 난독화조차 되어있지 않아 개발자 도구만 열어도 `<script>` 태그 안에 날 것 그대로의 소스 코드가 노출된다.

이런 상황 속에서 다른 출처의 애플리케이션이 서로 통신하는 것에 대해 아무런 제약도 존재하지 않는다면, 악의를 가진 사용자가 소스 코드를 훑어본 후 [CSRF(Cross-Site Request Forgery)](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%9A%94%EC%B2%AD_%EC%9C%84%EC%A1%B0)나 [XSS(Cross-Site Scripting)](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8C%85)와 같은 방법으로 정상적인 애플리케이션에서 코드가 실행된 것처럼 꾸며 사용자의 정보를 탈취하기 매우 쉬워진다.

### 같은 출처와 다른 출처의 구분

그렇다면 정확히 어떤 경우에 출처가 같고 다른지 구분하는 기준을 알아둘 필요가 있다.

두 개의 출처가 서로 같다고 판단하는 로직 자체는 굉장히 간단하다. 두 URL의 구성 요소 중 `Scheme`, `Host`, `Port`, 이 세 가지만 동일하면 된다.

`https://shacomiro.github.io:443`라는 출처를 예로 들면, `https://`이라는 스키마에 `github.io`이라는 호스트를 가지고 `:443`번 포트를 사용하고 있다는 것만 같다면 나머지는 전부 다르더라도 같은 출처로 인정된다.

`https://shacomiro.github.io`와 같은 출처로 인정되는 예시는 대략 다음과 같다.

| URL                                         | 같은 출처 여부 | 이유                        |
| ------------------------------------------- | :------------: | --------------------------- |
| `https://shacomiro.github.io/about`         |       O        | 스키마, 호스트, 포트가 동일 |
| `https://shacomiro.github.io/about?q=안녕`  |       O        | 스키마, 호스트, 포트가 동일 |
| `https://user:password@shacomiro.github.io` |       O        | 스키마, 호스트, 포트가 동일 |
| `https://shacomiro.github.io`               |       X        | 스키마가 다름               |
| `https://api.github.com`                    |       X        | 호스트가 다름               |
| `https://shacomiro.naver.com`               |       X        | 호스트가 다름               |
| `https://shacomiro.github.com`              |       X        | 호스트가 다름               |
| `https://shacomiro.github.io:8000`         |       ?        | 브라우저의 구현에 따라 다름 |

맨 마지막 경우는 만약 출처에 `https://shacomiro.github.com:443`처럼 포트 번호가 명시되어 있다면 명백하게 다른 출처로 인정되는 부분이지만, 예시로 든 출처의 경우 포트 번호가 포함되지 않았기 때문에 판단하기가 모호하다. RFC 6454의 [Comparing Origins](https://tools.ietf.org/html/rfc6454#section-5) 목차에서는 "만약 출처가 스키마/호스트/포트의 삼중 체계라면..."이라는 전제가 붙어있기 때문에 어떻게 해석하느냐에 따라 구현이 달라질 수 있다.

따라서 이같은 경우 각 브라우저들의 독자적인 출처 비교 로직을 따르게 된다. 여기서 중요한 사실은 출처를 비교하는 로직이 서버에 구현된 스펙이 아닌 브라우저에 구현된 스펙이라는 것이다.

만약 CORS 정책을 위반하는 리소스 요청을 하더라도 해당 서버가 같은 출처에서 보낸 요청만 받겠다는 로직을 갖추지 않은 경우 서버는 정상적으로 응답하고, 이후 **브라우저가 이 응답을 분석해서 CORS 정책 위반이라고 판단하면 해당 응답을 사용하지 않고 파기**한다.

|                            ![CORS-02](./images/CORS-02.png)                             |
| :-------------------------------------------------------------------------------------: |
| 서버는 CORS를 위반하더라도 정상적으로 응답하고, 응답의 파기 여부는 브라우저가 결정한다. |

즉, CORS는 브라우저의 구현 스펙에 포함되는 정책이기 때문에, 브라우저를 통하지 않고 서버 간 통신을 할 때는 이 정책이 적용되지 않는다. 또한 CORS 정책을 위반하는 리소스 요청 때문에 에러가 발생하더라도 서버 쪽 로그에는 정상적으로 응답했다는 로그만 남기 때문에, CORS가 동작하는 방식을 정확히 알지 못하면 에러 추적에 난항을 겪을 수도 있다.

## CORS의 작동 원리

기본적으로 웹 클라이언트 애플리케이션이 다른 출처의 리소스를 요청할 때는 HTTP 프로토콜을 사용하여 요청을 보내게 된다. 이때 브라우저는 요청 헤더에 `Origin`이라는 필드에 요청을 보내는 출처를 함께 담아 보낸다.

```http
Origin: https://shacomiro.github.io
```

이후 서버가 이 요청에 대해 응답할 때 응답 헤더의 `Access-Control-Allow-Origin`이라는 값에 "이 리소스를 접근하는 것이 허용된 출처"를 내려주고, 이후 응답을 받은 브라우저는 자신이 보냈던 요청의 `Origin`과 서버가 보내준 응답의 `Access-Control-Allow-Origin`을 비교해본 후 이 응답이 유효한 응답인지 아닌지를 결정한다.

기본적인 흐름은 이처럼 간단하지만, 사실 CORS가 동작하는 방식은 한 가지가 아니라 세 가지의 시나리오에 따라 변경되기 때문에 사용자의 요청이 어떤 시나리오에 해당되는지 잘 파악한다면 CORS 정책 위반으로 인한 에러를 해결하는 것이 한결 수월해진다.

### Preflight Request

**프리플라이트(Preflight)** 방식은 일반적으로 우리가 웹 애플리케이션을 개발할 때 가장 빈번하게 마주치는 시나리오다. 이 시나리오에 해당하는 상황에서 **브라우저는 요청을 한번에 보내지 않고 예비 요청과 본 요청으로 나누어 서버로 전송**한다.

이때 브라우저가 본 요청 전에 보내는 예비 요청을 Preflight라고 부르며, 이 예비 요청에는 HTTP 메서드 중 `OPTIONS` 메서드가 사용된다. 예비 요청의 역할은 본 요청을 보내기 전에 브라우저 스스로 이 요청을 보내는 것이 안전한지 확인하는 것이다.

이 과정을 플로우 차트로 간단히 나타내면 다음과 같다.

|                         ![CORS-03](./images/CORS-03.png)                          |
| :-------------------------------------------------------------------------------: |
| 브라우저는 본 요청을 보내기 전 예비 요청을 먼저 보내고, 요청의 유효성을 검사한다. |

자바스크립트의 `fetch` API를 사용하여 브라우저에게 리소스를 받아오라는 명령을 내리면 브라우저는 서버에게 예비 요청을 먼저 보내고, 서버는 이 예비 요청에 대한 응답으로 현재 자신이 어떤 것들을 허용하고 어떤 것들을 금지하는지에 대한 정보를 응답 헤더에 담아서 브라우저에게 다시 보낸다.

이후 브라우저는 자신이 보낸 예비 요청과 서버가 응답에 담아준 허용 정책을 비교한 후, 이 요청을 보내는 것이 안전하다고 판단되면 같은 엔드포인트로 다시 본 요청을 보낸다. 이후 서버가 이 본 요청에 대해 응답하면 브라우저는 최종적으로 응답 데이터를 자바스크립트에게 넘겨준다.

이 흐름은 브라우저의 개발자 도구 콘솔에서도 간단하게 재현할 수 있다. `https://shacomiro.github.io`에서 Evan Moon님의 티스토리 서버인 `https://evanmoon.tistory.com/rss`에 요청을 보내면 브라우저가 본 요청을 보내기 전에 `OPTIONS` 메서드를 사용하여 예비 요청을 보내는 것을 확인할 수 있다.

```javascript
const headers = new Headers({
  'Content-Type': 'text/html',
});
fetch('https://evanmoon.tistory.com/rss', { headers });
```

```http
OPTIONS /rss HTTP/1.1
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: ko,ko-KR;q=0.9,en;q=0.8
Access-Control-Request-Headers: content-type
Access-Control-Request-Method: GET
Connection: keep-alive
Host: evanmoon.tistory.com
Origin: https://shacomiro.github.io
Referer: https://shacomiro.github.io/
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: cross-site
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36
```

실제로 브라우저가 보낸 요청을 살펴보면, 단순히 `Origin`에 대한 정보 뿐만 아니라 예비 요청 이후에 보낼 본 요청에 대한 정보들도 함께 포함되어 있다는 것을 알 수 있다.

예비 요청에서 브라우저는 `Access-Control-Request-Headers`를 사용하여 자신이 본 요청에서 `Content-Type` 헤더를 사용할 것을 알려주거나, `Access-Control-Request-Headers`를 사용하여 이후 `GET` 메서드를 사용할 것을 서버에게 미리 알리는 것이다.

이렇게 서버에 예비 요청을 보내면, 이제 서버가 해당 예비 요청에 대한 응답을 보낸다.

```http
HTTP/1.1 200 OK
Date: Thu, 04 Aug 2022 09:41:44 GMT
Content-Type: text/xml; charset=utf-8
Content-Length: 660
X-UA-Compatible: IE=Edge
P3P: CP='ALL DSP COR MON LAW OUR LEG DEL'
Access-Control-Allow-Origin: https://evanmoon.tistory.com
Vary: Accept-Encoding
Content-Encoding: gzip
```

여기서 눈여겨 봐야할 부분은 서버가 보내준 응답 헤더에 포함된 `Access-Control-Allow-Origin: https://evanmoon.tistory.com`이라는 값이다.

서버는 해당 리소스에 접근 가능한 출처는 오직 `https://evanmoon.tistory.com` 뿐이라고 브라우저에게 고지한 것이고, 이 요청을 보낸 출처는 `https://shacomiro.github.io`이므로 서버가 허용한 출처와는 다른 출처이다.

결국 브라우저는 이 요청이 CORS 정책을 위반했다고 판단하고 다음과 같은 에러가 발생한다.

> 🚨 Access to fetch at 'https://evanmoon.tistory.com/rss' from origin 'https://shacomiro.github.io' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: The 'Access-Control-Allow-Origin' header has a value 'https://evanmoon.tistory.com' that is not equal to the supplied origin. Have the server send the header with a valid value, or, if an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.

이때 예비 요청에 대한 응답에서 에러가 발생하지 않고 정상적으로 `200` 응답이 발생한 반면, 콘솔에서는 붉은색의 에러 메시지가 표시되어 혼란스러울 수도 있는데, CORS 정책 위반으로 인한 에러는 예비 요청의 성공 여부와 별 상관이 없다. 브라우저가 CORS 정책 위반 여부를 판단하는 시점은 예비 요청에 대한 응답을 받은 이후이기 때문이다.

물론 예비 요청 자체가 실패하더라도 동일하게 CORS 정책 위반으로 처리될 수도 있지만, 중요한 것은 예비 요청의 성공/실패 여부가 아니라 "응답 헤더에 유효한 `Access-Control-Allow-Control` 값이 존재하는가?"이다. 만약 예비 요청이 실패하여 `200`이 아닌 상태 코드가 응답되더라도 헤더에 `Access-Control-Allow-Control` 값이 제대로 포함되어 있다면 CORS 정책 위반이 아니라는 의미이다.

대부분의 경우 이렇게 예비 요청, 본 요청을 나누어 보내는 프리플라이트 방식을 사용하지만, 모든 상황에서 이렇게 두 번씩 요청을 보내는 것은 아니다. 까다로운 조건이 필요하긴 하나 예비 요청 없이 본 요청만으로 CORS 정책 위반 여부를 검사하기도 한다.

### Simple Request

이 시나리오는 정식 명칭은 없으나, MDN의 [CORS 문서](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS#%EC%A0%91%EA%B7%BC_%EC%A0%9C%EC%96%B4_%EC%8B%9C%EB%82%98%EB%A6%AC%EC%98%A4_%EC%98%88%EC%A0%9C)에서는 이 시나리오를 <b>단순 요청(Simple Request)</b>이라고 부르고 있다.

단순 요청은 예비 요청을 보내지 않고 바로 서버에게 본 요청을 보낸 후, 서버가 이에 대한 응답의 헤더에 `Access-Control-Allow-Control`과 같은 값을 보내주면 그때 브라우저가 CORS 정책 위반 여부를 검사하는 방식이다. 즉, 프리플라이트와 단순 요청 시나리오는 전반적인 로직 자체는 같되, 예비 요청의 존재 유무만 다르다.

|                 ![CORS-04](./images/CORS-04.png)                 |
| :--------------------------------------------------------------: |
| 단순 요청은 예비 요청 없이 바로 본 요청으로 대체하는 시나리오다. |

하지만 아무 때나 단순 요청을 사용할 수 있는 것은 아니고, 특정 조건을 만족하는 경우에만 예비 요청을 생략할 수 있다. 게다가 이 조건이 조금 까다롭기 때문에 일반적인 방법으로 웹 애플리케이션 아키텍처를 설계하면 충족시키기 어려운 조건들이기에 경험하기 어렵다.

조건은 다음과 같다.

1. 요청 메서드는 `GET`, `HEAD`, `POST` 중 하나여야 한다.
2. `Accept`, `Accept-Language`, `Content-Lanquage`, `Content-Type`, `DPR`, `Downlink`, `Save-Data`, `Viewport-Width`, `Width`를 제외한 헤더를 사용하면 안된다.
3. 만약 `Content-Type`을 사용하는 경우, `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain`만 허용된다.

사실 1번 조건의 경우는 단순히 `PUT`이나 `DELETE` 같은 메서드를 사용하지 않으면 될 뿐이기에 보기 드문 상황은 아니지만, 2번과 3번 조건의 경우는 조금 까다롭다.

애초에 저 조건에 명시된 헤더들은 정말 기본적인 헤더들이기 때문에 복잡한 상용 웹 애플리케이션이면서 이 헤더들 외에 추가적인 헤더를 사용하지 않는 경우는 드물다. 당장 사용자 인증에 사용되는 `Ahtorization` 헤더조차 저 조건에는 포함되지 않는다.

게다가 대부분의 HTTP API는 `text/xml`이나 `application/json` 컨텐츠 타입을 가지도록 설계되기 때문에 사실상 이 조건들을 모두 만족시키는 상황을 만들기는 그렇게 쉽지 않은 것이 현실이다.

### Credentialed Request

마지막 시나리오는 인증된 요청을 사용하는 방법이다. 이 시나리오는 CORS의 기본적인 방식이라기 보다는 다른 출처간 통신에서 보안을 조금 더 강화하고 싶을 때 사용하는 방법이다.

기본적으로 브라우저가 제공하는 비동기 리소스 요청 API인 `XMLHttpRequest` 객체나 `fetch` API는 별도의 옵션 없이 브라우저의 쿠키 정보나 인증과 관련된 헤더를 함부로 요청에 담지 않는다. 이때 요청에 인증과 관련된 정보를 담을 수 있게 해주는 옵션이 바로 `credentials` 옵션이다.

이 옵션에는 총 3가지의 값을 사용할 수 있으며, 각 값들이 가지는 의미는 다음과 같다.

| 옵션 값              | 설명                                            |
| -------------------- | ----------------------------------------------- |
| same-origin (기본값) | 같은 출처 간 요청에만 인증 정보를 담을 수 있다. |
| include              | 모든 요청에 인증 정보를 담을 수 있다.           |
| omit                 | 모든 요청에 인증 정보를 담지 않는다.            |

만약 `same-origin`이나 `include`와 같은 옵션을 사용하여 리소스 요청에 인증 정보가 포함된다면, 이제 브라우저는 다른 출처의 리소스를 요청할 때 단순히 `Access-Control-Allow-Origin`만 확인하는 것이 아니라 좀 더 빡빡한 검사 조건을 추가하게 된다.

로컬 환경에서 Evan Moon님의 깃허브 서버와의 통신을 통해, 어떤 제약이 추가되었는지 살펴보자. Evan Moon님의 블로그는 `Access-Control-Allow-Control` 값으로 모든 출처를 허용한다는 의미인 `*`가 설정되어 있기에 다른 출처에서 리소스를 요청할 때 CORS 정책 위반으로 인한 제약을 받지 않는다.

그래도 `http://localhost:8080`와 같은 로컬 개발 환경에서도 `fetch` API를 사용하여 리소스를 요청하고 받아올 수 있다.

```javascript
fetch('https://evan-moon.github.io/feed.xml');
```

```http
:authority: evan-moon.github.io
:method: GET
:path: /feed.xml
:scheme: https
accept: */*
accept-encoding: gzip, deflate, br
accept-language: ko
origin: http://localhost:8080
referer: http://localhost:8080/
sec-ch-ua: ".Not/A)Brand";v="99", "Google Chrome";v="103", "Chromium";v="103"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
sec-fetch-dest: empty
sec-fetch-mode: cors
sec-fetch-site: cross-site
user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.0.0 Safari/537.36
```

```http
accept-ranges: bytes
access-control-allow-origin: *
age: 531
cache-control: max-age=600
content-encoding: gzip
content-length: 1524994
content-type: application/xml
date: Thu, 04 Aug 2022 10:38:48 GMT
etag: W/"62b5defb-7d407b"
expires: Thu, 04 Aug 2022 06:06:03 GMT
last-modified: Fri, 24 Jun 2022 15:57:47 GMT
permissions-policy: interest-cohort=()
server: GitHub.com
vary: Accept-Encoding
via: 1.1 varnish
x-cache: HIT
x-cache-hits: 1
x-fastly-request-id: 2fcab8069bac1f175cd659bb3fcab16e4657f249
x-github-request-id: 6B32:2F46:1E2B3B:314BCC:62EB5F73
x-proxy-cache: MISS
x-served-by: cache-nrt-rjtf7700040-NRT
x-timer: S1659609529.749895,VS0,VE2
```

또한 구글 크롬 브라우저의 `credentials` 기본값은 같은 출처 내에서만 인증 정보를 사용하겠다는 `same-origin`이기 때문에, 로컬 환경에서 `https://evan-moon.github.io`로 보내는 리소스 요청에는 당연히 브라우저의 쿠키와 같은 인증 정보가 포함되어 있지 않다.

그렇기 때문에 브라우저는 단순히 `Access-Control-Allow-Control: *`이라는 값만 보고 "이 요청은 안전하다"라는 결론을 내리는 것이다. 그러나 `credentials` 옵션을 모든 요청에 인증 정보를 포함하겠다는 의미를 가진 `include`로 변경하고 같은 요청을 보내면 상황이 조금 달라진다.

```javascript
fetch('https://evan-moon.github.io/feed.xml', {
  credentials: 'include', // Credentials 옵션 변경!
});
```

`credentials: include` 옵션을 사용하여 동일 출처 여부와 상관없이 무조건 요청에 인증 정보가 포함되도록 설정하면, 요청에 브라우저의 쿠키 정보가 함께 담겨있는 모습을 확인할 수 있다.

브라우저는 이전 예제처럼 동일한 응답을 보내지만, 브라우저의 반응은 다르다.

> 🚨 Access to fetch at 'https://evan-moon.github.io/feed.xml' from origin 'http://localhost:8080' has been blocked by CORS policy: The value of the 'Access-Control-Allow-Origin' header in the response must not be the wildcard '\*' when the request's credentials mode is 'include'.

브라우저는 인증 모드가 `include`일 경우, 모든 요청을 허용한다는 의미의 `*`를 `Access-Control-Allow-Origin` 헤더에 사용해서는 안된다고 설명하고 있다.

이처럼 요청에 인증 정보가 담겨있는 상태에서 다른 출처의 리소스를 요청하게 되면 브라우저는 CORS 정책 위반 여부를 검사하는 규칙에 다음 두 가지를 추가한다.

1. `Access-Control-Allow-Origin`에는 `*`를 사용할 수 없으며, 명시적인 URL이어야 한다.
2. 응답 헤더에는 반드시 `Access-Control-Allow-Credentials: true`가 존재해야 한다.

인증까지 얽혀있는 이 시나리오는 다른 시나리오에 비해서 다소 복잡하게 느껴질 수 있다. 이처럼 CORS 정책에 대한 다양한 시나리오를 알아두면 실제 상황에서 CORS 정책 위반으로 인한 문제가 발생했을 경우 시행착오를 거치는 시간을 크게 단축시킬 수 있으니 숙지해두는 것이 좋다.

## CORS를 해결할 수 있는 방법

그렇다면 실질적으로 CORS 정책 위반으로 인한 문제가 발생했을 경우 해결하는 방법은 무엇이 있을까?

## Access-Control-Allow-Origin 설정하기

CORS 정책 위반으로 인한 문제를 해결하는 가장 대표적인 방법은, 정석대로 서버에서 `Access-Control-Allow-Origin` 헤더에 알맞은 값을 설정하는 것이다.

이때 와일드카드인 `*`을 사용하여 이 헤더를 설정하게 되면 모든 출처에서 오는 요청을 허용하겠다는 의미이므로 당장은 편할 수 있지만, 바꿔 생각하면 정체도 모르는 이상한 출처에서 오는 요청까지 모두 허용하겠다는 것이므로 심각한 보안 이슈가 발생할 수도 있다.

그러므로 가급적 `Access-Control-Allow-Origin: https://shacomiro.github.io`와 같이 출처를 명시해주는 것이 좋다.

이 헤더는 Nginx나 Apache와 같은 서버 엔진의 설정에서 추가할 수도 있지만, 복잡한 설정을 하기에는 불편한 점이 많기에 소스 코드 내에서 응답 미들웨어 등을 사용하여 설정하는 것이 좋다. Spring, Express, Django와 같이 이름있는 백엔드 프레임워크의 경우에는 모두 CORS 관련 설정을 위한 옵션이나 미들웨어 라이브러리를 제공하고 있으니 설정 자체가 어렵지는 않다.

## Webpack Dev Server로 리버스 프록싱하기

사실 CORS를 가장 많이 마주치는 환경은 바로 로컬에서 프론트엔드 애플리케이션을 개발하는 상황이라고 해도 과언이 아니다. 백엔드는 이미 `Access-Control-Allow-Origin` 헤더가 설정되어 있겠지만, 이 중요한 헤더에 `http://localhost:3000`과 같은 범용적인 출처를 넣는 경우는 드물기 때문이다.

프론트엔드 개발자는 대부분 웹팩과 `webpack-dev-server`를 사용하여 자신의 머신에 개발 환경을 구축하는데, 이 라이브러리가 제공하는 프록시 기능을 사용하면 편리하게 CORS 정책을 우회할 수 있다.

```javascript
module.exports = {
  devServer: {
    proxy: {
      '/api': {
        target: 'https://api.shacomiro.com',
        changeOrigin: true,
        pathRewrite: { '^/api': '' },
      },
    },
  },
};
```

이러게 설정하면 로컬 환경에서 `/api`로 시작하는 URL로 보내는 요청에 대해 브라우저는 `localhost:8000/api`로 요청을 보낸 것으로 알고 있지만, 실제로는 웹팩이 `https://api.shacomiro.com`으로 요청을 프록싱해주기 때문에 마치 CORS 정책을 지킨 것처럼 브라우저를 속이면서도 개발자는 원하는 서버와 자유롭게 통신할 수 있다. 즉, 프록싱을 통해 CORS 정책을 우회할 수 있는 것이다.

만일 [webpack-dev-middleware](https://github.com/webpack/webpack-dev-middleware)와 Node 서버의 조합으로 개발 환경을 직접 구축했더라도 [http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware) 라이브러리를 사용하면 손쉽게 프록시 설정을 할 수 있다. (`webpack-dev-server`도 내부적으로는 `http-proxy-middleware`를 사용한다.)

다만 이 방법은 실제 프로덕션 환경에서도 클라이언트 애플리케이션의 소스를 서빙하는 출처와 API 서버의 출처가 같은 경우에 사용하는 것이 좋다. 물론 로컬 개발 환경에서야 웹팩이 요청을 프록싱해주니 아무 이상이 없겠으나, 애플리케이션을 빌드한 후 서버에 올리고 나면 더 이상 `webpack-dev-server`가 구동하는 환경이 아니기 때문에 이상한 곳으로 API 요청을 보내기 때문이다.

예를 들어 API 서버의 출처는 `https:api.shacomiro.com`이고 클라이언트 애플리케이션을 서빙하는 서버의 출처는 `https//www.shacomiro.com`이라면, 다음과 같은 상황이 발생한다.

```javascript
fetch('/api/me');
```

```http
//로컬환경에서는
GET https://api.shacomiro.com/me 200 OK

//실제 서버에는 프록싱 로직이 없음
GET https://www.shacomiro.com/api/me 404 Not Found
```

물론 비즈니스 로직 내에서 `process.env.NODE_ENV`와 같은 빌드 환경 변수를 사용하여 분기 로직을 작성하는 방법도 있다. 그러나 비즈니스 로직에 이러한 개발 환경 전용 소스가 포함되어도 괜찮은지 충분히 고민할 필요가 있다.

## 요청을 img 태그에 담기?

앞서 SOP(Same-Origin Policy) 정책에는 다른 출처의 리소스에 접근할 수 있는 몇 가지 예외조항이 존재하고, 그 중 하나가 CORS 정책을 지킨 요청이라고 설명했다. 그리고 CORS 정책을 지킨 요청을 제외한 SOP의 예외 조항은 실행 가능한 스크립트, 렌더될 이미지, 스타일 시트 정도가 있다.

그렇다면 다른 예외 조항이 적용된 요청을 보내면 CORS를 우회할 수 있지 않을까? 다음과 같이 말이다.

```html
<img src="https://evanmoon.tistory.com/res" />
<script src="https://evanmoon.tistory.com/res"></script>
```

물론 이런 방식으로 접근하면 CORS를 위반하지 않고 요청 자체는 성공한다. 그리고 브라우저의 개발자 도구의 네트워크 탭에서 이 요청들의 헤더를 자세히 살펴보면 `Sec-Fetch-Mode: no-cors`라는 값이 포함되어 있는 것을 볼 수 있다.

`Sec-Fetch-Mode` 헤더는 요청 모드를 설정하는 필드인데, 브라우저는 이 필드의 값이 `no-cors`인 경우에는 다른 출처라고 해도 CORS 정책 위반 여부를 검사하지 않는다. 하지만 브라우저가 이 헤더에 값이 포함된 요청의 응답을 자바스크립트에게 알려주지 않는다는 것이 문제이다. 즉, 코드 레벨에서 이 응답에 담긴 내용에 절대 접근할 수가 없다.

따라서 CORS를 우회하려는 시도는 하지 말고 정해진 CORS 정책을 지키는 것이 바람직하다.

## 참고 자료

- [CORS는 왜 이렇게 우리를 힘들게 하는걸까?](https://evan-moon.github.io/2020/05/21/about-cors/)
