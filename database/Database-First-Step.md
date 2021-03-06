# 데이터베이스 첫걸음

<br>

> 이 문서는 [<데이터베이스 첫걸음 - 미크, 기무라 메이지>](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788968487316&orderClick=LEa&Kc=) 책을 보고 정리한 문서입니다.

<br>

## 데이터베이스란

- 최근에는 폭증하는 데이터양과 더불어 다양한 데이터가 데이터베이스에 기록되어 시스템에서 처리됨.
- 데이터베이스의 4가지 기본 기능
  - 데이터 조작(검색/등록/수정/제거)
  - 동시성 제어
  - 장애 대응
  - 보안
- 우리가 이용하는 시스템에서는 '반드시'라고 할 정도로 데이터베이스가 사용되고 있지만, 데이터베이스는 사용자로부터 보이지 않도록 깊숙이 숨겨져 있음.
- 데이터베이스에는 몇 가지 종류가 있지만, 현재 주류를 이루는 것은 '관계형 데이터베이스'임.

- NoSQL 데이터베이스 : Not only SQL의 줄임말, 관계형 데이터베이스에 있는 기능 일부를 버려서 성능을 높임. 대량의 데이터를 고속으로 처리해야 하는 웹 서비스와 잘 맞아 최근 자주 이용됨.
- 홀러리스가 발명한 펀치 카드 시스템(천공카드) : 구멍을 뚫은 종이를 기계가 읽게 함으로써 데이터 처리를 자동화하는 시스템으로, 현재 컴퓨터도 기본적으로는 이 시스템과 동일한 아이디어를 채용하고 있음. 홀러리스가 설립한 회사가 현재의 IBM으로 발전.

<br>

## 관계형 데이터베이스란

- 관계형 데이터베이스(RDBMS)에서 '관계'는 '2차원 표'라는 의미임.
- RDBMS에는 2차원 표를 '테이블'이라고 부름.
- RDBMS는 데이터를 직관적으로 관리하기 위한 언어인 'SQL'이 있음. 이 SQL을 사용하여 전문가가 아니더라도 데이터를 조작할 수 있음.
- '데이터베이스'와 'DBMS'는 같은 의미로 사용되는 경우가 많지만, 원래눈 '추상'과 '구상'이라는 차이가 있음.
- 데이터베이스는 OS와 애플리케이션 사이에 낀 미들웨어임.

- 사용 시스템 개발할 때 자주 사용되는 OS
  - 카테고리 : 실제 OS 제품
  - Windows : Windows Server 시리즈
  - Linux : Red Hat, CentOS, Debian(데비안), Ubuntu(우분투)
  - UNIX : HP-UX, AIX, Solaris

- DBMS
  - Oracle
  - SQL Server
  - DB2
  - PostgreSQL
  - MySQL
  - Firebird

<br>

## 데이터베이스 초기비용과 운영비용

- DBMS에 한정하지 않고, 소프트웨어나 하드웨어에 드는 비용은 초기비용과 운영비용으로 분류할 수 있음.
- 소프트웨어의 초기비용은 주로 라이선스료, 운영비용은 기술지원 비용(유지보수 비용)이 됨.
- 초기비용이 없고 운영비용만으로 운용하는 서브스크립션이라는 과금 형태도 있음. 집에 비유하면 임대 모델과 유사함.
- 인간의 현재지향 편향을 이용하여 초기비용을 운영비용에 포함하여 이익을 회수하는 수단은 과금 방식에서 상투적인 수단임. 구체적인 총 비용을 계산하여 선택해야 함.

