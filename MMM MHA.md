## MMM MHA

**MMM(Multi-Master Replication Manager)**

- Multi-Master의 단점을 보완하기 위해서 Manager 장비를 두어 가용성을 보장한다.

- Master(Active)와 Master(Passive) 양방향으로 복제한다.

  <img src="https://user-images.githubusercontent.com/40616436/79136197-92afc200-7deb-11ea-81e2-777d13dc0174.png" alt="image" style="zoom:50%;" />



**MMM의 작동 순서**

1. Active Master에서 장애 발생 시 MMM Manager는 장애를 감지한다.
2. Active Master의 접속을 차단하고 Passive Master로 서비스의 접속을 넘긴다.

- Fail Back은 수동으로 진행하는 것이 원칙이나, 패치 등으로 인한 정상 종료일 경우에는 Active Master를 재가동 시켜도 무관하다.

- read/write DB와 read DB로 운영 중에 read/write DB에서 장애가 발생 시 Manager DB가 이를 감지하여 VIP read DB로 이동시키는 구조를 가지고 있다.(FailOver)

  <img src="https://user-images.githubusercontent.com/40616436/79136509-18cc0880-7dec-11ea-816b-3a22b7556dfc.png" alt="image" style="zoom:50%;" />



**MMM의 장단점**

*장점*

- MySQL Replication이 지원하지 않는 auto FailOver를 지원한다.
- MMM Manager Server의 관리가 쉽고, MMM Manager는 Virtual IP 관리도 가능하다.
- MMM Manager에서 Active Master와 Passive Master 관리가 가능하다.
- MySQL의 성능에 영향을 주지 않는다.



*단점*

- MMM Manager 이중화가 불가능하다.
- Multi Master로 사용이 가능하나 Multi Master 사용 시 데이터 불일치 가능성이 있다.
- Active Master 장애 시 데이터 유실 가능성이 있다.



출처 : https://www.slideshare.net/ohnew/mmmmultimaster-replication-manager





**MHA**

- Master DB가 장애로 서비스가 불가능한 상태가 될 시, 자동으로 FailOver를 수행하여 slave DB를 master DB로 승격시켜 서비스 다운 타임을 최소화 하는 auto FailOver 솔루션이다.



**MHA 서버 구성**

- MHA Manager, Master, Slave 서버로 총 3개로 구성되어 있으며, 상황에 따라서는 1개의 Master와 N개의 Slave로 최소 2대까지 사용할 수 있다.
- Master와 Slave는 하나의 VIP를 공유하며 DB 접속은 해당 VIP를 이용하며 장애 발생 시 VIP를 이용하여 절제를 진행한다.



**MHA의 장점**

- 최소한의 다운 타임으로 Master의 장애 처리 및 Slave의 새로운 Master로 변경 수행이 가능하다.
- Master의 장애로 각 노드(Master 및 Slave)의 데이터 불일치가 발생하지 않는다.
- 현재 MySQL 서버의 설정을 변경할 필요가 없다.(MySQL 5.0 이상)
- 서버를 많이 늘릴 필요가 없다. (MySQL Cluster 대비)
- 스토리지 엔진에 제약을 받지 않고, MySQL 성능에 전혀 제약사항이 없다.



**MHA의 장애 체크**

- MHA Manager가 3초마다 마스터 DB를 CONNECT / SELECT / INSERT 체크하며 3회 실패 시 장애로 인식하여 FailOver를 수행한다.



**MHA의 FailOver**

- MasterDB에 장애가 발생하면 MHA Manager는 Master 장비의 VIP를 down시킨다.
- 이 후 Replication을 진행 후 down 시킨 VIP를 Slave장비에 올려 FailOver를 진행한다.



**MHA의 Replication**

1. MySQL에서는 Lossless Replication은 지원한다. 이는 Master에서 데이터 변깅이 진행되면 Slave 어딘가에 반드시 변경이력이 남아있다는 것을 보장한다.
2. MHA에서는 장애 발생 시 가장 최근에 변경된 Slave를 Master DB로 승격 시킨 후 Lossless Replication을 통한 relay log로 데이터 복구를 수행한다.
3. 장애 발생부터 Master DB 승격, 데이터 복구까지 걸리는 시간은 10~30초 이다.