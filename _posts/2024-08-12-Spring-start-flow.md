---
layout: post
title: 
subtitle: 
author: 차정우
categories: Spring
tags: [Spring, Springboot, 스프링, 스프링부트, 스프링 컨테이너, 톰캣, WAS]
---

이전에 요청이 들어 왔을 때 스프링 흐름과 관련해서 mvc 디자인 패턴이 어떻게 흘러가는지에 대해서 정리한 적이 있습니다. [참고](https://techblog.nbyte.kr/spring/2024/07/08/Springboot-Flow.html)

이번에는 요청이 들어 오기전, 즉 스프링이 실행 되자마자 일어나는 일들에 대해 알아보겠습니다.

---


![](https://velog.velcdn.com/images/chjw147/post/f25928ee-a231-4a3d-a914-974098dd7c04/image.png)




### 1. 웹 애플리케이션 실행 흐름 (Spring 기반)

1. **웹 애플리케이션 서버(Tomcat)의 시작**  
   웹 애플리케이션이 실행되면, 가장 먼저 Tomcat과 같은 웹 애플리케이션 서버(WAS)가 구동됩니다. 이때 서버는 웹 애플리케이션의 배포 디스크립터인 `web.xml` 파일을 읽습니다. `web.xml` 파일은 서블릿, 필터, 리스너 등의 설정 정보를 담고 있습니다.

2. **ContextLoaderListener의 초기화**  
   `web.xml`에서 정의된 `ContextLoaderListener`가 가장 먼저 생성됩니다. 이 클래스는 `ServletContextListener` 인터페이스를 구현하고 있으며, 서블릿 컨텍스트가 초기화될 때 함께 초기화됩니다. 이 리스너는 Spring의 `ApplicationContext`를 생성하여, 전체 애플리케이션의 루트 컨텍스트 역할을 하는 `ApplicationContext`를 설정합니다.

3. **루트 컨텍스트 설정 파일 로딩 (root-context.xml)**  
   `ContextLoaderListener`는 `root-context.xml`을 로딩하여 애플리케이션의 루트 컨텍스트(`Root ApplicationContext`)를 설정합니다. 이 설정 파일은 주로 애플리케이션 전반에 걸쳐 사용되는 Bean들을 정의하며, 비즈니스 로직과 관련된 서비스, DAO, VO 객체 등이 포함됩니다. (정의만 될 뿐 스프링컨테이너에 등록되는 것은 바로 다음의 스프링컨테이너가 실행(생성)되고 나서 입니다.)

4. **스프링 컨테이너의 초기화**  
   `root-context.xml`에 정의된 대로 스프링 컨테이너가 초기화(생성)됩니다. 이 과정에서 애플리케이션에 필요한 각종 Bean들이 생성되고, 의존성 주입이 이루어집니다. 예를 들어, 비즈니스 로직을 처리하는 서비스 클래스, 데이터베이스 접근을 담당하는 DAO, 데이터 객체인 VO 등이 이 단계에서 인스턴스화됩니다.

5. **클라이언트 요청 처리 시작**  
   애플리케이션이 구동된 후, 클라이언트로부터 HTTP 요청이 오면, 요청이 웹 애플리케이션 서버로 전달됩니다.

6. **DispatcherServlet의 초기화**  
   클라이언트의 요청이 들어오면, 스프링의 `DispatcherServlet`이 초기화됩니다. `DispatcherServlet`은 스프링 MVC의 프론트 컨트롤러 역할을 하며, 모든 요청을 중앙에서 받아 처리하는 역할을 합니다. `DispatcherServlet`은 요청을 분석하고, 적절한 핸들러(Controller)에게 전달하여 요청을 처리합니다. 이 과정에서 요청에 따라 알맞은 페이지 컨트롤러가 선택되고, 실제 비즈니스 로직이 실행됩니다. 이를 통해 응답을 생성하고 반환하는 역할을 합니다.

7. **서블릿 컨텍스트 설정 파일 로딩 (servlet-context.xml)**  
   `DispatcherServlet`이 초기화되는 동안, `servlet-context.xml`이 로딩됩니다(`Web ApplicationContext" 또는 **"Servlet ApplicationContext"**`). 이 파일은 주로 웹과 관련된 설정을 담고 있으며, 컨트롤러, 뷰 리졸버, 핸들러 매핑 등 웹 요청을 처리하는 데 필요한 Bean들이 정의되어 있습니다.(정의만 될 뿐 스프링컨테이너에 등록되는 것은 바로 다음의 스프링컨테이너가 실행(생성)되고 나서 입니다.)

8. **두 번째 스프링 컨테이너 초기화**  
   `servlet-context.xml`이 로딩되면서, 또 다른 스프링 컨테이너가 초기화(생성)됩니다. 이 컨텍스트는 웹 요청과 응답 처리를 위한 Bean들을 관리하며, 앞서 초기화된 루트 컨텍스트와 협력하여 애플리케이션 로직을 처리합니다. 이 과정에서 특정 페이지 컨트롤러가 호출되며, 루트 컨텍스트에서 생성된 서비스, DAO 객체들과 협력하여 요청을 처리하고, 그 결과를 클라이언트에게 응답합니다.



---


### 2. 웹 애플리케이션 실행 흐름 (Springboot기반)

1. **SpringApplication.run() 호출**  
   스프링 부트 애플리케이션은 main 메서드에서 `SpringApplication.run()` 메서드를 호출하여 시작됩니다. 이 메서드는 스프링 부트 애플리케이션의 실행 진입점이며, 여러 초기화 작업을 수행합니다.

2. **애플리케이션 환경 설정 로딩**  
   `SpringApplication`은 애플리케이션 환경을 초기화합니다. 이 과정에서 `application.properties` 또는 `application.yml`과 같은 환경 설정 파일이 로딩되며, 필요한 설정 값들이 애플리케이션 컨텍스트에 적용됩니다.

3. **애플리케이션 컨텍스트 생성 및 구성**  
   `SpringApplication`은 `ApplicationContext`를 생성합니다. 스프링 부트에서는 일반적으로 `AnnotationConfigServletWebServerApplicationContext` 또는 `AnnotationConfigReactiveWebServerApplicationContext`가 사용됩니다.  이 시점에서 스프링 컨테이너가 생성되며, 이후의 모든 빈 등록 및 의존성 주입, 컴포넌트 스캔이 이 컨텍스트를 통해 이루어집니다.

4. **컴포넌트 스캔 및 Bean 등록**  
   `@SpringBootApplication` 어노테이션이 붙은 클래스의 패키지를 기준으로 컴포넌트 스캔이 이루어지며, `@Component`, `@Service`, `@Repository`, `@Controller`와 같은 어노테이션이 붙은 클래스들이 자동으로 스프링 컨테이너에 등록됩니다. `@Configuration` 클래스 내에서 정의된 `@Bean` 메서드도 호출되어 필요한 Bean들이 등록됩니다.

5. **자동 설정 및 빈 등록**  
   `@EnableAutoConfiguration` 어노테이션에 의해 스프링 부트의 자동 설정 기능이 활성화됩니다. 이 과정에서 스프링 부트는 애플리케이션의 실행 환경을 분석하고, 필요한 빈들을 자동으로 구성합니다. 예를 들어, 웹 애플리케이션의 경우 내장된 `DispatcherServlet`과 같은 서블릿이 자동으로 설정되며, 내장 웹 서버도 이 단계에서 초기화됩니다.

6. **내장 웹 서버 초기화**  
   스프링 부트 애플리케이션은 Tomcat, Jetty, Undertow와 같은 내장 웹 서버를 자동으로 초기화하고 실행합니다. 이 과정에서 서블릿 컨텍스트가 생성되고, `DispatcherServlet`이 등록됩니다. 스프링 부트 애플리케이션이 시작되면 내장 웹 서버가 HTTP 요청을 수신할 준비를 마칩니다.

7. **CommandLineRunner 및 ApplicationRunner 실행**  
   스프링 부트는 애플리케이션 컨텍스트가 초기화된 후 `CommandLineRunner` 또는 `ApplicationRunner` 인터페이스를 구현한 빈들을 실행합니다. 이 단계에서 애플리케이션 시작 직후 실행되어야 할 로직을 추가할 수 있습니다.

8. **클라이언트 요청 처리 및 응답 반환**  
   내장 웹 서버가 실행된 후, 클라이언트로부터 HTTP 요청이 들어오면, `DispatcherServlet`이 요청을 처리합니다. `DispatcherServlet`은 요청을 분석하고, 적절한 컨트롤러에게 전달하여 비즈니스 로직을 처리합니다. 컨트롤러에서 처리된 응답이 `DispatcherServlet`을 통해 클라이언트에게 반환되며, 이때 `ViewResolver`가 응답을 처리할 뷰를 결정하고, 최종 결과를 렌더링합니다.
   
   
---

### 3. 비교

| 단계 | 스프링(Spring) 실행 순서                                | 스프링 부트(Spring Boot) 실행 순서                            |
|------|---------------------------------------------------------|---------------------------------------------------------------|
| 1    | 웹 애플리케이션 서버(Tomcat) 시작                        | `SpringApplication.run()` 호출                                |
| 2    | `web.xml` 파일 로드                                      | 애플리케이션 환경 설정 로딩                                   |
| 3    | `ContextLoaderListener` 초기화 및 `ApplicationContext` 생성 | `ApplicationContext` 생성 및 구성                              |
| 4    | `root-context.xml` 로딩 및 스프링 컨테이너 초기화         | 자동 설정 및 Bean 등록 (`@EnableAutoConfiguration`)           |
| 5    | 클라이언트 요청 처리 시작                                | 내장 웹 서버 초기화 및 실행                                    |
| 6    | `DispatcherServlet` 초기화                               | `CommandLineRunner` 및 `ApplicationRunner` 실행                |
| 7    | `servlet-context.xml` 로딩                               | 클라이언트 요청 처리                                           |
| 8    | 두 번째 스프링 컨테이너 초기화 및 페이지 컨트롤러 동작    | 응답 반환 및 처리 완료                                         |




1. 웹 서버 초기화
- **스프링**: 외부 WAS(Tomcat 등)를 사용하는 반면, 
- **스프링 부트**: 내장된 웹 서버를 자동으로 실행합니다.

2. 설정 파일 로딩
- **스프링**: `web.xml`, `root-context.xml`, `servlet-context.xml`과 같은 설정 파일을 명시적으로 로드해야 합니다.
- **스프링 부트**: `application.properties` 또는 `application.yml`을 통해 간단하게 설정을 자동으로 로드합니다.

3. 컨텍스트 초기화
- **스프링**: 여러 컨텍스트(`Root ApplicationContext`, `Web ApplicationContext" 또는 **"Servlet ApplicationContext"**`)를 명시적으로 초기화합니다.
- **스프링 부트**: 이를 자동화하고 간소화하여, 단일 `AnnotationConfigServletWebServerApplicationContext 또는 AnnotationConfigReactiveWebServerApplicationContext`에서 대부분의 설정을 관리합니다.

4. 자동 설정
- **스프링 부트**: 주요 차별점은 `@EnableAutoConfiguration`을 통해 많은 설정을 자동으로 처리한다는 점입니다.

5. 클라이언트 요청 처리
- **둘 다**: `DispatcherServlet`을 사용하여 클라이언트의 요청을 처리하지만,
- **스프링 부트**: 이 설정이 자동화되어 있습니다.

6. 추가 실행 로직
- **스프링 부트**: 애플리케이션 시작 후 추가적인 로직을 실행할 수 있는 `CommandLineRunner` 및 `ApplicationRunner`를 제공합니다.


---

### 4. 스프링 컨테이너 개수(Context)


 스프링 부트는 일반적으로 하나의 스프링 컨테이너를 사용하는 것이 일반적입니다. (원한다면 여러개를 사용할 수도 있습니다.)
 하지만 스프링에서는 두 개의 스프링 컨테이너가 사용되는 것이 일반적입니다. 이를 이해하려면, 스프링 MVC의 구조와 컨텍스트 계층의 개념을 이해해야 합니다.

- **루트 컨텍스트 (Root ApplicationContext)**
 루트 컨텍스트는 애플리케이션 전반에서 사용되는 공통적인 빈(Bean)들을 관리하는 컨테이너입니다. 이 컨텍스트는 `ContextLoaderListener`에 의해 초기화되며, 주로 비즈니스 로직을 처리하는 서비스, 데이터 접근을 위한 DAO, 애플리케이션에서 널리 사용되는 유틸리티 빈들이 정의됩니다.

    주요 역할:
  - 데이터베이스 연결, 서비스 계층, 공통 유틸리티 등을 관리.
  - 애플리케이션 전역에서 공유되는 빈을 관리.

- **서블릿 컨텍스트 (Web ApplicationContext)**
 서블릿 컨텍스트는 주로 웹 계층에 특화된 빈들을 관리하는 컨테이너입니다. 이는 `DispatcherServlet`에 의해 초기화되며, 컨트롤러, 뷰 리졸버, 핸들러 매핑 등과 같은 웹 요청 처리를 위한 빈들이 이 컨텍스트에 포함됩니다.

    주요 역할:
  - HTTP 요청을 처리하는 컨트롤러, 뷰 리졸버, 핸들러 매핑 등을 관리.
  - 루트 컨텍스트에서 정의된 빈을 참조하여 웹 요청을 처리.

두 컨텍스트의 관계
- **부모-자식 관계**: 서블릿 컨텍스트(Web ApplicationContext)는 루트 컨텍스트(Root ApplicationContext)의 자식 컨텍스트로 동작합니다. 즉, 서블릿 컨텍스트는 루트 컨텍스트에서 정의된 빈을 참조할 수 있지만, 그 반대는 불가능합니다.

이점:
- **계층화된 구조**: 애플리케이션의 각 계층(비즈니스 로직, 웹 계층 등)에 맞는 빈을 구분하여 관리함으로써, 책임의 분리와 모듈화를 촉진합니다.
- **유연한 구성**: 특정 서블릿마다 별도의 컨텍스트를 두어 독립적인 설정을 유지할 수 있습니다. 예를 들어, 서로 다른 URL 패턴에 대응하는 여러 `DispatcherServlet`을 설정하고, 각 서블릿이 독립적인 컨텍스트를 가질 수 있습니다.


따라서 두 개의 스프링 컨테이너를 사용함으로써 애플리케이션의 계층 구조를 보다 명확하게 관리할 수 있고, 애플리케이션 전반에서 공통적으로 사용되는 부분과 웹 계층에 특화된 부분을 구분할 수 있게 됩니다. 이러한 구조는 스프링 애플리케이션의 확장성과 유지보수성을 높이는 데 기여합니다.


---


참고자료 <br>
[https://asfirstalways.tistory.com/334](https://asfirstalways.tistory.com/334)