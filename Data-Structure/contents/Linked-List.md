# 링크드 리스트(Linked List)

1. [정의](#정의)
   1. [논리적 연속성](#논리적-연속성)
   2. [배열(Array)과의 차이점](#배열array과의-차이점)
2. [특징](#특징)
   1. [장점](#장점)
   2. [단점](#단점)
3. [활용](#활용)
4. [구현](#구현)
5. [시간복잡도와 공간복잡도](#시간복잡도와-공간복잡도)
6. [참고 자료](#참고-자료)

## 정의

> 링크드 리스트를 구성하는 각각의 노드가 다음 노드의 주소를 가리킴으로써 논리적인 연속성을 가진 자료구조.

링크드 리스트는 **노드(Node)라는 구조체**로 이루어져 있다. 노드는 **데이터 값**과 <b>다음 노드의 주소(포인터)</b>를 저장한다. 링크드 리스트는 물리적인 메모리상에서는 **비연속적으로 저장**되나 각 노드들이 다음 노드들을 가리키며 **논리적으로 연속성**을 지닌다.

### 논리적 연속성

각 노드들은 다음 노드의 주소 정보를 가지고 있기 때문에 **논리적으로 연속성을 유지하면서 연결**되어 있다.

배열의 경우 연속성을 유지하기 위해 물리적 메모리 상에서 순차적으로 저장하지만, 링크드 리스트는 메모리에서 연속성을 유지하지 않아도 되기 때문에 **메모리 사용이 조금 더 자유로운 대신**, 다음 노드의 주소(포인터)를 추가적으로 저장해야 하기에 **단일 데이터가 차지하는 메모리 비중이 더 크다.**

### 배열(Array)과의 차이점

- 배열  
  번호가 붙여진(인덱싱이 된) 칸에 원소들을 채워 넣어 관리함.
- 링크드 리스트  
  각 원소들을 줄줄이 엮어서 관리함.

## 특징

배열과는 달리 첫번째 데이터의 추가/삭제가 `O(1)`의 시간안에 수행된다. 배열의 경우 데이터를 추가 또는 삭제할 때 해당 지점 뒤쪽의 데이터를 모두 이동해야 하나 연결 리스트는 그럴 필요가 없다. 하지만, 첫번째가 아닌 중간에 있는 데이터들의 추가/삭제는 해당 데이터를 검색하는데까지 시간이 걸리기 때문에 `O(n)`의 수행시간을 갖게된다.

다만 탐색시에는 문제가 되는데 각 데이터에 한번에 접근할 수가 없어 항상 순차적으로 도달해야 한다. 이 때문에 데이터 검색에 취약하다. 정렬은 `O(nlogn)`시간에 가능하다. 이 단점을 해결한 것이 `B+tree`이다.

### 장점

- 데이터 공간을 미리 할당하지 않아도 됨. (array는 미리 데이터 공간을 할당해야 함.)
- 삽입과 삭제가 빠름. 따라서 삽입/삭제가 빈번히 일어날 때 많이 사용됨.

### 단점

- 데이터 구조 표현에 소요되는 저장공간이 비교적 큼.
- 연결을 위한 별도의 데이터공간이 필요하기 때문에 저장공간 효율이 높지 않음.
- 데이터를 찾는 시간이 오래 걸림.
- 인덱싱이 된 배열와는 달리, 특정 N번째 원소에 접근하려면 링크드 리스트의 처음부터 순차적으로 원소를 훑으며 N번째 원소를 찾아가야 함.
- 중간에 위치한 데이터 삭제 시, 앞뒤 데이터의 연결을 다시 구현해야하는 부가적인 작업이 필요함.

## 활용

데이터의 검색보다 추가 및 삭제가 빈번한 경우 데이터 재구성이 용이하므로, 배열처럼 인덱스가 지정되어 있어서 추가/삭제시 데이터의 재이동이 필요한 경우보다 효율적이다.

단, 탐색(검색)이 빈번한 경우에는 각 노드마다 가리키는 포인터를 통해 순차적으로 탐색해야 하므로 속도가 느리므로 적합하지 않다. 또한 데이터 외에 추가로 포인터를 가지고 있어야 하므로 필요한 메모리 공간이 배열보다 크다는 단점이 있다.

## 구현

> [자료구조 : 연결리스트 (Linked list)](https://sycho-lego.tistory.com/17) 참고

## 시간복잡도와 공간복잡도

- 싱글 링크드 리스트

  - 시간복잡도

    | Operation | Average | Worst  |
    | :-------: | :-----: | :----: |
    |  Access   | `Θ(n)`  | `O(n)` |
    |  Search   | `Θ(n)`  | `O(n)` |
    | Insertion | `Θ(1)`  | `O(1)` |
    | Deletion  | `Θ(1)`  | `O(1)` |

  - 공간복잡도(Worst)

    | Worst  |
    | :----: |
    | `O(n)` |

- 듀얼 링크드 리스트

  - 시간복잡도

    | Operation | Average | Worst  |
    | :-------: | :-----: | :----: |
    |  Access   | `Θ(n)`  | `O(n)` |
    |  Search   | `Θ(n)`  | `O(n)` |
    | Insertion | `Θ(1)`  | `O(1)` |
    | Deletion  | `Θ(1)`  | `O(1)` |

  - 공간복잡도(Worst)

    | Worst  |
    | :----: |
    | `O(n)` |

## 참고 자료

- PRACTICAL DATA STRUCTURE 데이터 구조 - 오상엽, 장성식, 정호일 저
- #정의
  - [자료 구조(Data Structure) 개념 및 종류 정리](https://bnzn2426.tistory.com/115)
  - [자료구조 : 연결리스트 (Linked list)](https://sycho-lego.tistory.com/17)
- #특징
  - [Linked-list](https://velog.io/@riceintheramen/Linked-list)
  - [[KR] 자료구조 & 알고리즘 : 링크드 리스트(Linked List)](https://lucaseo.github.io/posts/2021-02-01-python-datastructure-linked-list/#5-%ec%a0%95%eb%a6%ac%ed%95%b4%eb%b3%b4%ea%b8%b0)
- #활용
  - [자료구조 : 연결리스트 (Linked list)](https://sycho-lego.tistory.com/17)
- #시간복잡도와 공간복잡도
  - [Big-O Cheat Sheet](https://www.bigocheatsheet.com/)
