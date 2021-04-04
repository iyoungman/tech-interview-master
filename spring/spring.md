
## TOC
<!-- TOC -->

- [TOC](#toc)
- [Spring, Spring MVC, Spring Boot 차이](#spring-spring-mvc-spring-boot-차이)
- [@Bean, @Component 차이](#bean-component-차이)
- [컨테이너란?](#컨테이너란)
- [ApplicationContext](#applicationcontext)
- [DI 주입 방식](#di-주입-방식)
- [ExceptionHandler](#exceptionhandler)
- [JPA](#jpa)
- [스프링 프레임워크 핵심기술 3가지](#스프링-프레임워크-핵심기술-3가지)
- [서블릿 컨테이너 동작과정](#서블릿-컨테이너-동작과정)
- [서블릿 컨테이너와 스프링 컨테이너](#서블릿-컨테이너와-스프링-컨테이너)
- [스프링 싱글톤](#스프링-싱글톤)
- [Gradle vs Maven](#gradle-vs-maven)
- [Filter, Interceptor, AOP](#filter-interceptor-aop)
- [스프링 MVC 동작과정](#스프링-mvc-동작과정)
- [스프링 Batch](#스프링-batch)
- [REST API](#rest-api)
- [MVC와 RestAPI에서 View 리턴](#mvc와-restapi에서-view-리턴)
- [BeanFactory vs ApplicationContext](#beanfactory-vs-applicationcontext)
- [new vs ProtoType Scope](#new-vs-prototype-scope)
- [@PostConstruct](#postconstruct)
- [CORS](#cors)
- [CSRF](#csrf)
- [XSS](#xss)
- [CSRF vs XSS](#csrf-vs-xss)
- [Profile 과 Property](#profile-과-property)
- [Bean의 LifeCycle과 초기화 메서드](#bean의-lifecycle과-초기화-메서드)
- [도메인 객체](#도메인-객체)
- [서비스 계층의 역할](#서비스-계층의-역할)
- [참고](#참고)

<!-- /TOC -->


***

## Spring, Spring MVC, Spring Boot 차이
* [참고 사이트](http://blog.naver.com/PostView.nhn?blogId=sthwin&logNo=221271008423&parentCategoryNo=&categoryNo=50&1viewDate=&isShowPopularPosts=true&from=search)  


***

## 컨테이너란?
* 프로그래머가 작성한 코드를 스스로 참조하여
* 인스턴스의 생명주기를 관리.

***


## DI 주입 방식
* 생성자 주입
* Setter 주입
* Autowired 주입
* 생성자 주입을 사용해야 하는 이유
    - 생성자 주입는 객체가 필수적으로 필요로하는 의존성을 **객체 생성 시점에 표현**할 수 있다
    - 또한 불변객체인 final 키워드를 사용할 수 있다
     
***

## JPA
* ORM을 위한 Java 스펙
* 구현체는 여러가지가 있는데 그 중 Hibernate 사용
* 유연성
    - 자바 코드 변경에 따른 비용을 줄일 수 있다
    - 문자 기반이 아닌 코드 기반으로 쿼리를 할 수 있다

* **패러다임의 불일치를 해결할 방법들을 제공한다**
    - 방향성 문제의 경우 외래키를 관리할 주인관계를 설정한다
    - 설정에 맞게 쿼리로 변환해준다
    
***


## 스프링 프레임워크 핵심기술 3가지
* POJO(`Object`)
    - 객체지향적 원리에 충실하면서, 환경과 기술에 종속되지 않고 필요에 따라 재활용될 수 있는 방식으로 설계된 오브젝트(꼭 도메인 아니더라도 일반적인 객체)  

* IOC, DI
    - 의존관계
        + 객체 혼자 모든 일을 처리하기 힘들기 때문에 내가 해야 할 작업을 다른 객체에게 위임
        
    - 의존관계 주입
        + 객체 내부로 new가 아닌
        + 외부의 객체가 인스턴스를 생성한 후 이를 전달해서 의존성 해결하는 것
        + 주입이라는 의미가 외부에서 받는 것을 뜻한다
        + 외부에서 주입받아야 더 유연성을 가질 수 있다

        + PSA를 예로 들어도 PSA는 인터페이스로 추상화하고 구현체는 숨길 수 있다
        + 컴파일 시점에는 인터페이스로 두고 런타임 시점에 구현체를 di로 주입 받는 것이다
        + 따라서 외부에서 주입받는 것의 장점인 유연성이 나온다
        + DI 자체가 런타임시에 동적으로 의존 객체를 연결시켜주는 것을 의미한다

    - **개발자가 아닌 컨테이너(BeanFactory 혹은 ApplicationContext)가 주체가 되어 빈을 관리**
    - 어떤 객체를 직접 만들어 사용하는 것이 아니라 주입 받아서 사용
    - DI는 클래스 사이의 의존관계를 빈 설정 정보를 바탕으로 컨테이너가 자동으로 연결

    - 장점
        - 객체간의 의존성 관리(DI)를 컨테이너가 해준다.
        - 객체간의 결합도를 낮춰준다.
        - https://starkying.tistory.com/entry/IoC-DI

    - Bean
        + [stackoverflow - 빈 생성 시점](https://stackoverflow.com/questions/26020959/bean-instantiation-and-dependency-injection-in-spring)

* AOP
    - 로깅, 예외 처리와 같은 공통 코드를 한 곳에서 처리함으로써
    - 서비스 단에서는 비즈니스 로직의 코드를 집중

* PSA
    - 환경의 변화와 관계없이 일관된 방식의 기술로의 접근 환경을 제공하려는 추상화 구조  
    - JPA를 직접 사용X, Spring Data JPA  
    - 구현체를 Hibernate를 사용하건 EclipseLink를 사용하건 내부구현이 달라지더라도 동일한 인터페이스로 동일한 구동이 가능하게 설계되어있다  

***

## 서블릿 컨테이너 동작과정
* 서블릿 컨테이너는 Tomcat(WAS)
    - 정확히하면 WAS안에 서블릿 컨테이너가 포함

* 서블릿은 HTTP 요청을 받아 처리
    - 서블릿은 웹 기반의 요청에 대한 동적인 처리가 가능한 하나의 클래스
    - 스레드를 이용하여 서블릿 처리
    - 요청에 맞는 Contoller로 매핑
    - [참고 사이트](http://myblog.opendocs.co.kr/archives/425)

* Spring Boot에서는 `DispatcherServlet이 해당 역할`

***

## 서블릿 컨테이너와 스프링 컨테이너
![image](https://user-images.githubusercontent.com/25604495/69402304-c9047c00-0d3a-11ea-84e8-2ff97c9d25ea.png)  
* 서블릿 컨테이너는 DispatcherServlet
* 스프링 컨테이너는 ApplicationContext

***

## 스프링 싱글톤
* [자바 싱글톤과 스프링 싱글톤 차이](https://okky.kr/article/226163) 
* 스프링의 빈은 싱글톤으로 관리
* 하지만 전통적인 java 싱글톤 디자인패턴과는 다르다
    - java의 싱글톤 디자인패턴은 코드상으로 싱글톤을 생성하는 방법
    - spring은 편의를 이유로 framework에서 싱글톤으로 관리한다
* 싱글톤 객체는 멀티 쓰레딩에 주의

***

## Gradle vs Maven
* Gradle은 Maven보다 늦게 나왔기 때문에
* 장점이 더 많다
* 간단하게
    - 코드가 간결하다
    - 성능이 좋다
* 둘다 사용

***

## Filter, Interceptor, AOP
*  Filter(Spring 컨테이너 밖, Tomcat 안)
    - 스프링과 무관한 자원
    - 인코딩
    - HttpServletRequest를 바꿀 때
        - `크로스 사이트 스크립트` 대비
            - 악의적인 스크립트를 넣는 것
        - 방법
            - 데코레이터 패턴
      
* Interceptor(Spring 컨테이너 안)
    - **Controller 호출 전후에 실행**
    - Controller 요청에 관한 요청과 응답 처리
    - 사용자 요청에 대한 검사
    - 사용자 권한, 로그인 처리
      
* AOP
    - **메소드 호출 전후에 실행**
    - Controller, Service 전후 등 설정에 의해 달라질 수 있다
    - 비즈니스 로직의 공통 작업 처리
    - 예외 처리, 로깅

***

## 스프링 MVC 동작과정
* https://server-engineer.tistory.com/253  
* https://tinkerbellbass.tistory.com/40
* `HandlerMapping 처리하는 클래스 잊지 말자`
    * HandlerMapping
        * 요청에 알맞은 Controller 객체를 찾는 인터페이스
    * HandlerAdapter
        * DispatcherServlet은 HandlerAdapter에게 컨트롤러의 메서드 호출 반환 위임
    * 순서
        * DispatcherServlet -> HandlerMapping -> DispatcherServlet -> HandlerAdapter -> DispatcherServlet
* `ViewResolver가 View를 찾아 DispatcherServlet에게 넘겨준다`

***

## 스프링 Batch
* 용어
    - Job
        + 하나의 배치
        + Job은 Step이 순차적으로 실행
        + Tasklet // ItermReader, ItemProcessor, ItemWriter
        + Tasklet은 Step에서 수행되는 로직
        + ItermReader는 데이타를 읽는 컴포넌트
        + ItemProcessor는 읽은 데이타를 처리
        + ItemWriter는 처리한 데이타를 저장 

    - Job Instance 
        + 배치가 실행될 때 각각의 실행

    - Job Excution
        + 실제 수행된 Job Instance를 정의
        + 예를 들어 첫번째 시도에 실패, 두번째 시도에 성공한 배치가 있다면
        + Job Excution에는 따로 기록되지만
        + 같은 작업이므로 같은 Job Instance

* Chunk
    - Chunk 단위로 트랜잭션을 수행
    - 장점
         + 실패할 경우엔 해당 Chunk 만큼만 롤백이 되고
         + 이전에 커밋된 트랜잭션 범위까지는 반영이 된다
         + `따라서 대용량 데이터의 처리에 적합하다`

* Page Size와 Chunk Size
    - Page Size
        + 한번에 읽어올 데이터의 양
    - Chunk Size
        + `한번에 트랜잭션에 커밋될 데이터의 양`

* Tasklet // ItermReader, ItemProcessor, ItemWriter
    - Tasklet
        + `데이터가 많지 않고 단순한 로직`
        + `chunk 단위의 트랜잭션 관리 등이 필요없을 경우`

    - 일반적으로 ItermReader, ItemProcessor, ItemWriter으로 구성
    - **Reader에선 Page 단위로 읽고(설정), Writer에선 Chunk 단위로 처리**

* Reader에서 다양한 통계 쿼리를 dto로 조회
* Proceesor는 dto를 한 테이블에 저장하기 위해 entity객체로 변환
* Wirter에서 enetity객체를 저장

***  

## REST API
* `리소스` 중심으로 보다 간략하게 상태 정보를 주고 받는 것
    - 대표적인 규칙으로 URI 설계라고 생각한다
    - URI를 통해 자원을 명시하고 HTTP Method를 통해 해당 자원의 행위를 명시한다
        + 자원은 명사로 표현하고
        + 자원의 행위는 GET, POST, PUT, DELETE 이 있다.
        + 따라서 URI는 동사형태 '/getCoffees' 형태로 만들지 않는다.
        + URI를 최대한 심플하게 설계한다.  

<br>

* 리소스
    * 처리가 되는 대상

* 특징
    * Client-Server 구조
    * 무상태성
    

<br>

* 참고
    * https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html
    * https://stackoverflow.com/questions/2190836/what-is-the-difference-between-http-and-rest

***

## MVC와 RestAPI에서 View 리턴
* MVC
    * `ViewResolver`를 통해 적절한 View 리턴
    * text/html 컨텐츠 타입 리턴
* RestAPI
    * `MessageConverter`를 통해 Json 리턴
        * Default MessageConverter는 Jackson
    * appication/json, text/plain 등 리턴
`* MVC vs RestAPI 어떻게 구분할까?`
    * `@ResponseBody 어노테이션`
    * 해당 어노테이션이 선언되어있으면 HTTP RsponseBody에 응답을 리턴한다.
    * @RestController
        * @Controller + @ResponseBody

* 참고
    * https://wckhg89.github.io/archivers/understanding_jackson

***

## BeanFactory vs ApplicationContext
* BeanFactory
    * Lazy Loading
    * 실제 빈 사용시점에 빈 생성

* ApplicationContext
    * Eager Loading
    * 컨테이너 구동시에 빈 생성

* 둘다 인터페이스이다.
> 인터페이스에 따라 Loading이 달라지는 것은 아니다.
>
> 실제 구현체에 따라 Loading이 달라진다.
>
> 다만 구현체 이름에 ~BeanFactory, ~ApplicationContext로 나눠지며
>
> 그에 따라 Loading이 달라지기 때문이다.
>
> http://wonwoo.ml/index.php/post/1571

***

## new vs ProtoType Scope
* ProtoType
    * `의존성 관계 주입`시마다 새로운 객체가 생성되어 주입

*** 

## @PostConstruct
* 객체 생성 직후 별도의 초기화 작업
* `생성자에서 호출하는 것과 차이가 있을까?`

***

## CORS
* `브라우저의 다른 도메인간 데이터 전송을 가능하게 해준다.`
* 보안상의 이유로 Default 는 CORS를 제한하고 있다.
    * SOP(Same-Origin Policy, 동일 출처 정책) 때문이다.
    * Origin
        - URL 스키마 (http, https)
        - hostname (naver.com, localhost)
        - 포트 (8080, 18080)
    * 프로토콜, 포트, 호스트가 같아야하는 정책.
    * 참고 
        * https://goodgid.github.io/CORS/
        * https://heowc.dev/2018/03/13/spring-boot-cors/

* 기본적인 해결방법
    * 서버에서 크로스도메인 허용
    * 클라이언트에서 크로스도메인 허용
        * `일반적으로 서버측에서 해결`
    * 서버의 응답헤더에 아래와 같은 정보 설정
    > Access-Control-Allow-Origin: "*"
    > @CrossOrigin  
    * 참고
        * https://undefine.me/posts/5

* `서버의 응답헤더로 보내는 이유`
    * CORS 동작방식과 연관되어있다.
    * `실제 요청전에 사전 요청을 보낸다.`
        * HTTP Option 메서드 이용
        * 특정 엔드포인트가 어떤 메소드를 허용하는지 알고자 할 때 사용한다
        * https://ychae-leah.tistory.com/83
    * 참고
        * https://uiandwe.tistory.com/1244

* 스프링에서 해결
    * @CrossOrgin
    * Config 파일
    * 참고
        * https://engkimbs.tistory.com/781
        * http://jmlim.github.io/spring/2018/12/11/spring-boot-crossorigin/

***

## CSRF
* https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%9A%94%EC%B2%AD_%EC%9C%84%EC%A1%B0
* `사용자의 인증된 정보를 이용하여 악의적인 요청을 하는것`

***

## XSS
* https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8C%85
* `서버로 악의적인 스크립트를 심어서 보내는것`

***

## CSRF vs XSS
* CSRF는 공격대상이 서버.
* XSS는 공격대상이 클라이언트.
* https://glasgowkiss.tistory.com/16

***

## Profile 과 Property
* https://iyoungman.github.io/spring/SpringFramework-Environment-Property/
* Profile
    * 환경
* Property
    * 외부 설정값

***

## Bean의 LifeCycle과 초기화 메서드
* https://iyoungman.github.io/spring/Bean-LifeCycle-and-Init-Method/
* 초기화 메서드
    * Bean 의존성 주입이 다 끝난후에 초기화하는 역할.

***

## 도메인 객체
* 비즈니스 로직을 나타내는 객체
* Entity와 VO가 있다.
* `Entity는 구분키(PK)를 갖는 것`
* `VO는 단순 값들만 나타내는 것`

***

## 서비스 계층의 역할
* 도메인 객체들이 로직을 구현하도록 도메인 객체 조합.
* 로직 처리 완료된 상태값을 DAO을 활용해 DB에 저장하는 역할 담당.


***

## 참고
* https://github.com/WeareSoft/tech-interview/blob/master/contents/spring.md
