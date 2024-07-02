---
layout: post
title: Gradle-Wrapper에 대해
subtitle: Gradle-Wrapper 의 종류와 역할을 알아봅시다
author: 차정우
categories: Spring
tags: [스프링, spring, gradle]
---

### Gradle

Gradle은 빌드 자동화 시스템으로, JAVA, C/C++, Python 등의 프로젝트를 빌드, 테스트, 배포하는 데 널리 사용됩니다. Gradle Wrapper는 Gradle을 설치하지 않은 시스템에서도 프로젝트를 쉽게 빌드할 수 있도록 도와줍니다.



---

### Gradle Wrapper의 구성 요소

1. Gradle Wrapper 스크립트:
 
   + gradlew: Unix 계열 운영체제용 실행 스크립트.
   + gradlew.bat: Windows 운영체제용 실행 스크립트.  
   <br>
2. gradle 폴더(Gradle Wrapper 설정 파일):

   + gradle/wrapper/gradle-wrapper.properties: Gradle Wrapper가 사용할 Gradle 버전과 다운로드 URL 등을 정의합니다.
   + gradle/wrapper/gradle-wrapper.jar: Gradle Wrapper의 핵심 실행 파일로, 지정된 버전의 Gradle을 다운로드하고 실행합니다.  
   <br>
3. .gradle 폴더:

   + gradle/wrapper/gradle-wrapper.jar: Gradle Wrapper의 핵심 실행 파일로, 지정된 버전의 Gradle을 다운로드하고 실행합니다.

---

### 필수 파일들
 
1. Gradle Wrapper 파일들

   이들은 Gradle을 실행하고 다운로드하는 데 필요합니다.

   + gradlew
   + gradlew.bat
   + gradle/wrapper/gradle-wrapper.properties
   + gradle/wrapper/gradle-wrapper.jar
   + .gradle (디렉토리)  
   <br>

2. 빌드 설정 파일들

   이들은 프로젝트의 빌드 설정과 의존성을 정의합니다.

   + **build.gradle**: 프로젝트의 빌드 설정, 의존성, 플러그인 등을 정의합니다.
   + **settings.gradle**: 멀티 프로젝트 구성등의 프로젝트 구조를 정의합니다.

---

### 역할
 
+ **gradlew 와 gradlew.bat**: Gradle Wrapper 스크립트로, 이 스크립트들이 Gradle을 다운로드하고 실행할 수 있도록 설정 파일(gradle-wrapper.properties)과 JAR 파일(gradle-wrapper.jar)을 필요로 합니다.
+ **gradle/wrapper/gradle-wrapper.properties**: Gradle Wrapper가 사용할 Gradle 버전을 지정하고, 필요한 경우 Gradle을 다운로드할 URL을 제공합니다.
+ **gradle/wrapper/gradle-wrapper.jar**: Gradle Wrapper의 실행 파일로, 지정된 Gradle 버전을 다운로드하고 실행하는 역할을 합니다.
+ **build.gradle**: 프로젝트의 빌드 설정 파일로, 프로젝트의 의존성, 플러그인, 빌드 태스크 등을 정의합니다. Gradle이 프로젝트를 빌드할 때 이 파일의 내용을 읽고 처리합니다.

+ **.gradle**: 프로젝트의 빌드 과정에서 생성되는 캐시 파일, 잠금 파일(lock files) 등을 저장합니다. 이 디렉토리는 Gradle이 빌드 작업을 더 빠르게 수행할 수 있도록 돕습니다.

---

#### 전체 구조 예시

다음은 Gradle Wrapper가 포함된 스프링 부트 프로젝트의 기본 구조 예시입니다:

```
my-spring-boot-app
│
├── gradlew
├── gradlew.bat
├── build.gradle
├── settings.gradle (멀티 프로젝트일 경우)
├──.gradle
│     └──caches/
│     └──daemon/
│     └──native/
│     └──notifications/
│     └──wrapper/
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
└── src
    └── main
    └── test
```

---

### Gradle Wrapper 사용 방법

- 빌드 실행:  
 
   Unix/Linux/macOS:
   bash
   ```
   ./gradlew build
   ```

   Windows:
   cmd
   ```
   gradlew.bat build
   ```
<br>

- 다른 Gradle 작업 실행:

   예를 들어, 테스트를 실행하려면:
   Unix/Linux/macOS:
   bash
   ```
   ./gradlew test
   ```
   Windows:
   cmd
   ```
   gradlew.bat test
   ```

<br>


---

###  결론

필요한 파일들: 

+ gradlew (unix 기반 즉 mac 이나 리눅스 환경을 위한 실행스크립트)
또는 
gradlew.bat  (윈도우를 위한 실행파일)
+ gradle/wrapper/gradle-wrapper.jar (지정된 Gradle 버전을 다운로드하고 실행하는 역할)
+ gradle/wrapper/gradle-wrapper.properties (사용할 gradle 버전지정 및 다운할 url 지정 )
+ build.gradle (여기서 각종 dependency 설정)
+ settings.gradle (멀티 프로젝트일 경우 사용하는데 단일 프로젝트라도 루트 디렉토리를 나타내기 위해 필요)
+ .gradle (빌드 과정에서 생성되는 캐시 파일, 잠금 파일(lock files) 등을 저장 => 빌드 속도 향상)
