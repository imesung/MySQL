## MyISAM vs Inno DB

트랜잭션 처리가 필요하고 대용량 데이터를 다루기 위해서는 **InnoDB**가 효율적이다.

트랜잭션 처리가 필요없고 운영에  Read Only 기능이 많은 서비스일수록 **MyISAM**이 효율적이다.

**SELECT가 많은 서비스 : MyISAM**

**데이터 변화가 많은 서비스 : InnoDB**



**MyISAM과 InnoDB의 가장 대표적인 차이**

두 스토리지 엔진에서 가장 대표적인 차이는, **Locking**에 대한 것이다.

**MyISAM** : Table Rocking으로 Row Level Locking을 지원하지 못해, **SELECT INSERT UPDATE DELETE 시 해당 테이블 전체에 Locking이 걸리게 된다.** 

**InnoDB** : Row Level Locking을 지원함으로써, 특정 데이터가 변경, 삭제, 업데이트 시 해당 범위의 Row들을 함께 Locking할 수 있다. **트랜잭션 처리가 필요한 대용량 데이터에 유리한 점이 있다. **

