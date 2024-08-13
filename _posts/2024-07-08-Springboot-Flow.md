---
layout: post
title: Springboot Flow
subtitle: 스프링부트의 흐름 파악하기
author: 차정우
categories: Spring
tags: [Spring, Springboot, Flow, MVC]
---

## &lt; 스프링 흐름 &gt;


&lt;정적 컨텐츠 이미지&gt;

![](https://velog.velcdn.com/images/chjw147/post/a4b5aa1e-438d-4611-b7af-1ce825e196b4/image.png)

&lt;템플릿 엔진&gt;

![](https://velog.velcdn.com/images/chjw147/post/e13f2441-ba63-45ed-8c33-905546d78181/image.png)


&lt;API&gt;

![](https://velog.velcdn.com/images/chjw147/post/5c30c9e7-f6de-4c4a-b5be-c8f307c789d3/image.png)


---

위 그림들을 조금 더 자세히 나타낸다면 이렇게 나타낼 수 있습니다.





![](https://velog.velcdn.com/images/chjw147/post/ef616994-1556-4612-8066-9670e97f723e/image.png)


![](https://velog.velcdn.com/images/chjw147/post/4a8553ab-5790-4444-97f3-26d289a0e191/image.png)



---


100% 꼭 맞는 예는 아니지만 이해를 돋기 위해 음식점을 예로 들어 보겠습니다.


+ 톰캣 서버: 음식점 전체(홀 + 주방 + 화장실등...)
  + 서블릿 컨테이너: 주방 및 홀
  + 디스패처 서블릿: 서빙도 하는 매니저(관리자). 추가적으로 핸들러매핑(어떤 요리사가 적합한지) 과 핸들러 어뎁터(요리사 부르기) 를 이용합니다.

+ 스프링 컨테이너: 요리사와 가구 및 요리도구 제공 업체
  + 핸들러매핑, 핸들러어뎁터
  + 컨트롤러/서블릿: 요리사
  + 뷰리졸버(viewresolver): 조리가 끝난 요리를 데코레이션하는 사람
  + 뷰(view): 요리를 담을 접시

+ DB: 요리재료를 파는 마트

+ 요청 데이터: 손님의 주문
+ 응답 데이터: 완성되어 서빙되는 음식

---

1. **톰캣 서버 (Web Application Server, WAS): 음식점 전체**

   역할: 음식점의 전체 운영을 담당하며, 손님의 요청(HTTP 요청)을 받고, 요청을 적절히 처리하고, 응답을 돌려주는 역할을 합니다. 톰캣은 웹 서버와 서블릿 컨테이너 역할을 모두 포함합니다.<br>
   <br>
   예시: 손님을 맞이하고, 주문을 받고, 주방에 주문을 전달하고, 준비된 음식을 손님에게 제공하는 전체 시스템(스프링 컨테이너와는 별개로 존재합니다)



2. **서블릿 컨테이너: 주방 및 홀**

   역할: 서블릿을 실행하고, 요청을 처리하는 환경을 제공합니다. 톰캣서버 안에 자신이 포함되어 있으며 자신의 내부에 디스패처 서블릿을 포함하고 있습니다.<br>
   <br>
   예시: 주방 및 홀과 같은 개념으로 서빙도 하는 매니저(디스패처 서블릿)가 활동하는 공간 입니다.


3. **스프링 컨테이너 (IoC 컨테이너): 요리사 및 도구 제공 업체**

   역할: 스프링 애플리케이션에서 빈(bean)을 생성하고, 관리하며, 의존성을 주입하는 역할을 합니다. 스프링 컨테이너는 애플리케이션의 비즈니스 로직과 컴포넌트를 관리합니다.
   **handlermapping, handleradapter, viewresolver 등은 이 스프링 컨테이너 안에 있습니다.**<br>
   <br>
   예시: 요리사와 칼이나 도마 등 도구들을 제공합니다.


4. **디스패처 서블릿 (DispatcherServlet): 서빙도 하는 매니저(관리자)**

   역할: 클라이언트의 요청을 받아서 적절한 컨트롤러로 전달하고, 컨트롤러의 응답을 받아 클라이언트에게 전달하는 역할을 합니다. 서블릿 컨테이의 일부라고 보면 됩니다.<br>
   <br>
   예시: 손님의 주문을 받아서 요리사(컨트롤러)에게 전달하고, 준비된 음식을 손님에게 서빙하는 역할을 합니다.


5. **컨트롤러/서블릿: 요리사**

   역할: 요청 데이터를 처리하고 응답 데이터를 생성하는 역할을 합니다. 요리사는 주문을 받아서 요리를 준비하는 역할을 합니다.<br>
   <br>
   예시: 손님의 주문을 받아 요리를 하고, 준비된 음식을 웨이터에게 전달합니다.



+ **viewresolver** 는 요리사(컨트롤러)가 만든 요리(가공된 데이터)를 접시(view)에 담아 데코레이션 하는 사람이라고 볼 수 있습니다.(스프링 컨테이너 소속)

+ **view** 는 요리(데이터)를 담을 접시(예를들면 html파일)입니다.(스프링 컨테인너 소속)

+ **DB** 는 요리재료(데이터)들을 파는 마트라고 합시다.(외부 소속)

---


![](https://velog.velcdn.com/images/chjw147/post/546a5ff0-ad2f-45ce-857e-bcba362b0be3/image.png)


**pink square : 디스패처 서블릿 소속**<br>
**puple square : 톰캣(WAS) 소속** <br>
**yellow cirlce : 스프링 컨테이너 소속(servlet-context 기반 스프링 컨테이너)**

> **좀 더 자세히 말하자면 일반적으로** <br>
**스프링은 2개의 스프링 컨테이너(root-context 기반, servlet-context기반)를 사용하고** <br>
**스프링 부트는 2가지가 통합된 하나의 스프링 컨테이너를 사용합니다.** <br>
**root-context 기반 컨테이너 : ServiceImpl, VO, DAO** <br>
**servlet-context 기반 컨테이너 : Controller, HandlerMapping, ViewResolver 등**
<br>
**[참고](https://techblog.nbyte.kr/spring/2024/07/08/Springboot-Flow.html)**


**디스패처 서블릿은 톰캣 서버 안에 포함되어 있다고 생각하시면 됩니다.**

**핸들러 매핑, 핸들러 어댑터, 핸들러, 뷰 리졸버 등은 모두 스프링의 일부로서 스프링 컨테이너 안에서 관리되고 실행됩니다.**






---

아래 그림들을 보면 실제 개발자들이 (코드를 써)개발을 해야하는 파트와 <br>
미리 구현된 스프링부트 기능을 사용해 스프링부트가 처리하는 파트를 파악할 수 있습니다. <br>



![](https://velog.velcdn.com/images/chjw147/post/6ec3287a-a9e6-4792-949a-511b77e73130/image.png)


![](https://velog.velcdn.com/images/chjw147/post/81fc737d-2dbf-4dc2-b408-99694cb4503d/image.png)



---

참고자료 <br>
[https://velog.io/@junyoungs7/Spring-MVC](https://velog.io/@junyoungs7/Spring-MVC) <br>
[https://velog.io/@woply/spring-%EB%A9%94%EC%8B%9C%EC%A7%80-%EB%B0%94%EB%94%94%EC%9D%98-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%B2%98%EB%A6%AC%EB%A5%BC-%EB%8B%B4%EB%8B%B9%ED%95%98%EB%8A%94-HttpMessageConverter](https://velog.io/@woply/spring-%EB%A9%94%EC%8B%9C%EC%A7%80-%EB%B0%94%EB%94%94%EC%9D%98-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%B2%98%EB%A6%AC%EB%A5%BC-%EB%8B%B4%EB%8B%B9%ED%95%98%EB%8A%94-HttpMessageConverter) <br>
[인프런 김영한님의 스프링-입문-스프링부트 강의] <br>
[https://mangkyu.tistory.com/49](https://mangkyu.tistory.com/49)