- 기술 지원이 없으면 사고가 일어났을 때 자력으로 해결해야 하고, 그것대로 비용이 들며 해결할 수 있다는 보장도 없음. 더 중요한 점은 이 경우 장애의 책임을 자신의 조직이 져야 한다는 것. 유상으로 기술지원 서비스를 받는 이유의 하나는 이와 같은 '책임의 분산'이라는, 기술적인 이유보다는 보험으로써의 의미도 있음.
- 초기비용 없음 + 운영비용 있음 : 오픈소스 소프트웨어 이용하는 것. 라이선스료는 무료로 하고 '기술지원료'만을 유상으로 하거나 서브스크립션 형식을 취할 수 있음.
- Paas(Platform as a Service) : 미들웨어까지 포함한 클라우드 서비스의 임대 모델. 예를 들면 Amazon사가 제공하는 클라우드 서비스인 AWS에서는 클라우드에 준비된 Oracle이나 MySQL을 이용할 수 있는 서비스가 함께 제공됨. 이처럼 데이터베이스 그 자체를 '임대'하는 구조도 점점 많아지고 있음.

<br>

## 데이터베이스와 아키텍처 구성

- 아키텍처 : 시스템을 만들기 위한 물리 레벨의 조합. 어떤 기능을 가진 서버를 준비하고 어떠한 저장소나 네트워크 기기와 조합해서 시스템 전체를 만들 것인가. 하드웨어와 미들웨어의 구성을 가리킴.
- 아키텍처 설계 : 하드웨어와 미들웨어의 구성을 시스템이 완수해야 할 목적과 비교하면서 결정해 가는 것.
- 아키텍처의 역사
  - Stand-alone : 데이터베이스가 동작하는 머신이 LAN이나 인터넷 등의 네트워크에 접속하지 않고 독립되어 동작하는 구성. 물리적으로 떨어진 장소 접근 불가, 복수 사용자 동시 작업 불가, 가용성 낮음, 확장성 부족, 구축 간단하고 보안 매우 높음.
  - 클라이언트/서버 : 계층 분리하여 상호 간에 네트워크로 접속. 먼 곳에서 이용 가능, 복수의 사용자 동시 작업 가능, 보안 위험함, 관리비용 높음.
  - Web 3계층
    - 웹 서버 계층 : 클라이언트로부터 접속 요청(HTTP 요청)을 직접 받아서 그 처리를 뒷단의 애플리케이션 계층(애플리케이션 서버)에 넘기고 그 결과를 클라이언트에 반환. 애플리케이션 서버와 클라이언트 웹 브라우저의 가교 역할을 함. 아파치(Apache), IIS(Internet Information Services), NginX가 유명함.
    - 애플리케이션 계층 : 비즈니스 로직을 구현한 애플리케이션이 동작하는 계층. 웹 서버로부터 연계된 요청을 처리하고, 필요하면 데이터베이스 계층(DB 서버)에 접속해서 데이터를 추출하고 이를 가공한 결과를 웹 서버로 반환함. 톰캣(Tomcat), 웹로직(WebLogic), 웹스피어(WebSphere) 등이 유명함.
    - 데이터베이스 계층.
  - Web 3계층은 사용자로부터 직접적인 접속 요청을 받는 역할을 웹 서버 계층에 한정하여 애플리케이션 계층과 데이터베이스 계층의 보안을 높일 수 있음.

- 클러스터링 : 동일한 기능의 컴포넌트를 병렬화하는 것.
- 클러스터 구성으로 시스템의 가동률을 높이는 것을 '여유도(Reducdancy)를 확보한다' 또는 '다중화'라고 말함.
- 서버 증설에는 수확 체감의 법칙이 존재함. 서버 대수가 증가하면 증가할수록 1대를 추가함에 따라 얻을 수 있는 가동률의 향상 폭이 작아짐.
- 단일 장애점(SPOF, Single Point of Failure) : 다중화되어 있지 않아서 시스템 전체 서비스의 계속성에 영향을 주는 컴포넌트.
- 시스템 세계에서 가용성 99%는 1%의 시간은 이용 불가능한 상황이 된다는 의미이므로 1년 중 3일 15시간 36분은 서비스 다운이 일어날 수 있음을 의미하고 이는 가용성이 상당히 낮은 것임.

