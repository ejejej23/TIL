# 디자인 패턴

- lntercepting Filter 패턴 : 요청 타입에 따라 다른처리를 하기 위한패턴이다
- Front Controller 패턴: 요청 전후에 처리하기 위한 컨트롤러를 지정하는 패턴이다. 
- View Helper 패턴 : 프레젠데이션 로직과 상관 없는 비즈니스 로직을 헬퍼로 지정하는패턴이다 
- Composite View 패턴: 최소 단위의 하위 컴포넌트를 분리하여 화면을 구성하는 패턴이다. 
- Service to Worker 패턴 : Front Controller와 View Helper 사이에 디스패처를 두어 조합하는 패턴이다. 
- Dispatcher View 패턴 : Front Controller와 View Helper로 디스패처 컴포넌트를 형성한다. 뷰 처리가 종료될 때까지 다른 활동을 지연한다는 점이 Service to Worker 패턴과 다르다. 
- Business Delegate 패턴 : 비즈니스 서비스 접근을 캡슐화하는 패턴이다 
- Service Locator 패턴: 서비스와 컴포넌트 검색을 쉽게 하는 패턴이다 
- Session Facade 패턴: 비즈니스 티어 컴포넌트를 캡슐화하고， 원격 클라이언트에서 접근할 수 있는 서비스를 제공하는 패턴이다. 
- Composite Entity 패턴 : 로컬 엔티티 빈과 POJO를 용하여 큰 단위의 엔티티 객체를구현한다. 
- Transfer Object 패턴 : 일명 Value Object 패턴이라고 이 알려져 있다. 데이터를 전송하기 위한 객체에 대한 패턴이다， 
- Transfer Object Assembler 패턴 : 하나의 Transfer Object 로 모든 데이터를 처리할 수 없으므로， 여러 Transfer Object 를 조합하거나 변형한 객체를 생성하여 사용하는패턴이다. 
- Value List Hancller 패턴 : 데이터 조회를 처리하고， 결과를 임시 저장하며， 결과 집합을 검색히여 필요한 항목을 선택하는 역할을 수행한다 
- Data Access Object 패턴 : 일영 DAO 라고 알려져 있다 에 접근을 전담하는 클래스를 추상화하고 캡슐화한다. 
- Service Activator 패턴 : 비동기적 호출을 처리하기 위한패턴이다

### J2EE패턴중 성능에 직접적인 패턴은 Service Locator 패턴.
### 어플리케이션 개발시 반드시 사용해야 하는 Transfer Object 패턴도 중요

## Serialize는 왜 구현(implements) 했을까?
: 이 인터페이스를 구현하면 객체를 직렬화할 수 있다. 즉, 서버사이의 데이터 전송이 가능해진다

그러므로 원격지 서버에 데이터를 전송하거나 파일로 객체를 저장할 경우에는 이 인터페이스를 구현해야한다.
