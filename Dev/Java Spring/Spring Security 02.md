---
title: "Spring Security 02 - Custom SecurityFilterChain"
aliases:
  - Customizing Spring Security
  - SecurityConfig class
  - HttpSecurity customizer
tags:
  - spring-security
  - spring-boot
  - java
  - security-filter-chain
  - csrf
  - lambda-dsl
  - study-notes
series: Spring Security (Spring 6)
part: 2
created: 2026-11-26
source: video transcript (speech-to-text, cleaned)
status: fact-checked
---

# Spring Security 02 - Custom SecurityFilterChain

**Series:** Spring Security (Spring 6) · **Part 2 of 2** · ← Previous: [[Spring Security 01 - Filters, Sessions and Basic Auth]]

> [!abstract] Executive Summary
> This lesson moves from Spring Security's default setup to a **custom configuration**. You create a `SecurityConfig` class that defines your own **`SecurityFilterChain`** bean and shape it through the **`HttpSecurity`** object: disabling CSRF, requiring authentication on every request, and enabling form login, HTTP Basic and stateless sessions. The second half shows what the Lambda code expands to (the **`Customizer`** functional interface) and how the **builder pattern** lets you chain all settings into one statement. This is the foundation for the upcoming lessons on database users and JWT.

> [!note] Transcript clean-up
> Speech-to-text errors fixed: "srf" = CSRF, "lamb/lambra" = lambda, "authorize HTTP request" = `authorizeHttpRequests`, "Anonymous in a class" = anonymous inner class, "Teliscope/Tesco" = the sample password.

