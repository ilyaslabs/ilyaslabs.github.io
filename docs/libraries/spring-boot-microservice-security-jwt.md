---
title: Spring Boot Microservice Security with JWT
description: A simplified, practical guide to securing Spring Boot APIs with JWT using the spring-boot-microservice-security-jwt library.
---

# Spring Boot JWT Security (Simple + Production-Friendly)

Want secure APIs without spending hours on boilerplate? This starter gives you JWT auth with RSA keys, refresh tokens, and scope-based authorization out of the box.

## What you get

- JWT access token generation (`Jwt` object)
- Refresh token generation (`Jwt` object)
- Automatic JWT validation in secured endpoints
- Scope/authority-based endpoint protection
- Custom claims support
- Pre-configured Spring Security defaults for JWT APIs

---

## Quick Start (5 Steps)

### 1) Add dependency

```xml
<dependency>
  <groupId>io.github.ilyaslabs</groupId>
  <artifactId>spring-boot-microservice-security-jwt</artifactId>
  <version>1.0.0</version>
</dependency>
```

> Tip: check repository releases/tags and use the latest available version.

### 2) Generate RSA key pair

Run these commands in `src/main/resources`:

```bash
openssl genrsa -out keypair.pem 2048
openssl rsa -in keypair.pem -pubout -out publickey.pem
openssl rsa -in keypair.pem -out privatekey.pem
```

### 3) Configure keys in `application.yml`

```yml
io:
  github:
    ilyaslabs:
      microservice:
        security:
          jwt:
            rsa:
              rsa-private-key: classpath:privatekey.pem
              rsa-public-key: classpath:publickey.pem
```

> You can also inline PEM key text in YAML, but classpath files are cleaner for most projects.

### 4) Generate tokens in your auth endpoint

Use `JwtTokenService` (updated API):

```java
@Autowired
private JwtTokenService jwtTokenService;

Jwt accessJwt = jwtTokenService.generateToken(
    "user",                      // subject
    "https://your-domain.com",  // issuer
    Map.of("tenant", "acme"),  // custom claims (optional)
    List.of("ADMIN")             // scopes
);

String accessToken = accessJwt.getTokenValue();
```

Generate refresh token:

```java
Jwt refreshJwt = jwtTokenService.generateRefreshToken(
    "user",
    "https://your-domain.com",
    null,
    null
);

String refreshToken = refreshJwt.getTokenValue();
```

### 5) Call secured APIs

Send access token as Bearer:

```http
Authorization: Bearer <your-access-token>
```

That’s it—JWT validation is handled automatically for protected routes.

---

## Common Configurations

### Change token expiry

Default access token validity is 1 hour. You can override both access and refresh token durations:

```yml
io:
  github:
    ilyaslabs:
      microservice:
        security:
          jwt:
            expiryUnit: HOURS        # MINUTES, HOURS, DAYS, WEEKS, etc.
            expiry: 1
            refreshExpiryUnit: DAYS
            refreshExpiry: 30
```

### Read JWT claims from current request

```java
jwtTokenService.getAuthenticatedPrincipal();
```

Returns `org.springframework.security.oauth2.jwt.Jwt` (token + claims).

---

## Scope-Based Authorization

A default `SecurityFilterChain` is already configured. You can override it when needed:

```java
@Bean
public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
    http.authorizeHttpRequests(customizer -> customizer
            .requestMatchers("/api/auth").permitAll()
            .requestMatchers("/api/data").hasAuthority("SCOPE_ADMIN")
            .requestMatchers("/api/**").authenticated()
            .anyRequest().permitAll());

    return http.build();
}
```

Important:

- While generating token, scopes are plain values (example: `ADMIN`).
- In Spring Security checks, use `SCOPE_` prefix (example: `SCOPE_ADMIN`).
- Refresh tokens include `REFRESH_TOKEN` scope by default, useful for refresh-only endpoints.

---

## Built-in Security Defaults

The injected `HttpSecurity` is preconfigured to:

1. Use JWT authentication
2. Disable CSRF
3. Disable form login
4. Disable HTTP Basic
5. Return `401 Unauthorized` for unauthorized access

So you can focus on business endpoints instead of repetitive security setup.

---

## Related

- This library already includes `spring-boot-microservice` dependency.
- Base framework docs: [spring-boot-microservice](https://github.com/ilyasdotdev/spring-boot-microservice)
