# Spring Security JWT 프로젝트 셋업 가이드

## 1. 프로젝트 기본 설정

### 프로젝트 생성 환경
- Spring Boot 3.2.1
- JDK 18 (Java 17 설정)
- Gradle (빌드 도구)

### 기본 의존성
```groovy
dependencies {
    // Spring 기본 의존성
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-security'
    implementation 'org.projectlombok:lombok'
    
    // 데이터베이스 의존성 (초기 설정시 주석 처리)
    // implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    // runtimeOnly 'com.mysql:mysql-connector-j'
    
    // JWT 의존성 (0.12.3 버전)
    implementation 'io.jsonwebtoken:jjwt-api:0.12.3'
    implementation 'io.jsonwebtoken:jjwt-impl:0.12.3'
    implementation 'io.jsonwebtoken:jjwt-jackson:0.12.3'
}
```

## 2. 컨트롤러 구현

### MainController.java
```java
@Controller
@ResponseBody
public class MainController {
    @GetMapping("/")
    public String mainP() {
        return "main Controller";
    }
}
```

### AdminController.java
```java
@Controller
@ResponseBody
public class AdminController {
    @GetMapping("/admin")
    public String adminP() {
        return "admin Controller";
    }
}
```

## 3. 주요 구현 사항
1. **API 서버 구성**
    - `@ResponseBody` 어노테이션을 사용하여 View가 아닌 데이터 응답
    - REST API 형태로 구현

2. **JWT 버전 선택**
    - 최신 버전(0.12.3) 사용
    - 기존 버전(0.11.5)과 메소드 구현 방식이 상이함

3. **데이터베이스 설정**
    - 초기 설정시 데이터베이스 의존성은 주석 처리
    - 추후 강의 진행시 설정 추가 예정

## 4. 프로젝트 구조
```
src/main/java
├── controller
│   ├── MainController.java
│   └── AdminController.java
└── Application.java
```

## 다음 단계
- Spring Security Configuration 구현
- JWT 인증/인가 로직 구현
- 데이터베이스 연동 설정

## 참고사항
- 초기 실행 시 Spring Security 기본 로그인 페이지가 표시됨
- 데이터베이스 연동은 추후 강의에서 진행 예정
- JWT 구현은 0.12.3 버전 기준으로 진행되며, 0.11.5 버전과의 차이점도 추후 다룰 예정