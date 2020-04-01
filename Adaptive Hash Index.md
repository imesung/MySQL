## Adaptive Hash Index

- MySQL의 InnoDB에는 Adaptive Hash Index 기능이 있는데, 어떤 상황에서 어떤 효과가 있고 사용 시 주의해야할 점에 대해서 살펴보자



### InnoDB의 B-Tree 인덱스

- MySQL과 같은 RDBMS에서 대표적으로 가장 많이 사용되는 자료구조는 B-Tree이다.
- 데이터 사이즈가 아무리 커져도 특정 데이터 접근에 소요되는 비용이 크게 증가되지 않는다는 장점에 의해 B-Tree를 많이 사용하나, 상황에 따라서 최대의 효율을 발휘하지 못할 경우도 있다.
- 이런 경우에 대한 해결책으로 InnoDB에서는 Adaptive Hash Index 기능을 사용하는 것이다.



**InnoDB에서 B-Tree의 동작 방식**

- InnoDB에서 데이터들은 Primary Key 순으로 정렬되어 관리되고, Secondrary Key는 인덱스 + PK 조합으로 정렬되어 있다.

- 특정 데이터를 찾을 때는 먼저 Secondrary Key에서 PK를 찾고, 해당 PK를 통해 다시 원하는 데이터로 찾아가는 형태이다.

  <img src="https://user-images.githubusercontent.com/40616436/78142399-dd365380-7467-11ea-93f1-c1ec6d18764e.png" alt="image" style="zoom:50%;" />

  - B-Tree에서 특정 데이터 접근에 소요되는 비용은 O(log n)이다.
  - **InnoDB에서 B-Tree의 비용은 PK 접근 시 O(log n)이고, Secondrary Key 접근 시 2*O(log n)이다.**

- 그러나, 데이터 접근에 소요되는 비용이 크게 증가되지 않더라도, 상황에 따라 효율이 좋지 않는 경우가 있다.

  - 바로 자주 사용되는 데이터 탐색에도 매번 트리의 경로를 쫓아가야 한다는 것이다.
  - 또한, Mutex Lock(Lock을 가지고 있을 때만 공유 데이터 접근 가능)이 과도하게 잡히게 되면 적은 데이터 셋에도 불구하고 DB 자원 사용 효율이 떨어지게 된다.



**InnoDB Adaptive Hash Index**

- 위와 같은 단점을 해결하기 위해서 등장한 것이 Adaptive Hash Index이다.
- **Adaptive Hash Index를 간단히 말하면, 자주 사용되는 컬럼을 해시로 정의하여 B-Tree를 타지 않고 바로 데이터에 접근이 가능하다는 것이다.**

<img src="https://user-images.githubusercontent.com/40616436/78143456-479bc380-7469-11ea-97f7-390e3394ffd5.png" alt="image" style="zoom:50%;" />

- 하지만 모든 값이 해시로 생성되는 것이 아니라 자주 사용되는 데이터 값만 해시 값으로 생성하므로, Adaptive Hash Index에 할당되는 메모리는 **전체 Innodb_Buffer_Pool_Szie의 1/64** 만큼으로 초기화 된다.



**정리**

- InnoDB Adaptive Hash Index는 B-Tree의 한계를 보완할 수 있는 좋은 기능이다.
  - 특히 단일 랜덤 키 접근이 빈도있게 발생하는 경우라면 B-Tree를 통하지 않고 해시로 인헤 데이터에 접근/처리가 가능하기 때문에 매우 좋다.
- 그러나, 자주 사용되는 데이터를 옵티마이저가 판단하여 해시 키로 만들기 때문에 제어가 어렵고, 수개월 동안 사용되지 않던 테이블일지라도 기존 해시 자료 구조에 데이터가 남아 있게 되면 테이블 Drop시 영향을 줄 수 있다.