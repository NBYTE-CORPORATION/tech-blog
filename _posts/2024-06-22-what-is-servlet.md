---
layout: post
title: Servlet이란?
subtitle: 스프링의 핵심 요소 서블릿에 대해 알아봅니다.
author: 이원모
categories: Spring
tags: [servlet, 서블릿, spring, 스프링]
---

스프링에 관심이 있으신분들은 Dispatcher Servlet 또는 FrontController라는 개념을 한번쯤은 들어보셨을 것이다. 이것은 스프링개발에 있어서 꼭 알아두셔야하는 중요한 핵심 요소 중 하나인데, 그전에 서블릿에 대한 선행학습이 필요하다. 
그래서 이번 포스트에서는 Dispatcher Servlet의 선수지식이 되는 서블릿에 대해 살펴보고자한다.

## 개념
---
서블릿은 Java에서 웹 애플리케이션을 개발할 때 사용되는 서버 측 컴포넌트(프로그램)이다. 서블릿은 Java EE (Enterprise Edition)에서 제공하는 기술 중 하나로 Java Servlet API를 통해 구현되며, 웹 서버나 애플리케이션 서버에서 실행된다. 서블릿은 주로 HTTP 요청을 처리하고, 동적인 웹 콘텐츠를 생성하며, 클라이언트와 서버간의 상호작용을 담당한다.

> - 서블릿의 주요기능
>   1. HTTP 요청 및 응답 처리 : 클라이언트로부터의 HTTP 요청을 받아 적절한 응답을 생성한다.
>   2. 동적 콘텐츠 생성: HTML, XML, JSON 등의 동적 웹 콘텐츠를 생성한다.
>   3. 상태 관리: 세션을 통해 클라이언트의 상태를 관리할 수 있다.

<br>

## 서블릿 컨테이너
---
서블릿을 보다 쉽게 이해하려면 서블릿을 실행할 수 있는 서블릿 컨테이너라는 환경을 먼저알아야한다. 잠깐 살펴보면, 서블릿 컨테이너(서블릿엔진)는 자바 서블릿을 실행할 수 있는 환경을 제공하는 소프트웨어 컴포넌트다. 서블릿 컨테이너는 자바 웹 애플리케이션의 서버 측 컴포넌트로, 클라이언트로부터 HTTP요청을 받아 들이고, 이를 처리하여 응답을 생성하는 역할을 한다. 주요기능은 다음과 같다.

1. 서블릿 라이프 사이클 관리  
    서블릿을 생성하고 초기화(init 메서드), 서비스(service 메서드), 종료(destroy 메서드)하는 등의 생명 주기를 관리한다.

2. 요청과 응답 관리  
    클라이언트로부터의 HTTP 요청을 서블릿에 전달하고, 서블릿이 생성한 HTTP 응답을 클라이언트에 반환한다.

3. 멀티스레딩 지원  
    다중 클라이언트 요청을 동시에 처리하기 위해 스레드를 관리한다.

4. 보안 관리  
    웹 애플리케이션의 보안 설정을 처리하여 인증 및 권한 부여를 관리한다.

5. 세션 관리  
    클라이언트와의 세션을 유지하고 관리하여 지속적인 사용자 상호작용을 가능하게 한다.

대표적인 서블릿 컨테이너로는 Tomcat, Jetty, JBoss 등이 있다. 서블릿 컨테이너는 보통 웹 서버와 결합되어 동작하며, 웹 서버로부터 HTTP 요청을 받아 이를 서블릿 컨테이너로 전달한다. 그리고 해당 요청을 처리할 서블릿을 찾아 요청을 전달하고, 서블릿이 생성한 응답을 웹 서버를 통해 클라이언트에 반환한다.

<br>

## 라이프 사이클
---
서블릿을 이해하기 위한 선수지식을 쌓았으니 이제 서블릿에 집중해보자.

서블릿의 라이프 사이클은 서블릿이 생성되고, 요청을 처리하고, 소멸되는 일련의 과정을 말한다. 서블릿의 라이플 사이클은 다음과 같은 주요 단계로 이루어진다.

