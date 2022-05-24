# 자바 버전별 특징

## 목차

1. [자바 버전의 현황](#자바-버전의-현황)
2. [Java 1.8](#java-18-20140318-release)
3. [Java 1.9](#java-19-20170921-release)

## 자바 버전의 현황

자바는 현재 6개월마다 신규 버전이 출시되고 있다. 그러나 이렇게 출시되는 각 자바 버전이 모두 활용되지는 않고 대부분 장기 지원 버전(LTS, Long Term Support)이 프로젝트 개발에 주로 사용된다.

## **Java 1.8 (2014.03.18 Release)**

일반 지원은 2015년 4월에 종료되었으며, 연장 지원은 2022년 7월에 종료될 예정이다.

- **Lamda Expression(람다 표현식)**  
  함수 지향적 표현 방식으로, 간단하게 식별자 없이 실행 가능한 함수 표현이다. 병렬 처리, 이벤트 지향적 프로그래밍에 적합하다.
  ```java
  //람다식 미사용
  Runnable runnable = new Runnable() {
      @Override
      public void run(){
          System.out.println("Hello world !");
      }
  };
  ```
  ```java
  //람다식 사용
  Runnable runnable = () -> System.out.println("Hello world two!");
  ```
  - 장점
    - 코드를 간결하고 직관적으로 구현할 수 있다.
    - 병렬 프로그래밍에 용이하다.
  - 단점
    - 재사용이 잦은 함수를 람다식으로 구성하면 코드가 난잡해질 수 있다.
    - 재귀에 사용하기 부적합하다.
    - 디버깅이 어렵다.
- Method Reference(메소드 참조)  
  자주 사용되는 패턴의 람다식을 간단하게 쓸 수 있는 방법으로, 코드 가독성을 높일 수 있다. 요약하자면 _'특정 패턴의 람다식을 줄여서 쓰는 기법'_ 이다.

  ```java
  //람다식
  Consumer<String> func = text -> System.out.println(text);
  func.accept("Hello");
  ```

  ```java
  //메소드 레퍼런스
  Consumer<String> func = System.out::println;
  func.accept("Hello");
  ```

  메소드 레퍼런스는 `ClassName:MethodName` 형식으로 입력한다. 메소드 호출이지만 괄호는 생략하며, 대부분의 코드가 생략되기 때문에 사용하려는 메소드의 인자와 리턴 타입을 알고 있어야 한다.

  - 장점
    - 메소드 레퍼런스 형태로 작성되는 람다식이 매우 많다.
    - 람다식을 모두 작성하는 것이 번거로울 경우 편리하게 코드를 작성할 수 있다.
  - 단점
    - 본래의 메소드 형태를 알고 있지 않으면 기능을 파악하기 어렵다.

- **Stream(스트림)**  
  Stream API를 사용하면 컬렌션과 배열을 처리하면서 발생하는 코드 반복 문제를 해결되어 코드량이 줄어들어 가독성이 향상된다. 또한 멀티 쓰레딩도 활용할 수 있다.  
  그러나 `forEach`에 비해 속도면에서 조금 느리기 때문에, 대량의 데이터를 처리하거나 효율성이 우선시되는 경우 `Stream` 대신 `forEach`를 사용하는 편이 낫다.
  ```java
  List<String> list = Arrays.asList(*"franz"*, *"ferdinand"*, *"fiel"*, *"vom"*, *"pferd"*);
  //Stream 사용
  list.stream()
      .filter(name -> name.startsWith(*"f"*))
      .map(String::toUpperCase)
      .sorted()
      .forEach(System.out::println);
  ```
- Default Method  
  디폴트 메소드는 인터페이스 내부에 있는 구현 메소드를 의미한다. 기존의 추상 메소드와 다른 점은 `default` 예약어와 구현부`{}`의 유무이다.  
  인터페이스에 신규 추상 메소드가 추가되면 인터페이스를 상속받는 모든 클래스들에 추가적인 구현이 필요한 상황이 발생하는데, 이는 OCP 원칙에서 변경에 닫혀(Close)있어야 하는 원칙에 위배된다. 디폴트 메소드가 등장하면 인터페이스에 default method를 사용하여 인터페이스를 상속받는 클래스들에 추가 변경을 막고, 수정이 필요한 클래스에서만 오버라이딩하면 되어 변경에 닫혀(Close)있을 수 있다.

  ```java
  public interface Interface {
      // 추상 메소드
      void abstractMethodA();
      void abstractMethodB();
      void abstractMethodC();

      // default 메소드
      default int defaultMethodA(){
        ...
      }
  }
  ```

- Optional  
  특정 객체의 참조 값의 `null` 여부를 확인하는 분기문들은 코드의 가독성을 떨어뜨리고 `null`을 가질 수 있는 객체인지, 필수값인지를 직관적으로 알 수 없어 에러의 근원이 되곤 했다. 이를 해결하기 위해 선택형 값을 캡슐화하는 클래스 `Optional`이 등장했다.

  ```java
  //빈 Optional 객체
  Optional<String> optStrA = Optional.empty();
  //null 미허용 Optional
  Optional<String> optStrB = Optional.of("not null");
  //null 허용 Optional
  Optional<String> optStrC = Optional.ofNullable(null);

  //활용
  String data = null;
  String result = Optional.ofNullable(data).orElse("it's null");
  ```

- 새로운 날짜, 시간 API([Joda Time](https://www.joda.org/joda-time/))  
  현재 기본 JDK를 대체하는 날짜, 시간 API 중 가장 널리 쓰이는 API이다. 주요 특징은 아래와 같다.
  - `LocalDate`, `DateTime` 등으로 지역 시간과 시간대가 지정된 시간을 구분했다. `LocalDate`, `LocalTime`으로 날짜와 시간을 별도의 클래스로 구분할 수도 있다.
  - `plusDays`, `plusMinutes`, `plusSeconds` 등 단위별 날짜 연산 메서드를 `LocalDate`, `DateTime` 클래스에서 지원한다. 메소드가 호출된 객체의 상태를 바꾸지 않고 새로운 객체를 반환한다. 불변 객체이다.
  - 월의 `int` 값과 명칭이 일치한다. 1월의 `int` 값은 `1`이다.
  - `GregorianChronology`를 썼을 때는 1582년 10월을 특별하게 취급하지는 않는다. `CJChronology`를 사용하면 JDK의 `GregorianCalendar`와 같이 10월 4일 다음날이 10월 15일로 나온다.
  - 서머타임 기간이면 `DateTimeZone.isStandardOffset()` 메소드의 반환값이 `false`이다.
  - 13월 같이 잘못된 월이 넘어가면 객체 생성 시점에서 `IllegalFieldValueException`을 던진다.
  - 요일 상수는 일관되게 사용한다.
  - 잘못된 시간대 ID 지정에는 `IllegalArguementException`을 던진다.
  - 시간 간격에대한 개념이 섬세하게 정의되어 있으며 `Duration`, `Period`, `Interval` 등으로 역할을 분담한 클래스로 구현했다.
  - 그레고리력과 율리우스력뿐만 아니라 불교, 이슬람교, 콥트 교회, 에티오피아의 달력까지도 지원한다. 다양한 달력은 `org.joda.time.chrono.BaseChronology` 클래스의 하위 클래스로 구현되어 있다.

## Java 1.9 (2017.09.21 Release)

일반 지원은 2018년 3월에 종료되었다.

- Project Jigsaw 기반 런타임 모듈화  
  대부분의 콘솔 프로그램 개발에서 더 이상 AWT나 Swing 같은 불필요한 라이브러리를 끌어쓸 필요 없이, 최상위 모듈이 Base만 사용해도 무방하다. 또한 특정 프로그램에 최적화된 최소 런타임을 제작할 수 있게 되어 패키징 역시 간편해졌다.

- JShell 추가  
  Java를 인터프리터 언어 셸처럼 사용할 수 있는 JShell이 추가되었다.

- 구조적 불변 컬렉션(Immutable Collection)  
  아이템의 추가, 수정, 제거가 불가능한 컬렉션으로 컬랙션 생성 후 변경되기를 윈치 않는 경우에 사용하며, 의도치 않은 컬렉션 변경 예방에 도움이 된다.

  ```java
  List<String> fruits = List.of("Apple", "Banana", "Cherry"); // [Apple, Banana, Cherry]

  fruits.add("Lemon"); // UnsupportedOperationException 발생
  ```

- private 인터페이스 메소드

- 통합 JVM 로깅

# 참고 서적/문서

- [Java 각 버전의 특징들 (~JAVA18)](https://marrrang.tistory.com/16)
- [Java Versions and Features](https://www.marcobehler.com/guides/a-guide-to-java-versions-and-features)
- Java8
  - [Java8 - 메소드 레퍼런스(Method Reference) 이해하기](https://codechacha.com/ko/java8-method-reference/)
  - [디폴트 메서드(Default Method)](https://velog.io/@heoseungyeon/%EB%94%94%ED%8F%B4%ED%8A%B8-%EB%A9%94%EC%84%9C%EB%93%9CDefault-Method)
  - [[자바/Java] Optional 개념,사용](https://lee1535.tistory.com/139)
  - [Java의 날짜와 시간 API](https://d2.naver.com/helloworld/645609)
- Java9
  - [Java9의 불변 컬렉션 생성](https://www.daleseo.com/java9-immutable-collections/)
