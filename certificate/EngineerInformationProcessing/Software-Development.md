### 소프트웨어 개발



## 1. 데이터 입출력 구현 (28%)

### 1.1. 자료 표현 단위와 진법(하)

- 자료 표현
  - 니블(Nibble) : 4개의 비트 모여 구성.
  - 워드(Word) : 컴퓨터에서 한 번에 처리될 수 있는 데이터의 양. 보통 범용 레지스터 길이와 같음. 컴퓨터 기종에 따라 다름.
  - 필드(Field) : 몇 개의 워드가 모여서 구성되며 정보 표현의 최소 단위. 보통 데이터베이스 테이블의 하나의 열을 의미하며 항목(Item)이라고도 함.
  - 레코드(Record) : 하나 이상의 필드가 모여서 구성하며 자료 처리의 기본 단위. 데이터베이스 테이블에서 하나의 행을 의미하며 튜플(Tuple)이라고도 함.
  - 블록(Block) : 실제 저장 매체에서 한 번에 읽어 올 수 있는 물리적인 크기로 입출력될 때의 기본 단위.
  - 파일(File) : 서로 관련 있는 레코드의 모임. 컴퓨터 저장 기본 단위.
  - 데이터베이스(Database) : 서로 관련성 있는 파일들의 집합으로 중복을 제거한 집합체.
- 2, 8, 16진수를 10진수 변환 : 거듭제곱으로 계산
- 10진수의 2, 8, 16진수 변환 : 8진수 3자리, 16진수 4자리씩 계산
- 8, 16진수 변환 : 중간에 2진수 거치기

<br>

### 1.2. 고정 소수점과 부동 소수점(하)

- 고정 소수점(정수)
  - 고정 소수점의 표현
    - 음수를 표현하기 위해 부호화 절대치에서 1의 보수, 2의 보수 방법으로 발전됨.
    - 보수는 컴퓨터에서 기본적으로 사용하는 덧셈 연산을 이용하여 뺄셈을 수행하기 위해서 사용함.
    - 현재는 2의 보수 방법으로 음수 표현함.
  - 부호화 절대치
    - 상위 1비트는 부호 비트. 0이면 양수, 1이면 음수.
    - 0이 2개 존재
    - n비트면 -(2^(n-1) - 1) ~ + 2^(n-1) - 1
  - 1의 보수
    - 각 자릿수의 값이 모두 1인 수에서 주어진 2진수를 빼면 1의 보수를 얻음.
    - 상위 1비트 부호 비트, 0이면 양수, 1이면 음수
    - 0이 2개 존재
    - 최대 표현수에 도달하면 그 다음수는 최소 표현수로 회전시킴.
    - n비트면 -(2^(n-1) -1) ~ 2^(n-1) -1
  - 2의 보수
    - 1의 보수에 1을 더한 값.
    - 상위 1비트 부호비트, 0이면 양수, 1이면 음수
    - 0이 하나만 존재함.
    - 최대 표현수에 도달하면 그 다음수는 최소 표현수로 회전시킴.
    - n비트면 -(2^(n-1)) ~ 2^(n-1) -1
- 부동 소수점(실수)
  - 부동 소수점 수의 표현
    - 정규화 과정을 통하여 부호부, 지수부, 가수부로 구성됨.
    - 고정 소수점보다 복잡, 실행 시간 오래 걸림, 아주 큰 수나 작은 수의 표현이 가능함, 정밀도가 제한적임.
  - 부동 소수점의 정규화
    - 47.5 -> 0.475x10^2 -> 0, 475, 2 기억
    - -255.75 -> -0.25575x10^3 -> 1, 25575, 3 기억
  - 부동 소수점 연산
- 자료형 변환 시 확장과 축소
  - 확장(Widening) : 정수형을 실수형으로 변환 시 정보의 손실이 없음.
  - 축소(Narrowing) : 실수형을 정수형으로 변환 시 정보 손실 발생.
- 기억 장치의 용량
  - 저장용량단위 : 2의 10승부터 10단위 차례대로 kb, mb, gb, tb, pb, eb, zb, yb
  - 속도및길이 단위 : deci, centi, 3milli, 6micro, 9nano, 12pico, 15femto, 18atto, 21zepto,24yocto



### 1.3. 자료 구조(중)

- 자료구조
  - 개념
    - 데이터 간 논리적 관계, 효율적인 데이터 컴퓨터 저장 방법
    - 선형 리스트, 스택, 큐, 트리 등이 있음.
  - 데이터 형태에 따른 자료 구조 분류
    - 단순 구조(Simple) : 기본 데이터 타입
    - 선형 구조(Linear)
      - 1:1 대응 구조. 순차 구조와 연결 구조(중간 삽입 용이).
      - 스택, 큐, 데크, 선형 리스트, 연결 리스트가 있음.
    - 비선형 구조(Non-Linear)
      - 1:N, N:M 대응 구조. 트리 구조와 그래프 구조.
    - 파일 구조(File)
      - 보조 기억 장치에 데이터값이 실제로 기록되는 자료 구조.
      - 순차 파일, 색인 파일 등이 있음.