- DB 서버의 다중화 종류. 위로 갈수록 가용성과 성능이 좋지만 가격이 비쌈.
  - Active-Active : 클러스터 구성하는 컴포넌트 동시에 가동 가능
  - Active-Standby(Hot Standby) : 클러스터 구성하는 컴포넌트 중 실제 가동은 Active, 남은 것은 Standby(대기)하고 있음. 평소에도 Standby DB가 작동하는 구성.
  - Active-Standby(Cold Standby) : 클러스터 구성하는 컴포넌트 중 실제 가동은 Active, 남은 것은 Standby(대기)하고 있음. 평소에는 작동하지 않다가 Active DB가 다운된 시점에 작동하는 구성.
- 리플리케이션 : DB 서버와 저장소 세트를 복수로 준비하는 것. DB 서버와 저장소가 동시에 사용 불능일 때, 예를 들어, 지진이나 태풍 등으로 하드웨어가 설치된 시설이 파괴된 경우에 다른 1세트가 멀리 떨어진 지점에 놓여 있다면 서비스를 계속하는 것이 가능하다는 점에서 매우 가용성이 높은 아키텍처라고 할 수 있음.
- Active DB와 Standby DB 사이의 동기화 갱신 주기와 성능 사이에 트레이드오프 관계가 발생함. 갱신 주기가 길면 성능을 향상시킬 수 있지만 저장소 망가진 경우 최대 갱신 주기만큼의 데이터가 소실됨.
- MySQL에서는 동기화하는 측의 부모(Active) 데이터베이스를 '마스터', 동기화되는 측의 자식(Standby) 데이터베이스를 '슬레이브'라고 부름. 이 마스터와 슬레이브에 따른 리플레케이션을 '마스터 슬레이브 방식'이라고 부름.
- Shared Disk : 복수의 서버가 1대의 디스크를 사용하는 구성. Active-Active 구성의 DB는 복수의 서버가 1대의 디스크(저장소)를 공유하도록 구성되어 있어 저장소 부분이 병목되는 경우도 있음.
- Shared Nothing : 네트워크 이외의 자원을 모두 분리하는(아무것도 공유하지 않는) 방식. 이 아키텍처는 서버와 저장소의 세트를 늘리면 병렬처리 때문에 선형적으로 성능이 향상되는 장점이 있음. Google은 자사가 개발한 Shared Nothing 구조를 샤딩(Sharding)으로 부름.

<br>

## DBMS를 조작할 때 필요한 기본 지식

- 데이터베이스를 조작하는 수단은 SQL과 관리 명령이 있음.
- 데이터베이스는 4계층 또는 3계층으로 구조화되어 있음.
- 계층은 위에서부터 차례로 인스턴스 -> 데이터베이스 -> 스키마 -> 오브젝트임.
  - 인스턴스(Instance) : 물리적 개념으로 DBMS가 동작할 때의 단위. OS 입장에서는 '프로세스'라고도 부름.
  - 데이터베이스(Database) : 인스턴스와 스키마 사이 계층. 데이터를 관리하는 기능의 집합체.
  - 스키마(Schema) : 폴더에 해당함. 복수 개의 테이블로 구성됨.
  - 오브젝트(Object) : 인덱스, 저장 프로시저 등 데이터베이스에 보존된 것들을 총칭해서 오브젝트라고 함. 테이블도 오브젝트의 일종임.
- 3계층 구조는 데이터베이스와 스키마 계층을 실질적으로 생략하고 있음(Oracle과 MySQL).

<br>

## 트랜잭션과 동시성 제어

