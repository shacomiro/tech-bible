# SOLID

1. [SOLID란?](#solid란)
   1. [간단한 요약](#간단한-요약)
2. [SRP(단일 책임의 원칙, Single Responsibility Principle)](#srp단일-책임의-원칙-single-responsibility-principle)
   1. [정의](#정의)
   2. [적용 방법](#적용-방법)
   3. [적용 사례](#적용-사례)
   4. [적용 이슈](#적용-이슈)
3. [OCP(개방 폐쇄의 원칙, Open Close Principle)](#ocp개방-폐쇄의-원칙-open-close-principle)
   1. [정의](#정의-1)
   2. [적용 방법](#적용-방법-1)
   3. [적용 사례](#적용-사례-1)
   4. [적용 이슈](#적용-이슈-1)
4. [참고 자료](#참고-자료)

## SOLID란?

> SOLID란 로버트 마틴이 2000년대 초에 명명한 **객체 지향 프로그래밍의 다섯 가지 기본 원칙**을 마이클 페더스가 원칙의 앞 글자를 따서 SOLID라는 이름으로 소개한 것이다.

SOLID는 입증된 객체 지향 프로그래밍 디자인 원리로 이를 사용하면 좀 더 유지보수하기 쉽고, 유연하고, 확장하기 쉬운 소프트웨어를 만들 수 있다. 이 원리들은 그 크기를 대비해 보면 패턴보다는 훨씬 작지만 표준화 작업에서부터 아키텍처 설계에 이르기까지 다양하게 적용되는 원칙이다.

### 간단한 요약

다섯 가지 기본 원칙들을 간단히 요약하면 아래와 같다.

1. **SRP(Single Responsibility Principle)** : 단일 책임의 원칙. 클래스는 한 가지 기능만을 담당해야 한다는 원칙이다. 예를 들어 은행 계좌 프로그램에서 입금/출금에 대해서 입금하는 기능을 단일 책임 원칙에 따르면 메소드로 나뉠것이 아니라 담당하는 클래스, 출금하는 기능을 담당하는 클래스로 나뉘어야 한다는 것이다.
2. **OCP(Open Close Principle)** : 개방 폐쇄의 원칙. 클래스는 확장에는 열려있고, 변경에는 닫혀 있어야 한다는 원칙이다.
3. **LSP(Liskov Substitution Principle)** : 리스코프 치환의 원칙. 서브 타입은 언제나 기반 타입으로 교체할 수 있어야 한다라는 원칙이다. 쉽게 설명하면 부모가 동작하는 기능은 자식도 동일하게 동작해야 한다는 것이다.
4. **ISP(Interface Segregation Principle)** : 인터페이스 분리의 원칙. 자신이 사용하지 않는 인터페이스는 구현하지 말아야 한다는 원칙이다. 바꿔 말하면, 하나의 큰 인터페이스보다는 여러개의 작은 인터페이스를 구현하는 것이 낫다라고도 할 수 있다.
5. **DIP(Dependency Inversion Principle)** : 의존 관계 역전의 원칙. 구조적 디자인에서 발생하던 하위 레벨 모듈의 변경이 상위 레벨 모듈의 변경을 요구하는 위계관계를 끊는 의미의 역전이다. 쉽게 말하면 코드에서는 인터페이스에서 구현하는 클래스로 그 의존 관계가 흐르지만 실행시에는 역전된다 정도로 생각하면 된다.

## SRP(단일 책임의 원칙, Single Responsibility Principle)

> There should never be more than one reason for a class to change.  
> **클래스는 한 가지 기능만을 담당해야 한다.** 클래스가 변경되는 이유가 한 가지를 초과해서는 안된다.

### 정의

**작성된 클래스는 하나의 기능만 가지며 클래스가 제공하는 모든 서비스는 그 하나의 책임(변화의 축, axis of change)을 수행하는 데 집중되어 있어야 한다**는 원칙이다.

이는 어떤 변화에 의해 클래스를 변경하는 이유는 오직 하나뿐이어야 함을 의미한다. SRP 원리는 적용하면 무엇보다도 책임 영역이 확실해지기 때문에 한 책임의 변경에서 다른 책임의 변경으로의 연쇄작용에서 자유로울 수 있다. 뿐만 아니라 책임을 적절히 분배함으로써 코드의 가독성 향상, 유리보수 용이라는 이점까지 누릴 수 있으며 객체지향 원리의 대전제 격인 OCP 원리뿐 아니라 다른 원리들을 적용하는 기초가 된다.

이 원리는 다른 원리들에 비해서 개념이 비교적 단순하지만, 이 원리를 적용해서 직접 클래스를 설계하기가 그리 쉽지만은 않다. 왜냐하면, 실무의 프로세스는 매우 복잡 다양하고 변경 또한 빈번하기 때문에 경험이 많지 않거나 도메인에 대한 업무 이해가 부족하면 나도 모르게 SRP 원리에서 멀어져 버리게 된다. 따라서 평소에 많은 연습('책임'이란 단어를 상기하는)과 경험이 필요한 원칙이다.

### 적용 방법

리팩토링(『Refactoring: Improving the Design of Exising Code』 - Martin Fowler)에서 소개하는 대부분의 위험 상황에 대한 해결 방법은 직/간접적으로 SRP 원리와 관련이 있으며, 이는 항상 코드를 최상으로 유지한다는 리팩토링의 근본 정신도 항상 객체들의 책임을 최상의 상태로 분배한다는 것에서 비롯되기 때문이다.

- **여러 원인에 의한 변경(Divergent change)**  
  `Extract Class`를 통해 혼재된 각 책임을 각각의 개별 클로스로 분할하여 클래스 당 하나의 책임만을 맡도록 하는 것이다. 여기서 관건은 책임만 분리하는 것이 아니라 분리된 두 클래스간의 관계의 복잡도를 줄이도록 설계하는 것이다. 만약 `Extract Class`된 각각의 클래스들이 유사하고 비슷한 책임을 중복해서 갖고 있다면 `Extract Superclass`를 사용할 수 있다. 이것은 Extract된 각각의 클래스들의 공유되는 요소를 부모 클래스로 정의하여 부모 클래스에게 위임하는 기법이다. 따라서 각각의 `Extract Class`들의 유사한 책임들은 부모에게 명백히 위임하고 다른 책임들은 각자에게 정의할 수 있다.
- **산탄총 수술(Shotgun surgery)**  
  `Move Field`와 `Move Method`를 통해 책임을 기존의 어떤 클래스로 모으거나, 이럴만한 클래스가 없다면 새로운 클래스를 만들어 해결할 수 있다. 즉, 산발적으로 여러 곳에 분포된 책임들을 한 곳에 모으면서 설계를 깨끗하게 한다. 즉, 응집성을 높이는 작업이다.

### 적용 사례

아래 그림의 간단한 클래스를 살펴보자. 변화가 예상되는 부분이 있는가? 천천히 살펴보자.

<center><img alt="그림1" src="./images/SOLID-01.png" width="47%"/></center>

```java
class Guitar {
   private String serialNumber;
   private double price;
   private Maker maker;
   private Type type;
   private String model;
   private Wood topWood;
   private Wood backWood;
   private int stringNum;

   public Guitar(String serialNumber, double price, Maker maker, Type type, String model, Wood backWood, Wood topWood, int stringNum) {
      this.serialNumber = serialNumber;
      this.price = price;
      this.maker = maker;
      this.type = type;
      this.model = model;
      this.backWood = backWood;
      this.topWood = topWood;
      this.stringNum = stringNum;
   }

   ...
}
```

위 그림에서 보는 바와 같이 `serialNumber`는 변화 요소라 할 수 없고 단지 고유 정보라고 할 수 있다. 동종의 다른 클래스와 구분되는 정보라고 할 수 있겠다. 그리고 `price`와 `Maker`, `Type`, `model`, `backWood`, `stringNum` 등은 모두 특성 정보군으로 변경이 발생할 수 있는 부분이라 할 수 있고, 이 부분은 변화 요소로 예상된다. 따라서 특정 정보군에 변화가 발생하면 항상 해당 클래스를 수정해야 하는 부담이 발생하게 됨으로 이 부분이 SRP 적용의 대상이 된다.

<center><img alt="그림1" src="./images/SOLID-02.png"/></center>

```java
class Guitar {
   private String serialNumber;
   private GuitarSpec spec;

   public Guitar(String serialNumber, GuitarSpec spec) {
      this.serialNumber = serialNumber;
      this.spec = spec;
   }

   ...
}

class GuitarSpec {
   private double price;
   private Maker maker;
   private Type type;
   private String model;
   private Wood topWood;
   private Wood backWood;
   private int stringNum;

   public Guitar(double price, Maker maker, Type type, String model, Wood backWood, Wood topWood, int stringNum) {
      this.price = price;
      this.maker = maker;
      this.type = type;
      this.model = model;
      this.backWood = backWood;
      this.topWood = topWood;
      this.stringNum = stringNum;
   }

   ...
}
```

위 그림의 다이어그램을 보면 변화가 예상되는 특성 정보군을 분리한 것을 확인할 수 있다. 따라서 특성 정보에 변경이 일어나면 `GuitarSpec` 클래스만 변경하면 된다. 훨씬 보기에도 좋아졌고 무엇보다도 변화에 의해 변경되는 부분을 한 곳에서 관리할 수 있게 되었다.

### 적용 이슈

클래스는 자신의 이름을 나타내는 일을 해야 한다. 올바른 클래스 이름은 해당 클래스의 책임을 나타낼 수 있는 게 가장 좋은 방법이다.

각 클래스는 하나의 개념을 나타내야 한다. 사용되지 않는 속성이 결정적 증거이다. 무조건 책임을 분리한다고 SRP가 적용되는 건 아니다. 각 개체간의 응집력이 있다면 병합이 순 작용의 수단이 되고, 결합력이 있다면 분리가 순 작용의 수단이 된다.

## OCP(개방 폐쇄의 원칙, Open Close Principle)

> You should be able to extend a classes behavior, without modifying it.  
> **클래스의 확장에는 열려있고, 변경에는 닫혀있어야 한다.**

### 정의

버틀란트 메이어(Bertrand Meyer) 박사가 1998년 『객체지향 소프트웨어 설계』라는 책에서 정의한 내용으로, **소프트웨어의 구성요소(컴포넌트, 클래스, 모듈, 함수)는 확장에는 열려있고, 변경에는 닫혀있어야 한다**는 원리이다.

이것은 변경을 위한 비용은 가능한 줄이고 확장을 위한 비용은 가능한 극대화해야 한다는 의미로, 요구사항의 변경이나 추가사항이 발생하더라도, 기존 구성요소는 수정이 일어나지 않아야 하며, 기존 구성요소를 쉽게 확장해서 재사용할 수 있어야 한다는 뜻이다.

로버트 C. 마틴은 OCP는 관리가능하고 재사용 가능한 코드를 만드는 기반이며, OCP를 가능케 하는 중요 메커니즘은 추상화와 다형성이라고 설명하고 있다. OCP는 객체지향의 장점을 극대화하는 아주 중요한 원리라 할 수 있다.

### 적용 방법

1. 변경(확장)될 것과 변하지 않을 것을 엄격히 구분한다.
2. 이 두 모듈이 만나는 지점에 인터페이스를 정의한다.
3. 구현에 의존하기보다 정의한 인터페이스에 의존하도록 코드를 작성한다.

### 적용 사례

위에서도 보았던 간단한 클래스 다이어그램이다.

<center><img alt="그림1" src="./images/SOLID-02.png"/></center>

별 문제가 없어 보인다. SRP 원리를 적용하여 `Guitar`에서 변경이 예쌍되는 부분을 뽑아 `GuitarSpec`이라는 새로운 클래스를 만들어 변화요소들을 하나로 모았다. 변화를 국소화 시킨것이다.

하지만 여기에서도 변경이 발생할 수 있다. 예를 들어 아래와 같이 `Guitar` 외에 바이올린이나 첼로, 비올라, 만돌린과 같은 다른 악기들도 다루어야 한다면 어떻게 될까? 그 해결책으로 만일 아래 그림과 같이 일일이 매번 새로운 악기들과 요소들을 만들어 간다면 어떻게 될까? 우리는 항상 변화를 염두해 두고 있어야 한다.

<center><img alt="그림1" src="./images/SOLID-03.png"/></center>

```java
//기타
class Guitar {
   private String serialNumber;
   private GuitarSpec spec;

   public Guitar(String serialNumber, GuitarSpec spec) {
      this.serialNumber = serialNumber;
      this.spec = spec;
   }
}

class GuitarSpec {
   ...
}

//바이올린
class Violin {
   private String serialNumber;
   private ViolinSpec spec;

   public Violon(String serialNumber, ViolinSpec spec) {
      this.serialNumber = serialNumber;
      this.spec = spec;
   }
}

class ViolinSpec {
   ...
}

//이외의 여러 악기들
...
```

변화를 막을 수 있는 사람은 아무도 없다. 다만 변화에 적절히 대응할 뿐이다. 위와 같이 변화에 몸을 맡겨버린다면 엄청난 재앙이 두고두고 여러 개발자들을 괴롭힐 것이다. 그러면 앞서 설명한 OCP 원리를 이용하여 위와 같은 변화에 대응해 보도록 하자.

먼저, `Guitar`와 추가 될 다른 악기들을 추상화하는 작업이 필요하다. 여기서는 추가될 악기들의 공통 속성을 모두 담을 수 있는 `StringInstrument`라는 인터페이스를 생성하겠다. 앞으로는 `StringInstrumnet`가 이들을 대표하게 될 것이다. 아래 그림은 OCP 원리가 적용된 다이어그램과 소스를 나타낸다.

<center><img alt="그림1" src="./images/SOLID-04.png"/></center>

```java
//기타
class Guitar extends StringInstrument {
   private String serialNumber;
   private GuitarSpec spec;

   public Guitar(String serialNumber, GuitarSpec spec) {
      this.serialNumber = serialNumber;
      this.spec = spec;
   }
}

class GuitarSpec extends StringInstrumentSpec {
   ...
}

//바이올린
class Violin extends StringInstrument {
   private String serialNumber;
   private ViolinSpec spec;

   public Violon(String serialNumber, ViolinSpec spec) {
      this.serialNumber = serialNumber;
      this.spec = spec;
   }
}

class ViolinSpec extends StringInstrumentSpec {
   ...
}

//이외의 여러 악기들
...
```

새로운 악기가 추가되면서 변경이 발생하는 부분을 추상화하여 분리하였음을 확인할 수 있다. 이렇게 해서 코드의 수정을 최소화하여 결합도는 줄이고 응집도는 높이는 효과를 볼 수 있다.

### 적용 이슈

확장되는 것과 변경되지 않는 모듈을 분리하는 과정에서 크기 조절에 실패하면 오히려 관계가 더 복잡해질 수 있다. 설계자의 좋은 자질 중 하나는 이런 크기 조절과 같은 갈등 상황을 잘 포착하여 (아깝지만) 비장한 결단을 내릴 줄 아는 능력에 있다.

인터페이스는 간으하면 변경되어서는 안된다. 따라서 인터페이스를 정의할 때 여러 경우의 수에 대한 고려와 예측이 필요하다. 물론 과도한 예측은 불필요한 작업을 만들고, 보통 이 불필요한 작업의 양은 상당히 크기 마련이다. 따라서 설계자는 적절한 수준의 예측 능력이 필요한데, 설계자에게 필요한 또 하나의 자질을 예지력이다.

인터페이스 설계에서 적당한 추상화 레벨을 선택해야 한다. 우리는 추상화라는 개념에 '구체적이지 않은' 정도의 의미로 약간 느슨한 개념을 갖고 있다. 그래디 부치(Grady Booch)에 의하면 '추상화란 다른 모든 종류의 객체로부터 식별될 수 있는 객체의 본질적인 특징'이라고 정의하고 있다. 즉, 이 '행위'에 대한 본질적인 정의를 통해 인터페이스를 식별해야 한다.

## 참고 자료

- [객체지향 개발 5대 원리: SOLID](https://www.nextree.co.kr/p6960/)
- [2018년 하반기 'ㅈ' 기업 개발자 면접 후기](https://gurumee92.tistory.com/95)
- [[Java] 객체지향 설계 5원칙 - SOLID란 무엇일까?](https://devlog-wjdrbs96.tistory.com/380)
- [객체지향 5원칙 : SOLID](https://jaeyeong951.medium.com/%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-5%EC%9B%90%EC%B9%99-solid-ac7d4d660f4d)
