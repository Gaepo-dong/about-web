# Index

### Index 란

Index란 책의 목차와 같은 색인이라 할 수 있다. 추가적인 쓰기 작업과 저장공간을 활용하여 테이블의 검색속도를 높일 수 있게 사용된다.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>simple index example</p></figcaption></figure>

### Index의 장점

그림에서 보이듯이 index는 정렬된 형태를 유지하고있기에, 원하는 값을 쉽게 탐색할 수 있어 부하를 줄여준다.

즉 ORDER BY 혹은 MIN/MAX 같은 경우 이미 정렬되어 효율적으로 탐색할 수 있다.

### Index의 단점

인덱스가 항상 정렬된 상태로 유지되어야 하기에, 이에 동반되는 단점들은 다음과 같다.

1. 인덱스를 관리하기 위한 작업 및 공간 필요 (대략 10%)
2. 남용되는 경우 검색 성능의 저하가 발생

즉 항상 정렬되어 있기에, INSERT, DELETE, UPDATE와 같은 작업인 경우 추가 작업이 필요하게 된다.

즉 규모가 크고, INSERT, UPDATE, DELETE 등이 자주 발생하지 않으며 WHERE, ORDER BY, JOIN등 SELECT가 자주 발생하는 테이블에 유용하게 사용된다.

### Index 자료구조

#### Hash Table

일반적으로 \<key, value> 쌍을 이루어 O(1)의 매우 빠른 구조를 가지고 있지만, equal 연산에 최적화 되어 있어 해시 테이블 내의 데이터들이 정렬되지 않고, 데이터베이스는 부등호 연산을 자주 사용하기에 많이 사용되지 않는다.

#### B+ Tree

일반적으로 B- Tree에서linked list가 추가되고, leaf node에만 데이터를 저장하는 형태로, 부등호 연산을 이용한 순차 검색이 자주 사용되는 데이터베이스의 특징상 더 효율적으로 검색이 가능하다.

### 동작방법

Index가 생성된 후 SELECT 쿼리문을 실행하게 되면, 옵티마이저가 판단하여 생성된 인덱스를 적용하여 쿼리를 실행하게 된다.

#### 옵티마이저

1. 인덱스가 설정되지 않았을 때 데이터를 검색하게된 경우, Full Table Scan을 하며 자원을 소모하며 Index를 생성하게 된다.
2. 다음 인덱스가 존재한 후 조회를 하게 되면, Index를 통해 Location을 쉽게 찾은 후 TABLE을 조회하여 빠르게 처리할 수 있게된다.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>simple table scanning comparison</p></figcaption></figure>

참고

{% embed url="https://mangkyu.tistory.com/96" %}

{% embed url="https://rebro.kr/167" %}