- 선형 구조
  - 스택(Stack)
    - 스택 포인터(TOP) : 입력된 데이터 위치 가리킴. 데이터 입력(PUSH)될 때마다 스택 포인터 1씩 증가하며 스택 크기보다 큰 값을 갖게 되면 오버플로우(Overflow) 상태가 됨. 데이터 출력될 때는 스택 포인터 1씩 감소함. 저장된 데이터 없을 경우 언더플로우(Underflow) 상태가 되어 0값을 기억함.
    - LIFO : 나중에 입력된 데이터가 먼저 출력하는 메모리 사용 방법. 후입선출.
    - 사용 알고리즘 : 함수호출복귀, DFS, 재귀함수, 수식 우선 연산, 0-주소명령어방법
  - 큐(Queue)
    - 한쪽 방향으로 데이터가 삽입되고 반대 방향으로 데이터가 삭제되는 데이터 구조.
    - 선입선출FIFO 구조.
    - 삽입포인터(Rear) : 데이터 삽입시 Rear=Rear+1
    - , 삭제포인터(Front) : 데이터 삭제시 Front=Front+1
    - 사용 알고리즘 : 프린터 스풀, 입출력 버퍼, 은행 번호표, 각종 스케줄링, 동영상 스트리밍.
  - 데크(Deque)
    - 양쪽 모두에서 입력, 출력 가능함.
    - Left포인터, Right포인터 존재.
    - 가장 많이 사용하는 메모리 사용 방법.
    - 데이터 가득 차면 스크롤, 데이터 비어 있으면 셀프 방법 이용하여 조절.
    - 스크롤 : 출력은 양쪽, 입력은 한쪽에서만 사용하는 방법.
    - 셀프 : 입력은 양쪽, 출력은 한쪽에서만 사용하는 방법.
  - 선형 리스트(Linear List)
    - 배열과 같이 연속되는 기억 공간에 저장되는 리스트 구조.
    - 간단한 구조, 접근 속도 빠름. 중간에 자료 삽입하려면 연속된 빈 공간 필요.
    - 삽입, 삭제가 많은 처리에 적합하지 않음.
    - 배열
      - 같은 데이터 타입이 동일한 크기로 순차적으로 나열되어 있는 집합. (레코드 : 이질형의 데이터 집단)
      - 데이터마다 변수 이름 따로 두지 않아 처리 간편함.
      - 배열이름[첨자]. arr[i]
      - 주어진 데이터를 찾을 때 가장 좋은 방법.
      - 논리적 순서와 물리적 순서 같음.
      - 삽입 시 데이터의 평균 이동 횟수 = (n+1) / 2
      - 삭제 시 데이터의 평균 이동 횟수 = (n-1) / 2
      - 정적 메모리 할당 방법을 사용하여 메모리 효율이 떨어짐. <-> 연결리스트 동적 메모리 할당.
  - 연결 리스트(Linked List)
    - 자료들을 임의의 공간에 기억시키되 각 자료의 포인터(Pointer)를 이용하여 연결한 구조.
    - 자료의 삽입과 삭제가 용이함.
    - 연결을 위한 포인터를 찾는 시간이 필요하기 때문에 선형 리스트에 비해 느림.
    - 포인터를 포함한 자료 구조를 노드(Node)라고 하고, 노드에는 포인터의 추가 공간이 필요함.
    - 연결 리스트에 비하여 배열은 임의의 위치에 있는 원소를 접근할 때 효율적.
    - 단일 연결 리스트(Single Linked List) : 최종 노드의 포인터 NULL
    - 단일 환형 연결 리스트(Single Circular Linked List) : 최종 노드의 포인터가 최초 노드를 가리킴.
    - 이중 연결 리스트(Double Linked List) : 한 노드에 2개의 포인터
    - 이중 환형 연결 리스트(Double Circular Linked List)
- 비선형 구조
  - 트리(Tree)
    - 노드와 선분으로 구성. 계층 구조 표현에 적합. 
    - 루트 노트 : 뿌리
    - 단(말)노드(Terminal) : 자식 없는 노드
    - 간노드(Nonterminal) : 자식이 있는 노드
    - 노드의 차수(Degree) : 자식 노드의 개수
    - 트리의 차수(Degree) : 노드의 차수가 가장 큰 값
    - 노드의 레벨(Level) : 특정 깊이를 가지는 노드 집합. 루트노트는 레벨1
    - 노드의 크기(Size) : 자신을 포함한 자식 노드의 수
    - 노드의 깊이(Depth) : 루트에서 거쳐 간 간선의 수
    - 노드의 높이(Height) : 루트에서 가장 깊숙히 있는 노드의 깊이
    - 부노드(Parent) : 부모 노드
    - 자노드(Children) : 자식 노드
    - 제노드(Sibling) : 형제 노드
    - 숲(Forest) : 루트를 제외한 나머지 부분
    - 서브 트리(Sub Tree) : 부분 집합 트리
  - 그래프(Graph)
    - 표시 방법 : G=(V, E) V : 정점, E : 간선
    - 정점(Vertex) : 표현할 자료, 원으로 표시
    - 간선(Edge) : 자료 관계, 선이나 화살표로 표시
    - 무방향성 그래프 : 화살표가 없는 간선
    - 방향성 그래프 : 화살표가 있는 간선
    - 완전 그래프 : 모든 정점에 간선이 연결된 경우
    - 부분 그래프 : 그래프의 일부분, 자기 자신도 부분 그래프
    - 경로(Path) : 정점과 정점으로 연결된 경로
    - 경로의 길이 : 경로에 존재하는 간선의 수
    - 단순 경로 : 중복 없는 경로
    - 사이클(Cycle) : 시작, 종료 정점이 같고 길이가 2 이상
    - 해밀톤 사이클 : 모든 정점을 한 번씩 거쳐 가는 사이클
    - 오일러 사이클 : 임의 경로에서 모든 간선을 한 번씩 사용한 사이클
    - 차수(Degree) : 정점에 연결된 간선의 수
    - 진입차수 : 정점으로 들어오는 차수
    - 진출차수 : 정점에서 나가는 차수
    - 인접(Adjacent) : 무방향 그래프에서 하나의 간선으로 연결된 정점
    - 부속(Incident) : 무방향 그래프에서 인접 관계에 있는 간선 
- 이진 트리 순회
  - 중위(In-order) 순회 방법 : 좌근우
  - 전위(Pre-order) 순회 방법 : 근좌우
  - 후위(Post-order) 순회 방법 : 좌우근
- 폴리쉬 표기법
  - 중위식(Infix) : A+B
  - 전위식(Prefix) : +AB
  - 후위식(Postfix) : AB+
  - 중위식-전위식 변환 A=(B-C)*D+E -> =A+\*\-BCDE
  - 중위식-후위식 변환 A=(B-C)*D+E -> ABC-D\*E\+=



### 1.4. 검색(Search, 탐색)(하)

- 검색
  - 대량의 자료 중에서 원하는 자료를 찾는 작업.
  - 내부 검색 : 주기억 장치를 이용하는 검색. 검색 대상의 자료가 적을 때 활용함. 검색 시간이 빠름.
  - 외부 검색 : 검 색할 자료들이 많을 때 사용하는 방법으로 보조 기억 장치에 대량의 자료를 보관하고, 부분적으로 주기억 장치에 적재하여 검색하는 방법.
  - 검색의 종류
    - 선형 검색
    - 제어 검색
      - 이분(이진) 검색
      - 보간 검색
    - 블록 검색
    - 이진 트리 검색
    - 해싱 검색
  - 시간 복잡도
    - 실행 시간 대 신 알고리즘의 연산 횟수를 세어서 판단.
    - 시간 복잡도 함수를 O(n), O(log_2n) 등으로 표시함.
    - O(1) : 알고리즘 수행 시간이 입력 데이터 수와 관계없이 일정함.
    - O(long_2n) : 로그형으로 이분 검색, 이진 트리 검색에  해당함.
    - O(n) : 선형으로 n만큼 시간이 소모됨. 수열이나 순차검색에 해당됨.
    - O(nlog_2n) : 로그 선형으로 퀵 정렬, 힙 정렬, 합병 정렬 시 비교 횟수에 해당됨.
    - O(n^2) : 2차형으로 선택 정렬, 버블 정렬 시 자료 비교 횟수에 해당됨.
    - O(n^3) : 3차형, 행렬 곱셈 알고리즘에 해당됨.
    - O(n^k) : K차형
    - O(2^n) : 지수형
    - O(n!) : 팩토리얼형
-  선형 검색(Linear Search)
  - 검색 대상의 자료를 처음부터 하나씩 비교하여 검색함.
  - 시간 복잡도 O(n)
  - 자료 n개 있을 때 평균 비교 횟수 : (n+1)/2
