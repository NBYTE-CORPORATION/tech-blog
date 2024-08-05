---
layout: post
title: Springboot3 - HTTP 클라이언트
subtitle: 스프링부트3 환경에서 api를 사용할 때 사용하는 http 클라이언트들에 대해 알아봅시다
author: 차정우
categories: Spring
tags: [스프링, spring, 스프링부트3, HTTP, HTTP 클라이언트, RestTemplate, WebClient, AsynchttpClient]
---

## 스프링부트에서 HTTP 클라이언트 활용하기: RestTemplate, WebClient, 그리고 AsynchttpClient 비교

스프링부트 3.0 이상에서는 HTTP 클라이언트 라이브러리로 **RestTemplate**과 **WebClient**를 제공합니다. 이 두 라이브러리는 각각 동기식 및 비동기식 HTTP 요청을 처리하는 데 사용되며, 사용자의 필요에 따라 선택할 수 있습니다. 또한, Java 생태계에서는 **AsynchttpClient**라는 비동기 HTTP 클라이언트 라이브러리도 존재합니다. 이번 포스팅에서는 이 세 가지 라이브러리의 특징과 장단점을 비교하고, 스프링 생태계에서의 활용 방법을 살펴보겠습니다.

### 1. RestTemplate: 동기식 HTTP 클라이언트

RestTemplate은 스프링에서 제공하는 HTTP 클라이언트로, 동기식 방식으로 HTTP 요청을 처리합니다. 즉, API 요청을 보내고 응답을 받을 때까지 호출한 스레드가 블로킹됩니다. 이는 간단한 API 호출에 적합하지만, 요청에 대한 응답이 느릴 경우 전체 애플리케이션의 성능을 저하시킬 수 있습니다.

#### 주요 특징

- **동기식 처리**: 요청을 보내고 응답을 기다려야 하므로, 요청에 대한 응답이 완료될 때까지 다른 작업을 수행할 수 없습니다.
- **간단한 사용법**: API 호출이 직관적이며 코드가 간단합니다.
- **HTTP 메소드 지원**: GET, POST, PUT, DELETE 등 다양한 HTTP 메소드를 지원합니다.

#### 예제 코드

```java
import org.springframework.web.client.RestTemplate;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class ApiController {

    private final RestTemplate restTemplate;

    public ApiController(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    @GetMapping("/api/users")
    public User[] getUsers() {
        String apiUrl = "https://jsonplaceholder.typicode.com/users";
        return restTemplate.getForObject(apiUrl, User[].class);
    }
}
```


### 2. WebClient: 비동기 HTTP 클라이언트

WebClient는 스프링 5.0 이상에서 도입된 비동기 HTTP 클라이언트로, 비동기 논블로킹 처리를 지원합니다. WebClient는 리액티브 프로그래밍 모델을 기반으로 하여, Mono와 Flux와 같은 리액티브 타입을 사용하여 데이터를 전송하고 수신합니다. 비동기 방식으로 작동하므로 응답을 기다리지 않고 다른 작업을 수행할 수 있습니다.

#### 주요 특징

- **비동기식 처리**: 요청을 보내고 응답을 기다리지 않아도 되므로, 다른 작업을 동시에 수행할 수 있습니다.
- **리액티브 타입 지원**: Mono와 Flux를 사용하여 데이터의 흐름을 관리할 수 있습니다.
- **논블로킹 I/O**: 스레드가 응답을 기다리는 것이 아니라, 응답이 완료되면 콜백을 통해 결과를 처리합니다.
- **JSON, XML 처리 용이**: 다양한 데이터 형식을 쉽게 처리할 수 있습니다.

#### 예제 코드

```java
import org.springframework.web.reactive.function.client.WebClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import reactor.core.publisher.Mono;

@RestController
public class ApiController {

    private final WebClient webClient;

    public ApiController(WebClient.Builder webClientBuilder) {
        this.webClient = webClientBuilder.baseUrl("https://jsonplaceholder.typicode.com").build();
    }

    @GetMapping("/api/users")
    public Mono<User[]> getUsers() {
        return webClient.get()
                .uri("/users")
                .retrieve()
                .bodyToMono(User[].class);
    }
}
```


### 3. AsynchttpClient: 비동기 HTTP 클라이언트 라이브러리

AsynchttpClient는 Java 생태계에서 제공하는 비동기 HTTP 클라이언트 라이브러리로, 아파치 HTTP 클라이언트와 유사하게 HTTP 요청을 비동기적으로 처리할 수 있습니다. 이 라이브러리는 응답을 기다리지 않고, 요청을 보내고 다른 작업을 계속 수행할 수 있는 장점이 있습니다.

#### 주요 특징

- **비동기 처리**: 요청 후 응답을 기다리지 않고 다른 작업을 수행할 수 있습니다.
- **성능 최적화**: 논블로킹 I/O를 통해 높은 성능을 제공합니다.
- **유연한 API**: 다양한 HTTP 메소드와 옵션을 지원하며, 설정이 용이합니다.

#### 예제 코드

```java
import org.asynchttpclient.AsyncHttpClient;
import org.asynchttpclient.Dsl;
import org.asynchttpclient.Response;

public class ApiService {

    private final AsyncHttpClient client = Dsl.asyncHttpClient();

    public void fetchUsers() {
        client.prepareGet("https://jsonplaceholder.typicode.com/users")
            .execute()
            .toCompletableFuture()
            .thenAccept(response -> {
                // 응답 처리
                System.out.println(response.getResponseBody());
            })
            .exceptionally(ex -> {
                // 예외 처리
                ex.printStackTrace();
                return null;
            });
    }
}
```


### 4. 동기식 vs 비동기식: 어떤 것을 선택할까?

- **RestTemplate**은 간단한 API 호출이 필요한 경우 적합합니다. 동기식으로 작동하므로, 코드가 간단하고 이해하기 쉬운 장점이 있습니다. 그러나 응답이 느릴 경우 애플리케이션의 성능에 영향을 미칠 수 있습니다.
- **WebClient**는 비동기식으로 작동하므로, 대량의 API 호출이나 사용자 요청이 많은 애플리케이션에서 더 높은 성능을 발휘합니다. 리액티브 프로그래밍 모델을 통해 데이터 흐름을 관리할 수 있어 복잡한 비즈니스 로직을 구현할 때 유리합니다.
- **AsynchttpClient**는 Java 생태계에서 비동기 HTTP 요청을 처리하는 데 최적화된 라이브러리로, 높은 성능과 유연성을 제공합니다. 특히, 다양한 HTTP 요청을 비동기적으로 처리해야 하는 경우 유용합니다.

### 결론

스프링부트에서 HTTP 클라이언트를 활용하는 방법으로 RestTemplate, WebClient, 그리고 AsynchttpClient를 살펴보았습니다. 각 라이브러리는 각각의 장단점이 있으며, 프로젝트의 요구 사항에 따라 적절한 방법을 선택하여 사용할 수 있습니다. 동기식 처리인 RestTemplate은 간단한 상황에 적합하고, 비동기식 처리인 WebClient와 AsynchttpClient는 복잡한 비즈니스 로직이나 성능이 중요한 경우에 적합합니다.