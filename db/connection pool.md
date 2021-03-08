# Connection Pool

## jdbc 라이브러리
```text
HikariCP
spring-jdbc
```
* **Spring-Boot-Starter-Web** 라이브러리를 통해 위의 것이 자동설정으로 들어온다
* 처음에는 Spring-Boot-Starter-Jdbc랑 헷갈렸다
* Spring-Boot-Starter-Jdbc는 Jdbc 관련해서 추가적으로 무엇인가 해주는 것 같다(추측)
* Jdbc 관련 기본적인 설정은 Spring-Boot-Starter-Web을 통해서 들어온다 
* HikariCP는 Database Connection Pool이다.

## spring-jdbc가 들어오면 적용되는 자동설정 중 중요한 것
* DataSourceAutoConfiguration
* JdbcTemplateAutoConfiguration

## Connection Pool
* DB에서 Connection관리는 System의 성능과 안전성에 영향
* JDBC에서 매번 클라이언트 요청마다 Connection하면 시간 부담
* 따라서 대부분의 DBMS에서 매번 ConnectionX, Connection Pool이용
* 클라이언트 요청 시점에 Connection을 연결하는 것이 아니라 미리 일정수의 
Connection(Connection객체)을 만들어 놓고 이용
* 미리 만들어두고 클라이언트가 요청하면 Connection Pool에서 Connection 객체를 받아
와 작업을 진행, 작업이 끝나면 다시 Connection Pool에 반납
* 기본 Connection 수, 최대 Connection 수, 필요 시 자동으로 증가하는 Connection 수 
지정 가능, 개발자에 의해 직접 구현 가능, 하지만 대부분의 WAS에서 제공한다
![image](https://user-images.githubusercontent.com/25604495/59162011-da5b2280-8b24-11e9-96e6-5296ce66515e.png)  

## DataSource
* Connection Pool에서 여러개의 Connection객체가 생성되어 운용하는데 Aplication에서 
이를 관리하면 체계적인 관리가 힘들다
* 따라서 DataSource라는 객체를 이용하는데 Connection Pool을 관리하는 사용되는 객체
이다. Application은 이 객체를 통해 Connection을 얻어오고 반납한다
```text
//1
DataSource 객체를 찾아온다
//2
DataSource 객체의 getConnection() 메서드를 통해 Connection Pool에서 Free 상태의 
Connection 객체를 얻는다
//3
얻어진 Connection 객체를 통한 DBMS 작업 수행
//4
작업이 끝나면 DataSource 객체를 통해서 Connection Pool에 Connection 객체 반납
```  

## 의문사항
1) DBCP가 비용이 낮게 드는 이유는 무엇일까?
* 미리 Connection을 만들어놓는다.
* 만약 요청시마다 만든다면?
  * `가장 느린것은 웹 서버에서 물리적으로 DB서버에 연결되어 Connection 객체를 생성하는 부분이다.`
  * 따라서 가장 오래걸리는 Connection 객체를 생성해두면 더 효율적일 것이다.
* Connection과정은
  * DB 서버 접속을 위해 JDBC 드라이버 로드
  * DB 접속 정보를 통해 DB Connection 객체를 얻는다.
  * 적절한 처리후에 close()
* 참고
  * https://www.holaxprogramming.com/2013/01/10/devops-how-to-manage-dbcp/

***

2) Connection을 만들어놓는 시점은?
* WAS가 실행 될 때 애플리케이션에서는 연동할 데이터베이스와의 연결을 미리 설정

***

3) Connection을 빌려주고 반납받는 시점은?
* DB와의 연동 작업이 필요한 요청의 경우에 ConnectionPool에서 필요한 메서드 호출
* 참고
  * https://gptjs409.github.io/java/2019/10/26/servlet-business-logic2.html

* `WAS의 Thread를 ConnectionPool의 Connection 객체보다 많이 설정하는 이유`
  * 모든 Request가 DB작업을 하는것은 아니기 때문이다.
  * 참고
    * https://www.holaxprogramming.com/2013/01/10/devops-how-to-manage-dbcp/

***

4) Connection Pool의 위치는?
* Tomcat

***

5) WAS에 있는 Connection Pool과 DB Server에 있는 Connection Pool의 차이는?

***

6) Connection Pool Library
* Apache Commons
* HickariCP