1. 서블릿 클래스 로딩  
    서블릿 컨테이너는 2가지 방식으로 서블릿 클래스 파일을 로드하며, 웹 애플리케이션의 초기화 시점이나 처음 클라이언트 요청이 있을 때 로드된다.

    - 최초의 요청 시 로딩(On-Demand Loading)  
        서블릿 컨테이너는 서블릿에 대한 최초의 요청이 들어올 때 서블릿 클래스를 로드하고 인스턴스를 생성한 후 init() 메서드를 호출하여 서블릿을 초기화한다. 이는 서블릿이 처음으로 필요할 때까지 메모리를 절약하는 방식이다.

        ```xml
        <!-- 최초의 요청 시 로딩 -->
        <servlet>
            <servlet-name>ExampleServlet</servlet-name>
            <servlet-class>com.example.ExampleServlet</servlet-class>
        </servlet>
        <servlet-mapping>
            <servlet-name>ExampleServlet</servlet-name>
            <url-pattern>/example</url-pattern>
        </servlet-mapping>
        ```

    - 서버 시작 시 로딩(Preloading or Pre-initialization)  
        서블릿을 웹 애플리케이션이 시작될 때 미리 로드하고 초기화할 수 있습니다. 이를 위해서는 web.xml 파일에 서블릿의 load-on-startup 요소를 설정합니다. 이 값을 설정하면 서블릿 컨테이너는 서버가 시작될 때 서블릿을 로드하고 초기화한다.

        ```xml
        <!-- 서버 시작 시 로딩 -->
        <servlet>
            <servlet-name>ExampleServlet</servlet-name>
            <servlet-class>com.example.ExampleServlet</servlet-class>
            <load-on-startup>1</load-on-startup>
        </servlet>
        <servlet-mapping>
            <servlet-name>ExampleServlet</servlet-name>
            <url-pattern>/example</url-pattern>
        </servlet-mapping>
        ```

        요소의 값을 양수로 설정하면 서블릿 컨테이너는 서버가 시작될 때 서블릿을 로드하고 초기화합니다. 이 값이 클수록 서블릿이 로드되는 순서가 뒤로 밀리게 된다.

2. 서블릿 인스턴스 생성  
    서블릿 컨테이너는 로드된 서블릿 클래스의 인스턴스를 생성합니다. 이 단계에서 서블릿 객체가 생성된다.

3. 서블릿 초기화  
    서블릿 컨테이너는 'init()' 메서드를 호출하여 서블릿을 초기화합니다. 이 메서드는 서블릿이 생성된 후 한 번만 호출되며, 서블릿이 클라이언트 요청을 처리하기 전에 필요한 초기화 작업을 수행한다.

    - init() 메서드 정의 예시

        ```java
        public void init(ServletConfig config) throws ServletException {
            // 초기화 코드
        }
        ```
        
    - init() 메서드는 서블릿의 초기화 매개변수를 받아 초기화 작업을 수행한다.  
    <br>

4. 요청처리  
    클라이언트 요청이 들어오면 서블릿 컨테이너는 서블릿의 'service()' 메서드를 호출한다. 이 메서드는 클라이언트의 요청을 처리하고, 적절한 응답을 생성한다.

    - service() 메서드 정의 예시

        ```java
        public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
            // 요청 처리 코드
        }
        ```

    - 'service()' 메서드는 HTTP요청 메서드(GET, POST 등)에 따라 'doGet()', 'doPost()' 등의 메서드를 호출한다.

        ```java
        protected void doGet(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
            // GET 요청 처리 코드
        }

        protected void doPost(HttpServletRequest req, HttpServletResponse res) throws ServletException, IOException {
            // POST 요청 처리 코드
        }
        ```

    <br>

