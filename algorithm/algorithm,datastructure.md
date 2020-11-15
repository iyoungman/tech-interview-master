## TOC

<!-- TOC -->

- [TOC](#toc)
- [시간복잡도](#시간복잡도)
- [시간복잡도 계산](#시간복잡도-계산)
- [O(logN) 시간 복잡도 분석](#ologn-시간-복잡도-분석)
- [피보나치](#피보나치)
- [선형, 비선형 자료구조](#선형-비선형-자료구조)
- [정렬의 종류](#정렬의-종류)
- [선택정렬](#선택정렬)
- [삽입정렬](#삽입정렬)
- [버블정렬](#버블정렬)
- [퀵정렬](#퀵정렬)
- [합병정렬](#합병정렬)
- [합병정렬 vs 퀵정렬](#합병정렬-vs-퀵정렬)
- [힙정렬](#힙정렬)
- [위상 정렬](#위상-정렬)
- [안정 정렬, 불안정 정렬](#안정-정렬-불안정-정렬)
- [빠른 선택 알고리즘](#빠른-선택-알고리즘)
- [자료구조 종류](#자료구조-종류)
- [ArrayList vs LinkedList](#arraylist-vs-linkedlist)
- [완전탐색](#완전탐색)
- [순열, 조합](#순열-조합)
- [백트래킹](#백트래킹)
- [DFS와 BFS](#dfs와-bfs)
- [스택, 큐 사용](#스택-큐-사용)
- [DFS, BFS 최적의 해](#dfs-bfs-최적의-해)
- [다익스트라 알고리즘](#다익스트라-알고리즘)
- [비트마스크](#비트마스크)
- [이분 탐색](#이분-탐색)
- [트라이](#트라이)
- [유니온 파인드](#유니온-파인드)
- [세그먼트 트리](#세그먼트-트리)
- [코딩테스트](#코딩테스트)
- [최장 수열](#최장-수열)
- [트리 자료구조 입력 방법](#트리-자료구조-입력-방법)
- [DP](#dp)
- [해시 알고리즘](#해시-알고리즘)
- [자바 자료구조 정리](#자바-자료구조-정리)
- [HashMap vs HashSet](#hashmap-vs-hashset)
- [자주 나오는 문제 유형](#자주-나오는-문제-유형)

<!-- /TOC -->

***  

## 시간복잡도
* 얼마나 걸리는지 구하는 것
* 연산 1억번에 1초정도라고 생각하면 된다  
[참고 사이트](https://skmagic.tistory.com/164)
  
* 참고로 공간복잡도는
    - 메모리를 얼마나 사용하는지 구하는 것

***  

## 시간복잡도 계산
* 실행 횟수를 구한다  
* Big O, O(N)은 최악의 시간을 구하는 것  
![image](https://user-images.githubusercontent.com/25604495/66132758-896dbd80-e630-11e9-80bb-0ab100211cef.png)
    - 1 : 항상 한번의 단계로 끝
        - 2회 걸려도 O(1)이다
        - 즉 n에 관계없이 상수 시간이 걸렸을 때는 O(1)이다
    - log n : n개를 계속해서 절반으로 나누는 연산(이진 탐색 같은..)  
    - log n의 경우 빠른 탐색 알고리즘, n log n의 경우 빠른 정렬 알고리즘  

* [참고 사이트](https://www.youtube.com/watch?v=6Iq5iMCVsXA)  

***  

## O(logN) 시간 복잡도 분석
* **시간 복잡도는 자료의 개수가 N일때 실행 횟수를 구하는 것**
* 이진탐색(logN)
    - [참고 사이트](https://zelord.tistory.com/11)  
    - [참고 사이트](https://jwoop.tistory.com/9)  
* 퀵정렬 O(NlogN)
    - 피봇과의 비교회수 N번
    - 분할하므로 logN번(단, 최악의 경우 N번)
        - `쉽게 말하면 분할의 횟수`
        - 8개 원소를 퀵 정렬한다면
        - 분할 횟수(3) * 회차별 비교(8)
    - 둘을 곱하면 N * logN
    - [참고 사이트](https://m.blog.naver.com/PostView.nhn?blogId=devace&logNo=220539413241&proxyReferer=https%3A%2F%2Fwww.google.com%2F)   

***  

## 피보나치
```java
//재귀 시간복잡도 2의 N승
int fibonacchi(int n) {
    if(n == 0) return 0;
    if(n == 1) return 1;

    return fibonacchi(n - 1) + fibonacchi(n - 2);
}

//반복문 시간복잡도 N
int[] array = new int[n];
array[0] = 0, array[1] = 1;
for(int i = 2; i<=n; i++) {
    array[i] = array[i-1] + array[i-2];
}
```

*** 

## 선형, 비선형 자료구조
* 하나의 자료 뒤에 여러개의 자료가 존재 가능할 때(1:N, N:N)  
* [참고 사이트](https://allg.tistory.com/29)  

***  

## 정렬의 종류
* 선택정렬
* 삽입정렬 - 안정
* 버블정렬 - 안정
* 퀵정렬
* 합병정렬 - 안정
* 힙정렬
* **정렬의 시간 복잡도**
![image](https://user-images.githubusercontent.com/25604495/66103164-d8dfc980-e5ef-11e9-906d-f07fbd425e74.png) 
***  

## 선택정렬
* 회차마다 가장 작은것을 찾아 앞에서부터 하나씩 정렬
* 앞에서부터 정렬된다
* (n-1) + (n-2) + .... + 2 + 1 => n(n-1)/2 -> O(n * n) 
```java
public int[] solution(int[] arr) {
        int[] result = arr;

        for (int i = 0; i < result.length - 1; i++) {
            int maxPos = i;
            for (int k = i + 1; k < result.length; k++) {
                if (result[maxPos] > result[k]) {//매번 Swap X, 제일 작은것만 Swap
                    maxPos = k;
                }
            }
            result = Utils.swapValue(result, i, maxPos);
        }
        return result;
    }
```

***  

## 삽입정렬 
* 회차마다 각 숫자를 적절한 위치에 삽입
* 두번째 인덱스부터 시작
* **자신보다 작은 값이 나올때까지 비교하므로 이미 정렬되어 있을 경우 효율적**
```java
public int[] solution(int[] arr) {
        for (int i = 1; i < arr.length; i++) {//2번째 원소부터
            int temp = arr[i];
            int k;
            for (k = i - 1; k >= 0; k--) {
                if (temp >= arr[k]) {//현재 선택된 것보다 작거나 같은 것 나오면 끝
                    break;
                }
                arr[k + 1] = arr[k];//크면 한칸씩 뒤로
            }
            arr[k + 1] = temp;//k는 --되었으므로
        }

        return arr;
    }
```  

***  

## 버블정렬
* 회차마다 인접한 값끼리 비교
* 뒤에서 부터 정렬된다
```java
  public void sort(int[] arr) {
        int temp = 0;
        for (int i = n - 1; i > 0; i--) {
            for (int j = 0; j < i; j++) {
                // j번째와 j+1번째의 요소가 크기 순이 아니면 교환
                if (list[j] > list[j + 1]) {
                    temp = list[j];
                    list[j] = list[j + 1];
                    list[j + 1] = temp;
                }
            }
        }
    }
```

***  
      
## 퀵정렬
* 피봇값 선택 후 작은것은 왼쪽으로, 큰 것은 오른쪽을 분할(partition)
  
* 분할한 왼쪽, 오른쪽에서 위의 과정을 반복
  
* 피봇의 선택 방식
    - 첫번째 요소
    - 중간 요소
    - 마지막 요소
    - 랜덤 요소
    - 첫번째 요소나 마지막 요소를 선택하면 최악의 경우 O(n2)이 나올 수 있다
    - **따라서 중간값을 선택하여 해결한다**

* 코드는 'algorithm-basic' 참고  

* **자바의 내장함수인 Arrays.sort()도 퀵정렬이다**  

* 1회전 정렬후에 모습
    * 왼쪽은 작은값들, 오른쪽은 큰값들
    ![1](https://user-images.githubusercontent.com/25604495/76852856-91908100-688f-11ea-8237-83a0223ac071.PNG)  


***  

## 합병정렬
* 배열의 크기가 1이 될 때 까지 계속 반으로 나눈 후, 나눠진 배열을 합병하면서 정렬

* Small -> Big
  
* [배열이 아닌 ArrayList로 합병정렬 구현](https://velog.io/@widian/JAVA%EB%A1%9C-%EB%B3%91%ED%95%A9-%EC%A0%95%EB%A0%ACMerge-Sort%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0.-21jtcjkoe2)  

***  

## 합병정렬 vs 퀵정렬
* 합병정렬
    - 쪼갤 수 있을 만큼 쪼갠 후 합병하며 정렬한다
    - 장점
        + 안정적이다(최악, 최선 모두 n log n)
    - 단점
        - 배열을 정렬할 때 임시 배열이 필요하다  

* 퀵정렬
    - `피봇을 선택하는 과정에서 정렬을 하면서 쪼갠다`
    - 장점
        + 일반적으로 속도가 가장 빠르다
        + 추가 메모리 공간이 필요 없다
    - 단점
        + 정렬된 리스트에 대해서는 퀵 정렬의 불균형 분할에 의해 수행시간이 오래 걸린다
        + 순차 정렬, 역순 정렬시 최악의 시간 복잡도
        + **피봇을 중간 요소로 선택하여 문제를 해결**

* 같은 O(NlogN) 이지만 퀵정렬이 더 빠른이유
    * `코드적으로 생각해보면 Swap의 횟수가 더 적다`

* LinkedList
    - 합병정렬은 LinkedList 정렬이 필요할 때 유용  
    - **퀵정렬을 이용해 LinkedList를 정렬하면 성능이 좋지 않다**
      - `정렬을 할때 Index 기반으로 정렬하기 때문이다.`
    - 퀵정렬은 순차적 접근이 아닌 임의적 접근이기 때문이다
    - 합병정렬의 merge() 메소드에서는 두 영역 배열로 만들어서 순차적으로 비교하며 정렬한다
    - `합병의 대상이 되는 두 영역은 각 영역에서 이미 정렬되어 있기 때문에 가능`

* [퀵정렬, 합병정렬 참고사이트](https://mygumi.tistory.com/309?category=677288)  
* https://ko.coder.work/so/java/577732

***  

## 힙정렬 
* **완전 이진 트리**를 기본으로 하는 힙 자료구조 기반 정렬방식
* 최대 힙 트리나 최소 힙 트리를 이용하여 정렬
> 여기서 완전 이진 탐색 트리가 아니라
>
> 완전 이진 트리라는 것이 중요하다.
> 
> 최대힙 or 최소힙을 이용하기 때문에
>
> 부모는 자식노드보다 크거나/작아야 한다.
>
> 이진 탐색 트리처럼 왼쪽 노드는 작은값, 오른쪽 노드는 큰값이 아니다.


* **퀵정렬 또는 합병정렬의 성능이 좋기 때문에 '힙 정렬'을 많이 사용하지는 않는다**
* 하지만 다음과 같은 경우에 유용하다
    - 가장 크거나 작은 값을 구할 때
    - 최대 k 만큼 떨어진 요소들 정렬
* https://mygumi.tistory.com/310?category=677288  

* 과정
    * 힙에서 최대값(Root)을 하나씩 추출하면서 정렬
    

***  

## 위상 정렬
* 정해진 순서대로 실행하는 과정이 필요할 때.
* 방향성이 있다.
* `큐와 Count 배열을 이용한다.`
    * Count 배열은 어떠한 정점이 자기자신이 다른곳으로부터 연결된 횟수이다.
    * 해당 Count가 0이면 큐에 넣는다.(먼저 뺀다)
    * 해당 Count가 1 이상이면 자신의 순서 앞에 누군가가 먼저 나와야하므로 빼지 않는다.

* 개념
    * https://www.crocus.co.kr/716

* 풀이
    * https://iyoungman.github.io/algorithm/Boj-2252-%EC%A4%84%EC%84%B8%EC%9A%B0%EA%B8%B0/


***

## 안정 정렬, 불안정 정렬
* 안정 정렬
    + 동일한 값에 대해 기존의 순서가 유지
    + **삽입 정렬, 버블 정렬, 병합 정렬**

* 불안정 정렬
    + 동일한 값에 대해 기존의 순서가 뒤바뀔 수 있다
    + **선택 정렬, 퀵 정렬, 힙 정렬**
    + [선택 정렬이 불안정 정렬인 이유](https://stackoverflow.com/questions/4601057/why-is-selection-sort-not-stable)  

***  

## 빠른 선택 알고리즘
* `K번째로 큰값 혹은 K번째로 작은값을 구할 때 사용`
* 퀵 정렬을 응용
* `O(N)의 시간복잡도인 이유`
  * n + n /2 + n / 4...
* `퀵 정렬을 모두 한 후에 찾는 것이 아니라 퀵 정렬을 하면서 찾는 방식이다`
* 주의 
  * K번째와 피봇값을 잡을때 K번째는 관계없다.
  * `결국 정렬된 후의 피봇값 인덱스를 반환하기 때문이다.` -> 헷갈리지 말것!
* 참고
  * https://sexycoder.tistory.com/101
  * https://hackability.kr/entry/Quick-Selection%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-On-%EC%84%A0%ED%83%9D-%EB%B0%A9%EB%B2%95

***

## 자료구조 종류
* List
    - ArrayList  
    - LinkedList  
    - Stack, Queue  
        + [Stack과 Queue의 사용](https://mygumi.tistory.com/357)  
    - Vector  
      
* Set
    - 특징
        + `HashSet은 내부적으로 HashMap으로 구현되어있다`
            + http://tcpschool.com/java/java_collectionFramework_set
        + 따라서 인덱스로 객체를 가져오지 않는다
        + `Key Value 형식`
        + 삽입, 검색(Contains) 모두 O(1)
    - HashSet은 중복 허용X, 저장 순서 유지 X
    - LinkedHashSet은 저장순서 유지 HashSet(**Linked는 순서**)
    - TreeSet은 이진탐색트리 형태로 데이터 저장, **검색과 정렬**에 특화
      
* Map
    - HashMap vs HashTable
        + 둘다 Map 인터페이스 상속받아 구현
        + 데이터를 키값으로 관리
        + `HashMap은 동기화 지원X`
        + `HashTable은 동기화 지원`
        + 동기화가 필요할때는 HashTable 사용하지 말고
        + ConcurrentHashMap 사용
        + HashTable은 느리다
        + `HashMap은 보조 해시 함수를 사용하기 때문에 해시 충돌이 덜 발생`
    - LinkedHashMap은 입력한 순서가 유지되는 HashMap(**Linked는 순서**)
    - TreeMap은 Key와 Value로 데이터를 다루면서 이진탐색트리 형태로 데이터 저장, **검색과 정렬**에 특화
    - 객체를 Key로 가질때
        - `해당 Key객체에 equals()와 hashcode() 재정의`
    - HashMap 자세한 내용 
        - https://nesoy.github.io/articles/2018-08/Algorithm-HashTable  
    - Hash에 대해서  
        - [iyoungman-정리](https://github.com/iyoungman/today-i-learned/blob/master/algorithm/Hash%EC%97%90%20%EB%8C%80%ED%95%B4%EC%84%9C.md)  
      
* 트리
    - 용어
        + 리프노드 : 자식 노드가 없는 노드  
    
    - 개념
        + `파일시스템 같은 계층구조에 많이 사용`
        + https://blog.naver.com/PostView.nhn?blogId=justkukaro&logNo=220548164184

    - 전위 순회, 중위 순회, 후위 순회  
        + `전위, 중위, 후위의 기준은 부모 노드 이다`  
        + 전위 순회
            - 부모 노드 - 왼쪽 자식 노드 - 오른쪽 자식 노드
            - https://hongku.tistory.com/160

    - 이진탐색트리
        + 검색에 주로 사용
        + 특정 노드의 왼쪽 서브 트리 값은 특정 노드보다 작다
        + 특정 노드의 오른쪽 서브 트리 값은 특정 노드보다 크다
        + **이진 트리에서 위의 조건을 추가한 것이다**(완전 이진트리 X)
            + 이진 트리
                + 자식 노드가 최대 두개인 트리
            + 완전 이진 트리
                + 이진 트리의 일종
                + 같은 레벨에서 왼쪽 부터 오른쪽으로 노드를 채우는 트리
                + `최하위 레벨 자식 노드가 꼭 두개일 필요는 없다`
        + `시간복잡도`
            + `삽입, 삭제, 탐색 모두 트리의 높이와 관련`
            + https://eremo2002.tistory.com/24  
          
    - AVL 트리(`완전 이진 검색 트리`)
        + 이진 검색 트리에서 불균형 이진 검색 트리 -> 균형 이진 검색 트리로 변경
        + **이진 검색 트리이면서 동시에 균형을 가지고 있는 트리**
        + 균형 이진 검색 트리로 변경을 위해 LL, RR, RL, LR 연산을 한다
        + 높이의 최대 차이가 1이하
        + LL : 전체 우회전, RR : 전체 좌회전, RL : 두번째 우회전 후 전체 좌회전, LR : 두번째 좌회전 후 전체 우회전
        + [참고 사이트1](https://www.zerocho.com/category/Algorithm/post/583cacb648a7340018ac73f1)  
        + [참고 사이트2](http://blog.naver.com/PostView.nhn?blogId=kwm5376&logNo=100201360590&parentCategoryNo=&categoryNo=14&viewDate=&isShowPopularPosts=false&from=postView) 

    - AVL 트리 vs 레드-블랙 트리
        + 둘다 균형 트리이다.
        + 즉, 완전 이진 탐색 트리
        + [참고 사이트1](https://m.blog.naver.com/PostView.nhn?blogId=pqzmggg&logNo=90120883769&proxyReferer=https:%2F%2Fwww.google.com%2F)

    - B-Tree
        + 데이터베이스(RDB 인덱스)와 파일시스템에서 많이 사용
        + 'DB 정리'에서 참고(자세히)
        + [DB 정리](https://github.com/iyoungman/my-dev-log/blob/master/%EA%B0%9C%EB%85%90%EC%A0%95%EB%A6%AC/DB%20%EC%A0%95%EB%A6%AC.md)  
        + https://potatoggg.tistory.com/174  
        * https://hyungjoon6876.github.io/jlog/2018/07/20/btree.html
     
* 그래프
    + [참고 사이트](https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html)  
  
* 트리와 그래프의 차이
    - 트리는 계층 구조, 그래프는 네트워크 구조  
    -  [참고 사이트](https://gmlwjd9405.github.io/2018/08/13/data-structure-graph.html)  

* 힙
    - **완전 이진 트리**의 일종으로 **우선순위 큐**를 위해 만들어진 자료구조
    - **최댓값, 최솟값 구하는데 유용**
    - `최대 힙, 최소 힙`
      
* 우선순위 큐
    - **힙으로 구현, 따라서 정렬은 힙정렬으로 된다**
    - **들어온 순서대로 나가는 큐 대신, 어떠한 우선순위를 두고 먼저 나가는 큐 방식이 필요할 때 사용**  
    - 선형구조인 큐와 다르게 트리구조라고 생각하면 된다(완전 이진 트리)  
    ![image](https://user-images.githubusercontent.com/25604495/68267320-61d99d00-0095-11ea-84d3-1a3c71fab3c3.png)  
    - Java 에서 사용
        - Priority를 사용하고 커스텀한 우선순위를 사용하려면 Comparator를 사용하면 된다
    - 따라서 삽입, 삭제에 O(LogN)의 시간복잡도를 가진다

    - 삽입, 삭제
        + 우선순위 큐를 구현한 힙의 삽입, 삭제 연산은 리스트보다 빠르다  
        + 최대 힙에서 자료를 하나 삭제하면 우선순위가 높은 순으로 반환  
        + 최소 힙에서 자료를 하나 삭제하면 우선순위가 낮은 순으로 반환  

    - [velog-우선순위 큐](https://velog.io/@pa324/%EC%9A%B0%EC%84%A0%EC%88%9C%EC%9C%84-%ED%81%90-1xk1cw46t2)  
        + 삽입 과정(상향식)
        + 삭제 과정(하향식)
            + 루트노드 삭제
            + `로트노드 자리로 마지막 노드가 일단 간다`
            + 하향식으로 교환

* `우선순위큐 구현`
> 우선순위큐는 힙으로 구현
>
> 힙은 완전 이진 트리로 구현
>
> 완전 이진 트리는 배열로 구현(단순 트리가 아닌 완전 이진 트리이므로 배열로 쉽게 구현할 수 있다.)

* [참고 사이트](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-algorithm.html)  
***  

## ArrayList vs LinkedList
* 검색
    - ArrayList가 유리
    - ArrayList는 인덱스 기반이기 때문이다
    - 시간복잡도
        - ArrayList : O(1)
        - LinkedList : O(n)

* 삽입, 삭제
    - LinkedList가 유리
    - LinkiedList는 자신의 뒤에 노드의 정보를 알고있다
    - ArrayList : O(n)
        - 삽입 삭제 이후 다른 데이터를 복제하기 때문에
    - LinkedList : O(1)

* 참고로 Vector는 List와 동작은 같지만 동기화 기능이 구현되어 있다
* 하지만, SynchronizedList와 같은 동기화 지원 List의 성능이 더 좋기 때문에 사용하지 않는다
  
***  

## 완전탐색
* 기본적으로 모든 부분을 탐색한다는 것은 동일하지만
* 유형이 여러가지가 있다
* 유형
    - Brute Force : for문과 if문을 이용
    - 순열 or 조합 : for문의 구현이 복잡하다면 고려해볼것
    - 비트마스크
    - 백트래킹(DFS 응용)  
    - BFS : BFS도 시작점과 연결된 모든 정점을 방문하는 완전 탐색이다, 최소 경로 찾기
* https://brenden.tistory.com/10  
* [코딩테스트에서 완전 탐색의 중요성](https://goodgid.github.io/Prepared-for-Coding-Test/)  

***    

## 순열, 조합
* 순열
    - permutation
    - 순서와 상관O
    - n개중 n개를 뽑으면 n!
    - n개중 r개를 뽑으면 n! / (n-r)!  
    - https://bcp0109.tistory.com/14
      
* 조합
    - combination
    - 순서와 상관X
    - n개중 n개를 뽑으면 1
    - n개중 r개를 뽑으면 n! / r!(n-r)!
    - https://limkydev.tistory.com/156  
  
* 중복
    - 중복순열, 중복조합이 있는데 각 개념에 중복허용이 들어간다
    - 예를 들어, 주어진 값이 1,2,3 이면 -> 1,1,1 가능  
    - 중복순열의 경우 **n의 r제곱**
    - 중복조합의 경우 **n+r-1 C r**

* **경우에 따라 다르므로 문제를 확인 후 순열, 조합을 구분하여 사용하면 된다**
* 순열, 조합 코드는 'algorithm-basic'에 위치
* https://limkydev.tistory.com/178  

***  

## 백트래킹
* **백트래킹은 dfs처럼 깊이 탐색이지만 문제에 맞게 가능성 있는 것들만 탐색하는 것이다**
    - 예를 들면 정답의 가능성이 없는 것은 돌아가는 행위..
    - 돌아가는 행위를 위해 방문했던 것의 체크를 해제하거나 등의 작업을 한다  
    
* **백트래킹과 조합, dfs은 유사하다고 생각하면 된다**
* 정확히 하면 백트래킹 = (조합 // dfs) + 가능성 탐색

* [참고 사이트](https://idea-sketch.tistory.com/29)  

* 문제 예시
    - [백준 - 로또](https://www.acmicpc.net/problem/6603)  
    - [백준 - N-Queen](https://www.acmicpc.net/problem/9663)  
    - [백준 - 스도쿠](https://www.acmicpc.net/problem/2580)
    <br>
    - '로또' 문제도 결국 depth가 6으로 되면 되돌아간다
    - dfs와 달리 끝까지 탐색하지 않는다
    - 따라서 백트래킹이다
        -  사실상 dfs와 백트래킹은 유사하다  
    <br>
    - 스도쿠
        - 처음 풀이
            - 처음에는 각 칸을 돌면서 검사하고, 가능한 후보를 추리는 방식을 생각했다
            - 하지만 후보를 ArrayList에 넣고 제거하는 방식이기 때문에 복잡하다
        - 나중 풀이
            - 백트래킹 방식으로 생각했다
            - **여러 후보가 있을 때 백트래킹을 고려하자. 각 후보로 갔다가 조건에 안맞으면 되돌아 오는 방식이기 때문이다**
***

## DFS와 BFS
* **방문 여부 체크가 필요하다**
* [iyoungman-dfs](https://github.com/iyoungman/my-dev-log/blob/master/algorithm/search/DFS(%EA%B9%8A%EC%9D%B4%20%EC%9A%B0%EC%84%A0%20%ED%83%90%EC%83%89).md)  
* [iyoungman-bfs](https://github.com/iyoungman/my-dev-log/blob/master/algorithm/search/BFS(%EB%84%88%EB%B9%84%20%EC%9A%B0%EC%84%A0%20%ED%83%90%EC%83%89).md)  
* [DFS와 BFS는 각각 언제 유용할까](https://stackoverflow.com/questions/3332947/when-is-it-practical-to-use-depth-first-search-dfs-vs-breadth-first-search-bf)  
    + ~~애매하다~~
    + `BFS는 최적의 경로를 찾을 떄`
        + DFS를 이용하면 모든 경로를 방문해야 최적을 알 수 있다.
    + DFS는 모든 경로를 방문해야할 때

* BFS보다 DFS가 유용한 문제
    - [백준 1987번 알파벳 :: 마이구미](https://mygumi.tistory.com/186)  
    ![image](https://user-images.githubusercontent.com/25604495/68119767-e964c600-ff46-11e9-8465-e1b7567a473e.png)  
    ![image](https://user-images.githubusercontent.com/25604495/68119724-c9350700-ff46-11e9-82ad-520e15b9e59f.png)  
    - BFS 방식으로 풀면 분기점 체크에서(alphabetCheck) 위와 같은 두가지 방식이 아닌
    - 한가지 방식으로 구현된다(오른쪽 A를 이미 방문)
    - 따라서 깊이 우선인 DFS로 푸는게 유용하다

- `BFS와 DFS를 그림으로 그려 자료구조와 함께 설명하기`
    - BFS
        - 큐를 이용해서 현재 위치에서 갈 수 있는 곳을 넣는다
        - 큐에 넣을 때 방문했다고 체크해야한다
    - DFS
        - 스택을 이용해서 최대한 많이 가고 더 이상 갈 수 없을 때 이전 정점으로 돌아간다
        - 스택에 넣는 이유는 더이상 갈곳이 없어 돌아왔을 때를 대비한 것
        - `스택의 pop은 현재 위치에서 더 이상 갈 수 없을때 수행한다.`
            - https://itdexter.tistory.com/86

- 방문 체크
    - DFS
        - 방문 했을 때 방문 체크
    - BFS
        - 큐에 넣을 때 방문 체크
        - DFS와 달리 해당 정점을 큐에 꺼낼때 방문 체크를 하지 않는 이유는
        - 중복해서 큐에 들어갈 수 있기 때문이다
        - A - B - C가 삼각형으로 이어져있는 형태 생각


***  

## 스택, 큐 사용
* 스택
    * 안드로이드의 액티비티.
* 큐
    * 매표소 줄 서기.

***

## DFS, BFS 최적의 해
* 최적의 해란 최적의 경로 라고 이해하면 된다.
* BFS를 이용하면 현재 시점에서 갈 수 있는 
* 너비 우선으로 탐색하기 때문에
* 최적의 경로를 찾을 수 있다.
* 참고
    * https://gompangs.tistory.com/entry/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EC%A2%85%EB%A5%98-%EB%B0%8F-%EC%8B%9C%EA%B0%84%EB%B3%B5%EC%9E%A1%EB%8F%84-%EC%A0%95%EB%A6%AC  

***

## 다익스트라 알고리즘
* `가중치가 있는 최단경로를 찾는 기법`
* 개념 설명
    * https://goodgid.github.io/Dijkstra-Algorithm/

* 예제
    * https://www.acmicpc.net/problem/1261

* 예제 풀이
    * https://kim6394.tistory.com/178


***

## 비트마스크
* [참고 사이트1](https://takhyeongmin.github.io/2019/02/01/bitmask/)  
* [참고 사이트2](https://kks227.blog.me/220787042377)  

*** 

## 이분 탐색
* **일반적으로 2가지 유형이 있다**
    - 정렬되어있는 데이터에서 특정한 값을 찾는 유형
        + Mid값의 일치 여부를 찾는다
    - 원하는 범위에 만족하는 최소, 최대를 구할 때(**parametric search**)
        + Mid값의 일치 여부를 찾지 않는다
        + [백준-기타레슨](https://www.acmicpc.net/problem/2343)  
        + [백준-나무자르기](https://www.acmicpc.net/problem/2805)  
        + [참고 사이트](https://sarah950716.tistory.com/16)  

* O(logN) 이라는 빠른 시간복잡도로 값을 찾을 수 있다
    - 물론 순차적으로 탐색해도 찾을 수는 있다
* [참고 사이트1](https://cjh5414.github.io/binary-search/)  
* [참고 사이트2](https://kks227.blog.me/220777333252?Redirect=Log&from=postView)  

* String배열의 이분탐색
    - String의 같음여부는 equals()메소드
    - String의 대소비교는 **compareTo()** 메소드를 사용한다
    - A.compareTo(B) 에서 A가 더 작다면 음수, A가 더 크다면 양수를 반환
    - 다만, compareTo메소드를 파고들면 char[]로 문자열길이 M만큼 비교하기 때문에
    - String배열의 이분탐색 시간복잡도는 **O(MlogN)** 이다

* 범위 주의할 것  
[iyoungman-정리](https://github.com/iyoungman/my-dev-log/blob/master/algorithm/%EC%9D%B4%EB%B6%84%20%ED%83%90%EC%83%89%20%EB%B2%94%EC%9C%84.md)  

* 구하고자 하는 답이 이분 탐색의 탐색 대상


***  

## 트라이
* String 배열에서 가장 긴 문자열의 길이를 M이라고 할 때
    - String 이분탐색의 시간복잡도는 O(MlogN)이다
    - 트라이를 사용하면 O(M)으로 줄일 수 있다

* 문자열 {"AE" , "ATV", "ATES", "ATEV", "DE" ,"DC"} 가 존재할 때
![image](https://user-images.githubusercontent.com/25604495/68447735-55377f00-0224-11ea-9b70-e666c64b5993.png)  
* [참고 사이트](https://jason9319.tistory.com/129)  
  
***  

## 유니온 파인드
* `언제 사용하면 좋을까?`
    * 두 수가 같은 집합에 있는지 확인(연결 되어 있는지 확인)
    * 두 수가 속합 집합간의 합병
* 부모는 최고 루트에 있는 부모만 나타낸다  
![image](https://user-images.githubusercontent.com/25604495/68736519-366e2980-0624-11ea-9ab4-3739639c9b5a.png) 
![image](https://user-images.githubusercontent.com/25604495/68736548-4423af00-0624-11ea-92d2-1f36e8f3318d.png)
    - Find 과정에서 위 -> 아래 그림 형태로 변경된다

* **주의할 것**
* 입력 과정에서 Union 과정을 거쳐야한다(find만 하면 안된다)
    * https://www.acmicpc.net/problem/1976
    ```java
    //반례
    4
    3
    0 0 0 1
    0 0 1 0
    0 1 0 1
    1 0 1 0
    1 2 3
    ```
    * 위의 문제에서 처음에 Union을 하지않고 Parent[max] = min 만했다
    * 하지만 Union은 부모를 지속적으로 통합시켜주는 과정이다


```java
//1번 잘못된 것
if (parent[i] == 1) {
    result++;
}

//2번 정답
if (find(i) == 1) {
    result++;
}   
```
* [백준-바이러스](https://www.acmicpc.net/problem/2606)
* 위 문제에서 처음 풀때 parent 값을 찾았다
* 하지만 입력이 2-3 , 1-2 순서로 들어올 때 배열값이 3의 배열값을 1로 할 수 없다
* 따라서 find를 통해 루트가 1인 것을 찾아야한다

* https://brenden.tistory.com/33

***  

## 세그먼트 트리 
* https://www.acmicpc.net/blog/view/9  

***  

## 코딩테스트
* 범위를 볼 때 가장 큰 값을 볼 것
* 가장 작은 값도 고려
    - 배열의 사이즈가 1..

***

## 최장 수열
* [iyoungman-최장 수열 정리](https://github.com/iyoungman/my-dev-log/blob/master/algorithm/%EC%B5%9C%EC%9E%A5%20%EC%88%98%EC%97%B4%20%EC%A0%95%EB%A6%AC.md)  

***

## 트리 자료구조 입력 방법
* 인접행렬 or 인접리스트
    * 트리역시 정점이 V, 간선이 V-1인 그래프
    * 따라서 그래프와 같은 방식으로 저장 가능

* 부모만 저장
    * 트리의 모든 노드는 부모를 0(Root), 1개 가진다
    * 따라서 부모의 정보만 저장
    * `부모를 찾을 때는 좋으나, 자식 정보를 찾기에는 시간이 걸린다`

* 일차원 배열로 저장
    * 이진트리의 경우
    * 부모 노드 인덱스 X
        * 왼쪽 자식노드 2 * X + 1
        * 오른쪽 자식노드 2 * X + 2

* 이차원 배열로 저장
    * 이진트리의 경우
    * arr[i][2]
        * arr[i][0]에는 왼쪽 자식노드
        * arr[i][1]에는 오른쪽 자식노드

* 참고
    * https://ldgeao99.tistory.com/401

***  

## DP
* TopDown or BottomUp 방식
* 큰 문제를 작은 문제로 나눠서 생각하자
* 가끔 '숫자'로 나눈 나머지를 구하라는 문제가 주어진다
    * 이는 연산의 `오버 플로우` 때문이다
    * 주로 더하기에서 범위를 넘었을 때 발생
    * 부분적으로 나눈 나머지는 전체 연산후 나머지와 같다는 것을 생각하자
    * `오버 플로우에 대한 문제이므로 전체 연산후 나머지를 구하면 안된다`
    * 아래와 같이 매 dp 연산마다 나눠주면 된다
    ```java
    d[i][j] = ( d[i - 1][j] + d[i][j - 1] ) % 1000000007;//왼쪽 + 위
    ```

***

## 해시 알고리즘
* `~에서 ~을 빠르게 찾을 때`
* 참고
    * http://theyearlyprophet.com/interviews-101-preparation.html

***  

## 자바 자료구조 정리
* 그림
![image](https://user-images.githubusercontent.com/25604495/77403059-fbaaa800-6df2-11ea-94ed-707b94efb11d.png)  

![image](https://user-images.githubusercontent.com/25604495/85667692-d468bd00-b6f8-11ea-86f8-40aa7b4bcda3.png)


* JAVA Collection Framework
    * JAVA에서 기본적인 자료 구조를 제공하기 위한 환경
    * `순서를 나타내는 자료구조인 Collection과는 다른의미`

* 참고
    * https://withwani.tistory.com/150

***  

## HashMap vs HashSet

![image](https://user-images.githubusercontent.com/25604495/85669638-02e79780-b6fb-11ea-941d-83cb9c2c7535.png)  

* 구현
    * Map vs Set
    * `Map은 Key, Value로 데이터 핸들`
    * Set은 Collection 자식

* 중복성
    * Set은 Value만 저장. Value 중복X
    * Map은 Value는 중복 가능, Key는 중복 X

* Dummy Value
    * Set은 내부적으로 Map 사용. Map의 Key를 이용한다.<br>
    Map의 Value에는 Dummy Value가 들어간다.
    
    ```java
    //Dummy value to associate with an Object in the backing Map
    private static final Object PRESENT = new Object();

    public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }
    ```

* 속도
    * 차이가 없다.
    * 내부적으로 HashSet은 HashMap으로 구현되어있기 때문이다.
    * [참고](https://stackoverflow.com/questions/16278995/why-is-hashmap-faster-than-hashset#:~:text=HashMap%20is%20faster%20than%20HashSet%20because%20the%20values%20are%20associated,the%20two%20objects%20are%20different.)  

* 참고
    * https://www.geeksforgeeks.org/difference-between-hashmap-and-hashset/


***

## 자주 나오는 문제 유형
* 완전 탐색
* Stack, Queue
* 정렬
* Map  

<br>

* `하지만 가장 중요한것은 시간을 정하고, 생각한 이후에 문제를 푸는 것`  
* 마지막에 테스트 케이스 한두개에서 문제가 있다면 고려할 것
    * 배열이라면 범위 -> 배열의 가장 큰 값, 가장 작은 값
    * 숫자라면 '0'을 고려했는지