- 이분(이진) 검색(Binery Search)
  - 검색 대상의 전체 자료를 이등분하면서 검색하는 방법.
  - 검색 자료의 전체 수를 알고 자료들이 정렬되어 있어야 함.
  - 시간 복잡도 O(log_2n)
- 보간 검색(interpolation Search)
  - 찾고자 하는 자료가 있음 직한 부분을 검색하는 방법. 사전식 검색이라고도 함.
  - 공식값 소수점 이하 버린 위치에서 첫 번째 자료를 찾고 첫 번째로 찾지 못할 경우에는 앞과 뒤로 임의 수만큼 확장해서 순차적으로 검색해 봄.
  - 공식 : (자료 - 최솟값) / (최댓값 - 최솟값) X 검색자료수 
- 블록 검색(Block Search)
  - 찾고자 하는 대량의 자료들을 그룹별로 블록화하고, 블록들을 인덱스(Index)로 만들어 검색하는 방법.
  - 블록 내부는 정렬되어 있지 않아도 되지만, 블록끼리는 정렬이 되어 있어야 함.
  - 가장 이상적인 블록의 개수 sqrt{n}
- 이진 트리 검색(Binery Tree Search)
  - 검색 대상의 자료를 이진 트리로 변형한 뒤 검색하는 방법.
  - 시간 복잡도 O(log_2n)
  - AVL 트리(Adelson, Velskii, Landis 균형 이진 트리)
    - 이진 트리에서 왼쪽 서브 트리와 오른쪽 서브 트리가 균형이 맞지 않으면 검색 속도가 늦어질 수 있으므로 트리의 균형이 맞도록 균형 인수를 +1, -1이나 0으로 맞추는 트리를 말함.
  - 2-3 트리
    - AVL 트리에서는 삽입과 삭제 시 전체 트리를 재구성해야 하는 부담이 발생함.
    - 2-3 트리는 이진 트리 구조는 아님.
    - 2-3 트리는 완전 균형 트리를 지향함.
  - 레드-블랙(Red-Black) 트리
    - 이진 트리를 구성할 때 조건을 부여하여 AVL 트리로 구성하는 방법.
- 해싱 검색(Hashing Search)
  - 개념
    - 자료를 찾는 특별한 규칙으로 검색 대상의 자료를 저장하여 자료를 찾음.
    - 특별한 규칙이란 해싱 함수를 말하며 해싱 함수의 결과로 자료들의 저장 위치(주소)가 결정되어짐.
    - 충돌이 발생하지 않는 해싱 함수를 사용한다면 해싱 검색의 시간 복잡도는 O(1)
  - 해싱 함수
    - 제산법(Division) : 레코드 키 값을 소수나 전체 자료수로 나누어 그 나머지 값으로 저장할 위치를 정하는 방법.
    - 폴딩법(Folding) : 레코드 키 값을 여러 부분으로 나누고, 나눈 부분의 각 숫자를 더하거나 XOR한 값을 홈 주소로 사용하는 방법.
    - 제곱법(Square) : 레코드 키 값을 제곱한 결과 값의 일부를 선택하여 저장할 위치를 정하는 방법.
    - 중간 제곱법(Mid-Square) : 레코드 키 값을 제곱하고, 이 값의 중간 부분을 취하여 홈 주소로 취하는 해상 방법.
    - 숫자 분석법(Digit-Square) : 레코드 키 값을 이루는 숫자들의 분포를 파악해서 분포가 고른 부분을 선택해 저장할 위치를 정하는 방법.
    - 기수 변환법(Radix Transformation) : 레코드 키 값을 숫자의 진수를 다른 진수로 변환시켜 주소 크기를 초과한 높은 자릿수는 절단하고, 이를 다시 버킷의 개수 범위에 맞게 조정하는 방법.
    - 의사 무작위법(Pseudo-random) : 난수(무작위의 수)를 발생시킨 후 그 난수를 이용하여 저장할 홈 주소의 위치를 정하는 방법.
  - 충돌 해결 방법
    - 개방 주소법(Open Addressing) : 해싱 함수로 얻은 주소에 데이터가 이미 있다면 다른 주소에 저장하도록 하는 방법.
    - 체이닝(Chaning) : 한 주소에 여러 개의 데이터를 저장할 수 있도록 연결 리스트 기법을 이용하여 저장하는 방법.
  - 해싱 용어 정리
    - 해시 테이블(Hash Table)
    - 버킷(Bucket) 주소 : 자료가 저장될 공간. 하나의 버킷에는 여러 개의 자료를 기억할 슬롯이 존재함.
    - 슬롯(Slot) : 한 개의 자료를 저장할 수 있는 공간으로 n개의 슬롯이 모여 하나의 버킷을 형성함.
    - 동거자(Synonym, 동의어) : 서로 다른 키 값이지만 햇이 함수에 의해 같은 버킷에 저장되는 키 값들.
    - 충돌(Collision) : 서로 다른 키가 같은 홈 주소를 갖게 되는 현상.
    - 오버플로우(Overflow) : 버킷에 할당된 슬롯 수보다 많이 발생하게 되면 버킷에 더 이상 항목을 저장할 수 없는 경우에 발생.
    - 프로빙(Probing) : 충돌이 발생하여 더 이상 같은 홈 주소를 갖는 버킷을 사용할 수 없을 때 사용하지 않는 다른 버킷을 찾아 저장하는 방법으로 1차 조사법, 2차 조사법 등이 있음.
    - 체인법(Chaining) : 해싱에서 오버플로우 발생 시 이를 해결하기 위한 방법으로 연결 리스트를 사용하며 버킷의 크기에 제한을 두지 않는 기법.
- 그래프 탐색(Traversal)
  - 그래프의 모든 정점을 방문하는 것을 그래프 탐색이라고 함.
  - 너비 우선 탐색(BFS : Breadth First Search) : 큐 구조에 이용, 시작 노드에서 인접한 노드를 모두 알파벳순으로 큐에 삽입.
  - 깊이 우선 탐색(DFS : Depth First Search) : 스택 구조를 이용. 시작 노드에서 인접한 노드 중 하나를 알파벳순으로 스택에 삽입.
- B-트리
  - 대용량 파일을 효율적으로 검색하고 수정하기 위해서 사용되는 트리
  - 일반적으로 데이터베이스와 파일 시스템에서 사용되는 자료 구조

​	

### 1.5. 정렬(Sort)(하)

- 정렬의 개념
  - 여러 개의 자료를 순서에 따라 나열하는 방법.
  - 자료에 속한 키 값에 따라 오름차순과 내림차순으로 정렬함.