5. 서블릿 소멸  
    서블릿 컨테이너는 서블릿을 종료하기전 메모리에서 해제할 때 'destroy()' 메서드를 호출하며, 이 메서드는 서블릿이 종료되기 전에 필요한 정리 작업을 수행한다.

    - destroy() 메서드 정의 예시

        ```java
        public void destroy() {
            // 정리 코드
        }
        ```
    
    근데 서블릿은 언제 소멸될까? 내용은 다음과 같다.

    1) 애플리케이션 재배포(재배포 또는 설정변경)  

    2) 서버 종료 또는 재시작  

    3) 서블릿 재로드  
    
    &nbsp;&nbsp;&nbsp;&nbsp;- 일부 서블릿 컨테이너는 서블릿 클래스를 변경할 때 자동으로 서블릿을 재로드한다. 이 때 기존 서블릿 인스턴스가<br> &nbsp;&nbsp;&nbsp;&nbsp; 종료되고 새로운 인스턴스가 로드된다.  

    4) 서블릿 컨테이너에 의한 종료  

    &nbsp;&nbsp;&nbsp;&nbsp;- 서블릿 컨테이너가 메모리 관리나 자원 해제 목적으로 서블릿을 종료할 수 있다. 이는 서버 설정에 따라 다를 수 있으며,<br> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;일반적으론 발생하지않는다.

<br>

> &nbsp;&nbsp;&nbsp;&nbsp;📌 &nbsp;__요약__  
> - 서블릿 라이프 사이클 전체흐름
>   1. 서블릿 클래스 로딩
>   2. 서블릿 인스턴스 생성
>   3. 초기화(init() 메서드 호출)
>   4. 요청처리(service() 메서드 및 doGet(), doPost() 메서드 호출)
>   5. 소멸(destroy() 메서드 호출)

<br>

## 동작 방식
---
그렇다면 서블릿은 라이프 사이클을 바탕으로 어떠한 흐름에서 어떻게 동작할까? 내용은 다음과 같다.

1. 클라이언트 요청  
    클라이언트가 HTTP 요청을 보낸다.

2. 웹 서버  
    웹서버(Apache, Nginx 등)가 요청을 받아 서블릿 컨테이너(Tomcat, Jetty등)에 전달한다.

3. 리퀘스트 큐(opt)  
    서블릿 컨테이너 내부에 있는 요청 큐에 요청이 추가된다. 이는 서블릿 컨테이너가 동시에 처리 할 수 있는 요청 수를 초과하는 경우, 추가 요청을 대기시키는 역할을 한다.

4. 서블릿 컨테이너  
    서블릿 컨테이너는 요청을 처리하기 위해 다음 단계들을 수행한다.  
        - 서블릿을 찾고, 필요하면 초기화(init() 메서드 호출)한다.  
        - service() 메서드를 호출하여 요청을 처리합니다.  
        - doGet(), doPost() 등의 메서드를 호출합니다.  

5. 스레드 풀  
    서블릿 컨테이너는 요청을 처리하기 위해 스레드 풀에서 사용 가능한 스레드를 가져온다. 스레드는 요청을 처리하고, 필요한 비즈니스 로직을 실행한다.

6. 응답 생성  
    서블릿이 요청 처리를 완료하면 HTTP 응답을 생성하며, 스레드는 스레드 풀로 반환된다.

7. 서블릿 컨테이너는 생성된 HTTP 응답을 웹 서버로 반환한다. 웹 서버는 클라이언트에게 HTTP 응답을 반환한다.

<br>

아래 이미지는 위 내용을 도식화한 것이다.

![서블릿 동작 방식](/assets/images/posts/이원모/20240622/Screenshot_1.png "서블릿 동작 방식"){: width="75%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">서블릿 동작 방식</div>

<br>

## 장단점
---
서블릿의 대표적인 장단점은 다음과 같다.

__장점__

- 플랫폼 독립성
    자바로 작성되어 다양한 OS에서 동작한다.

- 확장성
    웹 애플리케이션의 기능을 쉽게 확장할 수 있다.

- 안정성
    자바의 강력한 메모리 관리와 예외 처리를 통해 안정적인 웹 애플리케이션을 개발할 수 있다.

__단점__

- 복잡성
    서블릿을 직접 다루는 것은 비교적 복잡하며, 개발자가 HTTP 프로토콜을 잘 이해해야한다.

- 유지보수 어려움
    서블릿만으로 웹 애플리케이션을 구성하면 코드가 길어지고 복잡해져 유지보수가 어려울 수 있다.
