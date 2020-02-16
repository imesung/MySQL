## Inno DB

Inno DB는 높은 안정성과 성능을 유지하는 범용 스토리지 엔진이며, transactional-safe 테이블을 관리한다.

MySQL8.0에서는 InnoDB를 기본 스토리지 엔진으로 사용하고 있다.



**Inno DB 스토리지 엔진 기능**

| Feature                                                      | Support                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **B-tree indexes**                                           | Yes                                                          |
| **Backup/point-in-time recovery** (Implemented in the server, rather than in the storage engine.) | Yes                                                          |
| **Cluster database support**                                 | No                                                           |
| **Clustered indexes**                                        | Yes                                                          |
| **Compressed data**                                          | Yes                                                          |
| **Data caches**                                              | Yes                                                          |
| **Encrypted data**                                           | Yes (Implemented in the server via encryption functions; In MySQL 5.7 and later, data-at-rest tablespace encryption is supported.) |
| **Foreign key support**                                      | Yes                                                          |
| **Full-text search indexes**                                 | Yes (InnoDB support for FULLTEXT indexes is available in MySQL 5.6 and later.) |
| **Geospatial data type support**                             | Yes                                                          |
| **Geospatial indexing support**                              | Yes (InnoDB support for geospatial indexing is available in MySQL 5.7 and later.) |
| **Hash indexes**                                             | No (InnoDB utilizes hash indexes internally for its Adaptive Hash Index feature.) |
| **Index caches**                                             | Yes                                                          |
| **Locking granularity**                                      | Row                                                          |
| **MVCC**                                                     | Yes                                                          |
| **Replication support** (Implemented in the server, rather than in the storage engine.) | Yes                                                          |
| **Storage limits**                                           | 64TB                                                         |
| **T-tree indexes**                                           | No                                                           |
| **Transactions**                                             | Yes                                                          |
| **Update statistics for data dictionary**                    | Yes                                                          |



**Inno DB 스토리지 엔진 특징**

다른 엔진들에 비해 data 로딩 속도가 느리다.

특정 data와 index에 대해서 table space개념을 사용하여 저장하고, 각각 메모리 캐시를 지원한다.

index는 B-Tree, Clustered를 지원한다.

FK를 지원하며, 자동 에러 복구 기능을 지원한다.

Row 단위로 lock을 제공하며, 백업 및 특정 시점으로 복구가 가능하다.