- 정렬의 종류
  - 선택 정렬 : 큰(작은) 키 값을 찾아 교환함.
  - 버블 정렬 : 인접한 키 값을 교환, 이미 정렬되었으면 중단함.
  - 삽입 정렬 : 자신의 위치를 찾아서 삽입함.
  - 쉘 정렬 : 삽입 정렬을 개선한 방법
  - 퀵 정렬 : 자료의 중간 값을 정한 후 정렬함.
  - 힙 정렬 : 이진 트리 구조를 만들어 정렬함.
  - 이진 병합 정렬 : 두 개의 키를 한 쌍으로 정한 후 정렬함.
  - 버킷 정렬 : 기수 값에 따라 분배하여 정렬하는 방식.
- 선택 정렬(Selection Sort)
  - 오름차순으로 정렬하였을 때 가장 작은 값을 찾아 선택된 위치 자료와 교환하는 방법.
  - 평균 시간 복잡도 : O(n^2)
- 버블 정렬(Bubble Sort)
  - 주어진 파일에서 인접한 두 개의 레코드 키 값을 비교하며 그 크기에 따라 레코드 위치를 서로 교환하는 정렬 방식.
  - 평균 시간 복잡도 : O(n^2)
- 삽입 정렬(Insertion Sort)
  - 첫 번째 자료를 기준으로 두 번째부터 차례로 비교하여 자기 위치 찾아 삽입하면서 정렬하는 방법.
  - 평균 시간 복잡도 : O(n^2)
- 쉘 정렬(Shell Sort)
  - 삽입 정렬을 보완한 알고리즘. 간격을 정하고 간격을 점차 줄이면서 삽입 정렬하는 방법.
  - 10개의 자료에서 간격이 5라면 1번째 자료와 6번째 자료와 비교함.
  - 자료 수가 n개 일 때 가장 이상적인 첫 간격은 1.72x3sqrt{10}
  - 평균 시간 복잡도 : O(n^1.5)
- 힙 정렬(Heap Sort)
  - 임의의 자료에서 최솟값 또는 최댓값을 구할 경우 가장 적합한 정렬 방법
  - 자료를 순서적으로 완전 이진 트리 형태로 만들어 정렬하는 방법.
  - 오름차순 정렬인 경우에 자노드가 부노드보다 크면 자료를 교환함.
  - 평균 시간 복잡도 : O(nlog_2n)
- 이진 병합 정렬(2-Way Merge Sort)
  - 정렬 대상의 자료들을 그룹별로 정렬하여 병합하면서 정렬하는 방법
  - 평균 시간 복잡도 : O(nlog_2n)
- 버킷 정렬(Bucket Sort)
  - 스택을 이용하여 정렬하는 방법
  - 레코드의 키 값을 분석하여 같은 값끼리 그 순서에 맞는 버킷에 분배하였다가 버킷의 순서대로 레코드를 꺼내어 정렬함.
  - 평균 시간 복잡도 : O(dn)
- 퀵 정렬(Quick Sort)
  - 레코드의 많은 자료 이동을 없애고 하나의 파일을 부분적으로 나누어 가면서 정렬하는 방식
  - 레코드의 키를 기준으로 작은 값은 왼쪽에 큰 값은 오른쪽 서브 파일로 분해하여 정렬하는 방식
  - 정렬 방식 중에서는 가장 빠른 방식이며, 프로그램에서 재귀적(Recursion, 되부름) 함수를 이용하기 때문에 스택을 필요로 함.
  - 평균 시간 복잡도 : O(nlog_2n)



### 1.6. 모듈 구현(하)

- 단일 모듈 구현
  - 구조적 프로그래밍에서 모듈은 부품화된 프로그램을 나타내고, 객체지향 프로그램에서는 객체 하나를 모듈이라고 함.
  - 컴포넌트는 여러 개의 모듈을 모아놓은 모듈들의 집합을 말함.
  - 단일 모듈 구현은 상세 설계된 단위 모듈이나 환경 설정 모듈을 실제 프로그래밍 언어로 구현하는 것.
  - 단위 모듈의 종류
    - 화면 모듈
    - 화면에서 입력받은 데이터를 처리하는 서비스 컴포넌트
    - 트랜잭션 컴포넌트
    - 비즈니스 컴포넌트
    - 내외부 인터페이스 컴포넌트
    - 데이터베이스 접근 컴포넌트
    - 암복호화 컴포넌트
  - 공통 모듈의 구현
    - 모든 서비스 컴포넌트 혹은 트랜잭션 컴포넌트가 공통적으로 사용하는 컴포넌트.
    - 공통 모듈은 인터페이스 컴포넌트, DB 접근 컴포넌트, 암복호화 컴포넌트를 모아 놓은 공통 컴포넌트가 됨.
  - 단위 모듈 구현 시 고려사항
    - 응집도는 높게, 결합도는 낮게 구현함.
    - 공통 모듈을 먼저 구현한 후, 개별 단위 모듈 구현 시 공통 모듈을 재사용함.
    - 항상 예외 처리 로직을 고려하여 구현함.
- 화면 모듈 구현
  - HTML5
    - 웹 애플리케이션에서 화면 단위 모듈화 프로그래밍은 일반적으로 HTML5를 기반으로 구현됨.
    - HTML5는 온라인, 모바일, 패드 등에서 수정 없이 자유롭게 재사용할 수 있음.
  - HTML5 화면 구조
    - Header
    - Div : 섹션 레이아웃을 만들 때 주로 사용함.
    - Nav
    - Section
    - Article : 문서 내에 별도의 글 표시. 블로그나 뉴스 본문 등을 나타내는 경우에 사용함.
    - Aside : 문서의 주 내용이 아닌 관련성이 낮은 내용을 나타냄.
    - Footer
  - 반응형 웹(Responsive Web)
    - 각 장치의 특성에 맞게 자동적으로 설정해주는 웹 기술.
    - 리액트, 뷰, 앵귤러 같은 웹 컴포넌트를 사용하여 반응형 웹 페이지를 구현함.4



## 2. 통합 구현 (2%)

### 2.1. 통합 구현 도구(하)

- IDE 도구
  - IDE 도구 : IDE(Intergrated Development Environment)는 프로그램 개발에 관련된 모든 정보를 하나의 프로그램 안에서 처리하는 환경을 제공하는 프로그램임.
  - IDE 도구의 기능
    - 개발 환경 지원
    - 컴파일 및 디버깅 기능 지원
    - 외부 모듈과 통합 기능 지원
  - IDE 도구의 종류
    - 이클립스
    - 라자루스
    - 비주얼 스튜디오
    - 안드로이드 스튜디오
    - 엑스 코드
    - IntelliJ IDEA
    - C++ 빌더
    - J 빌더
- 협업 도구
  - 하나의 소프트웨어 개발 프로젝트에 참여하는 수십 명에서 수천 명의 개발자가 서로 다른 작업 환경에서도 원활하게 프로젝트를 수행할 수 있도록 도와주는 협업을 위한 도구. 협업 소프트웨어, 그룹웨어 등으로 불림.
  - 협업 도구의 기능
    - 개발자 간 의사 교환
    - 일정 및 이슈 공유
    - 개발자 간 집단 지성 활용
  - 협업 도구의 분류
    - 문서 공유 도구
      - Google Drive
      - Slide
    - 소스 공유 도구
      - GitHub
    - 아이디어 공유 도구
      - Evernote
      - Invision
    - 디자인 공유 도구
      - Redpen
    - 마인드 맵핑 도구
      - Mind Meister
    - 프로젝트 관리 도구
      - Trello
      - Redmine
      - JIRA
      - Task World
    - 일정 관리 도구
      - Google Calendar
      - Confluence
