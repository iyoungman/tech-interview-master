## TOC

<!-- TOC -->

- [TOC](#toc)
- [커넥션 풀](#커넥션-풀)
- [인덱스](#인덱스)
- [DB 엔진](#db-엔진)
- [트랜잭션 ACID](#트랜잭션-acid)
- [트랜잭션 Isolation Level](#트랜잭션-isolation-level)
- [정규화](#정규화)
- [다양한 Join쿼리](#다양한-join쿼리)
- [SQL vs NoSQL](#sql-vs-nosql)
- [쿼리 실행 과정](#쿼리-실행-과정)
- [파티셔닝, 샤딩](#파티셔닝-샤딩)
    - [파티셔닝](#파티셔닝)
- [Replication](#replication)
- [옵티마이저](#옵티마이저)
- [Inner Join, Outer Join 설명하기](#inner-join-outer-join-설명하기)
- [공유 잠금(Shared lock) vs 배타적 잠금(Exclusive lock)](#공유-잠금shared-lock-vs-배타적-잠금exclusive-lock)
- [Isolation Level과 공유, 베타 잠금(Pass)](#isolation-level과-공유-베타-잠금pass)

<!-- /TOC -->

***

```text

```

## 커넥션 풀
* WAS에서 처리
* https://github.com/iyoungman/tech-interview-master/blob/master/db/connection%20pool.md

***  

```text

```

## 인덱스
* https://github.com/iyoungman/tech-interview-master/blob/master/db/index.md

***

```text

```

## DB 엔진
* 엔진
    - **InnoDB를 사용**
    - MyISAM은 외래키를 생성X, 인덱스만 생성
  

* [참고 사이트](https://jojoldu.tistory.com/243)  
  

***  

```text

```

## 트랜잭션 ACID
> 데이터베이스의 상태를 변화시키기 위한 작업의 단위

* 트랜잭션 성질
    - 원자성(All or Nothing)
    - 일관성(전후 일관상태)
    - 독립성(**하나의 트랜잭션 실행동안 동일한 데이터를 다른 트랜잭션이 참조하지 못하게 하는 성질**)
    - 지속성(성공 트랜잭션 영원 반영)

* `일관성이란 정확히 무엇일까?`
    * 트랜잭션이 그 실행을 성공적으로 완료하면 언제나 일관성 있는 데이터베이스 상태를 가지고 있어야한다.
    * 즉 트랜잭션 전후에도 제약조건을 만족해야한다.
    > 기본키, 외래키 제약 같은 명시적인 무결성 제약 조건

    > 임의로 만든 제약조건
    >
    > 계좌 예제에서 계좌의 잔고는 0보다 작아서는 안된다.

    * 참고
        * https://d2.naver.com/helloworld/407507


* [참고 사이트1](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-db.html)  
* [참고 사이트2](https://victorydntmd.tistory.com/129)  

***  

```text

```

## 트랜잭션 Isolation Level
* `트랜잭션 독립성을 허용하는 수준(효율적인 Locking을 위함이다)`
* Level 높을 수록 **독립성** 증가, **공유성** 감소

* 문제
    - Dirty Read
        + **커밋되지 않은 데이터를 읽을 수 있다**
        + A 트랜잭션에서 데이터를 변경했지만 아직 커밋은 안했다
        + B 트랜잭션에서 해당 데이터를 읽었다
        + A 트랜잭션에서는 변경사항을 커밋하지 않고 롤백하거나 종료했다
        + B 트랜잭션이 읽은 데이터는 꼬이게 된다
        + **개별 데이터**

    - Non-Repeatable Read
        + **한 트랜잭션 내에서 같은 쿼리 두번 수행시 결과가 다르게 나타는 것**
        + A 트랜잭션에서 데이터를 읽고있다
        + B 트랜잭션에서 그 사이 데이터를 변경하고 커밋한다
        + A 트랜잭션에서 해당 데이터를 다시 읽을 때 변경된 데이터를 읽는다
        + **개별 데이터**

    - Phantom Read
        + **한 트랜잭션에서 레코드를 두 번 읽을 때 첫번째 쿼리에서 없던 레코드가 나타나는 현상**
        + 삽입을 허용하기 때문이다
        + A가 조건 검색을 했다
        + B가 같은 조건에 해당되는 데이터를 추가했다
        + A가 다시 읽을 때는 추가된 데이터까지 조회된다
        + **데이터 집합**
        + Phantom은 유령이라는 의미
            + 갑자기 생겼다

* Level 0
    - UnCommited Read
    - 문제 : Dirty Read, Non-Repeatable Read, Phantom Read

* Level 1
    - Commited Read
    - SQL Server Default
    - 문제 : Non-Repeatable Read, Phantom Read

* Level 2
    - Reapetable Read(다시 읽어도 똑같다.. 라는 의미)
    - **선행 트랜잭션이 읽은 데이터를 트랜잭션 종료시까지 후행 트랜잭션이 갱신, 삭제 하는 것을 불허**
    - 문제 : Phantom Read

* Level 3
    - Serializable
    - Phantom Read의 문제점인 선행 트랜잭션이 읽고있는 범위 데이터 조건에 삽입을 못하게 한다

* [참고 사이트1](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-db.html)
* [참고 사이트2](https://feco.tistory.com/45)  

***  

```text

```

***  

```text

```

## 정규화
 * 함수적 종속
    - X의 값을 알면 Y의 값을 바로 식별할 수 있고, X의 값에 Y의 값이 달라지는 것
    - **같은 X의 값에 대해 다른 Y의 값을 가르키면 함수적 종속이 아니다**
    - **단, 서로 다른 X의 값에 대해 같은 Y의 값을 가르킬 수는 있다**
    - X -> Y
      
* 이상 현상
    * `불필요한 데이터 중복으로 인해 발생`
        * 삽입 이상
            * 원하지 않는 값들로 인해 삽입 불가
        * 삭제 이상
            * 상관없는 값들도 삭제 -> 연쇄 삭제
        * 갱신 이상
            * 일부 튜플만 갱신
    * 참고
        * https://yaboong.github.io/database/2018/03/09/database-anomaly-and-functional-dependency/

* 정규화
    - `잘못된 함수적 종속으로 인해 발생하는 이상현상을 없애기 위한 것`
    - 1 : 도메인 원자값
    - 2 : 부분적 함수 종속 제거
        - 부분적 함수 종속은 `기본키의 일부에 종속될 수 있는 것` -> 완전 함수 종속으로 나눠야한다.
    - 3 : 이행적 함수 종속 제거
        - X -> Y, Y -> Z, X -> Z : Z는 X에 대해 이행적으로 종속되어있다.

* 참고
    * [참고 사이트](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Database)  
    * [참고 사이트2](https://3months.tistory.com/193)

***  

```text

```

## 다양한 Join쿼리
[stackoverflow-질문](https://stackoverflow.com/questions/406294/left-join-vs-left-outer-join-in-sql-server)  

***  

```text

```

## SQL vs NoSQL
* SQL 
    - Structed Query Language(구조화 쿼리 언어)
    - 구조화 되어있어 **무결성**을 지킬 수 있다
    - `Scale-Out이 가능은 하나(샤딩) 어렵다`
    - `디스크 기반이어서 느리다`

* NoSQL
    - Non Structed Query Language(비구조화 쿼리 언어)
    - 비구조화여서 사용하기 쉽다
    - `Scale-Out 이 쉽다`
    - `메모리 기반이어서 빠르다`
    - [참고 사이트](https://goodgid.github.io/NoSQL/)  

* 무결성이란
    - 데이터의 `정확성`을 의미한다
    - Null 무결성, `참조 무결성` 등
    - [참고 사이트](https://coding-factory.tistory.com/221)  
    
* Scale-Out과 Scale-Up은 요점이 아니다.
* MySQL만의 특징
    * Group By로 묶을때 Group By 대상이 아닌 컬럼도 조회할 수 있다.
    * https://stackoverflow.com/questions/1225144/why-does-mysql-allow-group-by-queries-without-aggregate-functions

* 참고
    * https://siyoon210.tistory.com/130

***  

```text

```

## 쿼리 실행 과정
* https://mysqldba.tistory.com/2?category=537397  
* https://mysqldba.tistory.com/8  

* 크게 3가지 부분.
1) Connection
    * JDBC API와 같이
    * MySQL 서버 부분에 접근하기 위해 Application에서 사용하는 모듈.
2) MySQL - Server 
    * Connection Pool이나 옵티마이저 등등
    * Clinet로 받은 쿼리의 최적화된 실행계획을 만드는 부분.
3) MySQL - Server Storyage Engine
    * InnoDB와 같이
    * 데이터를 저장하거나 추출하는 부분.


***  

```text

```

## 파티셔닝, 샤딩

### 파티셔닝
* 개념
    * DB의 규모가 커지면서 기존 DB에 용량과 성능 문제
    * `table을 작은 단위로 나누는 것`
        * 큰 table이나 index를, 관리하기 쉬운 partition이라는 작은 단위로 물리적으로 분할하는 것을 의미
        * 물리적으로 분할되어 있어도 접근하는 쪽에서는 인지하지 못한다

* 장점
    * 관리적 측면 : partition 단위 백업, 추가, 삭제, 변경
    * 성능적 측면 : partition 단위 조회 및 DML수행

* 단점
    * table간 JOIN에 대한 비용이 증가한다.
    * table과 index를 별도로 파티셔닝할 수 없다.
    * table과 index를 같이 파티셔닝해야 한다.

* 수평 파티셔닝(Horizontal)
    * `다수의 데이터베이스에 나누는 방법`
    * `샤딩과 동일`
    * `애플리케이션 레벨에서 서로 다른 테이블에 저장`
    * 예시
        * 전세계 고객 정보
        * 미국 고객, 한국 고객
    * 장점
        * 하나의 테이블당 데이터의 개수가 적어진다
    * 단점
        * 애플리케션의 복잡도 증가

* 수직 파티셔닝(Vertical)
    * `테이블의 일부 컬럼(열)을 빼내는 형태로 분할`
    * 즉, 하나의 엔티티를 2개 이상으로 분리
    * 예시
        * 하나의 고객 정보
        * 이름, 전화번호를 다른 테이블로 분리
    * 장점
        * 자주 사용하는 컬럼을 분리시켜 성능을 향상시킨다

* `인덱스 관점`
    * 수평 파티셔닝
        * 테이블 당 조회할 인덱스의 수가 줄어드므로 성능에서 좋아진다
    * 수직 파티셔닝
        * 테이블 당 조회할 인덱스의 경우는 같다
        * 하지만 가져온 이후 관리해야할 테이터 수가 줄어든다
        * 즉, 필요한 데이터만 가져올 수 있다

* 참고 사이트
    * https://nesoy.github.io/articles/2018-02/Database-Partitioning

***

```text

```

## Replication
* `두 개 이상의 DBMS 시스템을 Master/Slave로 나눠서 동일한 데이터 저장`
* 관계
    * Master : Slave = 1 : N
    * Master는 데이터 조작 처리(Insert, Update, Delete)
    * Slave는 데이터 조회 처리(Select)
    * Master와 Slave의 동기화 필요
        * MySQL에서는 Binary log

* 참고
    * https://snthewhitehacker.tistory.com/4
    * https://gywn.net/2012/03/mysql-replication-3/

***

```text

```

## 옵티마이저
* SQL을 최적의 비용으로 처리할 수 있도록 하는 DBMS 내부 엔진
* 비용 기반 옵티마이저
    * RDBMS에서 사용하는 방식

***

```text

```

## Inner Join, Outer Join 설명하기
* Inner Join
    * `조인이 되는 키값을 기준으로 키값이 교집합인것 출력`
* Outer Join
    * `조인이 되는 키값을 기준으로 기준테이블 Key 집합 출력`
    * 반대 테이블은 Null로 출력

* 예제
> Student  

![image](https://user-images.githubusercontent.com/25604495/82343716-25b4cb00-9a2e-11ea-8d4b-dd2869474907.png)   

#

> Department  

![image](https://user-images.githubusercontent.com/25604495/82343761-3402e700-9a2e-11ea-9f27-9abfc2de110d.png)

#

> Inner Join
>
> SELECT * FROM test.Student s inner join test.Department d on s.id = d.id;

![image](https://user-images.githubusercontent.com/25604495/82344003-7a584600-9a2e-11ea-878f-31b3c777ec17.png)

#

> Outer Join
>
> SELECT * FROM test.Student s left outer join test.Department d on s.id = d.id;
>
> 즉, `조인 키값 기준으로 교집합 + Left에만 있는 키 출력`

![image](https://user-images.githubusercontent.com/25604495/82344280-d0c58480-9a2e-11ea-9f12-d1743e94d4b6.png)  

***  

```text

```

## 공유 잠금(Shared lock) vs 배타적 잠금(Exclusive lock)

> 공유 잠금
 * 읽기 잠금.
 * Select 문에서 사용.
 * 해당 리소스를 해당 트랜잭션이 동시에 읽도록 하되<br>
변경은 못하게 한다.

> 베타적 잠금
* 쓰기 잠금.
* Insert, Update, Delete에서 사용.
* 어떤 트랜잭션에서 데이터를 변경하고자 할때<br>
다른 트랜잭션에서 해당 자원을 읽거나 쓰지 못하게 하는 것.  

<br>

* 참고
    * https://jeong-pro.tistory.com/94

***

```text

```

## Isolation Level과 공유, 베타 잠금(Pass)

* 베타 잠금
    * 모든 Level

* 공유 잠금
    * Level 0 빼고 필요


* https://stackoverflow.com/questions/7457628/transactions-locks-isolation-levels
