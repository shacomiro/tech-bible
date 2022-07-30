# JVM

1. [JVM(Java Virtual Machine)이란?](#jvmjava-virtual-machine이란)
2. [자바 가상 머신(JVM)의 동작 방식](#자바-가상-머신jvm의-동작-방식)
   1. [바이트 코드를 읽는 방식](#바이트-코드를-읽는-방식)
   2. [JIT(Just In Time) 컴파일러](#jitjust-in-time-컴파일러)
3. [JVM의 구조](#jvm의-구조)
   1. [클래스 로더(Class Loader)](#클래스-로더class-loader)
   2. [실행 엔진(Execution Engine)](#실행-엔진execution-engine)
   3. [가비지 컬렉터(Garbage Collector)](#가비지-컬렉터garbage-collector)
   4. [런타임 데이터 영역(Runtime Data Area)](#런타임-데이터-영역runtime-data-area)
      1. [메서드 영역(Method Area)](#메서드-영역method-area)
      2. [힙 영역(Heap Area)](#힙-영역heap-area)
      3. [스택 영역(Stack Area)](#스택-영역stack-area)
      4. [PC 레지스터(PC Register)](#pc-레지스터pc-register)
      5. [네이티브 메서드 스택(Native Mathod Stack)](#네이티브-메서드-스택native-mathod-stack)
4. [참고 자료](#참고-자료)

## JVM(Java Virtual Machine)이란?

자바 가성 머신(Java Virtual Machine, JVM)은 **자바 프로그램 실행 환경을 만들어주는 소프트웨어**이다. 자바 코드를 컴파일하여 `.class` 바이트 코드로 만들면 이 코드가 자바 가상 머신 환경에서 실행된다.

JVM은 자바 실행 환경 JRE(Java Runtime Environment)에 포함되어 있다. 현재 사용하는 컴퓨터의 운영체제에 맞는 자바 실행환경(JRE)가 설치되어 있다면 자바 가상 머신이 설치되어 있다는 뜻이다.

JVM을 사용함으로써 얻는 가장 큰 이점은 **하나의 바이트 코드(`.class`)로 모든 플랫폼에서 동작하도록 할 수 있다**는 것이다.

<table>
  <tr>
    <td><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbSyyF2%2FbtruTDnDSKz%2F73EXAY7aiTWRzHKlM2OFpK%2Fimg.png"/></td>
    <td><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F56cSc%2FbtruTEtjRXJ%2Fr1JNTkEuEeY8cSKtqcXCRK%2Fimg.png"/></td>
  </tr>
  <tr>
    <td style="text-align:center">C언어의 경우</td>
    <td style="text-align:center">Java의 경우</td>
  </tr>
</table>

C언어로 작성된 코드(`Test.c`)의 경우 윈도우 컴파일러를 사용해서 컴파일하면 `Test.exe`가 생성된다. 윈도우 컴파일러로 컴파일된 `Test.exe` 파일은 윈도우에서만 실행되는 실행 파일로, 리눅스 운영체제에서는 실행할 수 없다. **즉, C/C++에서는 컴파일 플랫폼과 타겟 플랫폼이 다를 경우에는 프로그램이 동작하지 않는다.** 만약 이 `Test.exe` 파일을 리눅스 운영체제에서 실행하려면 리눅스 환경을 타켓으로 크로스 컴파일하여 리눅스 운영체제에 맞는 실행 파일을 새로 만들어야 한다.

반면 Java 언어로 작성된 `Test.java`는 컴파일하면 `Test.class` 파일이 생성된다. 그리고 **이렇게 생성된 바이트 코드는 각자의 플랫폼에 설치되어 있는 자바 가상 머신(JVM)이 운영체제에 맞는 실행 파일로 바꿔준다.** 즉, Java에서는 C언어와는 달리 JVM을 사용하기 때문에 각자의 플랫폼에 맞게끔 컴파일을 따로 수행할 필요가 없다. 하나의 바이트 코드로 JVM이 설치되어 있는 모든 플랫폼에서 동작이 가능하다는 이야기다.

> **※ Java는 플랫폼에 종속적이지 않지만 JVM은 플랫폼에 종속적이다.**
>
> 이렇게 Java는 컴파일된 바이트 코드로 어떤 JVM에서도 동작시킬 수 있기 때문에 플랫폼에 의존적이지 않다. 하지만 반대로 자바 가상 머신(JVM)은 플랫폼에 의존적이다. 즉, 리눅스의 JVM과 윈도우의 JVM은 서로 다르다.
>
> 자바로 작성된 모든 프로그램은 자바 가장 머신에서만 실행될 수 있으므로, 자바 프로그램을 실행하기 위해서는 반드시 자바 가상 머신이 설치되어 있어야 한다. 따라서 오라클은 대부분의 주요 운영체제뿐만 아니라 웹 브라우저, 스마트폰, 가전기기 등에서도 자바 가상 머신을 손쉽게 설치할 수 있도록 지원하고 있다.

## 자바 가상 머신(JVM)의 동작 방식

![coding-factory.tistory.com/828-JVM_PROCESS](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcQRqku%2Fbtru0vJ6Ixx%2F9qCTW7ChXc80fGfQUrT4B0%2Fimg.png)

1. 자바로 개발된 프로그램을 실행하면 JVM은 OS로부터 메모리를 할당한다.
2. 자바 컴파일러(javac)가 자바 소스 코드(`.java`)를 자바 바이트 코드(`.class`)로 컴파일한다.
3. Class Loader를 통해 JVM Runtime Data Area로 로딩한다.
4. Runtime Data Area에 로딩 된 `.class`들은 Excution Engine을 통해 해석된다.
5. 해석된 바이트 코드는 Runtime Data Area의 각 영역에 배치되어 수행하며, 이 과정에서 Execution Engine에 의해 GC의 작동과 스레드 동기화가 이루어진다.

### 바이트 코드를 읽는 방식

JVM은 바이트 코드를 명령어 단위로 읽어서 해석하는데, **인터프리터(Interpreter) 방식과 JIT 컴파일 방식 두 가지를 혼합하여 사용**한다.

먼저 인터프리터(Interpreter) 방식은 바이트 코드를 한 줄씩 해석, 실행하는 방식이다. 초기 방식으로, 속도가 느리다는 단점이 있다.

느린 속도를 보완하기 위해 등장한 것이 JIT(Just In Time) 컴파일 방식이다. 바이트 코드를 JIT 컴파일러를 이용해 프로그램을 실제 실행하는 시점(바이트 코드를 실행하는 시점)에 각 OS에 맞는 네이티브 코드(Natvie Code)로 변환하여 실행 속도를 개선한 것이다. 하지만 바이트 코드를 네이티브 코드로 변환하는 데에도 비용이 소요되므로, JVM은 모든 코드를 JIT 컴파일러 방식으로 실행하지 않고, 인터프리터 방식을 사용하다가 일정 기준이 넘어가면 JIT 컴파일 방식으로 명령어를 실행한다.

### JIT(Just In Time) 컴파일러

![coding-factory.tistory.com/828-JIT_COMPILER](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcUaNHZ%2Fbtrvl7QE2QV%2Fxgly7fewMJsJ1CkP7om0Ek%2Fimg.png)

기존의 자바는 인터프리터 방식으로 명령어를 하나씩 실행하게끔 이루어져 있어 실행 속도가 느렸다. 그러나 하드웨어가 발전하면서 자바 컴파일러도 JIT 컴파일러 방식으로 개선되어 속도 측면에서 상당한 개선을 이루었다.

JVM은 JIT(Just In Time) 컴파일러라고 한다. 또한, JIT 컴파일러는 같은 코드를 매번 해석하지 않고, 실행할 때 컴파일을 하면서 해당 코드를 캐싱한다. 이후에는 바뀐 부분만 컴파일하고, 나머지는 캐싱된 코드를 사용한다. 이렇게 JIT 컴파일러는 운영체제에 맞게 바이트 실행 코드로 한 번에 변환하여 실행하기 때문에 이전의 자바 해석기(Java Interpreter) 방식보다 성능이 10~20배 정도 더 좋다.

## JVM의 구조

### 클래스 로더(Class Loader)

![coding-factory.tistory.com/828-CLASS_LOADER](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FIXgMz%2FbtrvglUCR65%2FMaffyNzf1y1oV8BTBKADKk%2Fimg.png)

자바는 동적으로 클래스를 읽어오므로, 프로그램이 실행중인 런타임에서야 모든 코드가 자바 가상 머신과 연결된다. 이렇게 동적으로 클래스를 로딩해주는 역할을 하는 것이 바로 클래스 로더(Class Loader)이다.

자바에서 소스를 작성하면 `.java` 파일이 생성되고 `.java` 소스를 컴파일러가 컴파일하면 `.class` 파일이 생성되는데 클래스 로더는 `.class` 파일을 묶어서 JVM이 운영체제로부터 할당받은 메모리 영역인 Runtime Data Area로 적재한다.

### 실행 엔진(Execution Engine)

앞서 설명한 클래스 로더에 의해 JVM으로 로드된 `.class` 파일(바이트 코드)들은 Runtime Data Area의 Method Area에 배치되는데, 배치된 이후에 JVM은 Method Area의 바이트 코드를 실행 엔진(Execution Engine)에 제공하여 정의된 내용대로 바이트 코드를 실행한다.

이 때, 로드된 바이트 코드를 실행하는 런타임 모듈이 실행 엔진(Execution Engine)이다. 실행 엔진은 바이트 코드를 명령어 단위로 읽어서 실행한다.

### 가비지 컬렉터(Garbage Collector)

![coding-factory.tistory.com/828-CARBAGE_COLLECTOR](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FesrrQe%2FbtrvdvpVX3u%2FTuWioGNha4g8vgGDKD3tQk%2Fimg.png)

자바 가상 머신은 가비지 컬렉터(Garbage Collector)를 이용하여 더이상 사용하지 않는 메모리를 자동으로 회수한다. 따라서 개발자가 별도로 메모리 관리를 수행하지 않아도 되므로, 더욱 손쉽게 프로그래밍을 할 수 있다.

Heap 메모리 영역에 생성(적재)된 객체들 중에 참조되지 않은 객체들을 탐색 후 제거하는 역할을 하며, 해당 역할을 수행하는 시점이 정확히 언제인지는 알 수 없다. GC 역할을 수행하는 스레드를 제외한 나머지 모든 스레드들은 일시정지 상태(Stop The World)가 된다.

> 가비지 컬렉터에 관련된 자세한 내용은 [가비지 컬렉션(GC)](./Garbage-Collection.md) 문서를 참고

### 런타임 데이터 영역(Runtime Data Area)

![coding-factory.tistory.com/828-RUNTIME_DATA_AREA](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbZR97z%2FbtrvtdBl1Md%2FLbUk2NVlgDmsKMcBiQ9f4K%2Fimg.png)

런타임 데이터 영역은 JVM의 메모리 영역으로, 자바 애플리케이션을 실행할 때 사용되는 데이터를 적재하는 영역이다.

- **모든 스레드가 공유해서 사용(GC의 대상)**
  - 힙 영역(Heap Area)
  - 메서드 영역(Method Area)
- **스레드(Thred)마다 하나씩 생성**
  - 스택 영역(Stack Area)
  - PC 레지스터(PC Register)
  - 네이티브 메서드 스택(Native Method Stack)

#### 메서드 영역(Method Area)

클래스 멤버 변수의 이름, 데이터 타입, 접근 제어자 정보와 같은 각종 필드 정보들과 메서드 정보, 데이터 타입 정보, Constant Pool, static 변수, final class 등이 생성되는 영역이다.

#### 힙 영역(Heap Area)

![coding-factory.tistory.com/828-HEAP_AREA](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcmeYY6%2FbtrveK8fwH2%2FNwdYae5gEQHih3lyV83as0%2Fimg.png)

1. `new` 키워드로 생성된 객체와 배열이 생성되는 영역이다.
2. 주기적으로 GC가 제거하는 영역이다.

힙 영역은 효율적인 GC을 위해 위 그림과 같이 크게 3가지 영역으로 나뉜다.

Young Generation 영역은 자바 객체가 생성되자마자 저장되고, 생성된 지 얼마 되지 않은 객체가 저장되는 공간이다. 힙 영역에 객체가 생성되면 최초로 Eden 영역에 할당된다. 그리고 이 영역에 데이터가 어느정도 쌓이게 되면 참조 정도에 따라 Survivor의 빈 공간이로 이동되거나 회수된다.

Young Generation(Eden + Survivor) 영역이 차게 되면 참조 정도에 따라 Old Generation 영역으로 이동되거나 회수된다. 이처럼 Young Generation에서의 GC를 Minor GC라고 한다. Old 영역에 할당된 메모리가 허용치를 넘기면 Old 영역에 있는 모든 객체들을 검사하여 참조되지 않는 객체들을 한번에 제거하는 GC가 수행된다. 오랜 시간이 걸리는 작업이고, 이 때 GC를 실행하는 스레드를 제외한 모든 스레드는 작업을 멈추게 된다. 이를 Stop The World라 한다. 이렇게 Stop The World가 발생하여 Old 영역의 메모리를 회수하는 GC를 Major GC라고 한다.

#### 스택 영역(Stack Area)

지역 변수, 매개변수(파라미터), 리턴 값, 연산에 사용되는 임시 값 등이 생성되는 영역이다.

#### PC 레지스터(PC Register)

스레드가 생성될 때마다 생성되는 영역으로 프로그램 카운터, 즉 현재 스레드가 실행되는 부분의 주소와 명령을 저장하고 있는 영역이다.

#### 네이티브 메서드 스택(Native Mathod Stack)

1. 자바 이외의 언어(C, C++, 어셈블리 등)로 작성된 네이티브 코드를 실행할 때 사용되는 메모리 영역으로 일반적인 C 스택을 사용한다.
2. 보통 C/C++ 등의 코드를 수행하기 위한 스택(JNI)을 말하며 자바 컴파일러에 의해 변환된 자바 바이트 코드를 읽고 해석하는 역할을 하는 것이 자바 인터프리터(Interpreter)이다.

## 참고 자료

- [[Java] 자바 가상머신 JVM(Java Virtual Machine) 총정리](https://coding-factory.tistory.com/827)
- [[Java] 자바 JVM 내부 구조와 메모리 구조에 대하여](https://coding-factory.tistory.com/828)