- 트랜잭션이란 복수 쿼리를 일관된 형태의 한 단위로 묶은 것을 말함.
- DBMS의 트랜잭션은 'ACID'의 특성이 있음.
  - Atomicity(원자성)
    - 데이터의 변경을 수반하는 일련의 데이터 조작이 전부 성공할지 전부 실패할지를 보증하는 구조.
    - 절차가 모두 잘 진행되면 트랜잭션에서는 절차를 처리한 후에 'COMMIT'을 실행해 처리를 확정함. 이 경우 각 데이터의 조작은 영구적으로 저장되어 결과가 손실되지 않음.
    - 절차 처리 도중 오류가 발생할 경우 'ROLLBACK'을 실행해 되돌려서 처음부터 다시 시작함.
    - 트랜잭션은 원자성(Atomicity)에 의해 전부 성공하거나 전부 실패하는 원칙으로 동작함.
  - Consistency(일관성)
    - 일련의 데이터 조작 전후에 그 상태를 유지하는 것을 보증하는 것.
    - 사용자 식별 번호에 유일성 제약을 설정하면 중복된 사용자 번호를 저장할 수 없는 것.
  - Isolation(고립성 또는 격리성)
    - 일련의 데이터 조작을 복수 사용자가 동시에 실행해도 '각각의 처리가 모순없이 실행되는 것을 보증한다'는 것.
    - 테이블에 대해 잠금(Lock)을 걸어서 후속 처리를 블록(Block)하는 방법.
    - 후속 처리는 해당 잠금이 해제될 때(COMMIT 또는 ROLLBACK)까지 대기하게 됨.
    - 고립성(Isolation)에 따라 병렬 실행이 일관성 있게 수행됨.
  - Durability(지속성) : 일련의 데이터 조작을 완료하고 완료 통지를 사용자가 받는 시점에서 그 조작이 영구적이 되어 그 결과를 잃지 않는 것을 나타냄.
- 트랜잭션의 4가지 격리 수준
  - 커밋되지 않은 읽기(Read Uncommitted) : 더티 읽기, 애매한 읽기, 팬텀 읽기
  - 커밋된 읽기(Read Committed) : 애매한 읽기, 팬텀 읽기
  - 반복 읽기(Repeatable Read) : 팬텀 읽기
  - 직렬화 기능(Serializable)
  - 직렬화 기능이 가능 엄격하고 위로 갈수록 완화됨. 커밋되지 않은 읽기가 가장 완화된 격리 수준.
- 격리 수준 완화에 따라 일어나는 현상
  - 더티 읽기(Dirty Read) : 어떤 트랜잭션이 커밋되기 전에 다른 트랜잭션에서 데이터를 읽는 현상.
  - 애매한 읽기(Fuzzy/NonRepeatable Read) : 어떤 트랜잭션이 이전에 읽어 들인 데이터를 다시 읽어 들일 때 2회 이후의 결과가 1회 때와 다른 현상.
  - 팬텀 읽기(Phantom Read) : 어떤 트랜잭션을 읽을 때 선택할 수 있는 데이터가 나타나거나 사라지는 현상.
- 실제 운용에서는 커밋된 읽기 또는 반복 읽기의 격리 수준을 이용함.
- 데이터 정의 언어(DDL, Data Definition Language) : 스키마 또는 테이블 등을 작성하거나 제거함. CREATE, DROP, ALTER 등
- 데이터 조작 언어(DML, Data Manipulation Language) : 테이블의 행을 검색, 조작함. SELECT, INSERT, UPDATE, DELETE 등
- 데이터 제어 언어(DCL, Data Control Language) : 데이터베이스에서 실행한 변경을 확정하거나 취소하는데 사용. COMMIT, ROLLBACK 등
- 실무에서 사용하는 SQL 문 대부분은 DML임.
- MVCC(Multi Versioning Concurrency Control)에 따른 MySQL의 특성
  - 읽기를 수행할 경우 갱신 중이라도 블록되지 않음.(읽기와 읽기도 서로 블록되지 않음)
  - 읽기 내용은 격리 수준에 따라 내용이 바뀌는 경우가 있음.
  - 갱신 시 배타적 잠금을 얻음. 잠금은 기본적으로 행 단위로 얻으며 트랜잭션이 종료할 때까지 유지함.
  - 갱신과 갱신은 나중에 온 트랜잭션이 잠금을 획득하려고 할 때 블록됨. 일정 시간을 기다리며 그 사이에 잠금을 획득할 수 없는 경우에는 '잠금 타임아웃(Lock Timeout)'이 됨.
  - 갱신하는 경우 갱신 전의 데이터를 UNDO 로그로 '롤백 세그먼트'라는 영역에 유지함. 이 'UNDO 로그'는 용도가 2가지임. 첫 번째는 갱신하는 트랜잭션의 롤백 시 갱신 전으로 되돌리는 것이고, 두 번째는 복수의 트랜잭션으로부터 격리 수준에 따라 대응하는 갱신 데이터를 참조하는 데 이용함.
