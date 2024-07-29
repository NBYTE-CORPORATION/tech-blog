---
layout: post
title: Spring MVC 와 Spring WebFlux
subtitle: 스프링 생태계에서 http의 통신 방식에 따른 디자인 패턴에 대해 알아봅시다.
author: 차정우
categories: Spring
tags: [Springboot, Springboot, 스프링, WebFlux, MVC]
---

## WebFlux와 MVC 구조의 차이점

웹 애플리케이션 개발에서 자주 사용되는 두 가지 주요 패러다임은 Spring MVC와 Spring WebFlux입니다. 두 구조 모두 스프링 프레임워크를 기반으로 하지만, 그 아키텍처와 사용 사례가 다릅니다. 이 글에서는 두 구조의 차이점과 각각의 장단점을 자세히 설명하겠습니다.

### 1. Spring MVC

#### 1.1 개요
Spring MVC(Model-View-Controller)는 전통적인 서버-클라이언트 아키텍처에서 널리 사용되는 동기식 웹 프레임워크입니다. 클라이언트의 요청을 서버가 처리하고, 그 결과를 다시 클라이언트에 반환하는 구조입니다.

#### 1.2 특징
- **동기식 처리:** 모든 요청과 응답이 순차적으로 처리됩니다. 즉, 하나의 요청이 완료될 때까지 다음 요청은 대기합니다.
- **블로킹 I/O:** 클라이언트의 요청을 처리하는 동안 스레드가 블로킹되어 대기합니다.
- **모델-뷰-컨트롤러 패턴:** 애플리케이션을 세 가지 주요 역할로 분리하여 관리합니다.
  - **모델:** 애플리케이션 데이터와 비즈니스 로직을 담당합니다.
  - **뷰:** 사용자에게 데이터를 보여주는 역할을 합니다.
  - **컨트롤러:** 모델과 뷰를 연결하고 사용자의 요청을 처리합니다.

#### 1.3 장점
- **단순성:** 동기식 처리 방식으로 인해 이해하고 구현하기 쉽습니다.
- **광범위한 라이브러리 지원:** 많은 서드파티 라이브러리와의 호환성이 뛰어납니다.
- **성숙한 에코시스템:** 오랜 시간 동안 발전해 온 많은 기능과 안정성을 제공합니다.

#### 1.4 단점
- **확장성 한계:** 동기식 처리와 블로킹 I/O로 인해 대규모 트래픽을 처리하는데 한계가 있습니다.
- **자원 소모:** 많은 요청을 처리할 때 스레드와 메모리 자원을 많이 소모합니다.



---

### 2. Spring WebFlux

#### 2.1 개요
Spring WebFlux는 비동기식 논블로킹 I/O를 사용하는 웹 프레임워크로, 리액티브 프로그래밍 모델을 기반으로 합니다. 이는 대규모의 동시 요청을 효율적으로 처리할 수 있도록 설계되었습니다.

#### 2.2 특징
- **비동기식 처리:** 요청과 응답이 비동기적으로 처리되어, 하나의 요청이 완료될 때까지 다른 요청이 대기하지 않습니다.
- **논블로킹 I/O:** 서버는 요청을 처리하는 동안 블로킹되지 않고, 다른 작업을 수행할 수 있습니다.
- **리액티브 프로그래밍:** 데이터 스트림과 변경 전파를 사용하여 비동기 데이터 처리 및 이벤트 기반 시스템을 구현합니다.

#### 2.3 장점
- **높은 확장성:** 비동기식 처리와 논블로킹 I/O로 인해 많은 동시 요청을 효율적으로 처리할 수 있습니다.
- **자원 효율성:** 스레드와 메모리를 효율적으로 사용하여 자원 소모를 최소화합니다.
- **리액티브 스트림 지원:** 데이터 스트림을 간편하게 처리할 수 있으며, 백프레셔(Backpressure)와 같은 고급 기능을 제공합니다.

