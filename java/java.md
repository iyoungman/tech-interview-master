
## TOC
<!-- TOC -->

- [TOC](#toc)
- [디자인 패턴](#디자인-패턴)
- [싱글톤 패턴](#싱글톤-패턴)
- [C와 C++의 차이](#c와-c의-차이)
- [C++과 Java의 차이](#c과-java의-차이)
- [C언어와 Java의 차이](#c언어와-java의-차이)
- [객체지향과 절차지향의 차이](#객체지향과-절차지향의-차이)
- [JAR vs WAR](#jar-vs-war)
- [JDK vs JRE](#jdk-vs-jre)
- [SOLID 원칙](#solid-원칙)
- [전역, 지역변수](#전역-지역변수)
- [객체 지향 개념](#객체-지향-개념)
- [프로세스와 스레드](#프로세스와-스레드)
- [Wrapper Class](#wrapper-class)
- [JVM 메모리](#jvm-메모리)
- [불변 클래스](#불변-클래스)
- [오류 vs 예외](#오류-vs-예외)
- [Call By Value, Call By Reference](#call-by-value-call-by-reference)
- [인터페이스와 추상클래스](#인터페이스와-추상클래스)
- [Integer.parseInt()와 Integer.valueOf()의 차이](#integerparseint와-integervalueof의-차이)
- [Enum](#enum)
- [final](#final)
- [객체와 인스턴스 차이](#객체와-인스턴스-차이)
- [JAVA8의 특징](#java8의-특징)
- [리플렉션](#리플렉션)
- [간단한 질문 리스트](#간단한-질문-리스트)
- [다중상속](#다중상속)
- [Integer[] vs int[]](#integer-vs-int)
- [BufferedReader](#bufferedreader)
- [얕은 복사 vs 깊은 복사](#얕은-복사-vs-깊은-복사)
- [직렬화](#직렬화)
- [GC 메커니즘](#gc-메커니즘)
- [접근 제한자](#접근-제한자)
- [toString()](#tostring)
- [Object 클래스](#object-클래스)
- [JavaBean이란?](#javabean이란)
- [Default Method](#default-method)
- [오버라이딩 vs 오버로딩](#오버라이딩-vs-오버로딩)
- [제네릭](#제네릭)
- [ThreadLocal](#threadlocal)
- [자료형](#자료형)
- [Math.random()](#mathrandom)
- [String Pool](#string-pool)
- [HashMap vs HashTable](#hashmap-vs-hashtable)
- [ClassPath란?](#classpath란)
- [동시성 vs 병렬성](#동시성-vs-병렬성)
- [코어](#코어)
- [하드웨어 스레드 vs 소프트웨어(자바) 스레드](#하드웨어-스레드-vs-소프트웨어자바-스레드)
- [병렬 스트림 vs CompletableFuture](#병렬-스트림-vs-completablefuture)
- [진정한 Call By Reference란?](#진정한-call-by-reference란)
- [Thread run vs start](#thread-run-vs-start)
- [Minor GC vs Major GC](#minor-gc-vs-major-gc)
- [Equals and HashCode](#equals-and-hashcode)
- [CompletableFuture](#completablefuture)
- [포크/조인 프레임워크 vs 스레드풀](#포크조인-프레임워크-vs-스레드풀)
- [멀티 스레드 프로그래밍](#멀티-스레드-프로그래밍)
- [암시적 잠금(Implicit Lock) vs 명시적 잠금(Explicit Lock)](#암시적-잠금implicit-lock-vs-명시적-잠금explicit-lock)
- [Concurrent Package](#concurrent-package)
- [불변 객체](#불변-객체)
- [Hash란](#hash란)

<!-- /TOC -->

***

## 디자인 패턴
* 싱글톤
    - 객체를 하나만 생성
    - private로 선언하여 한번만 생성하도록 한다(전역 인스턴스)
    - private static 변수와 static getInstance 함수를 사용 사용한다
    - 객체가 아닌 static 함수만으로 접근하는 Util 클래스와 비교하면 좋다
    - 단점  
        + `static함수를 통해 전역적으로 접근할 수 있기 때문에 어디서나 접근할 수 있다`
        + [참고 사이트](https://effectiveprogramming.tistory.com/m/entry/Singleton-%ED%8C%A8%ED%84%B4)
    - [stackoverflow-파라미터가 있는 싱글톤](https://stackoverflow.com/questions/1050991/singleton-with-arguments-in-java)
        - 단, 생성자 매개변수라는 것은 인스턴스마다 다른값이 할당될 수 있음을 의미한다
        - 하지만, 싱글톤은 전역적으로 객체가 하나인 값이다
        - `따라서 의미적으로 매개변수가 있는 싱글톤은 싱글톤이 아니다`
        - 생성자가 아닌 별도의 함수로 호출하는 것이 맞는것 같다
        
    - `아래 방식대신 선언과 동시에 인스턴스 생성 방식을 사용하자`
      
    ```java
    //싱글톤 -> 객체로 접근
    private static SigleTone INSTANCE;//static은 메모리 할당 한번
    private SigleTone() {};//생성자를 private로 생성함을 잊지 말자

    public static SigleTone getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new SigleTone();
        }
        return INSTANCE;
    }

    //유틸 클래스 -> static 메소드로 접근
    //인스턴스를 아예 생성하지 않는다
    private Math() {
    }
    public static double sin() {
        return 3.14;
    }
    ```
    - 장점
        + 객체를 하나 생성하여 메모리 낭비 방지
    - 단점
        + 멀티 쓰레딩시 getInstatnce() 동시에 호출하면 인스턴스가 두 번 생성될 여지가 있다
        + 이를 위해 Lock으로 동시성을 제어하는 synchronized 를 사용한다

* 빌더
    - 생성자의 `가독성`이 증가한다
    - 단순 new로 구현할 경우 생성자 `파라미터의 순서`를 신경써야 한다
    - 이는 같은 파라미터 타입이 여러개 나올 경우 잘못된 데이터가 들어갈 가능성이 있다
    - 빌더 패턴은 이러한 문제를 해결해준다

* 전략 패턴
    - 코드의 유연성
    
* 팩토리 메서드 패턴
    - 하위 클래스에서 구체적인 오브젝트 생성 방법 정의  
    - https://iyoungman.github.io/designpattern/DesignPattern-Factory-Method/

***

## 싱글톤 패턴
> [1] Eager initialization
* 사용하지 않아도 생성해야한다
* Thread-Safe(인스턴스가 여러개 생성X)

> [2] Lazy initialization
* 멀티 스레드 문제
* 인스턴스가 여러개 생성될 수 있다.

> [3] Thread safe Lazy initialization
* getInstance()에 synchronized 키워드
* Thread-Safe(인스턴스가 여러개 생성X)

> [3.1] Thread safe Lazy initialization + Double-checked locking
* synchronized 키워드를 메서드가 아닌 코드 내부에
* Thread-Safe(인스턴스가 여러개 생성X)

> [4] Holder에 의한 초기화
* Lazy initialization
* Thread-Safe(인스턴스가 여러개 생성X)
* 중첩 클래스는 Lazy Loading이다.
    * `static 이어서 한번 생성.`
    * https://stackoverflow.com/questions/17799976/why-is-static-inner-class-singleton-thread-safe

```java
public class InitializationOnDemandHolderIdiom { 
    private InitializationOnDemandHolderIdiom(){}
    
    private static class SingleTonHolder{
        private static final InitializationOnDemandHolderIdiom instance = new InitializationOnDemandHolderIdiom();
    }
     
    public static InitializationOnDemandHolderIdiom getInstance(){
        return SingleTonHolder.instance;
    }
}
```

> [5] Enum
* 기본적으로 static final class


* 참고
    * [참고 사이트](https://limkydev.tistory.com/67#:~:text=%EC%9D%B4%EB%A5%B8%20%EC%B4%88%EA%B8%B0%ED%99%94%20%EB%B0%A9%EC%8B%9D%EC%9D%80%20%EC%8B%B1%EA%B8%80,%EC%95%8A%EB%8A%94%20%EA%B2%BD%EC%9A%B0%EC%97%90%20%EC%82%AC%EC%9A%A9%ED%95%A9%EB%8B%88%EB%8B%A4.&text=%EC%9E%A5%EC%A0%90%20%3A%20static%EC%9C%BC%EB%A1%9C%20%EC%83%9D%EC%84%B1%EB%90%9C,%ED%86%A4%20%EA%B0%9D%EC%B2%B4%EA%B0%80%20%EC%83%9D%EC%84%B1%EB%90%A9%EB%8B%88%EB%8B%A4.)
    * [참고 사이트](https://yaboong.github.io/design-pattern/2018/09/28/thread-safe-singleton-patterns/)


***

## C와 C++의 차이
* 패러다임
    - C는 기본적으로 절차지향, C++은 객체지향 언어
* https://codingcoding.tistory.com/287  

***

## C++과 Java의 차이
https://preamtree.tistory.com/6  

***

## C언어와 Java의 차이
* 가비지 콜렉션
    - Heap 영역의 동적 객체들을 관리
* JVM
    - 자바의 이식성 UP
    - `자바 바이트코드를 OS에 맞게 해석(OS에 따른 영향X)`
    - **C언어의 경우 OS에 따라 코드 재사용이 어려운 단점이 있다**
    - [JVM 자세한 내용](https://www.holaxprogramming.com/2013/07/16/java-jvm-runtime-data-area/)

***

## 객체지향과 절차지향의 차이  
* 절차지향
    - 절차지향에서의 함수는 구조체 멤버가 아닌 프로그램의 한 기능으로써 작동
    - 전체 흐름이 함수를 위주로 실행된다
    - **함수의 실행 및 함수 사이의 데이터 공유 중시**  

* 객체지향
    - 주요 구성요소가 객체
    - 객체의 함수(멤버함수)를 통한 **객체 사이의 메시지로 흐름이 진행**
    - 객체지향도 절차에 맞게 실행된다. 다만 객체의 개념이 들어갔을 뿐이다

* 결론
    - 절차지향은 데이터와 함수가 따로 묶여있지만
    - 객체지향은 데이터와 함수가 함께 묶여있다
    - 따라서 객체지향은 구조화, 파악이 용이하다

* [참고 사이트](http://blog.naver.com/PostView.nhn?blogId=hirit808&logNo=221457311265&categoryNo=35&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=search)

***

## JAR vs WAR
* JAR
    - `클래스` 파일을 압축

* WAR
    - 웹 어플리케이션 관련 파일 압축

***

## JDK vs JRE
* JRE + @ = JDK
* `JRE` = JVM + JVM 실행하는데 필요한 파일
* `JDK` = JRE + `컴파일러`

***

## SOLID 원칙
* SRP(단일책임 원칙)
    - `한 클래스에서 하나의 책임만 수행하는 것`
    - 변경 사유를 하나로 만들어야한다
    - 하나의 클래스가 맡은 책임이 많아질수록 클래스 내부의 코드 파악, 변경에 따른 비용이 커진다
    - 장점
        - `변경에 따른 비용 감소`

* OCP(개방폐쇄의 원칙)
    - 기존 코드의 변경을 최소화하면서 새로운 기능을 추가할 수 있어야한다
    - 인터페이스를 사용하여 추상화하고 런타임때 객체 할당
        - `전략패턴과 연관`
    - 장점
        - `변경에 유연하다`
        - https://dev-momo.tistory.com/entry/SOLID-%EC%9B%90%EC%B9%99

* LSP(리스코프 치환 원칙)
    - `자식 클래스는 부모 클래스를 대체할 수 있어야한다.`
    - 클라이언트가 어떤 자식 클래스와도 협력할 수 있는 상속 구조를 제공
    - 클라이언트의 코드를 변경하지 않고도 새로운 자식 클래스와 협력할 수 있다
    - http://www.nextree.co.kr/p6960/
    - `적용 사례 : Collection`
    ```java
    public static void main(String[] args) {
        List list = new ArrayList();

        addToList(list);
    }

    private static void addToList(List list) {
        list.add(5);
    }
    ```
    > 컬렉션 프레임워크가 LSP를 준수하지 않고 구현되었다면 
    > 
    > List 인터페이스를 통해 수행하는 작업이 제대로 수행될 수 없었을것이다.
    >
    > 하지만 LSP를 준수하여 구현되었기때문에(ArrayList, LinkedList 모두)
    >
    > addToList()메서드가 잘 동작하며 OCP 구조가 될 수 있다. 따라서 LSP와 OCP는 연관이 깊다.

    - 장점
        - `유연한 설계(OCP)의 기반이 LSP이다.`

    - 자식 클래스는 부모 클래스에서 가능한 행위는 수행할 수 있어야한다(대체할 수 있어야한다)
    - [참고 사이트](https://defacto-standard.tistory.com/112?category=703460)  
    - [참고 사이트2](https://victorydntmd.tistory.com/291)
    - [참고 사이트3](https://nesoy.github.io/articles/2018-03/LSP)


* ISP(인터페이스 분리 원칙)
    - `클라이언트가 이용하지 않는 기능은 분리해야한다.`
    - `여러 인터페이스로 나누고 다중상속을 이용하는 방식`
    - 장점
        - 객체가 커지는 것을 막아준다.
        - 변경에 의한 영향을 줄인다.
    - [참고 사이트](http://wonwoo.ml/index.php/post/1675)
    - [참고 사이트2](https://ktko.tistory.com/entry/%EC%9E%90%EB%B0%94-%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5%EC%9D%98-%EC%9B%90%EB%A6%AC-SOLID-ISP-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4-%EB%B6%84%EB%A6%AC-%EC%9B%90%EC%B9%99)  

* DIP(의존성 역전 원칙)
    - 구체적인것보다 추상적인 것에 의존
    - 변화하기 쉬운것보다 변화가 어려운 것에 의존
    - 변화하기 어려운 것은 **추상 클래스, 인터페이스를 의미**  
    ![image](https://user-images.githubusercontent.com/25604495/69413168-a337a100-0d53-11ea-8f99-d8024886bfc4.png)  
    - 장난감은 변화가 어려운 개념
    - 때로는 로봇을, 모형 자동차를, 레고를 가지고 놀 수 있다
    - 따라서 변화가 쉬운 개념  
    - `변경에 영향을 덜 받는다`
    - [참고 사이트](https://defacto-standard.tistory.com/113)  
    - 장점
        - `변경에 의한 비용을 줄일 수 있다.`
        - 추상적인 것은 잘 변화하지 않음을 이용(인터페이스)
        - https://server-engineer.tistory.com/228

***

## 전역, 지역변수
* 전역 변수
    - 클래스 변수(static)
    - 인스턴스 변수
    - JVM Runtime 데이터 영역에서 `클래스 영역`에 저장
    - Thread-Safe 하지 못하다

* 지역 변수
    - JVM Runtime 데이터 영역에서 `스택 영역`에 저장
    - Thread-Safe 하다

***

## 객체 지향 개념
* `캡슐화`
    * 정보은닉을 통해 높은 응집도와 낮은 결합도를 갖도록 한다.
    * 키워드는 **정보은닉, 높은 응집도, 낮은 결합도**
    * 클래스의 퍼블릭 인터페이스만으로 사용 방법을 이해할 수 있는 코드가 캡슐화 관점에서 훌륭한 코드다.  
    - `높은 응집도`
    - 변경에 대한 비용과관련
      
* `상속`
    - 부모의 기능 재사용, 재정의를 통해 `코드 재사용`
    - 다형성 구현
    - 생성자
        - 부모의 생성자가 먼저 호출된 후 자식 생성자가 호출된다
      
* `다형성`
    - `하나의 객체타입이 런타임시 여러 타입을 할당 가능`
    - 여러 타입의 인터페이스 동일하다 -> 유연성  
    - **추상 클래스, 인터페이스(자식이 반드시 추상메소드를 구현하게 하기 위함)**
    - 자식들간의 다형성 관계 핵심

***

## 프로세스와 스레드
* 멀티 프로세싱
    * `여러 프로그램을`
    * `병렬적으로 처리할 수 있다`
    * 즉 여러 프로세스를 실행한다는 의미이다.
* 멀티 스레드
    * `하나의 프로그램안에서`
    * `다수의 스레드가 동시 처리`
    * 즉 한 프로세스를 실행한다는 의미이다.
    * `한 프로세스 내라는 점 때문에 데이터 공유가 쉽다.`

* 차이점
    * 멀티 프로세싱은 `여러 프로그램`을 동시 처리.
    * 멀티 스레딩은 `하나의 프로그램 안에서 여러 작업`을 동시 처리.

* 프로세서 vs 프로세스
    * 프로세서
        * 하드웨어 측면
        * 프로그램을 실행하도록 하는 하드웨어 유닛
        * CPU
    * 프로세스
        * 실행중인 프로그램

***

## Wrapper Class
* `기본 타입을 객체로 만든다`
    - Integer, String
* 형변환 용이, 여러 기능 사용

***

## JVM 메모리
* `자바 프로그램이 OS 관계없이 실행되도록 만든 가상머신`
* 인스턴스 변수는 Heap영역에 저장된다.
* https://itmining.tistory.com/21

***

## 불변 클래스
* `인스턴스 내부 값 수정X`
* 새로운 객체 생성
* 장점
    - Tread-Safe 하다
* 단점
    - 메모리 효율이 좋지 않다
* [참고 사이트](https://devonce.tistory.com/26)  

***

## 오류 vs 예외
* 오류
    - `시스템 레벨`
    - 사용자가 예측X 처리X  
      
* 예외
    - `개발자의 구현 로직에서 발생`

* Checked Exception vs UnChecked Exception
    - **Checked Exception**은 `컴파일 과정에서 체크 가능`
    - 따라서 개발자가 반드시 처리해야한다
    - IOException ,SQLException  
    <br>
    - **UnChecked Exception**은 `컴파일 과정에서 체크 불가능`
    - 개발가 처리를 해도 되고 안해도 된다(발생 할지 모르므로)
    - NullPointException, IndexOutException

***

## Call By Value, Call By Reference
* Call By -> 함수 호출
* `기본 타입 전달, 참조 타입 전달`
* 기본 타입과 참조 타입의 차이점은 **변수에 저장되는 값**
    - 기본 타입은 실제 값이 저장
    - 참조 타입은 주소 값이 저장

***

## 인터페이스와 추상클래스
* 공통점
    - `자식 클래스가 무언가 반드시 구현하도록 위임할 때 사용`  
    - 익명클래스를 이용하여 인스턴스 생성 가능
        - https://yookeun.github.io/java/2017/01/24/java-anonymousclass/  

* **차이점**
    - 추상클래스는 멤버 변수와 구현된 메소드도 존재 가능하기 때문에
    - 부모의 기능도 재사용하며 구현을 위임할 때(추상 메소드)
    - 즉, `부모 자식 관계를 통한 상속을 받아 기능을 확장하는게 목적`  
    <br>  
    
    - 인터페이스는 구현클래스에 같은 동작을 보장하면서 서로 다른 구현이 필요할때
    - `부모의 기능도 재사용하며 구현을 위임할 때(추상 메소드)`  
    - `상속을 받아서 기능을 확장시키는 데 목적`  

***

## Integer.parseInt()와 Integer.valueOf()의 차이
* parseInt()는 기본 Type을 반환(int)  
* valueOf()는 참조 Type을 반환(Integer)  
* [참고 사이트](https://m.blog.naver.com/sthwin/221000179980)  

***

## Enum
* 문자로 처리하는 것보다 Enum 타입으로 처리하면 안전하다
* `Grouping 성격`
* **즉, Type의 안정성을 가진다**
* [참고 사이트](https://limkydev.tistory.com/50) 

***

## final
* `더이상 변경을 못하도록 할 때 사용`
* static과 함께 사용하는 이유
    - 예를들어 원주율, 1년의 날짜 같은 값은 `상수의 의미`를 가지고 있다
    - **final을 사용해 더이상 변경을 못하게 함은 물론이고**
    - **static을 사용해 불필요한 객체생성을 막는다(언제 어디서나 동일한 값이기 때문이다)**
    - static은 클래스 변수
* [참고 사이트](https://hunit.tistory.com/159)  

***

## 객체와 인스턴스 차이
* 객체는 선언 되었을 때
* 인스턴스는 선언된 객체가 할당 되었을 때
    * Heap 영역에 올라갈 때
* [참고 사이트](https://gmlwjd9405.github.io/2018/09/17/class-object-instance.html)  

***

## JAVA8의 특징
> [전반] 함수형 프로그래밍(선언형)  

* 명령형 vs 선언형
    * 명령형은 코드적으로 어떻게 해결해나갈지 중심
    * `선언형은 코드적으로 무엇을 해결해나갈지 중심 -> 간결한 코드`
    * https://swiftymind.tistory.com/108

> 요구사항

* `간결한 코드`
* `멀티코어 프로세서의 쉬운 활용`

> [특징 1] 메서드의 인자로 메서드 동작을 전달(함수를 값 형태로 취급) = 람다식
* 코드가 유연해졌다.
    * [참고](https://medium.com/chequer/java8-in-action-1%EC%9E%A5-%EC%9E%90%EB%B0%948%EC%9D%84-%EB%88%88%EC%97%AC%EA%B2%A8%EB%B4%90%EC%95%BC-%ED%95%98%EB%8A%94-%EC%9D%B4%EC%9C%A0-a62b4243b1d8)
* 일급 시민
    * 런타임때 동적으로 전달 가능.
    * Java8부터 메서드를 일급 시민으로 취급(람다식).
    
* `람다식`
    * 다른 말로 익명 메서드.
    * 기존에는 객체를 전달해야했다.(익명 클래스)
        * 메서드만 전달할 수 없었기 때문이다.
        ```java
        new Thread(new Runnable() {
            @Override
            public void run() {
                
            }
        })
        ```
        
    * `람다식을 통해 메서드만 전달할 수 있다.`
    ```java
     new Thread(() -> {//Runnable 객체 생성하지 않고 run의 동작(메서드) 전달
            for(int i = 0; i < 6; i++) {
                System.out.println("hello");
            }
    })
    ```
    
    * 참고
        * http://hong.adfeel.info/backend/java-%EB%9E%8C%EB%8B%A4%EC%8B%9D%EC%9D%B5%EB%AA%85%EB%A9%94%EC%86%8C%EB%93%9C/

* 동작 파라미터화
    * `런타임때 실행할 코드를 선택하는 기법`
    * `Java8에서 나온 개념은 아니다!`
    * 전략 패턴도 동작 파라미터화의 한 방법이다.
        * 인터페이스 - 구현체


> [특징 2] 스트림 API 
* 컬렉션 처리에 사용.
* `스트림 API는 메서드 동작을 전달한다는 사상에 기초하기 때문에 위의 메서드 인자로 메서드 동작 전달과 연관이 깊다.`  
<br>
* 기존
    * 외부 반복
    * 프로그래머가 정의
* 스트림
    * 내부 반복
    * 라이브러리 내부에서 처리  
    
<br>

* 장점
    * 가독성
    * Lazy연산
        * 중간연산이 아닌 최종연산때 계산이 된다.
        * `장점1. 한번에 여러 중간 연산을 묶어서 사용할 수 있다.(가독성)`  
        * `장점2. 메모리 효율성`  
<br>

* `단점, 주의할점`
    * 무조건 빠른것은 아니다.
    * `단순히 출력과 같은 결과 연산만 하면 오히려 느릴 수 있다.`
    * `스트림은 요소를 박싱 처리하기 때문이다.`
> [특징 3] 인터페이스 디폴트 메서드
* 인터페이스에 구현체 작성.

* 도입
    * `메서드를 메서드 인자로 전달(동작 파라미터화)`
        * 스트림 API 
            * 컬렉션 관련 처리를 단순화.
            * 동작 파라미터화를 통해 스트림 API를 사용.
    * 스트림 API

***

## 리플렉션
* [iyoungman-정리](https://github.com/iyoungman/java-practice#Reflection)
* `컴파일 타임에 클래스 Type 정보를 알지 못해도 런타임에 Type 정보를 알 수 있다`
* 객체의 생성이나 메서드 호출을 동적으로 할 수 있다
* 사용 예시
    * private method 테스트
    * `annotation`
        * `어노테이션은 컴파일시 .class파일에 포함된다`
        * `JVM 실행시 런타임 때 refelction을 통해 자세하게 읽힌다`
* [참고 사이트](https://www.notion.so/API-4a7094eb42b0400780dfa56642db7886)
* [참고 사이트2](https://qssdev.tistory.com/27)
* [참고 사이트3](https://jwkim96.tistory.com/97)

***

## 간단한 질문 리스트
* Thread 가 3개 생성 되었을 때 t1, t2, t3의 순서가 보장 되는 코드
    * synchronized 키워드
    * Thread.join() 메서드
        * `join()을 호출한 스레드의 사용이 완료될 때까지` 다른 스레드를 호출하지 않는다
    ```java
    thread1.start();
    thread1.join();

    thread2.start();
    thread2.join();
    ```

    * 주의
        * Priority는 `순서가 보장되지 않는다`
        * 차례대로 실행되는 것이 아니라 `더 빨리 끝나는 것이다`
            * t1 -> 10, t2 -> 1 이면
            * 실행은 번갈아 가면서 되지만, t1이 우선 순위가 높아 더 일찍 끝날 뿐이다
        * https://endorphin0710.tistory.com/27

* Java8에서 Stream을 사용한 경우 발생 할 수 있는 문제점은?
    * 재사용의 문제
    * https://okky.kr/article/329818

* 가비지 컬렉션
    * `gc 시점`
    * 대상은 heap 영역이다

***

## 다중상속
* 인터페이스는 다중상속 가능
    * 어떤 메서드인지 모호해도 `구현`하면 된다
    * `인터페이스는 구현이 없기 때문이다`

* 클래스는 다중상속 불가능
    * 어떤 메서드인지 모호하다
    * 이미 구현되어있기 때문이다

* 참고
    * http://tcpschool.com/java/java_polymorphism_interface

***

## Integer[] vs int[]
* 차이가 있다  
* 배열 자체는 객체이므로 Heap에 들어가는것은 맞다.
* Integer[]는 Heap영역에서 각 원소의 주소값을 가지고있다.
* intp[]는 Heap영역에서 단순 값을 가지고 있다.
* https://stackoverflow.com/questions/18845289/int-and-integer-arrays-what-is-the-difference  

***

## BufferedReader
* read() vs readLine
    * https://yoonka.tistory.com/363
* Split 대신 StringTokenizer와 함께 사용하는 것이 좋다

***

## 얕은 복사 vs 깊은 복사
* 얕은 복사
    * 주소 복사
    ```java
    One cloneOne = otherOne;
    ```

    ```java
    public void printList() {
        Node tempNode = head;//(1)
        tempNode.data = 55;//(2)

        while (tempNode != null) {
            tempNode = tempNode.nextNode;//(3)
        }
    }
    ```
    * LinkedList의 코드 일부분이다.
    * 얕은 복사 이므로 (1)에서 tempNode와 head는 같은 주소를 바라보게된다.
    * 따라서 (2)와 같이 tempNode.data or head.data를 바꾸면 서로에게 영향을 주게된다.
    * (3)에서는 tempNode에 새로운 주소값(tempNode.nextNode)을 할당한다.
    * 따라서 head에는 영향이 없다.

* 깊은 복사
    * 실제 값 복사
    * 객체의 clone() 재정의
    * https://taetaetae.github.io/2018/08/21/how-to-use-cloneUtils  /
    ```java
    One cloneOne = otherOne.clone();
    ```

***

## 직렬화
* 직렬화란?
    * `Object 또는 Data를 외부의 자바 시스템에서도 사용할 수 있도록 byte 형태로 데이터를 변환하는 기술`

> 데이터의 전송은 '값' 형태만 가능하다.
> 
> 객체는 '주소값'을 가지고 있으므로 기본적으로 네트워크 같은 전송이 불가능하다.
>
> 또한 JVM을 재실행 시키면 메모리 주소는 바뀌기때문에 주소를 전송하는것은 옳지 않다.
>
> `직렬화는 객체의 주소값 실체를 다 끌어와서 Primitive 한 값 형식 데이터로 전부 변조하는 작업이다.`
>
> 결국 직렬화가 된 데이터는 최종적으로 객체 타입이 없으며 모두 Primitive한 값이다.
>
> https://okky.kr/article/224715

* 자바의 직렬화
```java
implements Serializable
```

* Json, Xml, Csv 모두 직렬화의 한 종류이다.

* 자바 직렬화 vs Json 방식
    * 자바 직렬화는 간편하게 직렬화 할 수 있다.
        * `implements Serializable`만 구현하면 된다.
    * Json을 이용한다면 특정 라이브러리를 이용해야 쉽게 직렬화 및 역직렬화 할 수 있다.
        * Jackson
        * Gson
    * Redis의 예제를 생각하면 쉽다

* 참고
    * https://woowabros.github.io/experience/2017/10/17/java-serialize.html
    * https://yangbongsoo.gitbook.io/study/effective-java-3rd-edition/serialization

***

## GC 메커니즘
* `힙 영역에서 사용되지 않는 객체를 메모리에서 제거`
    * `참조하고 있지 않은 힙 영역의 객체를 찾아 메모리 회수`
    * MarkAndSweep
        * Mark
            * Garbage Collector가 모든 변수들을 돌며 Heap영역에 참조하고 있는 객체 찾는 과정
            * 이때 프로그램내의 모든 스레드가 중단된다.
            * 이를 `stop the world`라고 한다.
            * 따라서 비용이 드는 System.gc()는 아무때나 호출하지 말자.
        * Sweep
            * Mark되지 않은 객체 제거

* 참고
    * https://yaboong.github.io/java/2018/05/26/java-memory-management/
    * https://asfirstalways.tistory.com/159

***

## 접근 제한자
> public > protected > default > private

* protected는 default에
* 외부 패키지라도 상속받는 클래스는 접근 가능하다.

* 주의할점
```java
public class Parent {
    protected String name = "Parent";
}
```

> 외부 패키지에서는 Parent를 상속받은 인스턴스만 접근가능하다.
>
> `Parent 인스턴스라도 외부 패키지에서는 당연히 접근 못한다.(위치가 기준이다)`

***

## toString()
* Object에 정의되어있다.
* 일반적인 객체의 경우 다음과 같이 정의.
```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

* 객체의 내부 값을 출력하고 싶다면 재정의
```java
@Override
public String toString() {
    return "Iyoungman{" +
            "name='" + name + '\'' +
            ", age='" + age + '\'' +
            '}';
}
```

* 참고로 객체를 Print할때는
* 객체.toString()으로 내부적으로 변환한다.
* 따라서 아래 두 결과는 같다.
```java
System.out.println(iyoungman);
System.out.println(iyoungman.toString());
```

* 이때 객체의 주소값을 출력하고 싶다면?
```java
//hashCode는 재정의하지 않았다고 가정
객체.hashCode();
```

***

## Object 클래스
* 암시적으로 상속
* https://shrtorznzl.tistory.com/42

***

## JavaBean이란?
* 자바 빈은 데이터를 표현하는 것을 목적으로 하는 자바 클래스.

* Pojo vs JavaBean
    * https://www.geeksforgeeks.org/pojo-vs-java-beans/  
    * Spring의 Entity는 형식에 보다 자유로운
    * Pojo에 가깝다.
    * Pojo는 특정 기술에 얽매이지 않고
    * 객체지향적으로 객체를 구성한것.


***

## Default Method
* 인터페이스 구현 가능
* 왜 사용?
    * 해당 인터페이스의 구현체를 변경하지 않아도 된다.


***

## 오버라이딩 vs 오버로딩
* 오버라이딩
    * 재정의

* 오버로딩
    * 같은 메서드 이름
    * `파라미터 타입, 개수`
    * `Return Type만 다를 경우는 해당되지 않는다.`

* 둘다 다형성과 관련있다.

***

## 제네릭
* Type의 안정성 보장.
* 잘못된 타입이 사용될 수 있는 문제를 `컴파일` 과정에서 제거.

```java
//여러 타입이 들어갈 수 있다.
//런타임때 예외가 발생한다.
List list = new ArrayList();

//Integer만 들어갈 수 있다.
List<Integer> list = new ArrayList();
```

***

## ThreadLocal
* 같은 Thread 내에서 전역적으로 값을 공유할 수 있도록 하는것.
* 주의사항
    * 쓰레드 풀 환경에서 ThreadLocal을 사용하는 경우
    * ThreadLocal 변수에 보관된 데이터의 사용이 끝나면 반드시 해당 데이터를 삭제
    * 그렇지 않은 경우 스레드가 재사용되므로 해당 값이 유지될 수 있다.
    * 요청이 끝나면 ThreadLocal의 정보 제거.(재사용 할 수 있기 때문에)

* 적용예제
    * Spring Security
        * 사용자 인증 정보를 ThreadLocal로 관리.
        * 필터에서 인증 확인 후 SecurityContextHolder에 저장한다.(ThreadLocal)
        * `이후 다른필터 and 요청된 서비스에서 인증 정보를 가져올때 ThreadLocal에서 가져온다.`

* 참고
    * [iyoungman-예제코드](https://github.com/iyoungman/java-practice/blob/master/src/main/java/threadlocal/Main.java)  
    * https://javacan.tistory.com/entry/ThreadLocalUsage
    * https://stackoverflow.com/questions/45725888/why-does-spring-security-store-securitycontext-in-thread-local-variable

***

## 자료형
![image](https://user-images.githubusercontent.com/25604495/81573765-e6f49480-93df-11ea-986c-bfd9dd5fd840.png)  

***

## Math.random()
> 1 ~ 10까지 랜덤함수 출력

```java
 //0.0 <= value < 1.0
double value = Math.random();

//1 ~ 10
int result = (int) (value * 10) + 1;
```

> (int) (value * 최대값) + 최소값


***

## String Pool
* Heap영역 내에 위치.
* `리터럴 방식으로 생성한 String 값은 같은 주소를 바라본다.`
> String a = "a";

* 사용 이유
> 불변객체인 String의 메모리 공간의 효율성을 위해서
* 참고
    * https://dololak.tistory.com/718?category=636500

***

## HashMap vs HashTable
* 공통점
    * 키에 대한 해시 값을 사용하여 값을 저장하고 조회
* 차이점
    * ThreadSafe 차이
    * 보조 해시 함수
        * HashMap은 보조 해시 함수를 사용하기 때문에 해시 충돌이 덜 발생
        * 성능상의 이점

***

## ClassPath란?
* JVM이 프로그램을 실행할 때 Class 파일을 찾는 경로.


***

## 동시성 vs 병렬성
* 동시성
    * 싱글코어에서 멀티 쓰레드 동작
    * `동시에 실행되는 것처럼 보이지만, 여러 쓰레드가 번갈아가면서 동시에 실행되는 것이다.`
* 병렬성
    * 멀티코어에서 멀티 쓰레드 동작
    * `자바8 병렬 스트림`
        * 전체 데이터를 멀티 코어의 수만큼 서브 데이터들로 나눈 후
        * 각각의 데이터들을 분리된 쓰레드에서 병렬 처리.

* 참고
    * https://goodgid.github.io/Concurrency-vs-Paraleelism/

***

## 코어
* 멀티코어
    * CPU 1당 여러개의 코어
    * 듀얼코어, 쿼드 코어
* `내 컴퓨터`
    * 4코어
    * 8논리 프로세서
    * 즉, 1코어당 2스레드를 할당 할 수 있다는 의미이다.
    * https://m.post.naver.com/viewer/postView.nhn?volumeNo=9140043&memberNo=10558726  
* 코어란?
    * CPU의 핵심 요소 중 하나.
    * 연산을 할 수 있도록 해주는 것.

***

## 하드웨어 스레드 vs 소프트웨어(자바) 스레드
* 하드웨어 스레드
    * 위의 CPU 코어와 관련이있다.
    * `운영체제는 CPU 논리 프로세서만큼 동시 실행 가능하다.`

* 소프트웨어(자바) 스레드
    * `위의 하드웨어 스레드와 다르다.`
    * `메모리가 허용하는한 얼마든지 만들 수 있다.`
    * `하나의 하드웨어 스레드는 많은 소프트웨어 스레드를 실행할 수 있다.`
    * 즉, 병렬로 실행되는 것이 아니라 `동시성`으로 실행되는 것이다.

* 참고
    * https://badcandy.github.io/2019/01/14/concurrency-01/

***

## 병렬 스트림 vs CompletableFuture
* 병렬 스트림
    * `병렬성`
    * `Runtime.getRuntime().availableProcessors()` 만큼 병렬 실행
* CompletableFuture
    * `동시성`
    * 따라서 소프트웨어 스레드와 관련이있다.
    * 즉, 동시 실행 스레드 개수의 제한이없다.

***

## 진정한 Call By Reference란?
* Java에서의 Call By Reference는
    * `매개변수에 주소값을 복사하여 넘기는 것이다.`
* C++에서는 참조변수를 이용해 원본을 직접 넘긴다.
* 참고
    * https://deveric.tistory.com/92?category=346694
    * https://m.blog.naver.com/star7sss/220852411905

***

## Thread run vs start
* run
    * 하나하나 block 되면서 실행
* start
    * 동시적으로 실행
* 참고
    * https://deveric.tistory.com/21?category=346694

***

## Minor GC vs Major GC
* Minor GC
    * Copy And Scavenger(스케빈절)
    * 복사 및 청소
    * Minor GC는 자주 일어나기 때문에
    * 시간이 짧은 알고리즘이 적합하다.
* Major GC
    * 여러가지 방식이 있다.
    * Mark-Sweep-Compaction vs Mark-Summary-Compaction
        * Old 영역을 단일 스레드가 훑는지 vs 여러 스레드가 훝는지.
* 참고
    * https://iyoungman.github.io/java/MinorGC-and-MajorGC/

***

## Equals and HashCode
* Equals
    * 객체의 동등성을 정의하기 위한 메서드.

* HashCode
    * 객체의 해시코드 값을 반환하는 메서드.

* equals를 재정의하려거든 hashCode도 재정의하라
    * equals()를 재정의해서 두 객체가 같을경우 논리적으로 동등하다는 의미
    * 따라서 hashCode도 재정의해서 동일한 해쉬코드를 반환하도록 해야한다.
    * Hash함수를 사용하는 HashMap같은 곳에서는 Map의 Key 해쉬코드를 비교한 후 Key의 Equals를 비교한다.(같은 해시값에 대해 여러 Key가 있을 수 있다.)

* https://iyoungman.github.io/java/Java-Equals-and-HashCode/  


***

## CompletableFuture
* https://iyoungman.github.io/java/Java8-CompletableFuture/

***

## 포크/조인 프레임워크 vs 스레드풀

![image](https://user-images.githubusercontent.com/25604495/85825247-b49acd00-b7bc-11ea-9b3d-7d7d8e5d6bb9.png)  

* ExcutorService
    * 여러 스레드를 하나의 스레드 풀로 묶어준다.

* 작업 훔치기
    * 각각의 태스크를 동일하게 분할한다.
    
* 참고
    * https://hamait.tistory.com/612
    * https://stackoverflow.com/questions/21156599/javas-fork-join-vs-executorservice-when-to-use-which

***

## 멀티 스레드 프로그래밍
> Thread-Safe를 위한 방법은 크게 3가지가 있다.

1) Lock
2) Concurrent Package
3) 불변 객체 


## 암시적 잠금(Implicit Lock) vs 명시적 잠금(Explicit Lock)
* 암시적 잠금 
    * `synchronized 키워드O`
    * 하나의 공유 자원을 하나의 스레드만 사용.
* 명시적 잠금
    * `synchronized 키워드X`
    * 동시에 여러 Lock 사용가능.

* 참고
    * [참고 사이트](https://medium.com/@sunminlee89/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EB%A9%80%ED%8B%B0%EC%8A%A4%EB%A0%88%EB%94%A92-%EC%9E%90%EB%B0%94%EC%9D%98-%EB%A9%80%ED%8B%B0%EC%8A%A4%EB%A0%88%EB%94%A9-caf9d0667c8)
    * https://deveric.tistory.com/104

***

## Concurrent Package
* 멀티 스레드에 Safe하다.
* 내부적으로 락으로 인한 성능 저하를 최소화하도록 구현되어있다.
* 동시에 여러 스레드가 하나의 자원에 접근하더라도 Safe하게 구현.

***

## 불변 객체
* `객체의 내부값이 변하지 않는것(객체 안의 객체도 포함)`
* `Thread-Safe를 위해 락을 걸 필요가 없다.`
* 불변 객체 만드는 법.
    * 생성자를 통해 객체를 만든다.
    * setter와 같은 수정 메서드 만들지 않는다.
* 참고
    * https://deveric.tistory.com/104

***

## Hash란
* HashMap의 해시버킷
    * `Key가 저장된 장소`
    * 각 해시버킷은 링크드 리스트 형태로 되어있다.

* 메모리 효율성을 위해 Key의 개수보다 해시버킷의 개수가 적다. -> 해시충돌

***
