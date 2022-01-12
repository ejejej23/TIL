# JMX(Java Management Extensions)
: 자바 기반의 모든 어플리케이션을 모니터링하기 위한 기술.

## JMX의 4단계 레벨
- 인스트루먼테이션 레벨Instrumentation Level
 : 하나 이상의 Mbeans(Management Beans,관리빈즈)를 제공. 이 Mbeans에서 필요한 리소스들의 정보를 취합해 에이전트로 전달한다
- 에이전트 레벨Agent Level
 : 리소스를 관리하는 역할을 수행. jmx의 데이터를 관리하는 관리자와 연계를 위한 어댑터나 커넥터를 이 레벨에서 제공
- 분산 서비스 레벨Distributed Services Level
 : jmx 관리자를 구현하기 위한 인터페이스와 컴포넌트를 제공
- 추가 관리 프로토콜 API들Additional Management Protocol Apis

## Mbean의 4가지 종류
- 표준Mbean : 변경이 많지 않은 시스템을 관리하기 위한 경우
- 동적Mbean : 애플리케이션이 자주 변경되는 시스템을 관리하기 위한 경우
- 모델Mbean : 어떤 리소스나 동적으로 설치가 가능한 mbean이 필요한 경우
- 오픈Mbean : 실행중에 발견되는 객체의 정보를 확인하기 위해 필요한 경우

### 에이전트가 제공해야 하는 기능
- 현재 서버에 있는 mbean의 다음 기능들을 관리한다
- Mbean의 속성값을 얻고 변경한다
- Mbean의 메소드를 수행
- 모든 Mbean에서 수행된 정보를 받음
- 기존클래스나 새로 다우로드된 클래스의 새로운 Mbean을 초기화하고 등록
- 기존 Mbean들의 구현과 관련된 관리정책을 처리하기 위해 에이전트 서비스를 사용되도록 한다