> [!warning] This version is fact-checked
> Several claims from the lecture (and my first draft) are corrected inline: what `@EnableWebSecurity` does, what an "empty" chain really contains, why logout behaved differently, how `STATELESS` works, and the status of `http.csrf().disable()`. See the [[#Fact-check log]].

---

## Lesson roadmap

| # | Goal | Section |
|---|---|---|
| 1 | Replace the default filter chain with your own | [[#1. Creating a custom security configuration]] |
| 2 | Disable CSRF | [[#2. Disabling CSRF]] |
| 3 | Require authentication on all requests | [[#3. Requiring authentication on every request]] |
| 4 | Enable form login (browser) and HTTP Basic (Postman) | [[#4. Enabling login mechanisms]] |
| 5 | Make the app stateless | [[#5. Making the application stateless]] |
| 6 | Understand the Lambda syntax (`Customizer`) | [[#6. Behind the scenes - the Customizer interface]] |
| 7 | Clean up with the builder pattern | [[#7. The builder pattern - chaining settings]] |

---

## 1. Creating a custom security configuration

### 1.1 Why customize?

- The Spring Security dependency alone gives you **defaults** only (default filter chain, generated password, form login).
- Real apps need more: database users, no form login, JWT, etc.
- In Spring, customization is done in a **config class that defines beans**.

**Key terms**

- **Config class**: a class annotated `@Configuration` that defines beans replacing or customizing framework defaults.
- **`SecurityFilterChain`**: the object representing the ordered chain of security filters applied to requests (see [[Spring Security 01 - Filters, Sessions and Basic Auth#1. Who handles security? The filter chain]]).
- **`HttpSecurity`**: the builder used to configure web security. Its `build()` returns a `SecurityFilterChain`.

### 1.2 Setup steps

1. Create a package `config` and a class `SecurityConfig` in it.
2. Annotate it with **`@Configuration`**.
3. Annotate it with **`@EnableWebSecurity`** (explicit; see the correction below).
4. Add a **`@Bean`** method returning `SecurityFilterChain`.
5. Take `HttpSecurity` as a parameter, declare `throws Exception`, and return `http.build()`.

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http.build();   // no rules configured: nothing is protected (see 1.3)
    }
}
```

| Annotation | What it really does |
|---|---|
| `@Configuration` | Marks the class as a source of bean definitions (required explicitly since Spring Security 6) |
| `@EnableWebSecurity` | Turns on Spring Security's web support; **Spring Boot applies it automatically**, so it is optional but harmless |
| `@Bean SecurityFilterChain` | **This is what replaces Boot's default chain** |

> [!warning] Correction to the lecture
> The instructor says `@EnableWebSecurity` tells Spring "don't use the default flow, use mine". In fact, Boot's default chain **backs off when you define your own `SecurityFilterChain` bean**. The bean does the replacing; the annotation is optional in Boot.

### 1.3 Experiment: default vs. empty custom chain

| State | Result |
|---|---|
| `@EnableWebSecurity` and `@Bean` both commented out | Default login form still appears (Boot's default security) |
| Bean active, nothing configured | **Page opens without login** (no protection) |

> [!danger] Empty chain = unprotected app
> Your bean replaces the defaults. With no `authorizeHttpRequests` rule and no login mechanism, every request is allowed. The instructor compares it to buying a lock and never closing it.

> [!info] Nuance on the "no filters" wording
> A bare `http.build()` still includes Spring Security's baseline filters (headers, CSRF, anonymous authentication, exception handling and so on). What it lacks is an **authorization rule** and a **login mechanism**, so nothing is blocked. This is also why a POST to an empty chain would still be rejected by CSRF protection.

---

## 2. Disabling CSRF

**Key terms**

- **CSRF (Cross-Site Request Forgery)**: an attack where a malicious site makes a logged-in user's browser send unwanted requests. Spring Security blocks it by default with a **CSRF token** on state-changing requests (POST, PUT, DELETE).
- **Why disable it here:** the lesson later makes the app stateless, and it also lets you test POST from Postman without a token.

### Old vs. new syntax

| Style | Code | Status |
|---|---|---|
| Old (non-lambda) | `http.csrf().disable()` | Deprecated in Spring Security 6.1 and removed in 7.0 (the lecture says it "doesn't work in Spring 6", which is only true from 7.0) |
| **New (Lambda DSL)** | `http.csrf(customizer -> customizer.disable())` | Use this |
| Without lambda | Pass a `Customizer` object | See [[#6. Behind the scenes - the Customizer interface]] |

```java
http.csrf(customizer -> customizer.disable());
```

> [!important] Remember
> In modern Spring Security, every `http.xxx(...)` setting takes a **`Customizer`** argument.

> [!warning] Disabling CSRF is a decision, not a shortcut
> It is commonly done for stateless APIs that authenticate with a token in a header. Note that browsers also cache and automatically resend **Basic Auth** credentials, so stateless + Basic is not automatically CSRF-safe in a browser context. Fine for learning; think twice for production.

---

## 3. Requiring authentication on every request

Disabling CSRF protects nothing. Without an authorization rule anyone, even in an incognito window, can open the app.

```java
http.authorizeHttpRequests(request -> request.anyRequest().authenticated());
```

- **`authorizeHttpRequests`** defines access rules.
- **`anyRequest()`** matches every URL.
- **`authenticated()`** requires a logged-in user.

| Client | Result | Why |
|---|---|---|
| Browser | "Access to localhost was denied" (Chrome's 403 page) | Authentication now required |
| Postman with Basic credentials | **403 Forbidden** | Credentials are sent, but **no authentication mechanism is enabled** to read them |

> [!important] Key insight
> *Requiring* authentication and *providing a way to authenticate* are two separate settings. Enable at least one mechanism (section 4).

---

## 4. Enabling login mechanisms

### 4.1 Form login (browsers)

```java
http.formLogin(Customizer.withDefaults());
```

- **`Customizer.withDefaults()`**: "use the default settings for this feature".
- The default login form appears and browser login works.

### 4.2 Why Postman then shows the login page

With only form login enabled, an unauthenticated request is answered with a **302 redirect to `/login`**. Postman follows the redirect automatically, so it displays **200 OK with the login form HTML** instead of your controller's output.

### 4.3 HTTP Basic (Postman and REST clients)

```java
http.httpBasic(Customizer.withDefaults());
```

- **HTTP Basic**: credentials travel in the `Authorization` header on every request.
- Needed for REST access; Postman now returns the real controller response.

| Setting | Used for | Behavior |
|---|---|---|
| `formLogin(...)` | Browser users | Shows an HTML login form |
| `httpBasic(...)` | Postman / REST clients | Reads credentials from the `Authorization` header |

> [!warning] Correction to the lecture: logout
> The instructor says logout stops working because "you must supply your own login page". Not quite. Default logout still works. The visible difference is that with **CSRF disabled**, `GET /logout` logs you out **immediately without the confirmation page** (with CSRF on, logout must be a POST with a token).

> [!tip]
> You can disable form login entirely and use only HTTP Basic if you just need Postman/REST access.

---

## 5. Making the application stateless

**Key terms**

- **Stateful**: the server keeps a **session** per user and the browser sends the session cookie.
- **Stateless**: the server keeps no session, so the **client sends credentials with every request**.
- **`SessionCreationPolicy`**: an enum controlling how Spring Security uses HTTP sessions.

```java
import org.springframework.security.config.http.SessionCreationPolicy;

http.sessionManagement(session ->
        session.sessionCreationPolicy(SessionCreationPolicy.STATELESS));
```

| Policy | Meaning |
|---|---|
| `ALWAYS` | Always create a session |
| `IF_REQUIRED` | Create only when needed (**default**) |
| `NEVER` | Never create one, but use an existing one if the app created it |
| **`STATELESS`** | Spring Security neither creates nor uses a session to store authentication |

### Observed effects

| Client | With `STATELESS` | Explanation |
|---|---|---|
| Browser + form login | Login form reappears on every new request | Authentication is not remembered between requests |
| Postman + Basic Auth | Works every time | Credentials sent with each request |
| Browser, form login disabled | Browser **popup** for Basic Auth, then it works | HTTP Basic resends credentials each time |

> [!important] Takeaway
> **Stateless + form login = poor combination.** Use HTTP Basic here (or JWT later in the series).

> [!info] About the changing session ID in Postman
> The lecture reads the changing ID as a new session per request. More precisely, Spring Security isn't using a session for authentication; any ID you print comes from your controller calling `request.getSession()`, which makes the container create one. It plays no part in authentication.

---

## 6. Behind the scenes - the Customizer interface

### 6.1 The question

What does `http.csrf(customizer -> customizer.disable())` actually mean? The instructor rewrites it without lambdas.

### 6.2 Key concepts

- **`Customizer<T>`** (`org.springframework.security.config.Customizer`): a **functional interface** with one method, `customize(T t)`, that lets you modify a configurer object.
- **Functional interface**: exactly one abstract method, so it can be implemented with a **lambda** or **method reference**.
- **Anonymous inner class**: an inline, unnamed class implementing an interface.

| `HttpSecurity` method | Expects |
|---|---|
| `csrf(...)` | `Customizer<CsrfConfigurer<HttpSecurity>>` |
| `authorizeHttpRequests(...)` | `Customizer<AuthorizeHttpRequestsConfigurer<HttpSecurity>.AuthorizationManagerRequestMatcherRegistry>` |
| `httpBasic(...)` | `Customizer<HttpBasicConfigurer<HttpSecurity>>` |
| `formLogin(...)` | `Customizer<FormLoginConfigurer<HttpSecurity>>` |

### 6.3 One statement, three forms

**Step 1: anonymous inner class (imperative)**

```java
Customizer<CsrfConfigurer<HttpSecurity>> custCsrf =
    new Customizer<CsrfConfigurer<HttpSecurity>>() {
        @Override
        public void customize(CsrfConfigurer<HttpSecurity> customizer) {
            customizer.disable();
        }
    };
http.csrf(custCsrf);
```

**Step 2: lambda**

```java
http.csrf(customizer -> customizer.disable());
```

**Step 3: method reference** (the IDE may suggest it)

```java
http.csrf(AbstractHttpConfigurer::disable);   // CsrfConfigurer::disable also compiles
```

> [!important] Golden rule
> A lambda here is shorthand for implementing `Customizer.customize()`. The same pattern applies to `authorizeHttpRequests`, `httpBasic` and the rest.

### 6.4 `Customizer.withDefaults()`

When you want a feature's default behavior (as with `formLogin` and `httpBasic`), pass `Customizer.withDefaults()` instead of writing your own customizer. It is effectively a no-op customizer.

---

## 7. The builder pattern - chaining settings

- **Builder pattern (fluent API)**: each method configures the object and returns it, so calls can be **chained** instead of repeating `http.`.
- **Analogy:** a factory conveyor belt, one object passing from machine to machine, each applying its change.

### Final consolidated configuration

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http
            .csrf(customizer -> customizer.disable())
            .authorizeHttpRequests(request -> request.anyRequest().authenticated())
            // .formLogin(Customizer.withDefaults())   // browser form; poor fit with STATELESS
            .httpBasic(Customizer.withDefaults())
            .sessionManagement(session ->
                    session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .build();
    }
}
```

*(Assembled from the settings shown in the lesson. The instructor toggled form login and stateless mode during the demos.)*

> [!tip] Style tips from the lesson
> - Put `return` directly before the chain and end with **`.build()`**.
> - One setting per line improves readability.
> - If the IDE suggests method references, it is optional.

---

## 8. Final verification (Postman)

| Test | Result | Why |
|---|---|---|
| GET homepage with Basic Auth | Response returned | Authenticated via HTTP Basic |
| GET `/students` (demo endpoint) | Response returned | Same |
| POST **without** a CSRF token | **Works** | CSRF disabled |

---

## 9. Concept map

| Concept | Purpose |
|---|---|
| `@Configuration` | Marks a class as bean configuration |
| `@EnableWebSecurity` | Enables web security support (auto-applied in Boot) |
| `SecurityFilterChain` bean | Replaces the default filter chain |
| `HttpSecurity` | Builder used to configure web security |
| `Customizer<T>` | Functional interface used by every setting |
| `Customizer.withDefaults()` | "Use default settings" shortcut |
| `csrf(...)` | CSRF protection |
| `authorizeHttpRequests(...)` | Which requests need authentication |
| `formLogin(...)` | Browser login form |
| `httpBasic(...)` | Credentials via header (Postman/REST) |
| `sessionManagement(...)` + `SessionCreationPolicy` | Stateful vs. stateless |

---

## 10. Quick-revision cheat sheet

> [!summary]
> 1. Customize with a **`@Configuration` class** containing a **`SecurityFilterChain` `@Bean`**.
> 2. The **bean** replaces Boot's default chain; `@EnableWebSecurity` is optional in Boot.
> 3. A chain with **no rules** protects nothing.
> 4. Modern settings are **Customizer-based**: `http.csrf(c -> c.disable())`.
> 5. `anyRequest().authenticated()` protects everything, but you must also enable a login mechanism.
> 6. **`formLogin`** = browsers; **`httpBasic`** = Postman/REST.
> 7. **`STATELESS`** = credentials with every request; avoid with form login.
> 8. `Customizer` is a **functional interface**: lambdas and method references work.
> 9. **`Customizer.withDefaults()`** accepts a feature's defaults.
> 10. **Chain** settings and finish with **`.build()`**.

---

## 11. Self-test

> [!question]- 1. What happens if your `SecurityFilterChain` bean just returns `http.build()`?
> It replaces the defaults with a chain that has no authorization rules or login mechanism, so the app is effectively unprotected.

> [!question]- 2. What do `@Configuration` and `@EnableWebSecurity` each do?
> `@Configuration` marks a source of beans. `@EnableWebSecurity` turns on Spring Security's web support (Boot applies it automatically). The `SecurityFilterChain` bean is what replaces the default chain.

> [!question]- 3. Why 403 in Postman after `anyRequest().authenticated()` but before enabling a login method?
> Authentication is required, but no mechanism (form login or HTTP Basic) is enabled to read the credentials.

> [!question]- 4. Why does Postman show the login-form HTML with 200 when only `formLogin` is enabled?
> The unauthenticated request is redirected (302) to `/login`, Postman follows it, and the login page returns 200.

> [!question]- 5. Difference between `formLogin` and `httpBasic`?
> `formLogin` provides an HTML form for browsers. `httpBasic` reads credentials from the `Authorization` header, which suits REST clients.

> [!question]- 6. What does `STATELESS` do, and why does it clash with browser form login?
> Spring Security doesn't store authentication in a session, so credentials must accompany every request. A form login would have to be repeated for each new request.

> [!question]- 7. What is a functional interface and how does it relate to `Customizer`?
> An interface with exactly one abstract method. `Customizer` has `customize()`, so lambdas and method references can implement it.

> [!question]- 8. What does `Customizer.withDefaults()` do?
> Applies a feature's default settings without changes.

> [!question]- 9. Why can you chain `.csrf(...).authorizeHttpRequests(...).httpBasic(...)`?
> Each method configures `HttpSecurity` and returns it (builder pattern), ending with `.build()`.

> [!question]- 10. Why does POST work without a CSRF token here?
> CSRF protection is disabled.

---

## 12. What's next

- Loading **usernames and passwords from a database**.
- Securing the API with **JWT**.
- More advanced filter-chain customization.

---

## Fact-check log

| # | Claim in lecture or first draft | Status | Correct information |
|---|---|---|---|
| 1 | `@EnableWebSecurity` makes Spring skip the default configuration | **Corrected** | The `SecurityFilterChain` bean replaces the default; the annotation is optional in Boot |
| 2 | An empty chain applies "no filters" | **Refined** | Baseline filters remain; there is no authorization rule or login mechanism, so everything is permitted |
| 3 | `http.csrf().disable()` "doesn't work in Spring 6" | **Corrected** | Deprecated in 6.1, removed in 7.0 |
| 4 | Postman gets 200 with login HTML | **Explained** | A 302 redirect to `/login`, followed automatically |
| 5 | Logout stops working with a custom config | **Corrected** | Logout works; with CSRF disabled, GET `/logout` signs out immediately without a confirmation page |
| 6 | Changing session ID in Postman means a new session per request | **Clarified** | `STATELESS` means Spring Security doesn't use a session; any ID shown comes from the controller's `getSession()` call |
| 7 | Stateless + Basic is safe once CSRF is off | **Caveat added** | Browsers cache and resend Basic credentials; token-in-header (JWT) is the usual safe pairing |
| 8 | 403 when authentication is required but no mechanism is enabled | **Verified** | Matches Spring Security 6 behavior |
| 9 | `Customizer` types, `SessionCreationPolicy` values, `withDefaults()` | **Verified** | Match the Spring Security 6 API |
| 10 | `@Configuration` needed with `@EnableWebSecurity` | **Added** | `@EnableWebSecurity` no longer includes `@Configuration` since Spring Security 6 |

Let's break this down from the basics so you understand not just what `STATELESS` does, but **why we use it in Spring Security**, especially in JWT-based authentication.

```
httpSecurity.sessionManagement(
    session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
);
```

## **1. What is session management?**

First, understand how a traditional web application handles authentication.

Imagine you log in to an application using:

- Username: `kaustav`
    
- Password: `password123`
    

The server verifies your credentials and creates a **session**.

A session is basically a way for the server to remember that you've already authenticated.

For example:

**Traditional session-based authentication**

1. **Login request**
    
    Client sends username and password.
    
2. **Server creates a session**
    
    Server stores session information in memory or a session store.
    
3. **Server sends a session ID**
    
    Client receives a session cookie, such as `JSESSIONID`.
    
4. **Subsequent requests**
    
    Client automatically sends the session cookie. Server uses it to identify the authenticated user.
    

The important thing is that **the server remembers the user's authentication state**.

## **2. What does STATELESS mean?**

Now imagine we don't want the server to maintain sessions.

Instead, we use JWT (JSON Web Token).

The flow becomes:

**JWT-based authentication (stateless)**

1. **Login request**
    
    Client sends username and password.
    
2. **Server generates JWT**
    
    After successful authentication, the server generates a signed JWT and returns it.
    
3. **Client stores JWT**
    
    Usually in a secure cookie or another appropriate client-side storage mechanism.
    
4. **Subsequent requests**
    
    Client sends the JWT with every protected API request, commonly through the Authorization header.
    
5. **Server validates JWT**
    
    Server verifies its signature and validity, extracts the user's identity, and authenticates the request.
    

Here, the server doesn't need to remember a session between requests.

Each request carries the information needed to authenticate the user.

That's what we mean by **stateless authentication**.

## **3. What exactly does this Spring Security line do?**

Let's examine the code.

```
httpSecurity.sessionManagement(
    session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
);
```

Breaking it down:

|Code|Meaning|
|---|---|
|`httpSecurity`|Configures Spring Security's HTTP security behavior.|
|`sessionManagement()`|Configures how HTTP sessions are handled.|
|`session ->`|Lambda expression to configure session management.|
|`sessionCreationPolicy()`|Specifies the session creation policy.|
|`STATELESS`|Spring Security will neither create an HTTP session nor use one to obtain the SecurityContext.|

In simple terms:

**Don't use HTTP sessions to remember authenticated users. Authenticate each request independently.**

## **4. What are the other SessionCreationPolicy options?**

Spring Security provides four policies:

|Policy|Behaviour|
|---|---|
|`ALWAYS`|Always creates an HTTP session.|
|`IF_REQUIRED`|Creates a session when required. This is the default.|
|`NEVER`|Doesn't create a session, but can use an existing one.|
|`STATELESS`|Doesn't create or use an HTTP session for the SecurityContext.|

The important distinction is between `NEVER` and `STATELESS`.

- `NEVER`: Won't create a session, but can use an existing session.
    
- `STATELESS`: Won't create or use a session to retrieve the SecurityContext.
    

## **5. Why do we use STATELESS with JWT?**

Suppose you're building an e-commerce application.

A user logs in and receives a JWT.

Every time they make a request:

```
GET /api/products
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...
```

Your JWT authentication filter:

1. Extracts the token.
    
2. Validates the token.
    
3. Extracts the username.
    
4. Loads or establishes the user's authentication.
    
5. Sets the authentication in Spring Security's `SecurityContextHolder`.
    

For example:

```
SecurityContextHolder.getContext()
    .setAuthentication(authentication);
```

Spring Security can now identify the authenticated user for that request.

When the request finishes, the security context is not preserved in an HTTP session for the next request.

The next request goes through authentication again using its JWT.

## **6. One important thing to remember**

`STATELESS` does NOT mean:

- The server cannot store user information in a database.
    
- The server cannot use JWT.
    
- The server cannot authenticate users.
    
- The server cannot use `SecurityContextHolder`.
    

It only means Spring Security doesn't rely on HTTP sessions to maintain authentication across requests.

**The key takeaway:**

- Session-based authentication: Server remembers who you are.
    
- JWT + STATELESS: Client sends proof of identity with every request, and the server validates it.
    

This is why you'll commonly see `SessionCreationPolicy.STATELESS` in Spring Boot REST APIs that use JWT authentication.