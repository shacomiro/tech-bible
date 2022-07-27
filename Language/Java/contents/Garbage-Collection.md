# 가비지 컬렉션

1. [가비지 컬렉션(Garbage Collection, GC)이란?](#가비지-컬렉션garbage-collection-gc이란)
2. [단점](#단점)
3. [가비지 컬렉션의 대상이 되는 객체들](#가비지-컬렉션의-대상이-되는-객체들)
4. [Minor GC와 Major CG](#minor-gc와-major-cg)
5. [가비지 컬렉션의 동작 방식](#가비지-컬렉션의-동작-방식)
   1. [Minor GC의 동작 방식](#minor-gc의-동작-방식)
   2. [Major GC의 동작 방식](#major-gc의-동작-방식)
   3. [가비지 컬렉션 요약](#가비지-컬렉션-요약)
6. [가비지 컬렉션 알고리즘](#가비지-컬렉션-알고리즘)
   1. [Serial GC](#serial-gc)
   2. [Parallel GC](#parallel-gc)
   3. [Parallel Old GC](#parallel-old-gc)
   4. [CMS(Concurrent Mark Sweep) GC](#cmsconcurrent-mark-sweep-gc)
   5. [G1(Garbage First) GC](#g1garbage-first-gc)
      1. [Minor GC](#minor-gc)
      2. [Major GC](#major-gc)
7. [참고 자료](#참고-자료)

## 가비지 컬렉션(Garbage Collection, GC)이란?

가비지 컬렉션은 자바의 **메모리 관리 기법** 중 하나로,JVM의 힙(Heap) 영역에서 **동적으로 할당했던 메모리 영역 중 필요 없게 된 메모리 영역을 주기적으로 삭제하는 프로세스**를 말한다.

C나 C++에서는 이러한 가비지 컬렉션이 없어 `malloc()`, `free()` 함수 등을 통해 개발자가 수동으로 메모리를 할당, 해제를 진행해야 하는 반면 Java는 **JVM에 탑재된 가비지 컬렉터가 메모리 관리를 대행**해주기 때문에 개발자 입장에서 메모리 누수(Memory Leak) 문제에 대해 완벽하게 관리하지 않고 개발에 집중할 수 있다는 장점이 있다. 물론 Java에서도 `System.gc()`를 이용해 수동 호출이 가능하지만, 소프트웨어 성능에 큰 영향을 미치므로 사용하는 것은 바람직하지 않다.

## 단점

그러나 Java의 가비지 컬렉션은 장점 뿐만 아니라 단점 역시 존재하는데, 대표적으로 다음과 같다.

1. 개발자가 **메모리가 해제되는 정확한 시점을 알 수 없다.**
2. 가비지 컬렉션(GC)이 동작하는 동안에는 다른 동작을 멈추기 때문에 **오버헤드가 발생**한다.
   - 경우에 따라 문제가 되는 단점으로, GC가 너무 자주 실행되면 소프트웨어 성능 하락의 원인이 될 수 있다.
   - 일시적인 지연에도 민감한 실시간 시스템에서는 GC의 사용이 적합하지 않을 수 있다.

## 가비지 컬렉션의 대상이 되는 객체들

![coding-factory.tistory.com/829-GC_대상](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbW5c5r%2FbtrvAb4nrdH%2FlYHuQZya8ECvEndRkQchjk%2Fimg.png)

객체들은 실실적으로 힙 영역(Heap Area)에서 생성되고 메소드 영역(Method Area)이나 스택 영역(Stack Area) 등 루트 영역에서는 힙 영역에 생성된 객체의 주소만 참조하는 형식으로 구성된다. 하지만 이렇게 생성된 힙 영역의 객체들이 메서드가 종료되는 등의 이벤트들로 인하여 힙 영역 객체의 메모리 주소를 가지고 있는 참조 변수가 삭제되는 현상이 발생하면 위의 그림에서의 빨간색 객체와 같이 힙 영역에서 어디에서도 참조되지 않는 객체들이 발생하게 된다. 이러한 객체들을 Unreachable하다고 하며, 주기적으로 가비지 컬렉터가 제거해준다.

> Reachable : 객체가 참조되고 있는 상태  
> Unreachable : 객체가 참조되고 있지 않은 상태 (GC의 대상이 됨)

## Minor GC와 Major CG

JVM의 힙 영역(Heap Area)은 처음 설계될 때 다음의 두 가지를 전제(Weak Generational Hypothesis)로 설계되었다.

- 대부분의 객체는 금방 접근 불가능한 상태(Unreachable)가 된다.
- 오래된 객체에서 새로운 객체로의 참조는 아주 적게 존재한다.

즉, **객체는 대부분 일회성이며, 메모리에 오랫동안 남아있는 경우는 드물다**는 것이다. 그렇기 때문에 객체의 생존 기간에 따라 물리적인 힙 영역을 나누게 되었고 Young Generation, Old Generation의 두 가지 영역으로 설계되었다. 초기에는 Perm 영역이 존재하였으나 Java8부터 제거되었다.

![mangkyu.tistory.com/118-GC_영역_및_흐름](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fva8qQ%2FbtqUSpSocbS%2FkxTvtnmrdhf4bnVPXth0UK%2Fimg.png)

- **Young 영역(Young Generation)**
  - 새롭게 생성된 객체가 할당(Allocation)되는 영역.
  - 대부분의 객체가 금방 Unreachable 상태가 되기 때문에, 많은 객체가 Young 영역에서 생성되었다가 사라진다.
  - Young 영역에 대한 가비지 컬렉션(Garbage Colection)을 Minor GC라고 부른다.
- **Old 영역(Old Generation)**
  - Young 영역에서 Reachable 상태를 유지하여 살아남은 객체가 복사되는 영역.
  - Young 영역보다 크게 할당되며, 영역의 크기가 큰 만큼 가비지는 적게 발생한다.
  - Old 영역에 대한 가비지 컬렉션(Garbage Collection)을 Major GC 또는 Full GC라고 부른다.

Old 영역이 Young 영역보다 크게 할당되는 이유는 Young 영역의 수명이 짧은 객체들은 큰 공간을 필요로 하지 않으며 큰 객체들은 Young 영역이 아니라 바로 Old 영역에 할당되기 때문이다.

예외적인 상황으로 Old 영역에 있는 객체가 Young 영역의 객체를 참조하는 경우도 존재할 수 있다. 이러한 경우를 대비하여 Old 영역에는 512 bytes의 덩어리(Chunk)로 되어 있는 카드 테이블(Card Table)이 존재한다.

![mangkyu.tistory.com/118-힙_영역](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FFOLU3%2FbtqUOBF35cJ%2FBMKuD1iqfq6R0lAqMlfkC0%2Fimg.png)

카드 테이블에는 **Old 영역에 있는 객체가 Young 여역의 객체를 참조할 때마다 그에 대한 정보가 표시**된다. 카드 테이블이 도입된 이유는 간단하다. Young 영역에서 가비지 컬렉션(Minor GC)이 실행될 때 Old 영역에 존재하는 모든 객체들을 검사하여 참조되지 않는 Young 영역의 객체를 식별하는 것이 비효율적이기 떄문이다. 그렇기 때문에 Young 영역에서 가비지 컬렉션이 진행될 때 **카드 테이블만 조회하여 GC의 대상인지를 식별**할 수 있도록 하고 있다.

## 가비지 컬렉션의 동작 방식

Young 영역과 Old 영역은 서로 다른 메모리 구조로 되어 있기 때문에, 세부적인 동작 방식은 서로 다르다. 하지만 기본적으로 가비지 컬렉션이 실행된다고 하면 다음의 두 가지 공통적인 단계를 따르게 된다.

1. **Stop The World**
   - Stop The World는 가비지 컬렉션을 실행하기 위해 **JVM이 애플리케이션의 실행을 멈추는 작업**이다. GC가 실행될 때는 **GC를 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단**되고, GC가 완료되면 작업이 재개된다. 당연히 모든 쓰레드들의 작업이 중단되면 애플리케이션이 멈추기 때문에, GC의 성능 개선을 위해 튜닝을 한다고 하면 보통 stop-the-world의 시간을 단축하는 작업을 하는 것이다. 또한 JVM에서도 이러한 문제를 해결하기 위해 다양한 실행 옵션을 제공하고 있다.
2. **Mark and Sweep**
   - **Mark** : 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업.
   - **Sweep** : Mark 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업.
   - Stop Thw World를 통해 모든 작업을 중단시키면, GC는 스택의 모든 변수 또는 Reachable 객체를 스캔하면서 각각이 어떤 객체를 참고하고 있는지를 탐색하게 된다. 그리고 **사용되고 있는 메모리를 식별**하는데, 이러한 과정을 Mark라고 한다. 이후에 **Mark가 되지 않은 객체들을 메모리에서 제거**하는데, 이러한 과정을 Sweep라고 한다.

### Minor GC의 동작 방식

Minor GC를 정확히 이해하려면 Young 영역의 구조에 대해 이해해야 한다. **Young 영역은 1개의 Eden 영역과 2개의 Survivor 영역**, 총 3개로 나뉘어진다.

- **Eden 영역** : 새로 생성된 객체가 할당(Allocation)되는 영역.
- **Survivor 영역** : 최소 1번 이상의 GC에서 살아남은 객체가 존재하는 영역.

객체가 새롭게 생성되면 Young 영역 중에서도 Eden 영역에 할당(Alloaction)된다. 그리고 Eden 영역이 꽉 차면 Minor GC가 발생하는데, 사용되지 않는 메모리는 해제되고 여전히 Eden 영역에 존재하는(사용되는) 객체는 Survivor 영역으로 옮겨진다. Survivor 영역은 총 2개이지만 반드시 1개의 영역에만 데이터가 존재해야 하는데, Young 영역의 동작 순서를 살펴보면 다음과 같다.

1. 새로 생성된 객체가 **Eden 영역에 할당**된다.
2. 객체가 계속 생성되어 Eden 영역이 꽉 차게 되고 **Minor GC가 실행**된다.
   1. Eden 영역에서 **사용되지 않는 객체의 메모리가 해제**된다.
   2. Eden 영역에서 **살아남은 객체는 1개의 Survivor 영역으로 이동**된다.
3. 1~2번 과정이 반복되다가 Survivor 영역이 가득 차게 되면 **Survivor 영역의 살아남은 객체들을 다른 Survivor 영역으로 이동**시킨다. (1개의 Survivor 영역은 반드시 빈 상태가 된다.)
4. 이러한 과정을 반복하여 **계속해서 살아남은 객체는 Old 영역으로 이동**(Promotion)된다.

객체의 생존 쵯수를 세기 위해 Minor GC에서 객체가 살아남은 횟수를 의미하는 age를 Object Header에 기록한다. 그리고 Minor GC 때 OBejct Header에 기록된 age를 보고 Promotion 여부를 결정한다.

또한 Survivor 영역 중 1개는 반드시 사용되어야 한다. 만약 두 Survivor 영역에 모두 데이터가 존재하거나, 모두 사용량이 0이라면 현재 시스템이 정상적인 상황이 아니라는 것을 의미한다.

이러한 진행 과정을 그림으로 살펴보면 다음과 같다.

![mangkyu.tistory.com/118-Minor_GC_동작_방식](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCyho2%2FbtqURvZRql6%2F4a7u6mMGofkpuURKQz0RT1%2Fimg.png)

HotSpot JVM에서는 Eden 영역을 객체에 빠르게 할당(Allocation)하기 위해 bump the pointer와 TLABs(Thread-Local Allcation Buffers)라는 기술을 사용한다. bump the pointer란 **Eden 영역에 마지막으로 할당된 객체의 주소를 캐싱해두는 것**이다. bump the pointer를 통해 새로운 객체를 위해 유효한 메모리를 탐색할 필요 없이 마지막 주소의 다음 주소를 사용하게 함으로써 속도를 높이고 있다. 이를 통해 새로운 객체를 할당할 때 객체의 크기가 Eden 영역에 적합한지만 판별하면 되므로 빠르게 메모리를 할당할 수 있다.

싱글 스레드 환경이라면 문제가 없지만 멀티 스레드 환경이라면 객체를 Eden 영역에 할당할 때 락(Lock)을 걸어 동기화를 해주어야 한다. 멀티 스레드 환경에서의 성능 문제를 해결하기 위해 HotSpot JVM은 추가로 TLABs(Thread-Lock Allocation Buffers)라는 기술을 도입했다. TLABs란 **각각의 스레드마다 Eden 영역에 객체를 할당하기 위한 주소를 부여함으로써 동기화 작업 없이 빠르게 메모리를 할당하도록 하는 기술**이다. 각각의 스레드는 자신이 갖는 주소에만 객체를 할당함으로써 동기화 없이 bump the pointer를 통해 빠르게 객체를 할당한다.

### Major GC의 동작 방식

Young 영역에서 오래 살아남은 객체는 Old 영역으로 Promotion됨을 확인할 수 있다. 그리고 Major GC는 **객체들이 계속 Promotion되어 Old 영역의 메모리가 부족해지면 발생**한다. Young 영역은 일반적으로 Old 영역보다 크기가 작기 때문에 GC가 보통 0.5~1초 사이에 끝난다. 그렇기 때문에 Minor GC는 애플리케이션에 큰 영향을 주지 않는다. 하지만 Old 영역은 Young 영역보다 크며 Young 영역을 참조할 수도 있다. 그렇기 떄문에 Major GC는 일반적으로 Minor GC보다 시간이 많이 소요되며, 10배 이상의 시간을 사용한다.

### 가비지 컬렉션 요약

여태까지의 내용을 요약하면 다음 표와 같다.

|  GC 종류  | Minor GC               | Major GC              |
| :-------: | ---------------------- | --------------------- |
|   대상    | Young Generation       | Old Generation        |
| 실행 시점 | Eden 영역이 꽉 찬 경우 | Old 영역이 꽉 찬 경우 |
| 실행 속도 | 빠르다                 | 느리다                |

## 가비지 컬렉션 알고리즘

JVM이 메모리를 자동으로 관리해주는 것은 개발자의 입장에서 상당한 매리트이다. 하지만 문제는 GC를 수행하기 위해 Stop The World를 수행할 때 애플리케이션이 중지된다는 것이다. 힙(Heap)의 사이즈가 커지면서 애플리케이션의 지연(Suspend) 현상이 두드러지게 되었고, 이를 막기 위해 다양한 가비지 컬렉션 알고리즘을 지원하고 있다.

### Serial GC

Serial GC의 Young 영역은 [앞서 설명한 알고리즘(Mark Sweep)](#가비지-컬렉션의-동작-방식)대로 수행된다. 하지만 Old 영역에서는 **Mark Sweep Compact 알고리즘**이 사용되는데, 기존의 Mark Sweep에 Compact라는 작업이 추가된 것이다. Compact는 힙 영역을 정리하기 위한 단계로 유효한 객체들이 연속되게 쌓이도록 힙의 가장 앞 부분부터 채워서 객체가 존재하는 부분과 객체가 존재하지 않는 부분으로 나누는 것이다.

```bash
java -XX:+UserSerialGC -jar Application.java
```

Serial GC는 서버의 CPU 코어가 1개일 때 사용하기 위해 개발되었으며, 모든 가비지 컬렉션 작업을 처리하기 위해 1개의 스레드만을 이용한다. 그렇기 때문에 CPU의 코어가 여러 개인 운영 서버에서 Serial GC를 사용하는 것은 반드시 피해야 한다.

### Parallel GC

Parallel GC는 Throughput GC로도 알려져 있으며, 기본적인 처리 과정은 Serial GC와 동일하다. 하지만 Parallel GC는 **여러 개의 스레드를 통해 병렬(Parallel)적으로 GC를 수행**함으로써 GC의 오버헤드를 상당히 줄여준다. Parallel GC는 멀티 프로세서 또는 멀티 스레드 장치에서 중규모 및 대규모의 데이터를 처리하는 애플리케이션을 위해 고안되었으며, 옵션을 통해 애플리케이션의 최대 지연 시간 또는 GC를 수행할 스레드의 갯수 등을 설정할 수 있다.

```bash
java -XX:+UserParallelGC - jar Application.java

//사용할 스레드의 갯수
-XX:ParallelGCThreads=THREAD_NUM

//최대 지연 시간
-XX:MaxGCPauseMillis=DELAY_TIME
```

Parallel GC가 GC의 오버헤드를 상당히 줄여주었고, Java8까지 기본 가비지 컬렉터(Default Garbage Collector)로 사용되었다. 그럼에도 불구하고 애플리케이션이 멈추는 것을 피할 수 없었고, 이를 개선하기 위해서 다른 알고리즘이 추가로 등장하게 된다.

## Parallel Old GC

Parallel Old GC는 JDK5 update6부터 제공한 GC이며, 앞서 설명한 Parallel GC와 비교하면 Old 영역의 GC 알고리즘만 다르다. Parallel Old GC에서는 Mark Sweep Compact가 아닌 Mark Summary Compaction이 사용되는데, Summary 단계에서는 앞서 GC를 수행한 영역에 대해서 별도로 살아있는 객체를 식별한다는 점에서 다르며 조금 더 복잡하다.

## CMS(Concurrent Mark Sweep) GC

CMS(Concurrent Mark Sweep) GC는 Parallel GC와 마찬가지로 여러 개의 스레드를 이용한다. 하지만 기존의 Serial GC나 Parallel GC와는 달리 **Mark Sweep 알고리즘을 동시적(Concurrent)으로 수행**한다.

![mangkyu.tistory.com/119-CMS_GC](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FexiT4S%2FbtqURuUWjwY%2FWsh133bXvARXfGMunpvSh1%2Fimg.png)

이러한 CMS GC는 애플리케이션의 지연 시간을 최소화하기 위해 고안되었으며, 애플리케이션이 구동중일 때 프로세서의 자원을 공유하여 이용 가능해야 한다. CMS GC가 수행될 때는 자원이 GC를 위해서도 사용되므로 응답이 느려질 수는 있지만 응답이 멈추지는 않게 된다.

하지만 이러한 CMS GC는 다른 GC 방식보다 더 많은 메모리와 CPU를 필요로 하며, Compaction 단계를 수행하지 않는다는 단점이 있다. 이 때문에 시스템이 장기적으로 운영되다가 조각난 메모리들이 많아 Compaction 단계가 수행되면 오히려 Stop The World 시간이 길어지는 문제가 발생할 수 있다.

```bash
//deprecated in java9 and finally dropped in java14
java -XX:+UseConcMArkSweepGC - jar Application.java
```

만약 GC가 수행되면서 98% 이상의 시간이 CMS GC에 소요되고, 2% 이하의 시간이 Heap의 정리에 사용된다면 CMS GC에 의해 OutOfMemoryError가 발생할 것이다. 물론 이를 비활성화하는 옵션이 있으나, CMS GC는 Java9 버전부터 deprecated 되었고 결국 Java14에서는 사용이 중지되었기 때문에 굳이 알아볼 필요는 없을 것으로 보인다.

## G1(Garbage First) GC

G1(Garbage First) GC는 장기적으로 많은 문제를 일으킬 수 있는 CMS GC를 대체하기 위해 개발되었고, Java7부터 지원되기 시작했다.

기존의 GC 알고리즘에서는 힙 영역을 물리적으로 Young 영역(Eden 영역, 2개의 Survivor 영역)과 Old 영역으로 구분하여 사용했다. G1 GC에서는 Eden 영역에 할당하고, Survivor로 복사하는 등의 과정을 사용하지만 물리적으로 메모리 공간을 나누지 않는다. 대신 지역(Region)이라는 개념을 새로 도입하여 힙을 균등하게 여러 개의 지역으로 나누고, 각 지역을 역할과 함께 논리적으로 구분하여 객체를 할당한다.

![mangkyu.tistory.com/119-G1_GC](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdHxPiT%2FbtqU0xWGaDI%2FwriFcFKPHND5pTAsyn47X1%2Fimg.png)

G1 GC에서는 Eden, Survivor, Old 역할에 더해 Humonogous와 Available/Unused라는 두 가지 역할이 추가된다. Humonogous는 지역의 크기가 50%를 초과하는 객체를 저장하는 지역을 의미하며, Available/Unused는 사용되지 않은 지역을 의미한다.

G1 GC의 핵심은 **힙(Heap)을 동일한 크기의 지역(Region)으로 나누고, 가비지가 많은 지역에 대해 우선적으로 GC를 수행하는 것**이다. 그리고 G1 GC도 다른 가비지 컬렉션과 마찬가지로 두 가지 GC(Minor GC, Major GC)로 나뉘어 수행된다.

### Minor GC

한 지역에 객체를 할당하다가 해당 지역이 꽉 차면 다른 지역에 객체를 할당하고, Minor GC가 실행된다. G1 GC는 각 지역을 추적하고 있기 때문에, 가비지가 가장 많은(Garage First) 지역을 찾아서 Mark and Sweep을 수행한다.

Eden 지역에서 GC가 수행되면 살아남은 객체를 식별(Mark)하고, 메모리를 회수(Sweep)한다. 그리고 살아남은 객체를 다른 지역으로 이동시키게 된다. 복제되는 지역이 Available/Unused 지역이면 해당 지역은 이제 Survivor 영역이 되고, Eden 영역은 Available/Unused 지역이 된다.

### Major GC

시스템이 계속 운영되다가 객체가 너무 많아 빠르게 메모리를 회수할 수 없을 때 Major GC(Full GC)가 수행된다. 그리고 여기서 G1 GC와 타 GC들의 차이점이 두각을 보인다.

기존의 타 GC 알고리즘들은 모든 힙 영역에서 GC가 수행되었으며, 그에 따라 처리 시간이 상당히 오래 걸렸다. 그러나 G1 GC는 어느 영역에 가비지가 많은지를 알고 있기 때문에 GC를 수행할 지역을 조합하여 해당 지역에 대해서만 GC를 수행한다. 그리고 이러한 작업은 동시적(Concurrent)으로 수행되기 때문에 애플리케이션의 지연도 최소화할 수 있다.

물론 G1 GC는 타 GC 방식과 비교하면 자주 호출될 것이다. 하지만 작은 규모의 메모리 정리 작업이고 동시적으로 수행되기 떄문에 지연이 크지 않으며, 가비지가 많은 지역에 대해 정리를 수행하므로 훨씬 효율적이다.

```bash
java -XX:+UseG1GC -jar Application.java
```

이러한 구조의 G1 GC는 당연히 앞서 설명했던 GC 방식들보다 처리 속도가 빠르며, 큰 메모리 공간에서 멀티 프로세스 기반으로 운영되는 애플리케이션을 위해 고안되었다. 또한 G1 GC는 타 GC 방식의 처리 속도를 능가하기 때문에 Java9부터 기본 가비지 컬렉터(Default Garbage Collector)로 사용된다.

## 참고 자료

- [[Java] Garbage Collection(가비지 컬렉션)의 개념 및 동작 원리 (1/2)](https://mangkyu.tistory.com/118)
- [[Java] 다양한 종류의 Garbage Collection(가비지 컬렉션) 알고리즘 (2/2)](https://mangkyu.tistory.com/119)
- [[Java] 가비지 컬렉션(GC, Garbage Collection) 총정리](https://coding-factory.tistory.com/829)
