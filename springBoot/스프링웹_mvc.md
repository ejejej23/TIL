# 스프링 MVC
M: 모델
V: 뷰
C: 컨트롤러

## @Controller 어노테이션
: 붙이면 스프링 구조에서 controller 역할을 수행

### 내부에 @RequestMapping 어노테이션 사용 -> 웹 브라우저에서 요청을 받아 연결

-----

### 서블릿 실행
: 톰캣 설치 필요
- 톰캣 설치 후 war:exploded 추가해주기
- war 을 풀어서 실행하겠다는 뜻
- application context 지정해주기

- 컨트롤러를 연결해 사용하려면 web.xml에 서블릿 등을 매핑해줘야 함

### 서블릿 리스너
: 웹 어플리케이션에서 발생하는 주요 이벤트를 감지하고 각 이벤트에 특별한 작업이 필요한 경우에 사용할 수 있다.

1. 서블릿 컨텍스트 수준의 이벤트
   1. 컨텍스트 라이프사이클 이벤트
   2. 컨텍스트 애트리뷰터 변경 이벤트
2. 세션 수준의 이벤트

### 서블릿 필터
: 들어온 요청을 서블릿으로 또는 서블릿의 응답을 클라이언트로 보내기 전에 특별한 처리가 필요한 경우에 사용할 수 있다. 체인구조.

### 서블릿, 필터는 web.xml에 연결해주면 사용가능
```
    <filter>
        <filter-name>myFilter</filter-name>
        <filter-class>me.whiteship.MyFilter</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>myFilter</filter-name>
        <servlet-name>hello</servlet-name>
    </filter-mapping>
```

```
   <servlet>
        <servlet-name>app</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextClass</param-name>
            <param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
        </init-param>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>me.whiteship.WebConfig</param-value>
        </init-param>
    </servlet>
```
## 서블릿 등록시 스프링/스프링부트의 차이점
### 스프링
: 서블릿 애플리케이션 안에 스프링을 연동
서블릿 컨텍스트로더 리스너 또는 디스패처 서블릿을 연결
- 서블릿 컨텍스트 안에 스프링이 들어가는 구조

### 스프링부트
: 스프링부트 애플리케이션이 먼저 동작하고 그 안에 톰캣(내장서버)이 포함됨. 톰캣 안에 서블릿을 코드로 등록
- 스프링 어플리케이션 안에 톰캣이 들어가있는 구조


-----
### @EnableWebMvc
: 애노테이션 기반 스프링 MVC를 사용할 때 편리한 웹 MVC 기본 설정
```
@Configuration
@EnableWebMvc
public class WebConfig {
}
```

- 장점중의 하나로 DelegatingWebMvcConfiguration를 import해 읽어온다는 점이 있음
- delegation 구조 : 위임해서 읽어오는 구조
- 즉 처음부터 핸들러 어댑터를 등록해야하는 게 아니라 클래스가 연결해주는 핸들러 어댑터에 조금만 수정해서 사용가능
- 핸들러 어댑터에 인터셉터를 추가하는 방식 등이 편해짐

- 가져온 설정을 확장해 사용할 수 있는 기능을 제공함 : 인터페이스 WebMvcConfigurer
-> 뷰 리졸버를 직접 등록하지 않아도 연결되어있는 것을 사용해 수정/사용가능

### WebMvcConfigurer 인터페이스
: @EnableWebMvc가 제공하는 빈을 커스터마이징할 수 있는 기능을 제공하는 인터페이스

```
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {
@Override
   public void configureViewResolvers(ViewResolverRegistry registry) {
      registry.jsp("/WEB-INF/", ".jsp");
   }
}
```

