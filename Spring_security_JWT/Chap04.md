# Spring Security JWT - 데이터베이스 설정 및 사용자 엔티티 구현 가이드

## 1. 데이터베이스 연결 설정

### application.properties 기본 설정
```properties
# MySQL 연결 설정
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://[DB_HOST]:3306/[DB_NAME]?useSSL=false&useUnicode=true&serverTimezone=Asia/Seoul&allowPublicKeyRetrieval=true
spring.datasource.username=[USERNAME]
spring.datasource.password=[PASSWORD]

# Hibernate 설정
spring.jpa.hibernate.ddl-auto=none
spring.jpa.hibernate.naming.physical-strategy=org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
```

### ddl-auto 옵션 설명
- `none`: 테이블 자동 생성 기능을 사용하지 않음 (기본값)
- `create`: 시작 시 테이블을 삭제하고 새로 생성
- `update`: 변경된 스키마만 반영
- **주의사항**: `create` 사용 시 기존 데이터가 삭제될 수 있으므로 주의

## 2. 사용자 엔티티 구현

### UserEntity.java
```java
@Entity
@Getter
@Setter
public class UserEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;

    private String username;
    private String password;
    private String role;
}
```

### 주요 구성 요소
- `@Entity`: JPA 엔티티 클래스 지정
- `@Id`: 기본키 지정
- `@GeneratedValue`: 기본키 생성 전략 설정
- 필수 필드:
    - username: 사용자 식별자
    - password: 암호화된 비밀번호
    - role: 사용자 권한 정보

## 3. Repository 구현

### UserRepository.java
```java
public interface UserRepository extends JpaRepository<UserEntity, Integer> {
}
```

### 특징
- `JpaRepository` 상속으로 기본 CRUD 작업 자동 구현
- 제네릭 파라미터:
    - `UserEntity`: 대상 엔티티 클래스
    - `Integer`: 기본키 타입

## 4. 프로젝트 구조
```
src/main/java
├── entity
│   └── UserEntity.java
├── repository
│   └── UserRepository.java
└── resources
    └── application.properties
```

## 5. 구현 순서
1. 데이터베이스 의존성 추가 (JPA, MySQL)
2. application.properties에 데이터베이스 연결 정보 설정
3. UserEntity 클래스 구현
4. UserRepository 인터페이스 생성
5. ddl-auto 설정으로 테이블 생성
6. 테이블 생성 후 ddl-auto를 none으로 변경

## 주의사항
1. 데이터베이스 접속 정보 보안 관리
2. ddl-auto 설정 값 관리 (운영 환경에서는 none 권장)
3. 엔티티 설계 시 필요한 필드 신중히 고려
4. 패스워드 암호화 처리 필요