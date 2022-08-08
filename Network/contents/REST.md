# REST

1. [REST](#rest-1)
   1. [REST란?](#rest란)
   2. [REST의 구체적인 개념](#rest의-구체적인-개념)
   3. [REST가 필요한 이유](#rest가-필요한-이유)
   4. [REST 설계 목표](#rest-설계-목표)
   5. [REST 구성](#rest-구성)
   6. [REST의 특징](#rest의-특징)
2. [REST API](#rest-api)
   1. [REST API란?](#rest-api란)
   2. [REST API 설계 기본 규칙](#rest-api-설계-기본-규칙)
   3. [REST API 설계 규칙](#rest-api-설계-규칙)
3. [RESTful API](#restful-api)
   1. [RESTful API란?](#restful-api란)
   2. [RESTful의 목적](#restful의-목적)
   3. [RESTful API 개발 원칙](#restful-api-개발-원칙)
4. [참고 자료](#참고-자료)

## REST

### REST란?

REST는 "**Representaional State Transfer**"의 약자로, 자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고받는 모든 것을 의미한다.

REST의 주요 정의를 요약하면 다음과 같다.

- 자원(Resource)의 표현(Representaion)에 의한 상태 전달을 뜻한다.
  1. **자원(Resource)의 표현(Representaion)**
     - 자원 : 해당 소프트웨어가 관리하는 모든 것
       - 문서, 이미지, 데이터, 해당 소프트웨어 자체 등
     - 자원의 표현 : 그 자원을 표현하기 위한 이름
       - DB의 학생 정보가 자원일 때, 'students'를 자원의 표현으로 정한다.
  2. **상태(정보) 전달**
     - 데이터가 요청되어지는 시점에서 자원의 상태(정보)를 전달한다.
     - JSON 또는 XML을 통해 데이터를 주고받는 것이 일반적이다.
- 월드 와이드 웹(WWW)과 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 개발 아키텍처의 한 형식이다.
  - REST는 기본적으로 **웹의 기존 기술과 HTTP 프로토콜을 그대로 활용**하기 때문에 **웹의 장점을 최대한 활용할 수 있는 아키텍처 스타일**이다.
  - REST는 네트워크 상에서 클라이언트와 서버 사이의 통신 방식 중 하나이다.

### REST의 구체적인 개념

- **HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, HTTP 메서드(POST, GET, PUT, DELETE)를 통해 해당 자원에 대한 CRUD 연산을 적용하는 것**을 의미한다.
  - 즉, REST는 자원 기반의 구조(Resource Oriented Architecture, ROA) 설계의 중심에 자원(Resource)이 있고 HTTP 메서드를 통해 자원을 처리하도록 설계된 아키텍처를 의미한다.
  - 웹 사이트의 이미지, 텍스트, DB 내용 등의 모든 자원에 고유한 ID인 HTTP URI를 부여한다.
  - CRUD 연산
    - Create : 생성(`POST`)
    - Read : 조회(`GET`)
    - Update : 수정(`PUT`)
    - Delete : 삭제(`DELETE`)
    - HEAD : header 정보 조회(`HEAD`)

### REST의 장단점

- **장점**
  1. HTTP 프로토콜의 인프라를 그대로 사용한다.
     - REST API 사용을 위한 별도의 인프라를 구축할 필요가 없다.
  2. HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있다.
  3. HTTP 표준 프로토콜을 따르는 모든 플랫폼에서 사용할 수 있다.
  4. Hypermedia API의 기본을 충실히 지키면서 범용성을 보장한다.
  5. REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
  6. 여러가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.
  7. 서버와 클라이언트의 역할을 명확히 분리한다.
- **단점**
  1. 명확한 표준이 존재하지 않는다.
  2. REST는 point-to-point 통신 모델을 기반으로 한다.
     - 서버와 클라이언트가 연결을 맺고 상호작용 해야하는 애플리케이션의 개발에는 적합하지 않다.
  3. REST는 URI, HTTP를 이용한 아키텍처링 방법에 대한 내용만을 담고 있다.
     - 보안과 통신규약 정책 같은 것은 전혀 다루지 않는다. 따라서 개발자는 통신과 정책에 대한 설계와 구현을 도맡아서 진행해야 한다.
  4. HTTP에 (상당히) 의존적이다.
     - REST는 설계 원리이기 때문에 HTTP와는 상관없이 다른 프로토콜에서도 구현할 수 있기는 하지만, 자연스럽게 녹여내기 어려울 수 있다. 대부분의 서비스가 웹으로 통합되는 상황에 비춰볼 때 큰 단점은 아니다.
  5. CRUD의 4가지 메서드만 제공한다.
     - 대부분의 작업을 처리할 수 있지만, 4가지 메서드만으로 처리하기엔 모호한 표현들이 있다.

### REST가 필요한 이유

- **애플리케이션의 분리 및 통합에 유리**하다.
  - 거대한 애플리케이션을 모듈, 기능별로 분리하기 쉽다. RESTful API를 서비스하기만 하면 어떠한 타 모듈 및 애플리케이션들이라도 RESTful API를 통해 상호간에 통신이 가능하기 때문이다.
- **다양한 종류의 클리이언트를 지원**할 수 있다.
  - 최근의 서버 프로그램은 다양한 브라우저와 각종 모바일 디바이스에서도 통신이 가능해야 한다. REST를 사용하면 클라이언트들이 요청하는 자원만 전송하면 되기에 멀티 플랫폼에 대한 지원이 수월하다.

### REST 설계 목표

REST의 핵심 설계 목표는 다음과 같다.

1. 컴포넌트들간의 유연한(쉽게 확장 가능한) **상호연동성** 확보
   - 상호연동성은 "서로 상이한 컴포넌트"들을 쉽게 연결할 수 있는 성질을 의미한다. 상호연동성은 두 개 이상의 컴포넌트들을 결합함으로써, 작업을 더 효율적으로 수행하도록 하는데 목적이 있다.
   - REST는 HTTP와 URI 기반인데, HTTP와 URI 모두 표준이며, 직관적이고 사용이 가능하며 광범위한 라이브러리와 소프트웨어 툴을 가지고 있어 어디에서든 동일하게 작동할 것을 보장받을 수 있다.  
     ![joinc.co.kr/w/man/12/rest/about-상호연동성](https://docs.google.com/drawings/d/1Y-wpPg52-WhAiwN9ZzbaGnGLXbvnis2tFaimiST03wM/pub?w=518&h=211)
2. **범용 인터페이스**
   - 상호연동성과 유사한 개념으로 REST 모델을 위한 HTTP와 URI는 표준으로 어디서든지 사용가능한 범용 인터페이스를 제공한다.
   - 개발자는 비즈니스 로직만 고민하면 된다.
3. 각 컴포넌트들의 **독립적인 배포**
   - 다른 컴포넌트들과 독립적으로 개발할 수 있다는 것을 의미한다.
   - 규격에 맞춰 개발된다면 다른 컴포넌트가 추가되도 연동에 지장이 없다.
4. 지연 감소, 보안강화, 레거시 시스템을 인캡슐레이션하는 **중간 컴포넌트**로의 역할
   - 클라이언트는 엔드 서버에 직접 연결할 필요 없이 서비스를 이용할 수 있다. REST 서버가 클라이언트와 엔드 서버의 중간에서 중계 역할을 수행할 수 있기 때문이다.
   - 중계 서버로 이용하면 로드 밸런싱, 공유 메모시(캐시) 등을 이용해서 가용성과 확작성/성능을 향상시킬 수 있고, 보안 정책을 적용하기 쉽다.  
     ![joinc.co.kr/w/man/12/rest/about-컴포넌트_중계_역할](https://docs.google.com/drawings/d/1ezdSB6yukeuiBmui4lmxLqoxugNtFr-pC4mxmBM8hgU/pub?w=569&h=304)

### REST 구성 요소

1. **자원(Resource) : URI**
   - 모든 자원에 고유한 ID가 존재하고, 이 자원은 서버(Server)에 존재한다.
   - 자원을 구별하는 ID는 `/group/:group_id`와 같은 HTTP **URI**이다.
   - 클라이언트는 URI를 이용해서 자원을 지정하고 해당 자원의 상태(정보)에 대한 조작을 서버에 요청한다.
2. **행위(Verb) : HTTP 메서드**
   - HTTP 프로토콜의 메서드(Method)를 사용한다.
   - HTTP 프로토콜은 `GET`, `POST`, `PUT`, `DELETE`와 같은 메서드를 제공한다.
3. **표현(Representaion of Resource)**
   - 클라이언트가 자원의 상태(정보)에 대한 조작을 요청하면 서버는 이에 적절한 응답(Representaion)을 보낸다.
   - REST에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representaion으로 나타낼 수 있다.
   - 현재는 JSON을 통해 데이터를 주고 받는 것이 일반적이다.

### REST의 특징

1. **서버-클라이언트(Server-Client) 구조**
   - 클라이언트는 유저와 관련된 처리를, 서버는 REST API를 제공함으로써 각각의 역할이 확실하게 구분되고 일괄적인 인터페이스로 분리되어 작동한다.
   - 자원이 있는 쪽이 서버(Server), 자원을 요청하는 쪽이 클라이언트(Client)가 된다.
     - REST 서버 : API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.
     - 클라이언트 : 사용자 인증이나 Context(세션, 로그인 정보) 등을 직접 관리하고 책임진다.
   - 서로 간 의존성이 줄어든다.
2. **무상태성(Stateless)**
   - HTTP 프로토콜은 무상태성(Stateless) 프로토콜이므로 REST 역시 무상태성을 갖는다.
   - 클라이언트의 컨텍스트(Context)를 서버에 저장하지 않는다.
     - 즉, 세션이나 쿠키와 같은 컨텍스트 정보를 신경쓰지 않아도 되므로 구현이 단순해진다.
   - 서버는 각각의 요청을 완전히 별개의 것으로 인식하고 처리한다.
     - 각 API 서버는 클라이언트의 요청만을 단순 처리한다. 즉, 이전 요청이 다음 요청의 처리에 연관되서는 안된다.
     - 단, 이전 요청이 DB를 수정하여 DB에 의해 바뀌는 것은 허용한다.
     - 서버의 처리 방식에 일관성을 부여하고 부담이 줄어들며, 서비스의 자유도가 높아진다.
3. **캐시 처리 가능(Cacheable)**
   - 웹 표준 HTTP 프로토콜을 그대로 사용하므로, 웹에서 사용하는 기존 인프라를 그대로 활용할 수 있다.
     - 즉, HTTP가 가진 가장 강력한 특징 중 캐싱 기능을 적용할 수 있다.
     - HTTP 프로토콜 표준에서 사용하는 Last-Modified 태그나 E-Tag를 이용하면 캐싱 구현이 가능하다.
   - 대량의 요청을 효율적으로 처리하기 위해 캐시가 요구된다.
   - 캐시 사용을 통해 응답시간이 단축되고 REST 서버 트랜잭션이 발생하지 않기 때문에 전체 응답시간과 성능, 서버의 자원 이용률을 향상시킬 수 있다.
4. **자체 표현 구조(Self-Descriptiveness)**
   - JSON을 이용한 메시지 포맷을 이용하여 직관적으로 이해할 수 있고, REST API 메시지만으로 그 요청이 어떤 행위를 하는지 쉽게 알 수 있는 자체 표현 구조로 되어 있다.
5. **계층화(Layerd System)**
   - 클라이언트는 REST API 서버만 호출한다.
   - REST 서버는 다중 계층으로 구성될 수 있다.
     - API 서버는 순수 비즈니스 로직을 수행하고 그 앞단에 보안, 로드밸런싱, 암호화, 사용자 인증 등을 추가하여 구조상의 유연성을 줄 수 있다.
     - 또한 로드밸런싱, 공유 캐시 등을 통해 확장성과 보안성을 향상시킬 수 있다.
   - 프록시, 게이트웨이와 같은 네트워크 기반의 중간 매체를 사용할 수 있다.
6. **인터페이스 일관성(Uniform Interface)**
   - URI로 지정한 자원(Resource)에 대한 조작을 통일되고 한정적인 인터페이스로 수행한다.
   - HTTP 표준 프로토콜을 따르는 모든 플랫폼에서 사용이 가능하다.
     - **특정 언어나 기술에 종속되지 않는다.**

## REST API

### REST API란?

- API(Application Programming Interface)
  - 데이터와 기능의 집합을 제공하여 컴퓨터 프로그램간 상호작용을 촉진하며, 서로 정보를 교환 가능하도록 하는 것을 말한다.
- REST API의 정의
  - **REST 기반으로 서비스 API를 구현한 것**이다.
    - 즉, 자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고받는 방식으로 API를 구현한 것이 REST API라고 할 수 있다.
  - 최근 OpenAPI, 마이크로 서비스 등을 제공하는 업체 대부분은 REST API를 제공한다.

### REST API의 특징

- REST 기반으로 시스템을 분산하면 확장성과 재사용성을 높일 수 있어 유지보수가 용이하고 운용을 편리하게 할 수 있다.
- REST는 HTTP 표준을 기반으로 구현하므로, HTTP를 지원하는 프로그램 언어로 클라이언트와 서버를 구현할 수 있다.
- 즉, REST API를 제작하면 델파이 클라이언트 뿐만 아니라 자바, C#, 웹 등을 이용해 클라이언트를 제작할 수 있다.

### REST API 설계 기본 규칙

> **※ 리소스 원형**
>
> - 도큐먼트 : 객체 인스턴스나 데이터베이스 레코드와 유사한 개념
> - 컬렉션 : 서버에서 관리하는 디렉터리라는 리소스
> - 스토어 : 클라이언트에서 관리하는 리소스 저장소

1. **URI는 정보의 자원을 표현해야 한다.**
   1. Resource는 동사보다는 명사를, 대문자보다는 소문자를 사용한다.
   2. Resource의 도큐먼트 이름으로는 단수 명사를 사용해야 한다.
   3. Resource의 컬렉션 이름으로는 복수 명사를 사용해야 한다.
   4. Resource의 스토어 이름으로는 복수 명사를 사용해야 한다.
      - `GET /Member/1` -> `GET /members/1`
2. **자원에 대한 행위는 HTTP 메서드(`GET`, `PUT`, `POST`, `DELETE` 등)로 표현한다.**
   1. URI에 HTTP 메서드가 들어가면 안된다.
      - `GET /members/delete/1` -> `DELETE /members/1`
   2. URI에 행위에 대한 동사 표현이 들어가면 안된다. (즉, CRUD 기능을 나타내는 것은 URI에 사용하지 않는다.)
      - `GET /members/show/1` -> `GET /members/1`
      - `GET /members/insert/2` -> `POST /members/2`
   3. 경로 부분 중 변하는 부분은 유일한 값으로 대체한다. (즉, ID는 하나의 특정 Resource를 나타내는 고유값이다.)
      - student를 생성하는 라우트 : `POST /students`
      - id=12인 student를 삭제하는 라우트 : `DELETE /students/12`

### REST API 설계 규칙

1. **슬래시 구분자(`/`)는 계층 관계를 나타내는데 사용한다.**
   - `http://restapi.example.com/houses/apartments`
2. **URI 마지막 문자로 슬래시(`/`)를 포함하지 않는다.**
   - URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며 URI가 다르다는 것은 리소스가 다르다는 것으로, 역으로 리소스가 다르면 URI도 달라야 한다.
   - REST API는 분명한 URI를 만들어 통신을 해야 하기 때문에 혼동을 주지 않도록 URI 경로의 마지막에는 슬래시(`/`)를 사용하지 않는다.
   - ⛔ `http://restapi.example.com/houses/apratments/`
3. **하이픈(`-`)은 가독성에 문제가 있는 경우에만 URI에 사용한다.**
   - 불가피하게 긴 URI 경로를 사용하게 된다면 하이픈을 사용해 가독성을 높인다.
4. **밑줄(`_`)은 URI에 사용하지 않는다.**
   - 밑줄은 보기 어렵거나 밑줄 때문에 문자가 가려지기도 하므로 가독성을 위해 밑줄은 사용하지 않는다.
5. **URI 경로에는 소문자가 적합하다.**
   - URI 경로에 대문자 사용은 피하도록 한다.
   - RFC 3986(URI 문법 형식)은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정하고 있다.
6. **파일 확장자는 URI에 포함하지 않는다.**
   - REST API에서는 메시지 바디 내용의 포맷을 나타내기 위한 파일 확장자를 URI 안에 포함시키지 않는다.
   - Accept header를 사용한다.
   - ⛔ `http://restapi.example.com/members/football/345/photo.jpg`
   - ✅ `GET /members/football/345/photo HTTP/1.1 Host: restapi.example.com Accept: image/jpg`
7. **리소스 간에 연관 관계가 있는 경우**
   - `/리소스명/리소스ID/관계가 있는 다른 리소스 명`
   - `GET /users/{userid}/devices`
     - 일반적으로 소유(has)의관계를 표현할 때 사용한다.

### REST API 설계 예시

간단한 REST API의 예시를 살펴보면 다음 표와 같다.

| CRUD                      | HTTP 메서드 | Route            |
| ------------------------- | ----------- | ---------------- |
| 리소스들의 목록을 표시    | `GET`       | `/resources`     |
| 단일 리소스의 내용을 표시 | `GET`       | `/resources/:id` |
| 리소스를 생성             | `POST`      | `/resources`     |
| 리소스를 수정             | `PUT`       | `/resources/:id` |
| 리소스를 삭제             | `DELETE`    | `/resources/:id` |

## RESTful API

### RESTful API란?

![gmlwjd9405.github.io-restful](https://gmlwjd9405.github.io/images/network/restful.png)

- RESTful API는 HTTP와 URI 기반으로 자원에 접근할 수 있도록 제공하는 애플리케이션 개발 인터페이스가. 기본적으로 개발자는 HTTP 메서드와 URI만으로 인터넷에 자료를 CRUD 할 수 있다.
- RESTful은 일반적으로 REST라는 아키텍처를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어이다.
  - 'REST API'를 제공하는 웹 서비스를 'RESTful'하다고 할 수 있다.
- **RESTful은 REST를 REST답게 쓰기 위한 방법**으로, 누군가가 공식적으로 발표한 것이 아니다.
  - 즉, REST 원리를 따르는 시스템은 RESTful이라는 용어로 지칭된다.

### RESTful의 목적

RESTful의 목적은 **이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것**이다.

RESTful한 API를 구현하는 근본적인 목적은 성능 향상에 있는 것이 아닌 일관적인 컨벤션을 통한 API의 이해도 및 호환성을 높이는 것이 주 동기이다. 따라서 성능이 중요한 상황에서는 굳이 RESTful한 API를 구현할 필요는 없다.

### RESTful API 개발 원칙

1. **자원을 식별할 수 있어야 한다.**
   - URL만으로 어떤 자원을 제어하려고 하는지를 알 수 있어야 한다. 자원을 제어하기 위해서, 자원의 위치는 물론 자원의 종류까지 알 수 있어야 한다는 의미이다.
   - 서버측 데이터베이스에 저장되어야 하는 정보는 JSON이나 XML 형태로 HTTP body 데이터로 전송한다.
   - java 게시판에 게시글을 올리는 상황을 예로 들면 다음과 같이 데이터를 전송하면 된다.
     ```json
     POST /bbs/java
     {
       "author": "shacomiro",
       "charset": "UTF-8",
       "title": "Hello World",
       "text": "안녕하세요.",
       "createdDate": "2022/08/08 18:00:00",
       "email": "shacomiro@abc.com"
     }
     ```
2. **행위는 명시적이어야 한다.**
   - REST는 아키텍처 혹은 방법론과 비슷하다. 따라서 이런 방식을 사용해야 한다고 강제하지는 않는다. 기존의 웹 서비스처럼, `GET`을 이용해서 `UPDATE`와 `DELETE`를 수행해도 된다.
   - 이런 행위를 막는 규칙같은 것은 없지만, REST 아키텍처에는 부합하지 않는 방식이므로 REST를 따른다고 할 수 없다.
     - REST에서 행위는 명시적이어야 한다.
     - CRUD 기능을 모두 `POST`로만 처리하는 API는 RESTful하다고 할 수 없다.
3. **자기 서술적이어야 한다.**
   - 데이터에 대한 메타 정보만 가지고도 어떤 종류의 데이터인지, 데이터를 위해서 어떤 애플리케이션을 실행해야 하는지를 알 수 있어야 한다.
   - 데이터 처리를 위한 정보를 얻기 위해서, 데이터 원본을 읽어야 한다면 이것은 자기 서술적이 아니다.
   - 동영상 처리 웹 서비스를 예로 들면 다음과 같다.
     ```json
     //자기 서술적이지 않은 정보
     //"application/video"만 보내는 것은 자기 서술적이 아니다. 동영상 정보를 알기 위해서는 원본 파일을 다운로드 한 후 분석해야 하기 때문이다.
     {
       "filename": "test.mp4",
       "type": "application/video"
     }
     ```
     ```json
     //자기 서술적인 정보
     //자기 서술적이라면 해상도와 코덱, 비트레이트, 자막파일의 위치, 플레이타임 등의 정보를 모두 포함해야 한다.
     {
       "filename": "test.avi",
       "title": "Hello World",
       "type": "application/video",
       "bitrate": "128kpbs",
       "resolution": { "height": 1280, "width": 1024 },
       "codec": "H.264/MPEG-4",
       "playtime": 1850,
       "subtitle": "abc.smi"
       //기타등등..
     }
     ```
4. **HATEOS(Hypermedia as the Engine of Application State)**
   - 클라이언트의 요청에 대해 응답할 때, 추가적인 정보를 제공하는 링크를 포함할 수 있어야 한다.
   - REST는 독립적인 컴포넌트들을 손쉽게 연결하기 위한 목적으로도 사용한다. 서로 다른 컴포넌트를 유연하게 연결하려면, 느슨한 연결을 만들어 줄 무언가가 필요하다.
     - 이것이 바로 `링크`다. 서버는 클라이언트 응용 애플리케이션에 하이퍼 링크를 제공한다.
     - 클라이언트는 이 하이퍼링크를 통해 전체 네트워크와 연결된다. HATEOS는 서버가 독립적으로 진화할 수 있도록 서버와 서버, 서버와 클라이언트를 분리할 수 있게 한다.
     - > 실제 적용 방법에 대한 설명은 [REST API 설계 가이드](https://sharplee7.tistory.com/49)의 _10. HATEOAS(Hypermedia as the Engine of Application State) 적용_ 문단을 참고

## 참고 자료

- [RESTful API 이란](https://velog.io/@somday/RESTful-API-%EC%9D%B4%EB%9E%80)
- [[Network] REST란? REST API란? RESTful이란?](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)
- [REST API 제대로 알고 사용하기](https://meetup.toast.com/posts/92)
- [REST에 대하여](https://www.joinc.co.kr/w/man/12/rest/about)
- [RESTful에 대해서 설명해주세요.(REST, RESTful, RESTful API 개념 정리)](https://jeong-pro.tistory.com/180)
