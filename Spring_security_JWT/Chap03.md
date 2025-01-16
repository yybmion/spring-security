# Spring Security Configuration 설정 가이드

## SecurityConfig 클래스 구현

### 1. 기본 구성
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
    // Security 설정을 구성하는 클래스
}
```

### 2. SecurityFilterChain 설정
```java
@Bean
public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
    // Security Filter 설정
    return http.build();
}
```

## 주요 보안 설정

### 1. CSRF 보호 비활성화
```java
http.csrf((auth) -> auth.disable());
```
- JWT는 Stateless 방식으로 동작하므로 CSRF 보호가 불필요

### 2. 인증 방식 설정
```java
// Form 로그인 비활성화
http.formLogin((auth) -> auth.disable());

// HTTP Basic 인증 비활성화
http.httpBasic((auth) -> auth.disable());
```

### 3. URL 별 접근 권한 설정
```java
http.authorizeHttpRequests((auth) -> auth
    .requestMatchers("/login", "/", "/join").permitAll() // 모든 사용자 접근 가능
    .requestMatchers("/admin").hasRole("ADMIN")          // ADMIN 롤을 가진 사용자만 접근 가능
    .anyRequest().authenticated());                      // 그 외 요청은 인증 필요
```

### 4. 세션 관리 설정
```java
http.sessionManagement((session) -> session
    .sessionCreationPolicy(SessionCreationPolicy.STATELESS));
```
- JWT 사용을 위한 Stateless 세션 정책 설정
- 서버에 세션을 저장하지 않음

### 5. 비밀번호 인코더 설정
```java
@Bean
public BCryptPasswordEncoder bCryptPasswordEncoder() {
    return new BCryptPasswordEncoder();
}
```
- 비밀번호 암호화를 위한 BCrypt 인코더 빈 등록

## 주의사항
1. Spring Security 6.2.1 버전 기준 구현
2. Stateless 세션 정책은 JWT 구현의 핵심
3. 추가 필터 구현 시 SecurityFilterChain에 추가 필요
4. URL 패턴 및 권한 설정은 프로젝트 요구사항에 맞게 조정

## 다음 단계
1. JWT 관련 필터 구현
2. 인증/인가 처리 로직 구현
3. 사용자 정보 관리 서비스 구현