---
layout: post
title: "Spring Boot3 REST API 문서화: Spring REST Docs 적용기"
subtitle: Spring Boot3에 Spring REST Docs 적용하는 방법을 공유합니다.
author: 이원모
categories: Spring
tags: [Spring, Springboot, API, Rest Docs, 자동화, 문서]
---

Spring Boot 애플리케이션에서 RESTful API 문서를 자동으로 생성하는 것은 매우 중요한 작업이라고 생각합니다. 이 글에서는 Spring Boot3에 Spring REST Docs와 Asciidoctor를 사용하여 REST API 문서를 자동으로 생성하고 이를 브라우저에서 확인할 수 있도록 설정하는 방법을 단계별로 설명합니다.

예제 소스는 [여기](https://github.com/enxec/spring-boot3-spring-rest-docs)에서 확인할 수 있습니다.

## build.gradle 설정
---
1. 플러그인 추가

    먼저, asciidoctor 플러그인을 추가합니다.

    ```groovy
    plugins {
        id 'org.asciidoctor.jvm.convert' version '3.3.2'
    }
    ```

    >💡 __참고__  
    > Asciidoctor는 Spring REST Docs이 추출하는 AsciiDoc 형식의 문서를 다양한 출력 형식으로 변환하는 도구로, Spring REST Docs와 함께 사용하면 API 문서를 HTML,
    > PDF 등으로 쉽게 변환할 수 있습니다. 때문에 가독성을 높이기 위해 Spring REST Docs와 asciidoctor를 함께 사용하는 것이 좋습니다.
    
    <br>

2. 의존성 추가

    Spring Boot, Spring REST Docs, Asciidoctor와 관련된 의존성들을 추가합니다.

    ```groovy
    dependencies {
        testImplementation 'org.springframework.restdocs:spring-restdocs-mockmvc'
        asciidoctorExtensions 'org.springframework.restdocs:spring-restdocs-asciidoctor:2.0.6.RELEASE'
    }
    ```

    <br>

3. 테스트 설정

    테스트 출력 디렉토리와 JUnit 플랫폼을 설정합니다. 테스트 실행 시 생성된 스니펫 파일들이 지정된 디렉토리에 저장됩니다.

    ```groovy
    ext {
        snippetsDir = file('build/generated-snippets')
    }

    test {
        outputs.dir snippetsDir
    }
    ```

    <br>

4. Asciidoctor 설정

    Asciidoctor 작업을 설정하여 문서 생성 경로, 속성 및 종속성을 정의합니다. 이 설정을 통해 Asciidoctor가 스니펫 파일들을 읽어 HTML 문서를 생성합니다.

    ```groovy
    configurations {
        asciidoctorExtensions
    }

    asciidoctor {
        configurations 'asciidoctorExtensions'
        sources {
            include 'index.adoc'
        }
        outputDir = layout.buildDirectory.dir('docs/asciidoc')
        dependsOn test
        attributes 'snippets': snippetsDir

        inputs.dir snippetsDir
        sourceDir = file('src/docs/asciidoc')
    }
    ```

    >💡 __참고__  
    > configurations블록은 dependencies블록 아래에 작성하면 dependencies 블록을 추가로 작성해야합니다.  

    <br>

5. 문서 복사 작업 설정

    생성된 문서를 정적 리소스 디렉토리로 복사하는 작업을 정의합니다. 이렇게 하면 Spring Boot 애플리케이션에서 HTML 문서를 정적 리소스로 제공할 수 있습니다.

    ```groovy
    task copyDocs(type: Copy) {
        from 'build/docs/asciidoc'
        into 'src/main/resources/static/docs'
        dependsOn asciidoctor
    }

    build {
        dependsOn copyDocs
    }
    ```

<br>

## Asciidoctor 문서 작성
---
'src/docs/asciidoc/index.adoc' 파일을 생성하여 API 문서를 작성합니다. 이 파일은 여러 스니펫 파일들을 포함하여 최종 문서를 구성합니다.

```text
= API Documentation
:doctype: book
:toc: left

== Example API

include::{snippets}/example/http-request.adoc[]
include::{snippets}/example/http-response.adoc[]
```

<br>

## REST 컨트롤러 작성
---
REST API 엔드포인트를 정의하는 컨트롤러를 작성합니다. 이 예제에서는 /example 엔드포인트를 제공합니다.

```java
package enxec.example.restdocs.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ExampleController {

    @GetMapping("/example")
    public String example() {
        return "Hello, World!!";
    }
}
```
<br>


## 테스트 클래스 작성 및 실행
---

Spring REST Docs를 사용하여 API 테스트 클래스를 작성하고, 작성된 코드를 실행하면 REST Docs 스니펫 파일들이 생성됩니다.

```java
package enxec.example.restdocs;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.restdocs.RestDocumentationContextProvider;
import org.springframework.restdocs.RestDocumentationExtension;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.context.WebApplicationContext;

import static org.springframework.restdocs.mockmvc.MockMvcRestDocumentation.document;
import static org.springframework.restdocs.mockmvc.MockMvcRestDocumentation.documentationConfiguration;
import static org.springframework.restdocs.operation.preprocess.Preprocessors.*;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

@SpringBootTest
@ExtendWith({SpringExtension.class, RestDocumentationExtension.class})
@AutoConfigureMockMvc
public class SpringRestDocsTest {
    @Autowired
    private WebApplicationContext context;

    private MockMvc mockMvc;

    @BeforeEach
    public void setUp(RestDocumentationContextProvider restDocumentation) {
        this.mockMvc = MockMvcBuilders.webAppContextSetup(this.context)
                .apply(documentationConfiguration(restDocumentation))
                .build();
    }

    @Test
    public void exampleTest() throws Exception {
        this.mockMvc.perform(get("/example"))
                .andExpect(status().isOk())
                .andDo(document("example",
                        preprocessRequest(prettyPrint()),
                        preprocessResponse(prettyPrint())
                ));
    }
}
```

<br>

## 프로젝트 빌드 및 실행
---
1. 프로젝트 빌드

    프로젝트를 빌드하여 HTML 문서를 생성합니다. Asciidoctor가 실행되어 스니펫 파일들을 기반으로 HTML 문서를 생성하고, 이 문서는 정적 리소스 디렉토리로 복사됩니다.

    ```bash
    ./gradlew build
    ```

    빌드가 완료되면 HTML 문서는 src/main/resources/static/docs 디렉토리에 복사됩니다.  
    <br>

2. Spring Boot 애플리케이션 실행 및 문서확인

    Spring Boot 애플리케이션을 실행합니다. 

    ```bash
    ./gradlew bootRun
    ```

    실행 후 브라우저에서 다음 주소로 생성된 API 문서에 접근할 수 있습니다.

    ```bash
    http://localhost:8080/docs/index.html
    ```

---

읽어주셔서 감사합니다. 😊 

__Reference__  
ChatGPT - OpenAI