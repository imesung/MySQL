## MyISAM

MyISAM은 **ISAM(Indexed Sequential Access Method)의 단점을 보완하기 위해 나온 버전**으로, 이 엔진은 비-트랜젝션-세이프(non-transactional-safe) 테이블을 관리한다.

MySQL 5.5 이전까지 기본 스토리지 엔진이었다.



**MyISAM 스토리지 엔진 기능**

| Feature                                                      | Support                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **B-tree indexes**                                           | Yes                                                          |
| **Backup/point-in-time recovery** (Implemented in the server, rather than in the storage engine.) | Yes                                                          |
| **Cluster database support**                                 | No                                                           |
| **Clustered indexes**                                        | No                                                           |
| **Compressed data**                                          | Yes (Compressed MyISAM tables are supported only when using the compressed row format. Tables using the compressed row format with MyISAM are read only.) |
| **Data caches**                                              | No                                                           |
| **Encrypted data**                                           | Yes (Implemented in the server via encryption functions.)    |
| **Foreign key support**                                      | No                                                           |
| **Full-text search indexes**                                 | Yes                                                          |
| **Geospatial data type support**                             | Yes                                                          |
| **Geospatial indexing support**                              | Yes                                                          |
| **Hash indexes**                                             | No                                                           |
| **Index caches**                                             | Yes                                                          |
| **Locking granularity**                                      | Table                                                        |
| **MVCC**                                                     | No                                                           |
| **Replication support** (Implemented in the server, rather than in the storage engine.) | Yes                                                          |
| **Storage limits**                                           | 256TB                                                        |
| **T-tree indexes**                                           | No                                                           |
| **Transactions**                                             | No                                                           |
| **Update statistics for data dictionary**                    | Yes                                                          |



**MyISAM 테이블 사용**

~~~sql
CREATE TABLE t (i INT) ENGINE = MYISAM;
~~~



**MyISAM 테이블 특징**

MyISAM은 별다른 기능이 없어 **데이터 모델 디자인이 단순하다**는 장점을 가지고 있다.

특히 select 작업 속도가 매우 빨라 **데이터를 읽는 작업에 매우 적합**하다.

Full-text 인덱싱이 가능해 검색하고 하는 내용에 대해 **복합검색이 가능**하다.

index는 B-Tree, R-Tree, Full-text Index를 징

table 단위로 lock을 지원하고, 백업 및 특정 시점으로 복구가 가능하다.

index에 대해서만 MySQL 서버가 관리하고, data에 대해서는 직접 관리하지 않는다.

