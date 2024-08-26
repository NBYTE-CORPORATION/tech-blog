---
layout: post
title: Spring-Data-JPA-Prefix
subtitle: Spring Data JPA에서 사용하는 접두사들에 대해 알아봅시다.
author: 차정우
categories: Spring
tags: [Spring, Springboot, JPA, Sprig-Data-JPA, 접두사, Prefix, SQL]
---



## Spring Data JPA Prefix

Spring Data JPA는 기본적인 CRUD 작업을 자동으로 지원하는 메서드를 제공합니다. 하지만 이 기본 메서드들만으로는 다양한 데이터베이스 쿼리 요구사항을 모두 충족할 수 없기 때문에, 개발자가 특정 조건에 맞는 쿼리를 쉽게 작성할 수 있도록 접두사(prefix)를 사용한 쿼리 메서드를 지원합니다.

예를 들어, findBy, countBy, existsBy 같은 접두사를 사용해 메서드를 정의하면, Spring Data JPA가 메서드 이름을 분석해 자동으로 적절한 SQL 쿼리를 생성합니다. 이렇게 하면 복잡한 SQL 쿼리를 직접 작성하지 않고도, 다양한 조건에 맞는 데이터를 손쉽게 조회하거나 조작할 수 있습니다

---

> ### 주의!
마치 일반 메서드(함수)의 호출과 유사하게 보여, 착각할 수 있지만 아래 예시 코드들은
호출이 아닌 선언부 입니다.
보통 repository interface를 만들 때, { }에 해당 코드들을 넣어 사용합니다.

```java
package com.example.demo.repository;

import com.example.demo.entity.Exercise;
import org.springframework.data.jpa.repository.JpaRepository;

public interface ExerciseRepository extends JpaRepository<Exercise, String> {
	Exercise findByExerciseName(String exerciseName);  // 이 부분
}
```

---

 ### `findBy`

- 설명: 주어진 조건에 맞는 엔터티(데이터)를 `조회`합니다.
- 사용법: 메서드 이름에 엔터티의 필드명을 추가하여 조건을 지정합니다.
- 예시: 아래 메서드는 `name` 필드가 주어진 `name`과 일치하는 모든 `User` 엔터티를 반환합니다.

```java
List <User> findByName(String name);
```

>옵션 (사용가능한 파라미터)
<br>
- `Sort` 객체
- 사용법: `Sort` 객체를 선언 및 초기화 하고, `findBy`를 사용할 때 파라미터로 함께 작성합니다.
- 예시: 아래는 각각 오름차순과 내림차순으로 정렬한 데이터를 조회 하는 방법입니다.
```java
Sort sort = Sort.by("name").ascending();
List<User> users = userRepository.findByAgeGreaterThan(18, sort);
```
```java
Sort sort = Sort.by("name").ascending().and(Sort.by("age").descending());
List<User> users = userRepository.findByAgeGreaterThan(18, sort);
```
<br>
>
- `orderBy`
```java
List<User> findByAgeGreaterThanOrderByNameAsc(int age);
```



### `findDistinctBy`

- 설명: 조건에 맞는 고유한(중복되지 않는) `레코드`를 반환합니다.
- 사용법: 메서드 이름에 필드명을 추가하고, 결과를 `고유하게` 반환하도록 설정합니다.
- 예시: 아래 메서드는 `name` 필드 값이 중복되지 않는 `User` 엔터티 목록을 반환합니다.
```java
List<User> findDistinctByName(String name);
```



---

### `getBy`

- 설명: `findBy`와 유사하게 데이터를 `조회`합니다. 다만, `getBy`는 `단수 값`을 기대하는 상황에서 주로 사용됩니다.
- 사용법: 메서드 이름에 엔터티의 필드명을 구하여 조건을 지정합니다.
- 예시: 아래 메서드는 `email` 필드가 주어진 `email`과 일치하는 `User` 엔터티를 반환합니다.

```java
User getByEmail(String email);
```

---

### `countBy`

- 설명: 조건에 맞는 레코드의 `개수`를 반환합니다.
- 사용법: 메서드의 이름에 엔터티의 필드명을 추가하여 조건을 지정하고, 반환 타입은 `long`이어야 합니다.
- 예시: 아래 메서드는 `age` 필드가 주어진 `age` 보다 큰 모든 `User` 엔터티의 개수를 반환합니다.

```java
long countByAgeGraterThan(int age);
```

### `countDistinctBy`

- 설명: 특정 조건에 대해 고유한 레코드의 `개수`를 반환합니다.
- 사용법: 메서드 이름에 필드명을 추가하고, `고유한 레코드의 개수`를 반환하도록 설정합니다.
- 예시: 아래 메서드는 `status` 필드 값이 고유한 `User` 엔터티의 개수를 반환합니다.
```java
long countDistinctByStatus(String status);
```


---

### `existsBy`

- 설명: 조건에 맞는 레코드가 `존재하는지 여부`를 반환합니다. 반환 타입은 `boolean`입니다.
- 사용법: 메서드 이름에 엔터티의 필드명을 추가하여 조건을 지정합니다.
- 예시: 아래 메서드는 `email` 필드가 주어진 `email` 과 일치하는 `User` 엔터티가 존재하는지 여부를 반환합니다.
```java
boolean existByEmial(String email);
```

---

### `deleteBy`  (`removeBy` 동일)

- 설명: 조건에 맞는 레코드를 `삭제`합니다.
- 사용법: 메서드 이름에 엔터티의 필드명을 추가하여 조건을 지정합니다. 삭제된 레코드의 개수를 반환하거나, 반환 타입을 void로 지정할 수 있습니다.
- 예시: 아래 메서드는 주어진 `id`에 해당하는 `User`를 삭제하거나, 주어진 `status`와 일치하는 모든 `User` 엔터티를 삭제합니다.

```java
void deleteById(Long id);
long deleteByStatus(String status);
```

---

## 조합 및 추가 키워드


- 설명: 하나의 엔터티가 아닌 `두 가지 이상의 엔터티`에 대해서 `조건`을 걸어 사용할 수 있습니다.
- 사용법: 접두사뒤 또는 접두사 뒤의 엔터티 사이나 뒤에 붙여 사용할 수 있습니다.
- 예시:


### `AND`

```java
List<User> findByNameAndStatus(String name, String status);
```

### `OR`

```java
List<User> findByNameOrEmail(String name, String email);
```

### `Between`

```java
List<User> findByAgeBetween(int startAge, int endAge);
```

### `LessThan/GreaterThan`

```java
List<User> findByAgeLessThan(int age);
List<User> findByAgeGreaterThan(int age);
```

### `Like`

```java

List<User> findByNameLike(String namePattern);
```



---
