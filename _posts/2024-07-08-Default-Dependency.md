---
layout: post
title: Default Dependency
subtitle: 스프링부트 3.0 이상 프로젝트의 기본 dependency
author: 차정우
categories: Spring
tags: [Spring, Springboot, Dependency]
---

### Spring Boot 3.0 이상 버전의 기본 의존성

Spring Boot 3.0 이상 버전의 프로젝트를 생성할 때 dependency 추가를 하나도 하지 않아도
기본적으로 프로젝트에 포함되는 의존성은 다음과 같이 3가지가 있습니다.

포함된 주요 의존성은 해당 dependency를 추가하면 gradle(또는 maven 같은 빌드 툴)이, 내부에 포함된 dependency나 추가한 dependency를 사용하기 위해 필요한 다른 dependency들까지 묶어서 가져오는 dependency들이라고 생각하시면 됩니다.

### 1. `implementation 'org.springframework.boot:spring-boot-starter'`


- **spring-boot-starter**는 여러 스프링 관련 의존성들을 간편하게 설정하기 위한 스타터입니다. 이 스타터는 다양한 기능들을 제공하며, 스프링 애플리케이션을 쉽게 설정하고 시작할 수 있게 해줍니다.

#### 포함된 주요 의존성
- **Spring Core**: 스프링의 핵심 기능을 제공하는 라이브러리입니다.IoC (또는 DI) 기능을 지원하는 영역을 담당합니다. 빈 저장소를 기반으로 빈 클래스들을 제어할 수 있는 기능을 지원하기도 합니다.
- **Spring Context**: Bean의 확장 버전으로 Spring이 Bean을 다루기 좀 더 쉽도록 기능들이 추가된 공간입니다. 단순히 Bean을 다루는 것 이외에도 추가적인 기능을 수행합니다. Bean은 모두 Context안에서 이루어집니다.
- **Logging**: 스프링 부트는 기본적으로 `spring-boot-starter-logging`을 통해 로깅 기능을 제공합니다. 이는 `Logback`, `SLF4J` 등을 포함합니다.

### 2. `testImplementation 'org.springframework.boot:spring-boot-starter-test'`


- **spring-boot-starter-test**는 스프링 부트 애플리케이션의 테스트를 쉽게 작성할 수 있도록 도와주는 의존성 모음입니다. 단위 테스트, 통합 테스트 등을 지원합니다.

#### 포함된 주요 의존성
- **JUnit 5**: 자바 테스트 프레임워크입니다. 단위 테스트와 통합 테스트를 작성할 때 사용됩니다.
- **Spring Test**: 스프링의 테스트 관련 기능을 제공합니다. 스프링 컨텍스트를 로드하여 테스트를 수행할 수 있게 해줍니다.
- **Mockito**: 자바 모킹 프레임워크로, 객체의 행동을 모방하여 테스트할 수 있게 합니다.
- **AssertJ**: Fluent 스타일의 테스트 작성 기능을 제공하는 assertion 라이브러리입니다.
- **Hamcrest**: 매처 기반의 assertion 라이브러리로, 읽기 쉽고 직관적인 테스트를 작성할 수 있게 해줍니다.
- **JSONassert**: JSON 데이터의 assertion을 도와주는 라이브러리입니다.
- **JsonPath**: JSON 데이터의 경로를 통해 값을 검증할 수 있게 해줍니다.

### 3. `testRuntimeOnly 'org.junit.platform:junit-platform-launcher'`


- **junit-platform-launcher**는 JUnit 플랫폼의 런처입니다. 이는 IDE나 빌드 도구(예: Gradle, Maven)에서 JUnit 테스트를 실행할 수 있도록 지원합니다.

#### 포함된 주요 의존성
- **JUnit Test Engine**: JUnit 5의 테스트 엔진을 실행하는 데 필요한 런처입니다.
- **Test Discovery**: 테스트 메소드와 클래스들을 검색하고 실행합니다.
- **Integration**: IDE 및 빌드 도구와의 통합을 지원하여, 테스트 실행 결과를 시각적으로 확인할 수 있게 해줍니다.

### 요약
- **spring-boot-starter**: 스프링 애플리케이션의 기본 기능들을 쉽게 설정하고 시작할 수 있게 도와줍니다.
- **spring-boot-starter-test**: 스프링 부트 애플리케이션의 테스트를 위한 다양한 도구와 라이브러리를 포함합니다.
- **junit-platform-launcher**: JUnit 테스트를 실행하고 관리하기 위한 런처로, IDE나 빌드 도구와의 통합을 지원합니다.

이러한 기본 의존성들은 스프링 부트 애플리케이션을 개발하고 테스트하는데 필요한 필수적인 도구들을 제공합니다.