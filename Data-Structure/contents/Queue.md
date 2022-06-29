# 큐

1. [정의](#정의)
2. [특징](#특징)
   1. [Enqueue & Dequeue](#enqueue--dequeue)
3. [활용](#활용)
4. [구현](#구현)
   1. [배열 기반 큐(Array-Based Queue)](#배열-기반-큐array-based-queue)
   2. [리스트 기반 큐(List-Based Queue)](#리스트-기반-큐list-based-queue)
   3. [스택(Stack) 두 개를 이용한 구현](#스택stack-두-개를-이용한-구현)
5. [시간복잡도와 공간복잡도](#시간복잡도와-공간복잡도)
6. [참고 자료](#참고-자료)

## 정의

> 선입선출(FIFO, First In First Out) 자료구조.

큐는 **선입선출**의 특성을 갖는 자료구조로 시간 순서상 가장 먼저 추가한 데이터가 가장 먼저 나오는 선입선출 형식으로 데이터를 저장하는 자료구조이다.

## 특징

### Enqueue & Dequeue

큐에서 데이터를 추가하는 것을 Enqueue라고 하며, 데이터를 추출하는 것은 Dequeue라고 한다.

- Enqueue  
  큐의 맨 뒤에 데이터를 추가하면 완료되기에 `O(1)`의 시간복잡도를 갖는다.
- Dequeue  
  큐의 맨 앞 데이터를 삭제하면 완료되기에 `O(1)`의 시간복잡도를 갖는다.

## 활용

큐의 개념에서 조금 확장한 자료구조들로는 양쪽에서 Enqueue, Dequeue가 가능한 데큐(Deque, Double-ended Queue)와 시작순서가 아닌 우선순위가 높은 순서로 dequeue 할 수 있는 우선순위 큐(Priority Queue)가 있다.

대표적으로 하나의 자원을 공유하는 프린터나 CPU 작업 스케쥴링, Cache 구현, 너비우선탐색(DFS) 등에 활용된다.

## 구현

### 배열 기반 큐(Array-Based Queue)

인큐와 데큐 과정에서 남는 메모리가 발생한다. 따라서 메모리의 낭비를 줄이기 위해 주로 **원형 큐(Circular Queue)** 형식으로 구현한다.

### 리스트 기반 큐(List-Based Queue)

리스트를 기반으로 구현하는 경우 재할당이나 메모리 낭비를 고려하지 않아도 된다.

> **배열 기반과 리스트 기반의 차이점**
>
> - 배열 기반 큐
>   원형 큐(Circular Queue)로 구현하는 것이 일반적이다. 이는 메모리를 효율적으로 사용하기 위함이다. 또한 Enqueue가 지속되면 fixed size를 넘어서기에 동적 배열(Dynamic Array)와 같은 방법으로 배열의 크기를 확장시켜야 한다. 그럼에도 enqueue의 시간복잡도는 `amortized O(1)`을 유지할 수 있다.
> - 리스트 기반 큐
>   싱글 링크드 리스트(Singly-Linked List)로 구현하는 것이 보통이다. Enqueue는 단순히 싱글 링크드 리스트에서 append를 하는 것으로 구현되고, `O(1)`의 시간복잡도를 갖는다. Dqeueue는 맨 앞의 원소를 제거하고 first head를 변경하면 되기 때문에 동일하게 `O(1)`의 시간복잡도를 갖는다.
>
> 요약하면 어떤 자료구조로 큐를 구현하더라도 Enqueue, Dequeue 연산은 모두 `O(1)`의 시간복잡도를 갖는다. 배열 기반이 전반적인 성능이 더 좋지만, 최악의 경우(Worst Case)에 resize로 인해 지연이 발생하여 훨씬 더 느릴 수 있다. 리스트 기반은 Enqueue를 할 때마다 메모리 할당을 해야 하기에 전반적인 런타임(Runtime)이 느릴 수 있다.

### 스택(Stack) 두 개를 이용한 구현

먼저 각 연산들(`enqueue()`, `dequeue()`)에 사용할 큐 2개(각각 `instack`, `outstack`)를 준비한다.

1. `enqueue()`  
   `instack`에 `push()`를 하여 데이터를 저장한다.
2. `dequeue()`
   1. 만약 `outstack`이 비어있다면 `instack.pop()`을 수행한 후 `outstack.push()`를 하여 `instack`에서 `outstack`으로 모든 데이터를 옮겨 넣는다. 이 결과로 가장 먼저 들어온 데이터는 `outstack`의 top에 위치한다.
   2. `outstack.pop()`을 수행하면 가장 먼저 들어온 데이터가 첫번째로 추출된다.(FIFO)

스택을 이용해 큐을 구현한 경우에는 시간복잡도가 조금 다르다.

- `enqueue()` : `instack.push()`를 한번 수행하면 되기에 `O(1)`의 시간복잡도를 갖는다.
- `dequeue()` : 두 가지 경우가 있다. 최악의 경우(Worst Case)는 `outstack`이 비어있는 경우로, 이 때는 `instack`에 있는 n개의 데이터를 `instack.pop()`을 한 이후 `outstack.push()`를 수행해야 한다. 따라서 총 2\*n번의 연산이 발생하여 `O(n)`의 시간복잡도를 갖는다. 그러나 `outstack`이 비어있지 않은 경우엔 `outstack.pop()`만 수행하면 되기에 `O(1)`의 시간복잡도를 갖는다. 이를 종합하면 `amortized O(1)`의 시간복잡도를 갖는다고 할 수 있다.

## 시간복잡도와 공간복잡도

- 시간복잡도

  |     Operation      | Average | Worst  |
  | :----------------: | :-----: | :----: |
  |       Access       | `Θ(n)`  | `O(n)` |
  |       Search       | `Θ(n)`  | `O(n)` |
  | Insertion(Enqueue) | `Θ(1)`  | `O(1)` |
  | Deletion(Dequeue)  | `Θ(1)`  | `O(1)` |

- 공간복잡도  
  `O(n)`

## 참고 자료

- 시간복잡도와 공간복잡도
  - [Big-O Cheat Sheet](https://www.bigocheatsheet.com/)
