# WEB 서버와 WAS를 분리하는 이유

1. [WEB 서버](#web-서버)
2. [WAS](#was)
3. [WEB 서버와 WAS의 차이](#web-서버와-was의-차이)
4. [굳이 WEB 서버와 WAS를 분리하는 이유](#web-서버와-was를-분리하는-이유)
   1. [기능을 분리하여 서버 부하 방지](#기능을-분리하여-서버-부하-방지)
   2. [물리적으로 분리하여 보안 강화](#물리적으로-분리하여-보안-강화)
   3. [WEB 서버에 여러 WAS들을 연결 가능](#web-서버에-여러-was들을-연결-가능)
   4. [다수의 Web Application을 서비스 가능](#다수의-web-application을-서비스-가능)
5. [풀스택을 프론트엔드-백엔드 분리 시 동작 원리](#풀스택을-프론트엔드-백엔드-분리-시-동작-원리)
6. [참고 자료](#참고-자료)

## WEB 서버

![web-server(MDN Web Docs)](https://mdn.mozillademos.org/files/8659/web-server.svg)

클라이언트가 요청하는 **HTML 문서나 각종 정적 리소스를 전달해주는 소프트웨어**이다. 소프트웨어 이외에도 해당 소프트웨어를 실행시키는 **하드웨어** 또한 `WEB 서버`라고 말할 수 있다. 여기서는 Apache, Ngnix, WebtoB와 같은 소프트웨어 WEB 서버를 기준으로 이야기한다.

## WAS

> **W**eb **A**pplication **S**erver

동적 컨텐츠를 제공하기 위해 만들어진 애플리케이션 서버로, 웹 프로그램을 실행할 수 있는 환경을 제공한다. 대표적인 `WAS`로 Tomcat, Jeus, WebLogic, WebSphere 등이 있다.

## WEB 서버와 WAS의 차이

- WEB 서버  
  **정적인 컨텐츠**를 제공하는 서버
- WAS  
  **동적인 컨텐츠**를 제공하는 서버

  과거의 WAS에서는 정적인 웹서버 기능을 제공하지 않았으나, 발전을 거듭하여 최근에 와서는 동적인 컨텐츠 뿐만 아니라 정적인 컨텐츠도 제공하게 되었다. 때문에, WEB 서버 없이 WAS 단독으로 서비스를 구축하는 것도 가능하다.

## 굳이 WEB 서버와 WAS를 분리하는 이유

상술했듯, `WAS`만으로도 서비스를 제공하는 데에는 문제가 없다. 그러나 주로 서버를 구성할 때는 `WEB 서버`와 `WAS`를 분리하는 것이 대다수다. 여러 이유가 있는데, 그 중 4가지를 알아본다.

### 기능을 분리하여 서버 부하 방지

`WAS` 혼자서 동적 컨텐츠와 정적 컨텐츠 요청을 모두 처리할 수 있다고 하지만, 그렇게 되면 `WAS`가 감당하는 부하가 커져 페이지 노출 시간이 늘어나게 된다. 이때문에 **기능을 분리하여 각 서버가 담당하는 부하를 줄일 수 있도록 `WEB 서버`와 `WAS`를 분리**한다.

### 물리적으로 분리하여 보안 강화

`WAS`는 실제 `Web Application`이 배포되어 있기에 외부와 직접 연결되어 있다면 중요한 설정 파일이나 리소스가 외부로 노출될 위험이 있다. 이를 막기 위해 **`WEB 서버`를 `WAS`의 앞단에 배치하여 SSL 암복호화 처리에 이용함으로서 리소스를 안전하게 보호**할 수 있다.

### WEB 서버에 여러 WAS들을 연결 가능

큰 규모의 서비스에서는 '단일 `WEB 서버`-단일 `WAS`' 구조만으로는 많은 요청을 처리하는 데에 어려움이 있다. 때문에 수많은 요청들을 하나가 아닌 여러 장소에서 처리할 수 있도록 다수의 `Web Application`을 배치한다. 이때 다수의 `WAS`에 각각 요청을 들어오도록 하는 대신 먼저 앞쪽에 `WEB 서버`를 두고, 각 `WAS`들을 `WEB 서버`에 연결해서 `WEB 서버`로 들어오는 수많은 요청을 각 `WAS`들에게 적절하게 분배해주도록 한다.

**이렇게 배치하여 로드밸런싱을 해줌으로써 하나의 `WAS`가 처리하는 요청의 양이 줄어들어 안정적으로 무중단 서비스가 가능하고, `WAS`에서 오류가 발생했을 경우 장애 처리에도 용이**하다.

### 다수의 Web Application을 서비스 가능

Java Application, PHP Application과 같이 서로 다른 웹앱을 단일 `WEB 서버`에 연결하여 서비스 할 수 있다.

## 풀스택을 프론트엔드-백엔드 분리 시 동작 원리

> 더 자세한 내용은 [[React] 프론트 엔드와 백 엔드 분리 시 동작 원리 (vs 풀 스택)](https://it-eldorado.tistory.com/85) 참조

앞서 `WEB 서버`-`WAS` 간의 분리를 설명했는데, `풀스택`의 `프론트엔드`-`백엔드` 분리 역시 이와 유사하다.

보편적인 풀스택(Full Stack)은 Java의 Spring, Python의 Django 등의 웹앱이 `클라이언트 요청 - DB에서 데이터 요청 - 응답받은 데이터를 정적, 동적 파일과 함께 클라이언트에게 제공`하는 과정을 거친다.

반면 이를 분리하게 되면 정적, 동적 파일을 분할하여 담당하게 된다.  
`프론트엔드`(React 기준)에서는 클라이언트에게 제공할 JS 파일들을 ES6 + JSX 문법으로 코딩하고, Babel 등의 컴파일러가 이것들을 모든 브라우저에서 호환 가능한 문법(ES5)로 코드를 변환한다. 또한 Webpack 등의 번들러가 HTML, CSS, JS 파일들을 효율적인 방식으로 적절히 번들링하여 준비한다.

이후 클라이언트가 요청을 보내면 `프론트엔드 서버`는 미리 준비해둔 정적 파일들(HTML, CSS, JS)을 클라이언트에게 제공한다. 클라이언트의 브라우저는 전달받은 JS 파일을 실행하여 페이지에 렌더링을 시작한다. (React 라이브러리를 활용한 JS 코드는 동적으로 DOM에 렌더링 하기 위한 코드이다.)

렌더링 과정에서 DB 데이터가 필요한 경우에는 `백엔드 서버`에게 API 요청을 보내서 필요한 데이터를 요청한다. 이러한 맥락에서 `백엔드 서버`를 `API 서버`라고 부르기도 한다. 필요한 동적 데이터를 전달해주는 역할만 수행할 뿐, 페이지 렌더링에 필요한 정적 리소스들을 제공해주는 것이 아니기 때문이다.

이후, 페이지 리로드가 아닌 부분 렌더링이 필요한 경우에는 사용자의 액션(버튼 클릭 등)을 감지하여 JS가 마찬가지로 `백엔드 서버`에 API 요청을 보내게 된다. 이 경우에는 리렌더링이 필요한 부분의 데이터만 요청하게 된다. 그러면 JS가 해당 응답 데이터를 가지고 필요한 부분만 리렌더링한다.(이것 또한 개발자가 React 라이브러리를 활용하여 미리 작성한 JS 코드의 역할이다.)

이같은 맥락에서 React, Vue, Angular 등을 SPA(Single Page Application)라고 부른다. 기본적인 정적 파일들(HTML, CSS, JS)만 제공받은 후 렌더링이 필요한 부분에 대해서만 직접 `백엔드 서버`에게 API 요청을 보내 리렌더링 하는 방식이기 때문이다. 한 번 받은 페이지를 깜빡거리는 일 없이 동적으로 제어가 가능해진다.

## 참고 자료

- [WEB서버 / WAS 분리 이유](https://velog.io/@change/WEB%EC%84%9C%EB%B2%84-WAS-%EB%B6%84%EB%A6%AC-%EC%9D%B4%EC%9C%A0)
- [WEB과 WAS를 분리하는 이유](https://lurutia.tistory.com/864)
- [[React] 프론트 엔드와 백 엔드 분리 시 동작 원리 (vs 풀 스택)](https://it-eldorado.tistory.com/85)