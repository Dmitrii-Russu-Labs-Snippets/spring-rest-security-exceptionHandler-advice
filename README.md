# Spring REST Security ExceptionHandler (ControllerAdvice)

Handle Spring Security authentication and authorization exceptions with `@ExceptionHandler` in `@RestControllerAdvice`. Returns **ProblemDetail (RFC 7807)** JSON responses for all security errors (**401 Unauthorized / 403 Forbidden**).  

---

## Overview

This repository demonstrates how to handle Spring Security exceptions (authentication and authorization) in a centralized way using `@RestControllerAdvice` and `@ExceptionHandler`.  

Instead of customizing `AuthenticationEntryPoint` or `AccessDeniedHandler` directly in `SecurityConfig`, exceptions are intercepted and mapped to **ProblemDetail** responses in a global error handler.  

---

Implementation difference vs other repos: handlers are defined inline as lambdas in the `SecurityFilterChain`,  not as [@Component classes](https://github.com/Dmitrii-Russu-Labs-Snippets/spring-rest-security-entrypointHandler-component)  or [@Bean methods](https://github.com/Dmitrii-Russu-Labs-Snippets/spring-rest-security-entrypointHandler-bean).

---

## Features

- **401 Unauthorized** → ProblemDetail with status `401`  
- **403 Forbidden** → ProblemDetail with status `403`  
- Centralized exception handling in `@RestControllerAdvice`  
- Consistent error format across the application  
- Easy to extend for custom exceptions  

---

## Example 401 response

Content-Type: `application/problem+json`
```json
{
 "type": "about:blank",
  "title": "Unauthorized,
  "status": 401,
  "detail": "Authentication failed",
  "instance": "/auth/admin"}
```

## Example 403 response

Content-Type: `application/problem+json`
```json
{
  "type": "about:blank",
  "title": "Forbidden",
  "status": 403,
  "detail": "Access Denied",
  "instance": "/auth/admin"
}
```

---

## How to Run
```
./mvnw spring-boot:run
```
---

## Example curl
401 (no credentials)
```
curl -i http://localhost:8080/auth/user
```
401 (wrong credentials)
```
curl -i -u wrong:wrong http://localhost:8080/auth/user
```
403 (authenticated but not authorized)
```
curl -i -u ann:1234 http://localhost:8080/auth/admin
```
200 (authorized)
```
curl -i -u jack:123 http://localhost:8080/auth/admin
```
---

## Related

- [spring-rest-security-entrypointHandler-component](https://github.com/Dmitrii-Russu-Labs-Snippets/spring-rest-security-entrypointHandler-component) — handlers as separate components  
- [spring-rest-security-entrypointHandler-bean](https://github.com/Dmitrii-Russu-Labs-Snippets/spring-rest-security-entrypointHandler-bean) — handlers as Spring beans