- 형상 관리 도구
  - 소프트웨어 구성 관리(Software Configuration Management) 또는 형상 관리는 소프트웨어의 변경 사항을 체계적으로 추적하고 통제하는 것으로, 형상 관리는 일반적인 단순 버전 관리 기반의 소프트웨어 운용을 좀 더 포괄적인 학술 분야의 형태로 넓히는 근간을 의미함.
  - 형상 관리 도구 기능
    - check-out : 형상 관리 저장소로부터 최신 소프트웨어 형상을 개발자 PC로 가져오는 기능.
    - check-in : 개발자가 수정한 소스를 형상 관리 도구 저장소로 업로드하는 기능
    - commit : 개발자가 소스를 형상 관리 도구 저장소에 업로드한 후 최종적으로 업데이트가 되었을 때 형상 관리 서버에 반영되도록 하는 기능
    - update : 변경 사항이 있는 경우 서버 형상을 로컬 형상으로 가져오는 기능
    - import : 아무것도 들어있지 않은 저장소에 맨 처음 소스를 넣는 기능
    - export
  - 형상 관리 도구
    - CVS(Concurrent Versions System) : 오래됨. 직관 단순.
    - SVN(Subversion) : 작업 모듬 단위로 변경 관리. CVS 대체 도구.
    - Git : 분산형. Commit은 로컬 저장소에서 이루어짐.
    - Perforce(P4D) : 상용툴이지만 가장 빠름. 게임회사에서 많이 사용함.



### 2.2. 연계 통합 구현(하)

- 연계 통합 구현을 위한 요구사항 분석
  - 연계 요구사항 분석 시 입력물
    - 시스템 구성도 : 네트워크, 하드웨어, 시스템 소프트웨어
    - 응용 프로그램 구성
    - ERD(엔티티 관계도), 테이블 정의서
  - 연계 요구사항 분석 시 도구 및 기법
    - 사용자 인터뷰, 핵심 사용자 그룹 면담
    - 연계 분석 체크리스트
    - 설문지 및 설문 조사
    - 델파이 기법
    - 연계 솔루션 비교 분석 (EAI, ESB, Open API)
  - 연계 요구사항 분석 시 출력물
- 연계 데이터 식별 및 표준화 절차
  - 연계 범위 및 항목 정의
  - 연계 코드 매핑 및 정의
  - 변경된 데이터 구분 방식 정의
  - 데이터 연계 방식 정의
- 연계 통합 구현
  - 송수신되는 연계 데이터 형식은 데이터베이스의 테이블과 필드, 파일로 분류할 수 있음.



### 2.3. 연계 메커니즘(하)

- 연계 데이터 생성 및 추출
- 코드 매핑(Mapping) 및 데이터 변환
- 연계 테이블 또는 파일 생성
- 연계 테이블 또는 파일의 레이아웃 설계
- 로그(Log)파일 생성
- 연계 서버 또는 송신 어댑터
- 전송
- 연계 서버 또는 수신 어댑터
- 운영 데이터베이스에 연계 데이터 반영



### 2.4. 연계 장애 및 오류 처리 구현(하)

- 장애 및 오류 유형
  - 연계 시스템의 오류
  - 송신 시스템의 연계 프로그램 오류
  - 수신 시스템의 연계 프로그램 오류
  - 연계 데이터 자체의 오류
- 오류 코드 부여 규칙
  - 1자리(A) / 1자리(B) / 1자리(C) / 3자리(D)
  - A 오류 시 E
  - B 오류 발생 위치가 연계 서버이면 S, 연계 프로그램이면 A
  - C 데이터 구조 오류이면 F, 데이터 길이 오류이면 L
  - D 오류 유형 및 분류를 일련번호 000~999까지 부여
- 가용성 향상을 위한 이중화
  - Active-Active 방식 : 두 개의 WAS가 동시에 서비스 제공
  - Active-Standby 방식 : 두 개의 WAS 중 하나는 서비스 제공, 하나는 대기
- 재해 복구 시스템(DRS, Disaster Recovery System)
  - 미러 사이트(Mirror Site) : 전산센터와 재해복구센터 모두 액티브, 재해 발생시 RTO 없이 즉시 이루어짐.
  - 핫 사이트(Hot Site) : 원격지 Standby. 
  - 웜 사이트(Warm Site) : 중요성이 높은 정보 기술 자원만 부분적으로 재해복구센터에 보유하는 방식.
  - 콜드 사이트(Cold Site) : 데이터만 원격지에 보관하고, 최소한의 정보 기술 자원을 확보하고 있다가, 재해 시 데이터를 근간으로 필요한 정보 자원을 조달하여 정보 시스템을 복구하는 방식



### 2.5. 연계 모듈 구현 환경 구성 및 개발(하)

- EAI/ESB 방식
  - 회사 외 업무 시스템 / 회사 내 업무 시스템 / 정보 통신 업무 시스템의 통합.
  - EAI(Enterprise Application Integration)
    - 이 기종 시스템 간의 연동을 가능하게 함.
    - 어댑터 제공.
    - 단일 접점인 허브 시스템을 통해 시스템을 통합하는 중앙 집중식 방식.
    - 통합 범위는 기업 내부 업무
  - ESB(Enterprise Service Bus)
    - 웹 서비스 기반으로 통신이 표준화됨.
    - 별도의 어댑터가 필요 없음
    - 서비스 버스라는 백본을 이용하여 통신
    - XML 변환 이용함.
    - 통합 범위는 기업 내부와 기업 외부 업무
  - 연계 모듈 구현 환경 구축 절차
    - 연계 데이터베이스 또는 계정 생성
    - 연계를 위한 테이블 생성
    - 연계 응용 프로그램 구현 : 방식은 DBMS의 트리거 활용
