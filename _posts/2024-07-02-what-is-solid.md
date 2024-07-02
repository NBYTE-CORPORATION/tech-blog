---
layout: post
title: SOLID란?
subtitle: 좋은 객체 지향 설계의 5가지 원칙(SOLID)에 대해 알아봅니다.
author: 이원모
categories: Spring
tags: [solid, oop, spring, 스프링]
---

## 개념
---
SOLID는 로버트 C. 마틴(Robert C. Martin), 흔히 "아저씨"로 알려진 소프트웨어 엔지니어에 의해 그의 책 "Agile Software Development, Principles, Patterns, and Practices"에서 처음으로 제시되었으며, 객체 지향 프로그래밍과 설계에서 중요하게 자리잡은 '좋은 객체 지향 설계의 5가지 원칙'의 약어입니다. 1990년대와 2000년대 초반에 객체 지향 설계와 소프트웨어 공학의 중요성에 대해 논의하면서 SOLID 원칙을 발전시켰으며, 소프트웨어 개발자들이 복잡한 시스템을 더 효율적으로 관리하고 유지보수할 수 있도록 돕기 위해 이러한 원칙들을 체계화했습니다. 이 원칙들은 특히 애자일 소프트웨어 개발 방법론과 잘 어울리며, 빠르게 변화하는 요구 사항에 대응하기 위한 설계 지침으로 널리 사용되고 있습니다. 이 원칙들은 코드의 유지보수성과 확장성을 높이기 위해 제안된 것들이며, 각각의 원칙은 소프트웨어 모듈이 보다 견고하고 유연하게 설계될 수 있도록 돕습니다.

<br>

## 5가지 원칙
---
좋은 객체 지향 설계의 5가지 원칙(SOLID)의 내용은 다음과 같다.

> 1. 단일 책임 원칙 (Single Responsibility Principle, SRP)
> 2. 개방-폐쇄 원칙(Open/Closed Principle, OCP)
> 3. 리스코프 치환 원칙 (Liskov Substitution Principle, LSP)
> 4. 인터페이스 분리 원칙 (Interface Segregation Principle, ISP)
> 5. 의존 역전 원칙 (Dependency Inversion Principle, DIP)

<br>
__1\. 단일 책임 원칙 (Single Responsibility Principle, SRP)__  
  단일 책임 원칙은 하나의 클래스는 하나의 책임만 가져야 한다는 원칙입니다. 즉, 클래스는 변경해야 할 이유가 하나만 있어야 합니다. 이를 통해 클래스의 응집도를 높이고, 변경으로 인한 영향을 최소화할 수 있습니다.  

  ```java  
  // SRP 적용 전
  public class UserService {
      public void registerUser(String username, String password) {
          // 사용자 등록 로직
      }

      public void sendWelcomeEmail(String email) {
          // 환영 이메일 전송 로직
      }
  }

  // SRP 적용 후
  public class UserService {
      private final EmailService emailService;

      public UserService(EmailService emailService) {
          this.emailService = emailService;
      }

      public void registerUser(String username, String password) {
          // 사용자 등록 로직
          emailService.sendWelcomeEmail(username);
      }
  }

  public class EmailService {
      public void sendWelcomeEmail(String email) {
          // 환영 이메일 전송 로직
      }
  }
  ```
<br>
__2\. 개방-폐쇄 원칙(Open/Closed Principle, OCP)__  
  개방-폐쇄 원칙은 소프트웨어 구성 요소는 확장에 대해 열려 있어야 하고, 변경에 대해서는 닫혀 있어야 한다는 원칙입니다. 즉, 기존 코드를 변경하지 않으면서 기능을 추가할 수 있어야 합니다. 이를 위해 인터페이스와 추상화를 활용하여 설계를 유연하게 합니다.

  ```java
  // OCP 적용 전
  public class DiscountService {
      public double applyDiscount(double price, String discountType) {
          if (discountType.equals("SUMMER")) {
              return price * 0.9;
          } else if (discountType.equals("WINTER")) {
              return price * 0.8;
          }
          return price;
      }
  }

  // OCP 적용 후
  public interface DiscountPolicy {
      double applyDiscount(double price);
  }

  public class SummerDiscountPolicy implements DiscountPolicy {
      public double applyDiscount(double price) {
          return price * 0.9;
      }
  }

  public class WinterDiscountPolicy implements DiscountPolicy {
      public double applyDiscount(double price) {
          return price * 0.8;
      }
  }

  public class DiscountService {
      private final DiscountPolicy discountPolicy;

      public DiscountService(DiscountPolicy discountPolicy) {
          this.discountPolicy = discountPolicy;
      }

      public double applyDiscount(double price) {
          return discountPolicy.applyDiscount(price);
      }
  }
  ```
