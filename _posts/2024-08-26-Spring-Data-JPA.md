---
layout: post
title: Spring-Data-JPA
subtitle: JPA를 좀 더 편하게 사용할 수 있게 해주는 Spring Data JPA에 대해 알아봅시다.
author: 차정우
categories: Spring
tags: [Spring, Springboot, JPA, Sprig-Data-JPA, SQL]
---

## Spring Data JPA

Spring Data JPA는 JPA(Java Persistence API)를 사용하여 데이터베이스와 상호작용하기 위한 강력한 도구입니다. 이 프레임워크는 개발자가 데이터베이스 작업을 쉽게 수행할 수 있도록 다양한 기본 제공 메서드와 자동 쿼리 생성 기능을 제공합니다.

---

## 기본 제공 메서드

Spring Data JPA는 다양한 CRUD(Create, Read, Update, Delete) 작업을 처리하기 위해 여러 기본 메서드를 제공합니다. 이 메서드들은 대부분의 엔티티 관련 작업을 간단하게 처리할 수 있도록 설계되었습니다.

### 저장/업데이트 관련 메서드
- **`save(S entity)`**: 단일 엔티티를 저장하거나 업데이트합니다.
- **`saveAll(Iterable<S> entities)`**: 여러 엔티티를 저장하거나 업데이트합니다.

### 조회 관련 메서드
- **`findById(ID id)`**: 주어진 ID로 엔티티를 조회합니다.
- **`findAll()`**: 모든 엔티티를 조회합니다.
- **`count()`**: 저장된 엔티티의 총 개수를 반환합니다.

### 삭제 관련 메서드
- **`deleteById(ID id)`**: 주어진 ID를 가진 엔티티를 삭제합니다.
- **`deleteAll()`**: 모든 엔티티를 삭제합니다.

이 메서드들은 스프링 데이터 JPA에서 자동으로 구현되어 있으며, 간단한 CRUD 작업을 위해 사용됩니다.

---

## 쿼리 메서드

기본 제공 메서드에서 제공하지 않는 필요한 메서드를 직접 만들어 사용하기 위해 사용되며,
Spring Data JPA는 메서드 이름(접두사: Spring Data JPA Prefix)을 기반으로 쿼리를 자동 생성하는 기능을 제공합니다. 이러한 메서드들을 "쿼리 메서드"라고 하며, 다양한 형태로 사용될 수 있습니다. 이들은 메서드 이름만으로도 원하는 데이터를 쉽게 조회할 수 있게 해줍니다.


### 주요 쿼리 메서드 종류

- **`findBy`**: 특정 필드 값을 기반으로 데이터를 조회합니다. 가장 많이 사용되는 쿼리 메서드입니다.
  - 예시: `findByUsername(String username)` - `username` 필드 값을 기준으로 데이터를 조회합니다.

- **`countBy`**: 특정 필드 값을 기준으로 데이터의 개수를 반환합니다.
  - 예시: `countByActive(boolean active)` - `active` 필드 값을 기준으로 활성 상태의 엔티티 수를 반환합니다.

- **`existsBy`**: 특정 조건에 해당하는 데이터가 존재하는지 여부를 확인합니다.
  - 예시: `existsByEmail(String email)` - `email` 필드 값을 기준으로 데이터가 존재하는지 확인합니다.

- **`deleteBy`**: 특정 필드 값을 기준으로 데이터를 삭제합니다.
  - 예시: `deleteByExpired(boolean expired)` - `expired` 필드 값을 기준으로 데이터를 삭제합니다.

이 외에도 다양한 쿼리 메서드들이 있으며, 필요에 따라 메서드 이름을 조합하여 사용자가 원하는 쿼리를 쉽게 작성할 수 있습니다.

### 왜 추가로 메서드를 정의하는가?

기본적으로 제공되는 메서드는 ID 기반 조회나 단순한 CRUD 작업을 처리할 수 있습니다. 그러나 특정 필드나 복잡한 조건을 기반으로 데이터를 조회하려면 위에서 설명한 쿼리 메서드를 추가로 정의해야 합니다.

#### 장점
- **간결함**: 복잡한 JPQL 쿼리를 작성하지 않고, 메서드 이름만으로 원하는 쿼리를 생성할 수 있습니다.
- **가독성**: 코드가 명확하고 직관적이어서 다른 개발자들이 코드를 쉽게 이해할 수 있습니다.
- **재사용성**: 특정 조건에 따라 자주 사용되는 조회 로직을 재사용할 수 있습니다.

#### 예시

```java
public interface MuscleRepository extends JpaRepository<Muscle, Long> {
    Muscle findByMuscleName(String muscleName);
    long countByType(String type);
    boolean existsByMuscleName(String muscleName);
    void deleteByIsInactive(boolean isInactive);
}
```