- 웹 서비스(Web Service) 연동
  - 연동 종류
    - 시스템 연동
    - 데이터 연동
    - 인터페이스 연동
    - 웹 서비스 연동
  - 웹(Web) 기술
    - WWW
    - Web Browser
    - HTML
    - XML
  - HTML의 동작 원리
    - 요청
    - 웹 서버가 웹 문서 검색
    - ASP, PHP, JSP 등이 웹 문서 해석
    - 데이터베이스에 데이터 요구
    - 데이터베이스 결과 반환
    - ASP, PHP, JSP 등이 결과를 HTML 문서로 변환
    - 웹서버로 결과 전송
    - 클라이언트에게 응답
  - 웹 서비스의 기본 구조
    - SOAP(Simple Object Access Protocol) : HTTP, HTTPS, SMTP 등을 사용하여 XML 기반의 메시지를 네트워크상에서 교환하는 프로토콜.
    - UDDI(Universal Description Discovery, and Integration) : 웹 서비스를 찾기 위한 XML 기반의 표준.
    - WSDL(Web Service Description Language) : 웹 서비스를 기술하기 위한 표준 형식.
  - 웹 서버와 웹 응용 서버
    - 웹 서버(Web Server) : 웹 브라우저의 요청을 받아 HTML 문서 파일이나, 이미지, 자바스크립트의 정적 데이터를 제공. Apache, IIS, NginX 등
    - 웹 응용 서버(WAS : Web Application Server) : 서버에서 응용 프로그램이 동작할 수 있는 환경을 제공함. 동적 데이터를 서비스함.
    - ...





## 3. 제품 소프트웨어 패키징(12%)

### 3.1. 제품 소프트웨어 패키징(하)

- 제품 소프트웨어 패키징
  - 완료된 제품 소프트웨어를 고객에게 전달하기 위한 형태로 묶어내는 것.
  - 특성
    - 개발자 아닌 사용자 중심으로 진행
    - 식별 소스를 모듈화하여 상용 제품으로 패키징.
    - 버전 관리 및 릴리즈 노트를 통해 지속적으로 관리해 감
    - 범용 환경에서 사용이 가능하도록 일반적인 배포 형태로 분류하여 패키징 진행
  - 소프트웨어 빌드 : 소스 코드 파일을 컴퓨터에서 실행할 수 있는 제품 소프트웨어의 단위로 변환하는 과정이나 결과물을 말함.
  - 빌드 도구 : 빌드를 도와주는 유용한 유틸리티이며 이를 활용하여 컴파일 이외에도 완성을 위한 다양한 일을 할 수 있음. Ant, Make, Maven, Gradel 등이 있음.
- 사용자 중심의 패키징
  - 사용자 실행 환경 고려
  - 작업 수행 순서 : 기능 식별 -> 모듈화 -> 빌드 진행 -> 사용자 환경 분석 -> 패키징 적용 시험 -> 패키징 변경 개선
- 제품 소프트웨어의 패키징 도구
  - 디지털 콘텐츠의 지적 재산권을 보호하고 관리하는 기능을 제공, 안전한 유통과 배포를 보장하는 도구이자 솔루션.
- 패키징에서의 릴리즈 노트
  - SW 변경 사항을 기록, 설명하는 문서.



### 3.2. 제품 소프트웨어 매뉴얼(하)

- 설치 매뉴얼
  - 설치 시작부터 완료할 때까지의 전 과정을 빠짐없이 순서대로 설명.
  - 구성요소 : 개요, 설치 관련 파일, 설치 아이콘 설명, 프로그램 삭제 방법, 관련 추가 정보, 설치 버전 및 작성자 정보
- 사용자 매뉴얼



### 3.3. 제품 소프트웨어 버전 관리(하)

- 버전 관리
  - 코드와 라이브러리, 관련 문서 등 시간에 따른 변경을 관리하는 전체 활동.
  - 버전 관리 항목
    - 가져오기(Import) : 버전을 관리하지 않은 로컬 디렉토리 파일을 처음으로 저장소에 복사함.
    - 체크아웃(checkout) : 저장소에 파일을 받음.
    - 체크인(checkin) : 저장소를 새로운 버전으로 갱신함.
    - 완료(commit) : 체크인 중에 이전 갱신 사항이 있는 경우 충돌을 알림. Diff(차이 비교) 도구 이용하여 수정하거나 commit 과정을 수행함.
    - 저장소(Repository) : 파일의 현재 버전과 변경 이력 정보를 저장하는 저장소.
- 버전 관리 도구
  - 형상 관리를 포함하여 관리하는 도구.
  - 버전 관리 도구 유형
    - 공유 폴더 방식(RCS, SCCS)
    - 클라이언트/서버 방식(CVS, SVN)
    - 분산 저장소 방식(Git, Bitkeeper, Clear Case)
- 빌드 자동화 도구
  - 제품 소프트웨어의 실행 파일을 자동으로 만들어 주는 도구
  - 자동화 도구 종류
    - Jenkins 가장 많이 사용하는 인터넷상의 빌드 자동화 도구, Java 기반의 오픈소스로 IC를 가능하게 함.
    - Gradle 안드로이드 환경에 적합한 빌드 자동화 도구. 



## 4. 애플리케이션 테스트 관리(44%)

### 4.1. 애플리케이션 테스트(하)

- 테스트 개념
  - 개념 : 구현된 소프트웨어를 대상으로 오류를 찾아내는 작업.
  - 필요성
    - 오류 발견 관점
    - 오류 예방 관점
    - 품질 향상 관점
  - 테스트 관련 용어
    - 디버그(Debug) : 논리적 오류 찾는 과정. 디버그는 찾아낸 오류를 수정하는 것.
    - 디버거(Debugger) : 디버그를 돕는 도구.
    - 워크스루(Walk-Through) : 소프트웨어 생명주기의 각 단계마다 산출된 명세서를 가지고 오류를 찾아내는 비정형 검토회의
    - 정형 기술 검토(FTR)의 검토 지침 : 제품 검토의 집중성, 사전 준비성, 의제의 제한성, 안건 고수성, 논쟁 반박의 제한성, 문제 공개성, 참가 인원의 제한성, 문서성.
  - 테스트의 원칙
    - 테스트는 계호기 단계부터 해야 함.
    - 결함을 밝히는 활동임.
    - 개발자가 테스트하지 않음.
    - 효율적인 결함 제거 법칙을 사용함.
      - 낚시의 법칙 : 제품 결함은 특정 기능, 모듈, 라이브러리에서 많이 발견됨.
      - 파레토의 법칙 : 결함의 80%는 코드의 20%에 집중되어 있음.
    - 완벽한 테스트는 불가능함.
    - 결함 집중(Defect Clustering)을 고려함. 대부분 소수의 특정한 모듈에 결함 집중됨.
    - 살충제 패러독스(Pesticide Paradox)를 고려함. 주기적으로 테스트 케이스 점검하고 개선.
    - 오류-부재의 궤변(Absence of Errors Fallacy)을 고려함. 오류 없어도 사용자 요구사항 만족 못하면 쓸모없음.
- 테스트 프로세스
  - 일반적인 테스트 프로세스
    - 테스트 계획
    - 테스트 분석 및 디자인
    - 테스트 케이스 및 시나리오 작성
    - 테스트 수행
    - 테스트 결과 평가 및 리포팅
  - 테스트 수행 절차
    - 단위 테스트
    - 통합 테스트 : 가장 오류가 많이 발견됨. 프로그램만을 대상으로 테스트. 인터페이스 연계 검증.
    - 시스템 테스트 : 프로그램 전체가 정상 작동하는지 점검.
    - 인수 테스트 : 사용자 요구분석 명세서에 나타난 사항을 모두 충족하는지 판단함.
    - 설치 테스트 : 사용자 컴퓨터에 설치하여 테스트
