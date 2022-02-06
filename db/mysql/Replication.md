# DB 리플리케이션
: Replication enables data from one MySQL databse server (known as a source) to be copied to one or more MySQL database servers (know as replicas)
- 하나의 데이터베이스(master/source)에서 다른 하나 또는 그 이상의 데이터베이스(slaves/replicas)로 데이터를 복제하여 저장하는 것
- Replication은 비동기로 동작한다. 따라서 replicas가 master에 지속적으로 연결되어는 동기식으로 동작하지 않는다. 

## 장점
1. Scale-out solutions
- 다수의 replicas를 두고 load를 분산해서 퍼포먼스를 높이는 장점이 있다.
- 대부분의 replication 적용이유이기도 하다.
- 쓰기 및 업데이트는 master 서버에서 이루어진다.
- 조회는 하나 또는 여러 slave 서버에 분산되서 처리된다.
2. Data security
- master 서버와 slave 서버가 분리되어 있으므로 하나의 slave 서버에 문제가 생겨도 다른 slave 서버에 영향을 미치지 않고 데이터를 보존할 수 있다.
- 하지만 Master server에 장애가 생기면 문제가 생긴다.
3. Analytics
- 실시간 데이터 생성 및 업데이터가 master 서버에서 이루어지는 동안 데이터 분석처리는 slave 서버에서 처리하여 master 서버에 성능저하를 전혀 일으키지 않도록 지원한다.
4. Long-distance data distribution
- 리모트에 필요한 데이터를 위한 local 데이터 복제를 master에 접촉하지 않고 slave 서버에서 처리할 수 있다.

### 참고자료
https://yjksw.github.io/db-replication/