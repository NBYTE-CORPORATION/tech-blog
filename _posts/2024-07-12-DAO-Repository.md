---
layout: post
title: DAO & Repository
subtitle: 스프링 생태계에서 대표적인 데이터액세스 디자인패턴을 비교해봅시다.
author: 차정우
categories: Spring
tags: [Spring, Springboot, Repository, DAO]
---

## DAO 와 Repository 비교를 위한 기초지식

**ORM(Object Relational Mapping)**<br>
 - 관계형 데이터베이스와 객체 지향 프로그래밍 언어 간의 호환되지 않는 데이터를 자동으로 매핑 (연결)해주는 프로그래밍 기법(방법)을 말합니다.

**JPA(Java Persistence API)**<br>
 - 자바 표준 사양으로, 자바 객체를 관계형 데이터베이스에 매핑하는 방법을 정의합니다. JPA는 Java EE (Enterprise Edition) 사양의 일부로서, 애플리케이션 개발자가 데이터베이스와 상호 작용하는 표준 방법을 제공합니다. 
 - JPA는 데이터베이스 작업을 위한 API를 정의하지만, 실제 구현체를 제공하지는 않습니다. 따라서 JPA를 사용하려면, JPA 사양을 구현한 라이브러리를 사용해야 합니다.
 - Hibernate, Spring JPA, EclipseLink등과 같은 구현체가 있는데 이들의 표준 인터페이스가 JPA입니다.
 - 스프링의 PSA에 의해서(POJO를 사용하면서 특정 기술을 사용하기 위해서)표준 인터페이스를 정해두었는데, 그중 ORM을 사용하기 위해 만든 인터페이스가 바로 JPA입니다.

**Hibernate**<br>
- ORM 중에서 java를 위한 프레임워크 입니다.
- JPA가 정의한 API를 실제로 구현한 구현체입니다.

**JDBC(Java Database Connectivity)**<br>
 - Java 기반 애플리케이션의 데이터를 데이터베이스에 저장 및 업데이트하거나, 데이터베이스에 저장된 데이터를 Java에서 사용할 수 있도록 하는 자바 API이다.


**Domain(=Entity =Model)**<br>
 - DB에서 자바애플리케이션 층으로 테이블 형태의 데이터를 끌어와 객체(자바애플리케이션에서 사용가능한 형태)로 만든 것입니다. 즉 데이터베이스에 존재하는 테이블과 동일한 내용을 가지고 있는 원본 데이터라고 생각할 수 있습니다.

**DTO(Data Transfer Object) = VO(Value Object)**<br>
 - 클라이언트(프론트엔드) 층이 요구하는 데이터 형식에 맞게끔 Domain을 가공시켜 (또는 그대로) 실제 응답으로 전달할 객체. 물론 프론트에서 데이터를 백으로 보낼 때도 사용합니다. 그래서 일반적인 메서드는 없고 필드속성(멤버변수)과 getter 와 setter만 가지고 있습니다.
 - DTO 와 VO를 혼용해서 많이 쓰지만 정확히 따지면 VO는 readonly 속성이라고 생각하시면 됩니다.
 
<br>
<br>

---

<br>

**Repository**<br>
 - **데이터베이스의 테이블보다는 자바애플리케이션 상 객체(Domain)에 집중합니다.이 객체의 수명등 상태관리도 하고 이 객체 기반으로 데이터베이스의 테이블을 동기화 시킨다고 생각하시면 됩니다**
 - Java 기반 애플리케이션과 데이터베이스를 properties 나 yaml 파일 같은 설정파일을 통해 연결합니다.(이때 JPA의 구현체인 Hibernate를 이용합니다)
 - **Hibernate라는 프레임워크를 사용하기 때문에 sql 쿼리를 직접 만들지 않고 이미 만들어져 있는 데이터액세스함수(CRUD 쿼리 같은..)를 사용할 수 있습니다.(실사용시에는 Hibernate를 좀 더 편하게 쓸 수 있게끔 하는 Spring Data JPA 의존성을 추가해서 사용합니다!)**
 
 
**DAO(Data Access Object)**<br>
 - Repository와 마찬가지로 데이터베이스의 데이터에 접근하기 위해 생성하는 객체입니다.
 - **주 목적은 비즈니스로직과 데이터액세스로직을 분리 시키는 것입니다.**
 - **자바애플리케이션 상 객체(Domain)보다 데이터베이스의 테이블에 집중한다고 볼 수 있습니다.(이 테이블을 조작하고 Domain이 변화를 감지해서 동기화된다고 보시면 될 것 같습니다)**
 - Repository와 달리 클래스에서 소스코드로 DB와 연결할 수 있습니다.
 - **JPA를 구현한 프레임워크를 사용하지 않기 때문에 직접 데이터액세스함수를 짜야합니다.**


 >
## 요약<br>
+ **JPA 와 Hibernate: JPA** 표준을 구현한 구현체가 Hibernate(프레임워크)입니다.
+ **DAO 와 Repository**: 모두 데이터 액세스 패턴입니다만,
  1. DAO는 데이터베이스에 접근하는 로직과 비즈니스 로직을 완전히 분리 시키는걸 목표로 하는데 Repository는 객체에 초점을 두고 집중하여 객체 생성주기등도 관리하며 비즈니스 로직도 함께 다룹니다.
  2. 자바 애플리케이션과 데이터베이스를 연결하는 방식이 다릅니다.
  3. Repository는 Hibernate(프레임워크) 기반이기 때문에 많은 표준화된 메서드   와 기능을 사용할 수 있습니다.(예를 들면 만들어져 있는 CRUD sql 쿼리등)