#### 2.4 단점
- **복잡성:** 비동기 처리와 리액티브 프로그래밍 모델로 인해 코드가 복잡해질 수 있습니다.
- **러닝 커브:** 리액티브 프로그래밍 패러다임을 이해하고 익히는 데 시간이 필요합니다.
- **제한된 서드파티 지원:** 모든 라이브러리와의 호환성이 보장되지 않으며, 리액티브 스타일의 라이브러리를 사용해야 합니다.

---


### 3. 비교

| 특징                       | Spring MVC       | Spring WebFlux    |
|--------------------------|-------------------|--------------------|
| 처리 방식                  | 동기식            | 비동기식           |
| I/O 모델                  | 블로킹 I/O        | 논블로킹 I/O       |
| 프로그래밍 모델            | 전통적인 MVC      | 리액티브 프로그래밍 |
| 확장성                    | 제한적            | 높은 확장성        |
| 자원 소모                  | 상대적으로 높음   | 자원 효율적        |
| 코드 복잡성                | 낮음              | 높음               |
| 러닝 커브                  | 낮음              | 높음               |
| 서드파티 라이브러리 호환성 | 광범위함          | 제한적             |



---

### 4. 결론
**Spring MVC**와 **Spring WebFlux**는 각각의 장단점이 뚜렷합니다. **Spring MVC**는 단순성과 광범위한 라이브러리 지원을 제공하며, 동기식 처리 방식으로 인해 이해하고 구현하기 쉽습니다. 반면, **Spring WebFlux**는 높은 확장성과 자원 효율성을 제공하지만, 비동기 처리와 리액티브 프로그래밍 모델로 인해 코드가 복잡해지고 러닝 커브가 높습니다.

따라서, 애플리케이션의 요구 사항에 따라 적합한 프레임워크를 선택하는 것이 중요합니다. 단순한 웹 애플리케이션이나 기존 시스템과의 호환성이 중요한 경우** Spring MVC**를 선택하는 것이 좋습니다. 반면, 높은 동시성을 요구하거나 비동기 데이터 처리가 중요한 경우 **Spring WebFlux**가 더 적합할 수 있습니다.



---

### 5. 간단한 예제 코드

#### Spring MVC 예제
먼저, Spring MVC 애플리케이션의 간단한 예제를 보겠습니다.

##### 1. Spring MVC 설정
build.gradle 파일에 필요한 의존성을 추가합니다.

```groovy

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
}
```

##### 2. Controller 클래스
HelloController.java 파일을 생성하고 간단한 컨트롤러를 작성합니다.

```java

package com.example.demo;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String sayHello() {
        return "Hello, Spring MVC!";
    }
}
```

##### 3. 애플리케이션 클래스
DemoApplication.java 파일을 생성하여 Spring Boot 애플리케이션을 시작합니다.

```java

package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

##### 4. 실행 및 테스트
애플리케이션을 실행하고 브라우저에서 http://localhost:8080/hello를 방문하면 "Hello, Spring MVC!" 메시지를 볼 수 있습니다.


---


#### Spring WebFlux 예제
다음으로, Spring WebFlux 애플리케이션의 간단한 예제를 보겠습니다.

##### 1. Spring WebFlux 설정
build.gradle 파일에 필요한 의존성을 추가합니다.

```groovy

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-webflux'
}
```

##### 2. Controller 클래스
HelloController.java 파일을 생성하고 간단한 컨트롤러를 작성합니다.

```java

package com.example.demo;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import reactor.core.publisher.Mono;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public Mono<String> sayHello() {
        return Mono.just("Hello, Spring WebFlux!");
    }
}
```

##### 3. 애플리케이션 클래스
DemoApplication.java 파일을 생성하여 Spring Boot 애플리케이션을 시작합니다.

```java

package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

##### 4. 실행 및 테스트
애플리케이션을 실행하고 브라우저에서 http://localhost:8080/hello를 방문하면 "Hello, Spring WebFlux!" 메시지를 볼 수 있습니다.