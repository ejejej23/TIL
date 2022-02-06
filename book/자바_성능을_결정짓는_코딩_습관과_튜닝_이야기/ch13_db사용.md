# DB 커넥션

### 수행순서
1. 드라이버 로드
2. DB서버의 IP와 PW, 등을 DriverManager클래스의 getConnection()을 사용해 Connection객체로 만든다
3. Connection으로부터 PreparedStatement 객체를 받는다
4. executeQuery를 수행, 결과로 resultset 객체를 받아 데이터를 처리한다.
5. 모든 데이터 처리 후 finally 구문을 사용해 resultset, preparedStatement, Connection객체들을 닫는다. 물론 close시 예외가 발생할 수 있기 때문에 해당 메소드에서 예외를 던지도록 처리가 필효하다.

### 이때 가장 느린부분은 connection을 얻는 부분. db와 was간 통신이 필요하기 때문
-> 이렇게 connection 객체를 생성하는 부분에서 발생하는 대기시간을 줄이고 네트워크의 부담을 줄이기 위해 사용하는것이 *DB connection pool*

## Statement 와 PreparedStatement 의 차이
: 가장큰차이는 캐시 사용여부

-> Statement 와 PreparedStatement 실행 프로세스
1. 쿼리문장 분석
2. 컴파일
3. 실행

: 이때 preparedStatement 는 처음 한번만 이 단계를 거치고 캐시에 담아 재사용한다.

### DB쓸때 닫아야 하는것들
- Connection
- Statement 관련 인터페이스
- ResultSet 관련 인터페이스

-> 닫아야 하는 이유: 자동으로 호출되기 전에 관련된 DB와 JDBC 리소스를 해제하기 위함. GC가 될때까지 기다리면 커넥션풀이 부족해질 수 있다.