+ **DTO 와 Domain**: 데이터베이스의 테이블과 동일한 내용을 객체(Domain)로 만들어  따로 보관을 하면서, 프론트와 백에서 데이터를 주고 받을 때 이 Domain을 그대로 보내도 되지만 적절한 형태에 맞게끔 가공해서 주고 받을 수 있는데 이렇게 가공한   것을 DTO라고 생각하시면 됩니다.

<br>
<br>

---

<br>
<br>


![](https://velog.velcdn.com/images/chjw147/post/487b42d1-852d-4220-a097-297895eedf93/image.png)

<br>


억지로(?) JPA를 이용해 연결하고 DAO를 사용하거나
SQLMapper를 이용해 연결하고 Repository를 사용할 수도 있지만
구조를 빨간색 원 이나 파란색 원의 흐름으로 설계하는게 좋습니다.

<br>

>
**사실 JPA 구현체를 이용해서 데이터베이스의 데이터에 액세스한다면 클래스 이름을 DAO 라고 하더라도 알맹이는 Repository 라고 생각해서 Repository 디자인 패턴이지 않나 싶습니다.
반대로
SQLMapper(예를들어 Mybatis)를 이용해서 데이터에 액세스한다면 Repository라고 이름을 짓더라도 DAO 디자인패턴 이라고 봅니다.**

<br>
<br>

---

<br>

## DAO (Data Access Object) 패턴
클래스 내부에서 연결 했을 경우

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

// DAO Interface
public interface UserDAO {
    void addUser(User user);
    User getUserById(int id);
    List<User> getAllUsers();
    void updateUser(User user);
    void deleteUser(int id);
}

// DAO Implementation
public class UserDAOImpl implements UserDAO {
    private Connection connection;

    public UserDAOImpl() {
        try {
            // Database connection setup
            String url = "jdbc:mysql://localhost:3306/mydatabase";
            String username = "root";
            String password = "password";
            this.connection = DriverManager.getConnection(url, username, password);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void addUser(User user) {
        try {
            String query = "INSERT INTO users (name, email) VALUES (?, ?)";
            PreparedStatement preparedStatement = connection.prepareStatement(query);
            preparedStatement.setString(1, user.getName());
            preparedStatement.setString(2, user.getEmail());
            preparedStatement.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    @Override
    public User getUserById(int id) {
        User user = null;
        try {
            String query = "SELECT * FROM users WHERE id = ?";
            PreparedStatement preparedStatement = connection.prepareStatement(query);
            preparedStatement.setInt(1, id);
            ResultSet resultSet = preparedStatement.executeQuery();
            if (resultSet.next()) {
                user = new User(resultSet.getInt("id"), resultSet.getString("name"), resultSet.getString("email"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return user;
    }

    @Override
    public List<User> getAllUsers() {
        List<User> users = new ArrayList<>();
        try {
            String query = "SELECT * FROM users";
            PreparedStatement preparedStatement = connection.prepareStatement(query);
            ResultSet resultSet = preparedStatement.executeQuery();
            while (resultSet.next()) {
                users.add(new User(resultSet.getInt("id"), resultSet.getString("name"), resultSet.getString("email")));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return users;
    }

    @Override
    public void updateUser(User user) {
        try {
            String query = "UPDATE users SET name = ?, email = ? WHERE id = ?";
            PreparedStatement preparedStatement = connection.prepareStatement(query);
            preparedStatement.setString(1, user.getName());
            preparedStatement.setString(2, user.getEmail());
            preparedStatement.setInt(3, user.getId());
            preparedStatement.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void deleteUser(int id) {
        try {
            String query = "DELETE FROM users WHERE id = ?";
            PreparedStatement preparedStatement = connection.prepareStatement(query);
            preparedStatement.setInt(1, id);
            preparedStatement.executeUpdate();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```
<br>
<br>

---

<br>

## Repository 패턴

Repository 패턴에서는 데이터베이스 연결이 주로 외부 설정 파일에 의해 관리됩니다. 여기서는 Spring Data JPA와 같은 프레임워크를 사용하는 예시를 들어보겠습니다.

application.properties 또는 application.yml 파일을 사용할 수 있습니다.
application.properties파일을 사용한 경우
```
spring.datasource.url=jdbc:mysql://localhost:3306/mydatabase
spring.datasource.username=root
spring.datasource.password=password
spring.jpa.hibernate.ddl-auto=update
```

```java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.List;

// User Entity
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;
    private String name;
    private String email;

    // getters and setters
}

// User Repository Interface
@Repository
public interface UserRepository extends JpaRepository<User, Integer> {
    // CRUD operations are provided by JpaRepository
}

// Usage in Service or Controller
@Service
public class UserService {
    @Autowired
    private UserRepository userRepository;

    public void addUser(User user) {
        userRepository.save(user);
    }

    public User getUserById(int id) {
        return userRepository.findById(id).orElse(null);
    }

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    public void updateUser(User user) {
        userRepository.save(user);
    }

    public void deleteUser(int id) {
        userRepository.deleteById(id);
    }
}

```