<br>
__3\. 리스코프 치환 원칙 (Liskov Substitution Principle, LSP)__  
  리스코프 치환 원칙은 서브타입은 언제나 기반 타입으로 교체할 수 있어야 한다는 원칙입니다. 이는 상속 구조에서 자식 클래스가 부모 클래스의 행위를 온전히 대체할 수 있도록 해야 함을 의미합니다. 이를 통해 다형성을 제대로 활용할 수 있습니다.

  ```java
  // LSP 적용 전
  import org.springframework.stereotype.Service;

  @Service
  public class BirdService {
      public void moveBird(Bird bird) {
          bird.fly();
      }
  }

  public class Bird {
      public void fly() {
          System.out.println("Bird is flying");
      }
  }

  public class Ostrich extends Bird {
      @Override
      public void fly() {
          throw new UnsupportedOperationException("Ostriches can't fly");
      }
  }

  // LSP 적용 후
  import org.springframework.stereotype.Service;

  @Service
  public class BirdService {
      public void moveBird(Bird bird) {
          bird.move();
      }
  }

  public abstract class Bird {
      public abstract void move();
  }

  public class FlyingBird extends Bird {
      @Override
      public void move() {
          fly();
      }

      public void fly() {
          System.out.println("Bird is flying");
      }
  }

  public class Ostrich extends Bird {
      @Override
      public void move() {
          run();
      }

      public void run() {
          System.out.println("Ostrich is running");
      }
  }
  ```
<br>
__4\. 인터페이스 분리 원칙 (Interface Segregation Principle, ISP)__  
  인터페이스 분리 원칙은 클라이언트가 자신이 사용하지 않는 메서드에 의존하지 않도록 인터페이스를 분리해야 한다는 원칙입니다. 이는 하나의 큰 인터페이스보다 여러 개의 작은 인터페이스를 사용하는 것이 더 좋다는 의미입니다.

  ```java
  // ISP 적용 후
  public interface Worker {
      void work();
      void eat();
  }

  // ISP 적용 전
  public interface Workable {
      void work();
  }

  public interface Eatable {
      void eat();
  }

  public class WorkerImpl implements Workable, Eatable {
      public void work() {
          // 작업 로직
      }

      public void eat() {
          // 식사 로직
      }
  }
  ```
<br>
__5\. 의존 역전 원칙 (Dependency Inversion Principle, DIP)__  
  의존 역전 원칙은 고수준 모듈이 저수준 모듈에 의존해서는 안 되며, 둘 다 추상화된 인터페이스에 의존해야 한다는 원칙입니다. 이를 통해 모듈 간의 결합도를 낮추고, 유연성과 재사용성을 높일 수 있습니다.

  ```java
  // DIP 적용 전 
  public class Light {
      public void turnOn() {
          // 전등 켜기 로직
      }
  }

  public class Switch {
      private final Light light;

      public Switch(Light light) {
          this.light = light;
      }

      public void operate() {
          light.turnOn();
      }
  }

  // DIP 적용 후
  public interface Switchable {
      void turnOn();
  }

  public class Light implements Switchable {
      public void turnOn() {
          // 전등 켜기 로직
      }
  }

  public class Switch {
      private final Switchable switchable;

      public Switch(Switchable switchable) {
          this.switchable = switchable;
      }

      public void operate() {
          switchable.turnOn();
      }
  }
  ```

<br>

## 목적
---
SOLID 원칙은 다음과 같은 목표를 달성하기 위해 만들어졌습니다.

1. 유지보수성 향상  
  코드를 쉽게 수정하고 개선할 수 있도록 설계함으로써, 장기적으로 시스템을 유지보수하는 데 소요되는 비용과 시간을 절감합니다.

2. 확장성 증대  
  새로운 기능을 추가할 때 기존 코드를 수정할 필요가 없도록 설계하여, 시스템의 확장을 용이하게 합니다.

3. 유연성 증가  
  시스템이 변화하는 요구 사항에 적응할 수 있도록 유연성을 높입니다. 이를 통해 코드 재사용성을 극대화할 수 있습니다.

4. 의존성 최소화  
  모듈 간의 의존성을 최소화하여, 하나의 모듈 변경이 다른 모듈에 미치는 영향을 줄입니다.

5. 가독성 및 이해도 향상  
  명확하고 일관된 설계 원칙을 따름으로써, 코드를 더 쉽게 이해하고 읽을 수 있게 합니다.