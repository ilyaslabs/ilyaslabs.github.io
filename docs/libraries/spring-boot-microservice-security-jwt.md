---
title: Spring Boot Microservice Security with JWT
description: A guide to implementing security in Spring Boot microservices using JWT (JSON Web Tokens).
---


# Getting Started

A library to quickly set up JWT authentication in a Spring Boot application.
This library provides a simple way to generate and validate JWT tokens using RSA keys.

## Features
- Generate JWT tokens with RSA keys
- Generate Refresh tokens with RSA keys
- Validate JWT tokens
- Restrict access to endpoints based on scopes
- Customizable JWT claims
- Basic authentication

## How to use

1. Create a spring boot application using [start.spring.io](https://start.spring.io/)
2. Add the dependency in your project.
3. 
    ```xml
    <dependency>
        <groupId>io.github.ilyaslabs</groupId>
        <artifactId>spring-boot-microservice-security-jwt</artifactId>
        <version>1.0.0</version>
    </dependency>
    ```

4. Execute below scripts in `src/main/resources` directory to create public and private keys.
    ```bash
    openssl genrsa -out keypair.pem 2048
    openssl rsa -in keypair.pem -pubout -out publickey.pem
    openssl rsa -in keypair.pem -out privatekey.pem
    ```
   
5. Add the keys to your `application.yml` file.
    ```yml
   io:
      github:
        ilyaslabs:
          microservice:
            security:
              jwt:
                rsa:
                  rsa-private-key: classpath:privateKey.pem
                  rsa-public-key: classpath:publicKey.pem
    ```

    - Keys can be generated anywhere.

    Alternatively, you can directly add the keys in the `application.yml` file as below.

    ```yml
    io:
      github:
        ilyaslabs:
          microservice:
            security:
              jwt:
                rsa:
                  private-key: |
                    -----BEGIN PRIVATE KEY-----
                    MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQCMpT7fJdHsa0hd
                    9nKznLWgGkkk+p/bedbAFAY7BK50XViY9O7kQvb+sep1dWKkvREYqt3yFXwVRr0A
                    ZmCcEMBsc4p3XwaIJYEJIfzSzI3H/CbknqVhEWWhJI3cJCXVIgp/FaTUfGs7+GVU
                    obNeIalofDcvqzDf6NYjLcGvN9lFi4t95KOmlp5hv6v2L2FhdTRVEZw7H24p5PLA
                    99Qc3nOjWsmsmXiSqjZaknW//wzRaVPAPp4MzkBP4gfWrURU6yHsLWTOZpDl5mSh
                    rCa3j5vxxvrbkIeTHyZX8/AuG8iQn6tXN9cweo5ZS+vwoXGlsbtV9Abye6hTRGRe
                    mb6f2LqZAgMBAAECggEAKu47yTiNoelDby8NZwb0J7kuT4PS7Nb9dqcGGdi9eZaO
                    ty24h+Nq6mabZxwcLqXphIqPcdgeBo6PnYIihjDU06XXA8X1Q/SStRtzRVMcCgnN
                    Q2arm3wIdg4m4SYFiE+6PX15UUTjJKyXHaS4EAkdYV/dJodORWKYjqdmYhodj4zr
                    dmwatOzork8IZaNNvDQbrBFhjfofbNb9UinLJkrmV24wYy6RNxz3pjwkeZ+6TvmZ
                    2Hs2kUtI+C/SEL6H7Uesj40KcvdizdAgoysaYKNEAGz2SoEdL3d598V4LaxuDIP9
                    oPuGtbnzFqapzQrhUnMT9mobjFIcuWWDaRadQLmUqwKBgQDDCuiY0MCYkoHtAKKJ
                    8H8CnL9fdE4hf6bk/pNhy12cnz49vWNmrpdbqVC/eDbo4MP75ohcrdeH2lqn7fe2
                    gT7fLdxyMSYQXeirdDNUDq/KK0kpMJtFpji9Q0rv3HEKXeUzbALOQ52vddgTUM3R
                    LJ1AR1DrSnmkycIADT3NbuIVrwKBgQC4mhkb/U0FkevKYOCGCEOZZMSmA/xMjoS+
                    ND8z81QhpqqiqEijy4N9vOk/zsG/xJlubGrZAqWvkMHi4VzHN8vYlqMumCKeCfC6
                    f5NBN6F4Vxlyy/nPlQOEszrm4GKt9ZVPZaRCMg4FNu5q6XcjLBF2McIIYn3qOxD8
                    ew1GHXGONwKBgGffrGaObrQS+r0VJHtgKNRkVItqrp2qlWDJsAZaP33FVWmeLo0m
                    GJgJgWaniF7YLag/a4ooT2wbv0JGOzHofWpwy0HJqSL4UIzXcuqmc7qw+OLF7zvV
                    vcwWRZefCFjkDsgnEwt0+UrT8QLAewyWvRzZnl/hJw27IeXTJ4H8Ns4jAoGBALM2
                    pvnNR2EI8OhgdJiqnTXl5iNl6yJHmgctoc5FhH/G1hFjXmHlyZngNHGFwAL0UiAp
                    kPFs6H0xA4nHT9L4ECYM2A78E19qNxJXmBXQdCnoJQSVkcg82lWRyrUpUaOgr3uN
                    KZI6FfJqCbwxO0AiIDGmzMBnHeavwSXcMF7JZtyxAoGBAKSkIDVjJH/hW+SaTew7
                    WyGI8PMGGMrt0lAhzoap85mahWRle+nHq3jjDb5RAInfjbBzA3EQK73H98aDFOxL
                    E+Pe+MYPIsjTejExBKDNm+86Z9JcQFX79DAM+TOxieXYDrbc34kGJfYYntjHAtSv
                    EDpbhJoDPIl/NexXd98QyKJb
                    -----END PRIVATE KEY-----
                  public-key: |
                    -----BEGIN PUBLIC KEY-----
                    MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAjKU+3yXR7GtIXfZys5y1
                    oBpJJPqf23nWwBQGOwSudF1YmPTu5EL2/rHqdXVipL0RGKrd8hV8FUa9AGZgnBDA
                    bHOKd18GiCWBCSH80syNx/wm5J6lYRFloSSN3CQl1SIKfxWk1HxrO/hlVKGzXiGp
                    aHw3L6sw3+jWIy3BrzfZRYuLfeSjppaeYb+r9i9hYXU0VRGcOx9uKeTywPfUHN5z
                    o1rJrJl4kqo2WpJ1v/8M0WlTwD6eDM5AT+IH1q1EVOsh7C1kzmaQ5eZkoawmt4+b
                    8cb625CHkx8mV/PwLhvIkJ+rVzfXMHqOWUvr8KFxpbG7VfQG8nuoU0RkXpm+n9i6
                    mQIDAQAB
                    -----END PUBLIC KEY-----
    ```


> That's all you need to do to get started with JWT authentication in your Spring Boot application.

--- 

# Detailed Documentation

## Generating JWT Token

While creating a web application there could be an end point which authenticate user and provide jwt token.

An `AuthService` bean is provided to generate JWT tokens. You can inject it in your controller class and can use to generate jwt token.

```java
String token = authService.generateToken(
                "user", // subject
                "https://www.domain.com", // issuer
                Map.of("k1", v1), // custom claims
                List.of("ADMIN") // scopes
        );
```
You can also generate refresh token using below method.

```java
String refreshToken = authService.generateRefreshToken("test", "https://ilyaslabs.github.io", null, null);
```

While generating refresh token you can also pass custom claims and scopes if needed.

> Auth service also adds a scope `REFRESH_TOKEN`, You can use this scope on spring security filter chain to restrict access to refresh token endpoint.

## Consuming Secured API's

While consuming secured API's this token should be passed in the `Authorization` header as a Bearer token.
Then application will automatically validate the token for you.

## Changing JWT Token Expiry

By default, the token will be valid for 1 hour. You can change this by setting below property in your `application.yml` file.

```yml
io:
  github:
    ilyaslabs:
      microservice:
        security:
          jwt:
            expiryUnit: HOURS # or MINUTES, DAYS, WEEKS // Any java.time.temporal.ChronoUnit
            expiry: 60 # value of the expiry unit
            refreshExpiryUnit: DAYS
            refreshExpiry: 30
```

## Retrieving JWT
If you want to retrieve the JWT token from the request, you can use the `AuthService` bean.

```java
authService.getAuthenticatedPrincipal();
```
This will return the `org.springframework.security.oauth2.jwt.Jwt` object which contains the token and its claims.

## Restricting endpoint for specific scope

By default, A SecurityFilter chain is configured for you which can be overridden by creating your own `SecurityFilterChain` bean.

```java
@Bean
public SecurityFilterChain SecurityFilterChain(HttpSecurity http) throws Exception {
    http
            .authorizeHttpRequests(customizer ->
                    customizer
                            .requestMatchers("/api/auth").permitAll()
                            .requestMatchers("/api/data").hasAuthority("SCOPE_ADMIN")
                            .requestMatchers("/api/**").authenticated()
                            .anyRequest().permitAll()
            );
    return http.build();
}

```
Notice while generation token the scope was passed without `SCOPE_` prefix, But when using in filter chain it should be prefixed by `SCOPE_`

The `HttpSecurity` which is injected in security filter chain bean is already configured to.

1. Use JWT authentication.
2. Disable CSRF.
3. Disable form login.
4. Disable httpBasic.
5. Exception handling is configured to return 401 Unauthorized for unauthorized requests.

So, You don't have to do these configurations again.

> This library is also uses `spring-boot-microservice` dependency So you don't have to add it again in your project.

- Have a look at [spring-boot-microservice](https://github.com/ilyasdotdev/spring-boot-microservice) documentation.