- 테스트 기법
  - 테스트 분석 기법
    - 동적 분석 테스트 : 프로그램의 실행을 요구하는 테스트. 화이트박스 테스트와 블랙박스 테스트가 있음.
    - 정적 분석 테스트 : 소스 코드 구조 분석 논리적으로 검증. 인스펙션, 코드테스트, Walk-Through 등이 있음.
  - 테스트 실행 기법
    - 화이트박스 테스트(White Box Testing, 투명 상자) : 프로그램의 내부를 보면서 테스트 수행.
    - 블랙박스 테스트(Black Box Testing, 불투명 상자) : 프로그램의 외부 사용자 요구사항 명세를 보면서 테스트. 프로그램 동작만으로 오류 찾기.
  - 테스트 설계 기법
    - 구조 기반 설계 테스트 : 내부 논리 흐름에 따라 테스트 케이스를 작성하고 확인하는 테스트. 구문기반,결정기반,조건기반,조건결정기반,변경조건결정기반,멀티조건기반커버리지 테스트 등
    - 명세 기반 설계 테스트 : 주어진 명세를 빠짐없이 테스트 케이스로 구현하고 있는지를 확인하는 테스트. 동등분할,경계값분석,결정테이블,정형명세기반,유스케이스,유한상태기계기반,페어와이즈,직교배열테스트 등
    - 경험 기반 설계 테스트 : 유사 소프트웨어나 유사 기술 평가에서 테스터의 경험을 토대로 한 직관과 기술 능력을 기반으로 수행하는 테스트를 말함.



### 4.2. 단위 테스트(중)

핵심포인트 : 드라이버 / 스터브 / 화이트박스 / 블랙박스 / 기초 경로 테스트 / 복잡도 경계값 테스트 / 단위 디버깅의 자동화 도구

- 단위 테스트
  - 개념 : 원시 프로그램의 모듈이나(함수, 프로시저, 독립적인 루틴 등) 컴포넌트 대상으로 화이트박스 테스트를 실시하는 방법.
  - 단위 테스트 수행 방법
    - 화이트박스 테스트 : 기본 방법.
    - 메소드 기반 테스트 : 메소드에 매개변수를 다르게 호출하면서 테스트해봄
    - 화면 기반 테스트 : 사용자 화면에 직접 데이터를 입력하여 테스트 수행
    - 드라이버와 스터브 활용 : 모듈 개발이 안 된 경우 이용. 사용자 화면이 없는 경우 드라이버 이용, 하위 모듈이 개발 안되었을 경우 스터브(시험용 모듈) 이용.
