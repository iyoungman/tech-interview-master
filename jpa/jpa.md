## TOC

<!-- TOC -->

- [TOC](#toc)
- [왜 사용하나?](#왜-사용하나)
- [패러다임의 불일치](#패러다임의-불일치)
    - [객체 vs 관계형 DB](#객체-vs-관계형-db)
- [ORM](#orm)
- [JPA](#jpa)
- [JPA 과 ORM](#jpa-과-orm)
- [JPA의 장단점](#jpa의-장단점)
- [JPA의 성능 최적화](#jpa의-성능-최적화)
- [기초 매핑](#기초-매핑)
- [데이터베이스 방언 처리](#데이터베이스-방언-처리)
- [엔티티 매니저 팩토리, 엔티티 매니저, 엔티티](#엔티티-매니저-팩토리-엔티티-매니저-엔티티)
- [필드와 컬럼 매핑](#필드와-컬럼-매핑)
- [연관관계 매핑](#연관관계-매핑)
- [양방향 매핑](#양방향-매핑)
- [JPA 내부구조](#jpa-내부구조)
- [JPA 객체지향쿼리](#jpa-객체지향쿼리)
- [Spring Data JPA와 QueryDSL 이해](#spring-data-jpa와-querydsl-이해)
- [지연로딩 vs 페치조인 실제 쿼리](#지연로딩-vs-페치조인-실제-쿼리)
    - [지연로딩](#지연로딩)
    - [페치조인](#페치조인)
- [즉시로딩 실제 쿼리](#즉시로딩-실제-쿼리)
- [CaseCade](#casecade)
- [@OneToMany vs @ElementCollection](#onetomany-vs-elementcollection)
- [ManyToMany 관계](#manytomany-관계)
- [그외에](#그외에)

<!-- /TOC -->

## 왜 사용하나?
* 일반적으로 객체를 관계형 DB에 관리

## 패러다임의 불일치
* 객체 지향은 추상화, 캡슐화, 상속, 다형성 등 시스템의 복잡성 제어를 위한 장치 제공
* `객체를 관계형 DB에 저장하려면 SQL로 변환해야 한다`

### 객체 vs 관계형 DB
* `JPA가 없었다는 가정에서 비교` 와
* `JPA가 주는 해결책.`

***

1) 상속
* 객체는 상속 관계O
* 관계형 DB는 상속 관계X

* 객체의 모델
```java
abstract class Item {
    Long id;
    String name;
    int price;
}

class Album extends Item {
    String artist;
}

class Movie extends Item {
    String director;
    String actor;
}

class Book extends Item {
    String author;
    String isbn;
}
```

* 관련 SQL
```java
-- Album 객체 저장
INSERT INTO ITEM ...
INSERT INTO ALBUM ...

-- Movie 객체 저장
INSERT INTO ITEM ...
INSERT INTO MOVIE ...
```

* `해결`
> JPA는 상속과 관련한 패러다임의 불일치 문제를 개발자 대신 해결.
>
> 몇가지 설정이 필요하다.
>
> 설정을 안하면 디폴트 설정으로 저장된다.
>
> https://ict-nroo.tistory.com/128  


**JPA 저장**
> jpa.persist(album);

JPA는 다음 SQL을 실행해서 객체를 ITEM, ALBUM 두 테이블에 나누어 저장
> INSERT INTO ITEM ...
> INSERT INTO ALBUM ...

**JPA 조회**
> String albumId = "id100";
> Album album = jpa.find(Album.class, albumId);

JPA는 ITEM과 ALBUM 두 테이블을 조인해서 필요한 데이터를 조회하고 결과를 반환
> SELECT I.*, A.*
> FROM ITEM I
> JOIN ALBUM A ON I.ITEM_ID = A.ITEM_ID

***

2) 연관관계
* 객체는 참조를 사용
    + 객체의 참조는 방향성이 있다 (단방향성)
* 테이블은 외래 키 사용
    + 테이블의 Join은 방향성이 없다 (양방향성)
    
* 객체를 테이블에 맞춰 모델링
```java
//1
class Member {
    String id;        
    Long teamId;//연관 객체의 FK를 그대로 포함
    String username;
}
```
       
* 객체지향 모델링
> 객체의 장점을 살리기 위한 모델링이다.
 ```java
//2
class Member {
    String id;        
    Team team;//Team의 정보 조회 가능
    String username;
}
```

> 객체지향 모델링을 사용하면 객체를 테이블에 저장하거나 조회하기는 쉽지 않다.
>
> 테이블은 참조가 필요 없고 외래 키만 있으면 된다.
>
> 결국, 개발자가 중간에서 변환 역활을 해야 한다.
  ```java
//개발자가 직접 변환 역할
public Member find(Strubg memberId) {
    // SQL 실행하고
    
    // DB에서 조회한 회원 관련 정보를 모두 입력하고
    Member member = new Member();
    
    // DB에서 조회한 팀 관련 정보를 모두 넣고,
    Team team = new Team();

    // 회원과 팀 관계 설정
    member.setTeam(team);
    return member;
}
```

* `해결`
> JPA는 연관관계와 관련한 패러다임 불일치 문제를 해결해준다.
>
> 객체를 조회할 때 외래 키를 참조로 변환하는 일도 JPA가 처리.  

***

* 결론
    * `상속`
        * 저장 : JPA가 알아서 두개의 insert 쿼리를 작성.
        * 조회 : JPA가 알아서 부모, 자식 클래스 Join해서 가져온다.
        * 참고
            * https://ict-nroo.tistory.com/128  
    * `연관관계`
        * 저장 : JPA가 알아서 객체를 객체 ID를 이용하혀 저장하도록 SQL 작성.
        ```java
        public void createMember() {
            Team team = new Team("team1");
            teamRepository.save(team);

            Member member = new Member(team, "iyoungman");
            memberRepository.save(member);
        }
        ```
        ![image](https://user-images.githubusercontent.com/25604495/82117813-cba5d280-97ad-11ea-93cb-75bd6659428d.png)  
        
        * 조회 : 외래키 참조로 가져오도록 SQL 작성.


***  

## ORM
* Object Relational Mapping
* 객체지향의 **객체**와 데이터베이스의 **개체**가 유사하다는 입장
* 따라서 데이터베이스와 객체지향을 한번에 처리할 수 있지 않을까?  
![image](https://user-images.githubusercontent.com/25604495/51487174-f82e2200-1de5-11e9-880b-45f4e7eb6f23.png)

* 유사하지만 분명 **차이점이 있기때문에** 변환할 때 중간과정이 필요하다
* 객체는 객체대로 설계하고 관계형 DB는 관계형 DB대로 설계하되 **개발자가 아닌 ORM 프레임워크가 중간에서 매핑 처리를 해준다**
    - 정확히 말하는 알아서 해결해주는 것은 아니다
    - 해결할 수 있도록 도와준다
* ORM 프레임쿼크가 없다면 개발자가 직접 해줘야 할 것이다

* **특정한 언어에 종속적인 개념이 아니다** -> 객체지향과 관계형 데이터베이스를 매핑시킨다는 추상화된 개념

## JPA
* Java Persistent Api
* 영속성은 현재의 상태가 변하지 않고 영원히 계속되는 것을 말한다
* 자바를 이용해 데이터를 관리하는 기법을 하나의 스펙으로 정리한 것 
* 자바 진영의 ORM 표준 스펙

## JPA 과 ORM
* **ORM의 개념을 Java 언어에서 구현하기 위한 스펙**
* JPA를 이용하면 데이터베이스에 대한 처리를 JPA 계층에서 처리하기 때문에 편리함을 가질 수 있다
![image](https://user-images.githubusercontent.com/25604495/51366953-beca8d80-1b2b-11e9-93e8-8763d87760d0.png)

* 저장
![image](https://user-images.githubusercontent.com/25604495/51367195-b9217780-1b2c-11e9-8702-c8bf7f71e458.png)

* 조회  
JPA를 통해 앨범객체를 조회하면 알아서 Join해서 가져온다
![image](https://user-images.githubusercontent.com/25604495/69003648-49953800-0949-11ea-8976-808a9a536ceb.png)  

* JPA 는 스펙이기 때문에 실제로 이를 구현한 제품이나 프레임워크가 필요하다 (구현체는 주로 Hibernate 이용)
![image](https://user-images.githubusercontent.com/25604495/51366981-e7528780-1b2b-11e9-93f7-819ab1cedbc9.png)


* **JPA는 패러다임의 불일치를 해결한 적절한 쿼리를 짜서 결과를 반환한다**

* MyBatis는 개발자가 쿼리를 직접 짜야한다


## JPA의 장단점
* 장점
    - SQL중심적인 개발에서 객체 중심으로 개발
        + 단순 반복적인 SQL문을 구현하지 않아도 된다
        + `비즈니스 로직에 집중할 수 있다`

    - `생산성`
    ![image](https://user-images.githubusercontent.com/25604495/69004141-b3194480-0951-11ea-8aff-586a1057a9ad.png)  

    - `유지보수`
    ![image](https://user-images.githubusercontent.com/25604495/69004142-ba405280-0951-11ea-9f17-4449fe17ba3e.png)  
    
    - 데이터베이스와 독립적
        + 쉽게 DB를 변경할 수 있다
        + 추상화 되어있기 때문에 가능하다

## JPA의 성능 최적화
* **애플리케이션 로직과 DB 사이에 JPA가 있음으로써** 성능이 떨어진다고 생각할 수 있다
* 하지만, 중간에 있음으로써 성능을 최적화 할 수 있다

* 1차 캐시
    + 같은 트랜잭션 안에서 같은 엔티티를 반환할 때
    + 두번째 부터는 DB가 아닌 캐시를 조회한다

* 트랜잭션을 지원하는 쓰기 지연
    + `트랜잭션을 커밋할 때까지 INSERT SQL을 모은다`
    + `이후 한번에 SQL을 DB로 전송한다`

* 지연 로딩과 즉시 로딩
    + 필요에 따라 사용

***  
## 기초 매핑
* 애노테이션
    + @Entity : JPA가 관리할 객체
    + @Id : DB PK와 매핑 할 필드

## 데이터베이스 방언 처리
* JPA는 방언을 사용하기 때문에 특정한 DB에 종속되지 않는다  
![image](https://user-images.githubusercontent.com/25604495/69003986-acd59900-094e-11ea-843d-330ddd44db84.png)  

## 엔티티 매니저 팩토리, 엔티티 매니저, 엔티티
* 엔티티 매니저 팩토리
    - 비용이 크다
    - 따라서 하나만 생성해서 애플리케이션 전체에서 공유

* 엔티티 매니저
    - 엔티티를 관리하는 대상
    - 쓰레드간에 공유하면 안된다
    - 매번 사용하고 버려야한다
    - 엔티티 매니저는 자신이 관리할 엔티티 객체들을 영속성 컨텍스트(Persistence Context)에 넣어두고 생사를 관리한다

* 엔티티
    - 데이터베이스상에서 데이터로 관리하는 대상

***  

## 필드와 컬럼 매핑
* 애노테이션
    + @Temporal : 날짜 타입 매핑

* @Id에서 Generate AUTO와 Generate IDENTITY 차이
    + AUTO는 1,2,3 순서대로 생성하되 전체 공유, 따라서 별도의 테이블 생성  
    + IDENTITY는 1,2,3 순서대로 생성하되 각 테이블마다, 따라서 별도의 테이블 생성X    

***  

## 양방향 매핑
* 객체
    - 단방향밖에 없다
    - 단방향을 2개만들어 양방향처럼 보이도록 한 것이다

* 테이블
    - 외래키 하나면 양쪽에서 사용할 수 있다

* 양방향 매칭 규칙
    - 둘중 하나를 주인으로 정한다
    - **주인은 외래 키를 관리(등록, 수정)**
    - 주인 아닌쪽은 읽기만 가능
    - 주인이 아닌쪽에 **mappedBy**를 이용하여 주인 지정

* **주인은 외래키가 있는 쪽을 주인으로 한다**

* 주의 할것  
![image](https://user-images.githubusercontent.com/25604495/69009205-905d4f00-0996-11ea-9fed-a90c2f6f845f.png)
    - **당연히 저장은 된다**  
    - **하지만 관계가 맺어지지 않는다(외래키)**  
    - 주인에서만 관계 등록, 수정 할 수 있다  

* 양방향 매핑의 장점
    - 단방향 매핑만으로 이미 연관관계 매핑 완료
    - 반대방향으로 탐색이 필요할 때 용이하다


***  

## JPA 내부구조
* 영속성 컨텍스트
    - 논리적인 개념
    - 엔티티 매니저를 통해 영속성 컨텍스트에 접근
    - 엔티티를 영구저장하는 환경

* 엔티티 생명주기
    + 비영속
    + 영속
        - **영속성 컨텍스트에서 관리되는 상태**
    + 준영속
    + 삭제

* 영속성 컨텍스트의 장점
    - 엔티티 조회시에 1차 캐시
        + JPA로 조회시에 1차 캐시로 먼저 찾고
        + 캐시에 없으면 DB에서 찾는다
        + 영속성 컨텍스트는 글로벌 캐시가 아니다
        + 요청이 100개 들어와서 Thread가 100개 생성되면, 캐시도 100개 생긴다
    
    - 쓰기 지연
        + commit() 할때 넣는다(commit() 하면 flush()도 자동으로 일어난다)
        + commit()하면 실제 DB에 flush하고 commit한다
        + **flush는 영속성 컨텍스트 변경 내용을 실제 DB에 동기화 하는 것이다**

    - 변경 감지
        + 영속 상태의 엔티티의 값만 바꿔도 변경을 감지한다
        + 따라서 자동 Update 가능  
          
    - 지연 로딩
        + 지연 로딩을 사용하면 해당 엔티티는 **프록시**로 가져온다
 
* 플러시를 호출하는 방법
    + 트랜잭션 커밋
    + JPQL 쿼리 실행
      
    + 자동으로 실행되는 이유
    ![image](https://user-images.githubusercontent.com/25604495/69005608-ce8f4a00-0967-11ea-9ba7-5c155308b0bc.png)      
        - 위의 경우 persist는 했기때문에 1차 캐시에만 저장되고 실제 DB에는 저장X
        - 생쿼리를 날렸을 때 조회되지 않는다
        - 이러한 실수를 보완하기 위해 JPQL 쿼리 실행시 플러시를 호출한다


***  

## JPA 객체지향쿼리
* 다양한 쿼리 방법 지원
    - JPQL
    - JPA Creteria
    - QueryDSL
    - 네이티브 SQL


* JPQL
    - JPA를 사용하면 엔티티 객체 중심 개발
    - 문제는 검색쿼리
    - 검색 역시 테이블이 아닌 엔티티 객체를 대상으로 한다
    - 조인은 아래와 같이 사용한다
        + 'SELECT m FROM Member m JOIN 
**m.team t**'

* 페치 조인
    - 지연 로딩으로 설정해놔도 페치 조인을 사용하면 한번에 조회된다
    ![image](https://user-images.githubusercontent.com/25604495/69009490-e206d900-0998-11ea-9cd2-e29fc2ec8794.png)  
  
    - **N+1 문제를 해결할 수 있다**
    - 기본 전략을 지연 로딩으로 설정해놓으면 아래와 같이 member.getTeam() 호출시에 매번 SQL문을 호출한다 -> 성능 문제
        - `members 1번 + 각 member의 team n번`
    - **이때 페치 조인을 사용한다**
   ![image](https://user-images.githubusercontent.com/25604495/69009535-58a3d680-0999-11ea-993c-ab6469dd1ed0.png)  


***  

## Spring Data JPA와 QueryDSL 이해

* Spring Data JPA
    + **지루하게 반복되는 CRUD 문제를 세련된 방법으로 해결**
    + 개발자는 인터페이스만 작성
    + 공통되는 구현체는 Spring Data JPA가 만들어놓는다
    ![image](https://user-images.githubusercontent.com/25604495/69009626-863d4f80-099a-11ea-9b0e-fd57d7a85de9.png)  
    ![image](https://user-images.githubusercontent.com/25604495/69009629-8d645d80-099a-11ea-9f70-977eb98a1ae7.png)  
 

* Paging
    - 다 구현되어 있으므로 사용하기만 하면 된다
    - Pageable은 인터페이스, PageRequest는 구현체이다
    ![image](https://user-images.githubusercontent.com/25604495/69009694-4dea4100-099b-11ea-859b-1772da99d98d.png)  
    ![image](https://user-images.githubusercontent.com/25604495/69009697-52165e80-099b-11ea-9e15-37a5c66662bc.png)  

* QueryDSL 
    - 장점
        + 문자가 아닌 코드로 작성
        + 컴파일 시점에 오류를 잡는다
        + 동적 쿼리
        + 가독성
        ![image](https://user-images.githubusercontent.com/25604495/69009851-006ed380-099d-11ea-8948-6cdb670b9316.png)  

* QueryDSL 적용 방법
    - QuerydslRepositorySupport
        + extends QuerydslRepositorySupport
        + QueryDSL을 사용하도록 해주는 역할

* QueryDSL vs Creteria
    - QueryDSL이 Creteria에 비해 가독성 부분에서 좋다  

***

## 지연로딩 vs 페치조인 실제 쿼리

### 지연로딩

```java
@Transactional
@Test
public void getMember() {
    Member member = memberTeamService.getMember(1L);

    log.info("==============================");

    member.getTeam().getTeamName();
}
```
* 쿼리
    * member 가져올때 join 하지 않는다.
    * team 가져올때 join 하지 않는다.
    * `즉, 각자의 것만 가져온다.`

### 페치조인
* N + 1 문제
> 처음에 List<Member>을 가져온다.(findAll)
>
> 각 Member의 Team을 가져온다.
>
> 이때, 한번 조회했던 Team이면 DB에서 가져오지 않지만,
>
> 각 Member가 다른 Team에 속하면 N + 1 문제가 발생한다.  

![image](https://user-images.githubusercontent.com/25604495/82649905-ddc3bd00-9c54-11ea-8415-cbea91ecaf04.png)  



* Fetch Join  
```java
public interface MemberRepository extends JpaRepository<Member, Long> {
    @Query("select m from Member m join fetch m.team")
    List<Member> findAllByFetchJoin();
}
```
![image](https://user-images.githubusercontent.com/25604495/82650749-1adc7f00-9c56-11ea-88c2-69eee3b32997.png)

> inner join 으로 가져온다.

***

## 즉시로딩 실제 쿼리 
![image](https://user-images.githubusercontent.com/25604495/82651159-bec62a80-9c56-11ea-94ff-ab55c2556eaf.png)  
* 쿼리
    * `outer join`으로 가져온다.
* 왜 outer join?
    * `연관객체(team)가 없는 상황에서도 가져오기 위해서`
    * Fetch Join은 inner join이므로 연관객체(team)가 없으면 memeber도 가져오지 않을 것이다.

***  

## CaseCade
* 영속성 전이
* 연관된 엔티티에 전파되는 범위
* 영속성이란?
    * 영속성 컨텍스트에 저장되는 상태

> CascadeType.PERSIST
* 부모 클래스 저장시 자식 클래스도 함께 저장

<br>

> CascadeType.REMOVE
* 부모 클래스 삭제시 자식 클래스도 함께 삭제
* `CaseCadeType.REMOVE 없을때`

```java
private static void deleteNoCascade(EntityManager entityManager){
    Parent parent = entityManager.find(Parent.class, 1L);
    Child child1 = entityManager.find(Child.class, 2L);
    Child child2 = entityManager.find(Child.class, 3L);

    entityManager.remove(child1);
    entityManager.remove(child2);
    entityManager.remove(parent);
}
```

* `CaseCadeType.REMOVE 있을때`

```java
@OneToMany(mappedBy = "parent" ,cascade = CascadeType.REMOVE)
private List<Child> children = new ArrayList<>();

private static void deleteWithCascade(EntityManager entityManager){
    Parent parent = entityManager.find(Parent.class, 1L);
    entityManager.remove(parent);
}
```

* CaseCadeType이 없다면 
* 특정 부모를 삭제할때 연관되는 자식 클래스를 먼저 삭제해줘야한다.(외래키를 관리하는 테이블)
```text
부모 DB (1)      (N) 자식 DB
                 (FK)부모 id
```
* 그렇지 않으면 `외래키 제약조건`으로 삭제되지 않는다.

<br>

> CascadeType.MERGE

> CascadeType.REFRESH

> CascadeType.DETACH

> CascadeType.ALL

***

## @OneToMany vs @ElementCollection
* @OneToMany
    * Entity 관계 기술
* @ElementCollection
    * Basic Type에 관한 관계 기술
    * 대상 Entity의 PK와 같은..
* 참고
    * https://devday.tistory.com/entry/JPA-OneToMany-vs-ElementCollection

***

## ManyToMany 관계
* @ManyToMany 어노테이션을 써도 중간 테이블이 생긴다.
* 하지만 별도의 다른값을 저장할 수 없다.
* 중간객체를 만들어 @ManyToOne 두개로 나누면 중간 객체에 별도의 값을 저장할 수 있다.
* 참고
    * https://ict-nroo.tistory.com/127  
