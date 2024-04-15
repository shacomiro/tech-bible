![tech-bible-banner](tech-bible-banner.png)

## 소개

개발/기술 지식의 기본 개념을 정리하는 저장소입니다.

> 내용에 오류가 있거나 추가할 내용이 있다면 `Issue` 또는 `Pull Request`를 통해 알려주시면 감사하겠습니다.

> 아래의 목차는 [백엔드 개발자 로드맵(by roadmap.sh)](https://roadmap.sh/backend)과 [알고리즘 공부 리스트 및 순서 (Algorithm Problem Solving Roadmap)](https://stack07142.tistory.com/228?category=234519)를 참고하여 작성되었습니다.

## 목차

1. [자료구조(Data Structure)](./Data-Structure/)
   <details>
   <summary>[열기/접기]</summary>
   <div markdown="1">

   - [자료구조 개요](./Data-Structure/contents/Datastructure-overview.md)
   - 분류
     - 선형 자료구조
       - [배열(Array)](./Data-Structure/contents/Arrays.md)
         - [동적 배열(Dynamic Array)](./Data-Structure/contents/Dynamic-Array.md)
       - [링크드 리스트(Linked List)](./Data-Structure/contents/Linked-List.md)
       - [스택(Stack)](./Data-Structure/contents/Stack.md)
       - [큐(Queue)](./Data-Structure/contents/Queue.md)
       - 데큐(Deque)
       - [해시 테이블(Hash Table)](./Data-Structure/contents/Hash-Table.md)
     - 비선형 자료구조
       - 트리(Tree)
         - 완전 이진 트리
           - [힙(Heap)](./Data-Structure/contents/Heap.md)
         - [이진 탐색 트리(BTS, Binary Search Tree)](./Data-Structure/contents/Binary-Search-Tree.md)
       - 그래프(Graph)
   - 비교
     - [배열 vs 링크드 리스트](./Data-Structure/contents/Array-vs-Linked-List.md)
     - [큐 vs 우선순위 큐](./Data-Structure/contents/Queue-vs-Priority-Queue.md)
       </details>

2. [알고리즘(Algorithms)](./Algorithms/)
   <details>
   <summary>[열기/접기]</summary>
   <div markdown="1">

   - 수학(Mathmetics) #1
     - 순열(Permutation)
     - 조합(Combination)
     - 소수(Prime Number)
       - 에라토스테네스의 체(Eratostheneen seula)
     - [최대공약수와 최소공배수(GCD, LCM)](./Algorithms/contents/GCD-and-LCM.md)
       - [유클리드 호제법(Euclidean algorithm)](./Algorithms/contents/Euclidean-algorithm.md)
     - 행렬(Matrix)
   - 완전 탐색(Exhaustive Search)
     - 브루트-포스(Brute-Force)
     - 백트래킹(Backtracking)
       - _N개의 퀸(N Queens) 문제_
     - 최적화 문제(Optimization Problem)
       - _외판원 순회(TSP) 문제_
     - 분할 정복(Divide & Conquer)
       - 이분 검색(Binary Search)
   - 탐욕법(Greedy Algorithm)
   - 비트마스크(Bitmask)
   - [다이나믹 프로그래밍(DP, Dynamic Programming) #1](./Algorithms/contents/Dynamic-Programming-01.md)
     - _0-1 배낭 문제(0-1 Knapsack Problem)_
     - _최장 공통 부분 수열(LCS), 최장 증가 부분 수열(LIS), ..._
       - 시간복잡도 O(N^2)으로 해결하는 방법
       - 시간복잡도 O(NlogN)으로 해결하는 방법
     - _부분집합(Subset)_
   - 문자열(String)
     - 회문(Palindrome)
       - Manacher's Algorithm
     - 허프만 코딩(Huffman coding)
     - 트라이(Trie)
     - 접미사 트리(Suffix Tree)
     - 매칭 문제(Matching Problems)
       - KMP 알고리즘(KMP Algorithm)
       - 라빈-카프 알고리즘(Krap-Rabin Algorithm)
       - 보이어-무어 알고리즘(Boyer-Moore Algorithm)
       - 아호-코라식 알고리즘(Aho-corasick)
       - Z 알고리즘(Z Algorithm)
       - 접미사 배열(Suffix Array)
   - 최소 신장 트리(MST, Minimun Spanning Tree)
     - 크루스칼 알고리즘(Kruskal's Algorithm)
     - 프림 알고리즘(Prim's Algorithm)
   - 그래프(Graph) #1
     - 탐색(Searching)
       - 깊이 우선 탐색(DFS)
       - 너비 우선 탐색(BFS)
     - 최단 거리(Shortest Path)
       - 다익스트라 알고리즘(Dijkstra's Algorithm)
       - 벨만-포드 알고리즘(Bellman-Ford Algorithm)
       - 플로이드-와샬 알고리즘(Floyd-Warshall Algorithm)
       - SPFA(Shortest Path Faster Algorithm)
     - 정렬(Sorting)
       - [위상 정렬(Topological Sort)](./Algorithms/contents/Topological-Sort.md)
   - 정렬(Sorting)
     - 버블 정렬(Bubble Sort)
     - 삽입 정렬(Insert Sort)
     - 선택 정렬(Selection Sort)
     - 퀵 정렬(Quick Sort)
     - 병합 정렬(Merge Sort)
     - 힙 정렬(Heap Sort)
     - 기수 정렬(Radix Sort)
     - 계수 정렬(Couting Sort)
     - 셸 정렬(Shell Sort)
   - 수학(Mathmetics) #2
     - 이항 계수(binomial coefficient)
       - 파스칼의 삼각형(Pascal's triangle)
     - 카탈랑 수(Catalan Number)
     - 오일러 피 함수(Euler's phi function)
     - 페르마의 소정리(Fermat's little theorem)
     - 가우스 소거법(Gaussian elimination)
     - 모듈러 연산(Modular Arithmetic)
     - 이산 수학(Discrete Mathematics)
       - 비둘기 집의 원리(The Pigeonhole Principle)  
         디리클레 서랍 원리(Dirichlet drawer principle)라고 알려짐
     - 제2종 스털링 수(Stirling numbers of the second kind)
   - 기하학(Geometry)
     - 내적과 외적(Cross/Dot Product)
     - 컨벡스 헐(Convex Hull)
     - 그레이엄 스캔(Graham Scan)
     - 각도 정렬(Angle Sort)
     - 선분 교차 판별(Line Intersection)
     - 반시계(CCW, Counter Colck Wise)
     - 평면/선분 스위핑(Plane/Line Sweeping)
     - 회전하는 캘리퍼스 알고리즘(Rotating Calipers)
   - 트리(Tree) #2
     - 최소 공통 조상(LCA, Lowest Common Ancestor)
       - _전위순회 DFS & 세그먼트 트리(Segment Tree)를 이용하는 방법_
       - _희소 테이블(Sparse Table)을 이용하는 방법 (권장)_
   - 범위 쿼리(Range Query)
     - 세그먼트 트리(Segment Tree)
       - 세그먼트 트리 게으른 전파(Segment Tree Lazy Propagation)
     - [투 포인터 알고리즘(Two Pointers Algorithm)](./Algorithms/contents/Two-Pointers.md)
     - 슬라이딩 윈도우 알고리즘(Sliding Window Algorithm)
   - 그래프(Graph) #2
     - 네트워크 흐름(Network Flow)
       - 최대 흐름(Maximum Flow)
       - 포드-폴커슨 알고리즘(Ford-Fulkerson)
         - 에드몬드-카프 알고리즘(Edmond-Karp)  
           (포드-폴커슨 알고리즘의 구현 형태)
       - 다닉 알고리즘(Dinic's Algorithm)
       - 심화
         - 최소 절단 최대 흐름(MCMF, Minumun Cut Maximum Flow)
         - 최소 비용 최대 흐름(MCMF, Minumun Cost Maximum Flow)
           - _SPFA의 벨만-포드 알고리즘(Bellman-Ford Algorithm)을 이용하는 방법_
           - _헝가리안 메소드(Hungarian Method)를 이용하는 방법_
         - 이분 매칭
           - 호프크로프트-카프 알고리즘(Hopgroft-Karp Algorithm)
   - 그래프(Graph) #3
     - 오일러 경로(Eulerian Path)
       - Hierholzer's Algorithm
     - SCC(Strongly Connected Component)
       - 타잔 알고리즘(Tarjan's Algorithm)
       - 코사라주 알고리즘(Kosaraju's Algorithm)
   - 다이나믹 프로그래밍(DP, Dynamic Programming) #2
     - DP 최적화(DP Optimization)
     - 크누스 최적화(Knuth Optimization)
     - 분할 정복 최적화(Dvide & Conquer Optimization)
     - 컨벡스 헐 최적화(Convex Hull Optimization)
       </details>

3. [언어(Language)](./Language/)
   <details>
   <summary>[열기/접기]</summary>
   <div markdown="1">

   - 공통
     - [컴파일 타임과 런타임](./Language/Common/Comfile-time-and-Runtime.md)
   - 프로그래밍 언어
     - [Java](./Language/Java/)
       - [Java 개요](./Language/Java/contents/Java-Overview.md)
       - [Java 버전별 특징](./Language/Java/contents/Java-feature-by-version.md)
       - Java 자료구조
         - 기본 타입
         - 참조 타입
       - [JVM](./Language/Java/contents/JVM.md)
         - 운영체제와의 연관성
       - [가비지 컬렉션(GC)](./Language/Java/contents/Garbage-Collection.md)
       - [메모리 관리](./Language/Java/contents/Java-memory-management.md)
       - 람다
       - 함수형 프로그래밍
       - 스트림
       - 자바 thread
         - 동시성 프로그래밍과의 연관성
       - 프레임워크
         - [Spring(스프링)](./Language/Java/contents/Spring.md)
     - JavaScript
   - 마크업 언어
     - XML
     - HTML
     - 마크다운
   - 스타일 시트 언어
     - CSS - 디자인 라이브러리 - Bootstrap - Sementic-UI - W3.CSS - 기능 보완 라이브러리 - Prefix Free - Fontello
       </ditails>

4. [소프트웨어 공학](./Software-Engineering/)
   <details>
   <summary>[열기/접기]</summary>
   <div markdown="1">

   - 개발·설계 원칙
     - GOF 디자인 패턴
     - 도메인 주도 설계(DDD)
     - 테스트 주도 개발(TDD)
     - [SOLID](./Software-Engineering/contents/SOLID.md)
     - [소프트웨어 개발 3대 원칙](./Software-Engineering/contents/3-key-software-principles.md)
       - [KISS](./Software-Engineering/contents/3-key-software-principles.md#kiss)
       - [YAGNI](./Software-Engineering/contents/3-key-software-principles.md#yagni)
       - [DRY](./Software-Engineering/contents/3-key-software-principles.md#dry)
     - 아키텍처 패턴
       - 모놀리식 애플리케이션
       - 마이크로서비스
       - SOA
       - CQRS와 이벤트 소싱
       - 서버리스
   - 테스트
     - 통합(Intergration) 테스트
     - 단위(Unit) 테스트
     - 기능(Function) 테스트
   - CI/CD
   - 빌드
     - Maven
     - Gradle
   - 버전 관리 시스템
     - Git 기본 사용법
     - 저장소 호스팅 서비스
       - GitHub
   - 확장성 있는 구축
     - 차이 이해하기
       - Intrumentation
       - Monitoring
       - Telemetry
     - 마이그레이션 전략
       - 단계적 기능 축소(Graceful Degradation)
       - 스로틀링(Throttling)
       - Backpressure
       - 서킷 브레이커(Circuit Breaker)
     - 수평적 확장 vs 수직적 확장
     - 관찰 가능성을 고려한 확장
       </details>

5. [네트워크(Network)](./Network/)
   <details>
   <summary>[열기/접기]</summary>
   <div markdown="1">

   - 인터넷
     - [인터넷의 작동 원리](./Network/contents/internet/How-does-the-internet-work.md)
     - [웹의 작동 원리](./Network/contents/internet/How-WEB-work.md)
     - [HTTP란?](./Network/contents/internet/What-is-HTTP.md)
     - [브라우저의 작동 원리](./Network/contents/internet/Browsers-and-how-they-work.md)
     - [DNS의 작동 원리](./Network/contents/internet/How-does-the-DNS-work.md)
     - 도메인 이름이란?
     - 호스팅이란?
   - 네트워크 기본 개념
     - [OSI 7계층과 TCP/IP 4계층](./Network/contents/network-basic/OSI-7-Layer-and-TCPIP-4-Layer.md)
       - [캡슐화와 역캡슐화](./Network/contents/network-basic/OSI-7-Layer-and-TCPIP-4-Layer.md#캡슐화encapsulation와-역캡슐화decapsulation)
     - [TCP와 UDP](./Network/contents/network-basic/TCP-and-UDP.md)
       - [3-Way Handshake](./Network/contents/network-basic/TCP-and-UDP.md#3-way-handshake)
       - [4-Way Handshake](./Network/contents/network-basic/TCP-and-UDP.md#4-way-handshake)
   - API
     - HATEOAS
     - 오픈 API 명세
       - Swagger
       - Spring REST Docs
     - 인증
       - Basic 인증
       - [쿠키 기반 인증](./Network/contents/api/Cookie-based-authentication.md)
       - 토큰 기반 인증
         - JWT
         - OAuth
       - OpenID
       - SAML
     - [REST](./Network/contents/api/REST.md)
     - JSON API
     - SOAP
     - gRPC
     - GraphQL
       - Apollo
       - Relay Modem
   - 캐시
     - CDN
     - 서버 사이드
       - Redis
       - Memcached
     - 클라이언트 사이드
   - 웹 보안 지식
     - HTTPS
     - [교차 출처 리소스 공유(CORS)](./Network/contents/web-security/CORS.md)
     - 콘텐츠 보안 정책(SCP)
     - SSL/TLS
     - OWASP 보안 취약점
     - 해시 알고리즘
       - MD5와 이를 사용하지 않는 이유
       - SHA 함수군
       - acrypt
       - bcrypt
   - 웹소켓
   - 웹 서버
     - Nginx
     - Apache
     - Caddy
     - MS IIS
     - [WEB 서버와 WAS를 분리하는 이유](./Network/contents/web-server/Reasons-for-separating-the-WEB-Server-and-WAS.md)
       </details>

6. [운영체제(OS)](./Operating-System/)
   <details>
   <summary>[열기/접기]</summary>
   <div markdown="1">

   - 터미널 사용법
     - 터미널 기본 명령
   - OS의 일반적인 작동 원리
   - [프로세스와 스레드](./Operating-System/contents/Process-and-Thread.md)
     - 프로세스 관리
     - 프로세스 간 통신
     - 스레드와 동시성
   - [동기화 문제](./Operating-System/contents/Synchronization-problem.md)
   - [교착 상태](./Operating-System/contents/Deadlock.md)
   - 메모리 관리
     - [페이징](./Operating-System/contents/Paging.md)
     - [세그먼테이션](./Operating-System/contents/Segmentation.md)
     - [가상 메모리](./Operating-System/contents/Virtual-memory.md)
   - 입출력(I/O) 관리
   - POSIX 기초  
   stdin, stdout, stderr, pipes
     </details>

7. [데이터베이스(Database)](./Database/)
   <details>
   <summary>[열기/접기]</summary>
   <div markdown="1">

   - 기초 데이터베이스 지식
     - 데이터베이스 기본 개념
     - 데이터베이스 관리 시스템(DBMS)
     - [데이터 모델링](./Database/contents/Data-Modeling.md)
       - [개체-관계 모델](./Database/contents/Data-Modeling.md#개체-관계-모델)
     - [관계 데이터 모델](./Database/contents/Relational-Data-Model.md)
       - [기본 용어](./Database/contents/Relational-Data-Model.md#관계-데이터-모델의-기본-용어)
       - [릴레이션의 특징](./Database/contents/Relational-Data-Model.md#릴리이션의-특징)
       - [키의 종류](./Database/contents/Relational-Data-Model.md#키의-종류)
     - 관계 데이터 연산
     - 데이터베이스 언어 SQL
     - 데이터베이스 설계
     - 정규화
     - [회복과 병행 제어](./Database/contents/Recovery-and-Concurrency-Control.md)
       - [트랜잭션](./Database/contents/Recovery-and-Concurrency-Control.md#트랜잭션transaction)
         - [ACID](./Database/contents/Recovery-and-Concurrency-Control.md#트랜잭션의-4가지-특성)
     - 보안과 권한 관리
   - 더 깊은 데이터베이스 지식
     - [인덱스](./Database/contents/Index.md)
     - SQL 심화
       - [조인(Join)](./Database/contents/Join.md)
     - N+1 문제
     - 데이터 레플리케이션
     - 샤딩 전략
     - CAP 이론
   - 관계형 데이터베이스
     - MySQL
     - MariaDB
     - Oracle
   - NoSQL 데이터베이스
     - MongoDB
   - 검색 엔진
     - Elasticsearch
     - Solr
   - 그래프 데이터베이스
     - Neo4j
   - ORM
     - JPA
   - SQL Mapper
     - MyBatis
   - 비교
     - [RDB와 NoSQL의 차이](./Database/contents/RDB-vs-NoSQL.md)
     - RDBMS와 검색 엔진의 차이
       </details>

8. 메시지 브로커
   <details>
   <summary>[열기/접기]</summary>
   <div markdown="1">

   - RabbitMQ
   - Kafka
     </details>

9. 컨테이너화 vs 가상화
   <details>
   <summary>[열기/접기]</summary>
   <div markdown="1">

   - Docker
     </details>