- 테스트 실행 기법
  - 화이트박스 테스트
    - 개념 : 단위 테스트 기본. 소스 코드의 모든 문장을 한 번  이상 수행.
    - 종류
      - 기초 경로 테스트
      - 루프 테스트
      - 데이터 흐름 테스트
      - 조건 커버리지
    - 화이트박스 테스트를 통해 찾을 수 있는 오류 및 예
      - 세부적 오류
      - 논리 구조상의 오류
      - 반복문 오류
      - 수행 경로 오류
      - 알고리즘 오류에 따른 원치 않는 결과
      - 탈출구가 없는 반복문의 사용
      - 틀린 계산 수식에 의한 잘못된 결과
  - 블랙박스 테스트
    - 개념
      - 프로그램의 외부 사용자 요구사항 명세를 보면서 테스트함.
    - 종류
      - 동등(균등) 분할
      - 경계값 분석
      - 오류 예측
      - 원인 결과 그래프
      - 비교 테스트
    - 블랙박스 테스트를 통해 찾을 수 있는 오류 및 예
      - 인터페이스 오류
      - 자료 구조상의 오류
      - 성능 오류
      - 시작과 종결상의 오류
      - 부정확하거나 빠진 오류
      - 비정상적인 자료를 입력해도 오류 처리를 수행하지 않는 경우
      - 정상적인 자료를 입력해도 요구된 기능이 제대로 수행되지 않는 경우
      - 경계값을 입력할 경우 요구된 출력 결과가 나오지 않는 경우
  - 기초 경로 테스트
    - 흐름 도표를 작성
    - 복잡도를 계산 (E-V+2, E는간선V는노드
    - 복잡도 나온 만큼 테스트 횟수 설계
    - 복잡도의 판정 : 5이하 단순, 6~10 안정적, 20이상 복잡, 50이상 불안정하므로 다시 작성
  - 경계값 테스트
    - 범위의 한계 부분을 집중적으로 검사하는 경우를 정하여 검사하는 방법.

- 명세 기반 설계 테스트
  - 동등 분할 테스트(Equivalence Partitioning) : 정상과 비정상 데이터 50%씩 정해서 입력해보는 방식
  - 경계값 분석(Boundary Value Anaysis)
  - 결정 테이블(Decision table) 테스트 : 어떤 결정을 위해 생각할 수 있는 복잡한 모든 조건과 각 조건에 대하여 취해야 할 행동을 모두 열거한 테이블. 행동 모두 대입하여 검사.
  - 정형 명세 기반(Formal Specification base) 테스트 : 테스트를 주기적으로 수행함으로써 오류를 발견 및 제거하는 방법으로 이해 관계자들 사이의 충분한 의사소통을 통해 오해의 소지를 없애는 방법.
  - 유스케이스(Usecase) 테스트 : 프로그램의 흐름에 따라 테스트 시나리오를 기반으로 테스트하는 방법.
- 단위 디버깅
  - 단위 테스트를 수행하여 논리적인 오류가 발견되었을 때 수행하는 과정.
  - 단위 디버깅의 자동화 도구
    - JUnit : java기반
    - CppUnit : C++기반
    - Unitest : Python기반



### 4.3. 통합 테스트(하)

핵심포인트 : 빅뱅 방식 / 하향식 통합 / 상향식 통합

- 통합 테스트
  - 개념 : 소프트웨어 각 모듈 간의 인터페이스 관련 오류 및 결함을 찾아내기 위한 체계적인 테스트 기법.
  - 순서
    - 케이스 설계
    - 데이터 준비
    - 수행 및 결과 확인
    - 결함 등록
    - 테스트 결과 보고 및 종료
  - 통합 테스트 수행 방법
    - 점증적인 방식
    - 빅뱅 방식(비점증적인 방식)
- 점증적인 통합 테스트
  - 하향식 통합(Top Down) 테스트
    - 메인 제어 모듈로부터 아래 방행으로 제어의 경로를 따라 이동하면서 하향식으로 통합하면서 테스트
    - 하향식 통합 테스트 순서
      - 메인 제어 모듈은 작성된 프로그램을 사용
      - 아직 작성되지 않은 하위 제어 모듈 및 모든 하위 컴포넌트를 대신하여 더미 모듈인 스터브를 개발
      - 깊이 우선 방식, 너비 우선 방식에 따라 하위 모듈인 스터브가 한 번에 하나씩 실제 모듈로 대체함.
      - 각 모듈 또는 컴포넌트를 통합하면서 테스트
      - 테스트가 완료되면 스터브는 실제 모듈 또는 컴포넌트로 대치
  - 상향식 통합(Bottom Up) 테스트
    - 애플리케이션 구조에서 최하위 레벨의 모듈 또는 컴포넌트로부터 위쪽 방향으로 제어의 경로를 따라 이동하면서 ㅔㅌ스트
    - 상향식 통합 테스트 순서
      - 최하위 레벨의 모듈 또는 컴포넌트들이 하위 모듈의 기능을 수행하는 클러스터(Cluster)로 결합함.
      - 상위의 모듈에서 데이터의 입력과 출력을 확인하기 위한 더미 모듈인 드라이버를 작성함.
      - 통합된 클러스터 단위를 테스트함.
      - 테스트가 완료되면 각 클러스터들은 프로그램의 위쪽으로 결합되며, 드라이버는 실제 모듈 또는 컴포넌트로 대체함.
  - 하향식과 상향식 비교
    - 테스트할 때 하향식은 프로그램 전체를 실행할 수 있지만 상향식은 불가능
    - 테스트할 때 하향식은 독립적인 구조를 갖지만 상향식은 불가능
    - 테스트할 때 하향식은 가짜 모듈 필요하지만 상향식은 필요 없음
    - 테스트할 때 하향식은 드라이버 필요 없지만 상향식은 필요함
    - 테스트할 때 하향식은 하위 모듈 클러스터 형성 필요 없지만 상향식은 필요함
    - 하향식은 중요 모듈을 우선 테스트할 때 부적당하고 하향식은 중요 모듈을 우선 테스트할 때 적당함.



### 4.4. 시스템 테스트(중)

핵심포인트 : V-모델 / 회귀 테스트 / 알파 테스트 / 베타 테스트 / 테스트 산출물 / 자동화 도구 / 테스트 장치 구성 / 테스트 오라클 / 테스트 시나리오 / 테스트 조건

- 시스템 테스트
  - 개념 : 단위, 통합 검사를 수행한 후 소프트웨어 단위로 테스트함. 프로그램 전체가 정상적으로 작동하는지 점검.
  - V-모델과 테스트 단계
    - 요구사항 분석 - 인수 테스트
    - 기능명세 분석 - 시스템 테스트
    - 설계 - 통합 테스트
    - 개발 - 단위 테스트
    - 테스트 계획 및 설계 - 테스트 수행
  - 시스템 테스트의 수행 방법
    - 요구사항 테스트(검증 테스트)
    - 무결성 테스트
    - 부피 테스트
    - 메모리 테스트
    - 성능 테스트
    - 신뢰성 테스트
    - 부하(Stress, 강도) 테스트
    - 수락(Acceptance) 테스트
    - 회복(Recovery) 테스트
    - 안전(Security) 테스트
    - 구조(Structure) 테스트
    - 회귀(Regression) 테스트 : 결함 수정 후 수정된 프로그램과 관련 프로그램 함께 테스트하는 방법.
    - 병행(Parallel) 테스트
- 인수 테스트
  - 개념
    - 사용자가 참여하는 테스트
  - 인수 테스트 수행 방법
    - 알파 테스트 : 사용자를 제한된 환경으로 초대하여 프로그램을 수행하게 하고 개발자는 사용자가 어떻게 수행하는가를 지켜보며 오류를 찾는 테스트임. 오류와 사용상의 문제점을 사용자와 개발자가 함께 확인하면서 검사함.
    - 베타 테스트 : 다수의 사용자를 제한되지 않은 환경에서 프로그램을 사용하게 하고 오류가 발견되면 개발자에게 통보하는 방식의 테스트 방법.
- 테스트 산출물
  - 테스트 산출물
    - 테스트 계획서
    - 테스트 케이스
    - 테스트 시나리오
    - 테스트 결과서
  - 테스트 명세서
    - 테스트 결과 정리
    - 테스트 요약 문서
    - 품질 상태
    - 테스트 결과서
    - 테스트 실행 절차 및 평가

- 테스트 자동화 도구
  - 정적 분석 도구
    - 작성된 소스 코드를 실행시키지 않음.
    - 소스 코드에 있는 오류나 잠재적인 오류를 찾아내기 위한 활동
  - 동적 분석 도구
    - 작성된 소스 코드를 실행함
    - 메모리 누구, 스레드 결함 등을 분석
  - 테스트 실행 도구(Test Execution Tools)
    - 데이터 주도 접근 방식 vs 키워드 주도 접근 방식
  - 성능 테스트 도구(Performance Test Tools)
    - 성능 테스트 도구 성능 목표를 달성하였는지를 확인하는 도구
    - 애플리케이션의 처리량, 응답 시간, 경과 시간, 자원 사용률에 대해 가상의 사용자를 생성하고 테스트를 수행함.
  - 테스트 통제 도구(Test Control Tools)
  - 테스트 장치(Test Driver) 구성
    - Driver
    - Stub
    - Suites
    - Test Case
    - Test Script
    - Mock Object
- 테스트 케이스
  - 개념
    - 명세 기반 테스트를 위한 설계 산출물.
    - 실제로 다양한 데이터를 입력하여 그 결과를 확인해 보는 과정
    - 인터페이스 테이블이나 파일 단위로 작성함
    - 테스트 케이스의 가장 핵심적인 사항은 테스트 항목의 도출
  - 테스트 케이스의 구성 항목(IEEE 829)
    - 식별자 : 항목 식별자, 일련번호임
    - 테스트 항목 : 테스트할 모듈 또는 기능임
    - 입력 명세
    - 출력 명세
    - 환경 설정
    - 특수 절차 요구
    - 의존성 기술
- 테스트 오라클(Test Oracle)
  - 텍스트의 결과가 참인지 거짓인지를 판단하기 위해서 사전에 정의된 참값을 입력하여 비교하는 기법 및 활동
  - 테스트 오라클의 유형
    - 참(True) 오라클
    - 샘플링(Sampling) 오라클
    - 휴리스틱(Heuristic) 오라클
    - 일관성 테스트(Consistent Test) 오라클
  - 테스트 오라클의 적용 방안
    - 참 오라클은 항공기, 발전소 등 작은 실수만으로 치명적인 결과를 초래하는 업무에 적용함
    - 샘플링/추정 오라클은 일반, 게임 등의 일반적인 업무에 적용함.

### 4.5. 테스트 결과 분석(하)







### 4.6. 연계 테스트 및 검증(하)







### 4.7. 테스트 커버리지(하)







### 4.8. 성능 분석 및 품질 평가(중)



## 5. 인터페이스 구현(14%)

### 5.1. 인터페이스 설계 명세







### 5.2. 인터페이스 구현 검증














