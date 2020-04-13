## Replication

**MySQL Replication이란**

- DB 이중화 방식 중 하나로서 MySQL에서는 리플레이케이션(복제) 기능을 제공하며, 해당 기능은 2대 이상의 DBMS를 나눠서 데이터를 저장하는 방식이다.
- 사용하기 위한 최소 구성은 Master와 Slave로 구성되어야 한다. 즉, Master와 Slave 간의 데이터 복제를 가능하게 한 것이다.
- Master DBMS
  - Master DBMS는 웹 서버로부터 데이터 등록/수정/삭제 요청 시 바이너리로그를 생성하여 Salve 서버로 전달하게 된다.
- Slave DBMS
  - Slave DBMS는 Master DBMS로부터 전달받은 바이너리로그를 데이터로 반영하게 된다.


## 
**MySQL Replication의 사용목적**

- MySQL Replication의 사용목적은 **실시간 Data 백업과 여러대 DB 서버의 부하를 분산**시킬 수 있다.

- **데이터 백업**

  - Master 서버가 데이터의 원본 서버이고, Slave 서버가 백업서버일 때,
  - Master 서버에 DBMS의 등록/수정/삭제가 생기는 즉시 Slave 서버에 변경된 데이터를 전달하게 된다.

  <img src="https://user-images.githubusercontent.com/40616436/78155906-04e1e780-7479-11ea-97a9-01dc25bc81df.png" alt="image" style="zoom:30%;" />

  - **이 과정으로 인해 데이터의 백업을 할 수 있으며, Master 서버에 장애가 발생했을 경우 Slave 서버로 변경하여 사용할 수 있다.**

- **DBMS 부하 분산**

  - 사용자의 폭주로 인해 1대의 DB 서버로 감당할 수 없을 때, MySQL Replication을 사용하여 같은 DB 데이터를 여러 대 만들어 부하를 분살할 수 있다.

  <img src="https://user-images.githubusercontent.com/40616436/78156169-5f7b4380-7479-11ea-861c-9ee2f4fb2684.png" alt="image" style="zoom:30%;" />

  - Master 서버에는 등록/수정/삭제를 사용하는 서버로 지정하고, Slave 서버에는 데이터를 읽는 용도로 지정하게 되면, DBMS의 부하를 분산할 수 있다.


## 
**MySQL Replication 주의 사항**

- 호환성을 위해 Replication을 사용하는 MySQL은 동일하게 맞추는 것이 좋다.
- Replication을 사용하기에 MySQL 버전이 다른 경우 Slave 서버가 상위 버전이어야 한다.
- Replication 가동 시, Master 서버 > Slave 서버 순으로 가동시켜야 한다.


