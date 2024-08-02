---
layout: post
title: "Spring Boot3 REST API 문서화: Swagger 적용기"
subtitle: Spring Boot3에 Swagger 적용하는 방법을 공유합니다.
author: 이원모
categories: Spring
tags: [Swagger, Spring, Springboot, API, 자동화, 문서]
---

Swagger를 활용하여 API 문서를 자동으로 생성하는 방법을 알아보겠습니다. 여기서는 Springdoc-openapi(RESTful 웹 서비스의 명세를 표준화한 문서 포맷) 라이브러리를 사용하여 설정하는 방법을 단계별로 설명합니다.

예제 소스는 [여기](https://github.com/enxec/spring-boot3-swagger)에서 확인할 수 있습니다.

## 의존성 추가
---
먼저 build.gradle 파일에 Springdoc-openapi 라이브러리를 추가합니다.

```gradle
dependencies {
    implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.0.4'
}
```

<br>

## 애플리케이션 클래스 설정
---
Spring Boot 애플리케이션 클래스에 @OpenAPIDefinition 애노테이션을 추가하여 OpenAPI 문서를 정의할 수 있습니다.

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import io.swagger.v3.oas.annotations.OpenAPIDefinition;
import io.swagger.v3.oas.annotations.info.Info;

@SpringBootApplication
@OpenAPIDefinition(info = @Info(title = "My API", version = "v1"))
public class MyApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

<br>

## REST 컨트롤러 작성
---
간단한 REST 컨트롤러를 추가하여 OpenAPI 문서에 반영되는지 확인합니다.

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import io.swagger.v3.oas.annotations.Operation;

@RestController
public class ExampleController {
    @GetMapping("/hello")
    public String hello() {
        return "Hello, World!";
    }
}
```

<br>

## Swagger UI 접근
---
애플리케이션을 실행한 후, 브라우저에서 Swagger UI를 통해 OpenAPI 문서를 확인할 수 있습니다. 기본 URL은 다음과 같습니다

```bash
http://localhost:8080/swagger-ui/index.html
```

<br>

## 추가 설정 (선택 사항)
---
application.yml 파일에 설정을 추가하여 OpenAPI 문서의 경로를 변경할 수 있습니다.

```yml
springdoc:
  api-docs:
    path: /api-docs
  swagger-ui:
    path: /swagger-ui
```