- MySQL에서는 교착 상태가 일어나면 이를 즉시 인식해 시스템에 영향이 작은 쪽의 트랜잭션을 트랜잭션 개시 시점까지 롤백함.
- 하지 말아야 하는 트랜잭션의 종류
  - 오토커밋 : 쿼리 단위로 커밋하는 설정.
  - 긴 트랜잭션

<br>

## 테이블 설계의 기초

- 테이블은 공통 속성을 가진 것의 집합임. 테이블은 현실 세계를 반영함.
- 테이블은 객체 지향 언어에서 '메소드를 뺀 클래스'임.
- 테이블은 고유한 기본키를 가지고 있음. 기본키는 NULL값을 가질 수 없고 중복 불가능함.
- 테이블은 함수임. 기본키를 입력으로 받으면 그 외의 열 값을 출력함. 
- 제1정규형(1Normal Form, 1NF) : 테이블 셀에 복합적인 값을 포함하지 않음. 관계형 데이터베이스의 테이블은 전부 제1 정규형을 자동으로 만족함.
- 제2정규형(2NF) : 제1정규형을 만족하면서 모든 열이 완전 종속 관계를 가지는 것.
- 제3정규형(3NF)
- 정규화를 해서 테이블 수가 증가하는 경우 ER 다이어그램을 그려서 정리하면 좋음.

<br>

## 백업과 복구

- 데이터베이스에는 ACID 특성 중 'Durability(지속성)'에 의해 커밋된 내용이 영속화되기 때문에 데이터를 잃어버리는 경우는 없음.
- 데이터 파일이 있는 파일 시스템의 파손이나 서버, 디스크의 물리 장애에 대비하기 위해 '백업' 프로세스가 필요함.
- 백업 관점에서 DBMS 정지 여부(콜드와 핫), 형식(논리와 물리), 범위(풀과 차등/증분)가 있음.
  - 핫 백업 : '온라인 백업'이라고도 하며 백업 대상의 데이터베이스를 정지하지 않고 가동한채로 백업 데이터를 얻음.
  - 콜드 백업 : '오프라인 백업'이라고도 하며 백업 대상의 데이터베이스를 정지한 후 백업 데이터를 얻음.
  - 논리 백업 : SQL 기반의 텍스트 형식으로 백업 데이터가 기록됨.
  - 물리 백업 : 데이터 영역을 그대로 덤프(데이터 파일이나 화면에 출력)하는 이미지로 바이너리 형식으로 기록됨.
  - 풀 백업 : '전체 백업'이라고도 하며 데이터베이스 전체 데이터를 매일 백업하는 방식.
  - 부분 백업 : 풀 백업을 한 후에 이후 갱신된 데이터를 백업.
- 기본 백업 방식은 풀 백업이지만, 필요에 따라 차등/증분 백업을 검토함.
- 백업에 걸리는 시간과 부하, 복원과 복구에 걸리는 시간을 고려함.
- DBMS의 3가지 구조
  - 로그 선행 쓰기(WAL, Write Ahead Log) : 데이터베이스의 데이터 파일 변경을 직접 수행하지 않고, 우선 로그로 변경 내용을 기술한 로그 레코드를 써서 동기화하는 구조.
  - 데이터베이스 버퍼 : WAL과 데이터베이스 버퍼, 데이터베이스 파일 3가지가 연계 플레이로 지속성을 담보하면서 현실적인 성능으로 DBMS가 동작함.
  - 크래시 복구
- PITR(Point-in-time Recovery) : 임의의 시점에서의 데이터 변경을 포함한 복원. 일반적인 DBMS에서는 데이터베이스에 실행된 갱신을 기록한 로그를 보존(Archive)해서 그것을 복원한 데이터베이스에 순차 반영해 백업 이후의 임의의 시점으로 복원할 수 있음.

<br>

# References

- [데이터베이스 첫걸음 / 미크, 기무라 메이지 / 박주향 옮김 / 한빛미디어 / 2016](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788968487316&orderClick=LEa&Kc=)