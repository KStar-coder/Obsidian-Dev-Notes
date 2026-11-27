
# Spring Security: Authenticating Users from a Database

*Study notes from a video transcript. Auto-caption errors were cleaned up (e.g. "Dow" → DAO, "Harding" → hardcoding). Corrections to the speaker are marked **⚠ Fact-check**, with a consolidated list in Section 9.*

## Executive Summary

This lesson moves a Spring Boot app from Spring Security's default credentials (set in `application.properties`) to **users stored in a PostgreSQL database**. First it shows how to customize authentication with an in-memory `UserDetailsService`. Then it replaces the default authentication provider with a `DaoAuthenticationProvider` backed by a custom `UserDetailsService`, a JPA repository, a `Users` entity, and a `UserPrincipal` class. It is a one-time setup, and passwords are still plain text at the end (encryption is left for the next video).

## 1. Why Move Beyond Hardcoded Credentials?

- Default Spring Security reads one username/password from `application.properties`.
- Hardcoding users in code, even through an in-memory manager, doesn't scale and isn't done in real projects.
- Real applications **store users in a database** and verify logins against it.
- This is **one-time infrastructure work** (typically 1–2 days or more on large projects). After that, development focuses on business features.

> **Takeaway:** Security and database wiring is set up once per project, then reused for its whole life.

## 2. Core Concepts & Definitions

| Term | Definition |
| --- | --- |
| **`UserDetailsService`** | Interface with one method, `loadUserByUsername(String)`, that fetches a user so credentials can be verified. |
| **`UserDetails`** | Interface representing a user: username, password, authorities, and account-status flags. |
| **`User`** (Spring's class) | Built-in implementation of `UserDetails`, created with a builder. |
| **`InMemoryUserDetailsManager`** | Built-in implementation (via `UserDetailsManager`, which extends `UserDetailsService`) that holds users in memory. |
| **`Authentication` object** | Carries login credentials. It starts *unauthenticated* and is marked *authenticated* after validation. |
| **`AuthenticationProvider`** | Interface that validates an `Authentication` object. |
| **`DaoAuthenticationProvider`** | `AuthenticationProvider` implementation that uses a `UserDetailsService` (usually backed by a DB) and a `PasswordEncoder`. |
| **`PasswordEncoder`** | Component that encodes and compares passwords. |
| **`NoOpPasswordEncoder`** | Encoder that does **no** encoding (plain text). |
| **`GrantedAuthority` / `SimpleGrantedAuthority`** | Represents a permission or role granted to a user. |
| **User Principal** | Spring Security's term for the *currently authenticating user*. Hence the class name `UserPrincipal`. |
| **JPA / `JpaRepository`** | Spring Data abstraction. You define an interface and get CRUD methods automatically. |
| **Entity** | A class mapped to a database table (`@Entity`, `@Id`). |

## 3. How Authentication Flows

### The model presented in the video

1. The client submits a username and password.
2. `UsernamePasswordAuthenticationFilter` takes the credentials.
3. Something verifies them. The speaker identifies this as the `UserDetailsService`.

### The fuller picture

| Step | Component | Role |
| --- | --- | --- |
| 1 | **Filter** | Builds an *unauthenticated* `Authentication` object |
| 2 | **`AuthenticationManager`** (`ProviderManager`) | Delegates to providers |
| 3 | **`AuthenticationProvider`** (`DaoAuthenticationProvider`) | Validates credentials and checks the password |
| 4 | **`UserDetailsService`** | Loads the user from its source (properties, memory, DB) |
| 5 | Result | An *authenticated* `Authentication` object |

> **⚠ Fact-check:** The filter doesn't call `UserDetailsService` directly. The chain is filter → `AuthenticationManager` → `AuthenticationProvider` → `UserDetailsService`. The video's simplification is fine for intuition but not literally accurate. Also, **Postman's Basic Auth** is handled by `BasicAuthenticationFilter`, while the **login form** uses `UsernamePasswordAuthenticationFilter`.

### Mapping to application layers (speaker's analogy)

| Layer | Responsibility |
| --- | --- |
| **Controller** | Accepts requests and returns responses |
| **Service** | Processing and business logic |
| **Repository** | Database access |

## 4. Step A: Customizing `UserDetailsService` (In-Memory Demo)

### Procedure

1. In the security config class, create a `@Bean` returning `UserDetailsService`. Spring Security will pick it up from the container.
2. `UserDetailsService` is an interface, so return an implementation: `InMemoryUserDetailsManager`.
3. An empty manager means **every login fails** (401), because the default `application.properties` user is no longer used.
4. `InMemoryUserDetailsManager` constructors accept:
   - nothing
   - a `Collection` of users
   - varargs `UserDetails...`, so you can pass many users separated by commas
5. `UserDetails` is also an interface, so use Spring's `User` class and its **builder**.

### Builder chain

```java
UserDetails user1 = User
        .withDefaultPasswordEncoder()
        .username("kiran")
        .password("k@123")
        .roles("USER")
        .build();

UserDetails user2 = User
        .withDefaultPasswordEncoder()
        .username("hush")
        .password("h@123")
        .roles("ADMIN")
        .build();

return new InMemoryUserDetailsManager(user1, user2);
```

### Test results

| Credentials | Result |
| --- | --- |
| Old default user (`navin` / old password) | ❌ 401 Unauthorized |
| `kiran` + correct password | ✅ OK |
| `hush` + wrong password | ❌ Fails |
| `hush` + correct password | ✅ OK |

> **⚠ Fact-check:** `withDefaultPasswordEncoder()` is **deprecated** and for demos only. It's a factory method on `User`, not a separate "assign password encoder" step as the speaker phrased it. The old default login stopped working because Spring Boot's default user is only auto-configured when you don't define your own `UserDetailsService` (or provider/manager) bean.

> **Limitation:** Users are still hardcoded. This step only shows *how* customization works. **Comment this bean out** before moving on to the DB approach.

## 5. Step B: Database-Backed Authentication

### 5.1 Overview of Required Pieces

| # | Component | Purpose |
| --- | --- | --- |
| 1 | Dependencies (`pom.xml`) | JPA + DB driver |
| 2 | Database + table | Store users |
| 3 | `application.properties` | DB connection |
| 4 | `Users` entity (model) | Maps to the table |
| 5 | `UserRepo` (repo) | Query the table |
| 6 | `MyUserDetailsService` (service) | Implements `UserDetailsService` |
| 7 | `UserPrincipal` (model) | Implements `UserDetails` |
| 8 | `AuthenticationProvider` bean (config) | Wires encoder + service |

### 5.2 Project Structure

Reorganize into packages: **`config`**, **`controller`**, **`service`**, **`repo`**, **`model`**. Controllers go in `controller`, entities like `Student` in `model`, and so on.

### 5.3 Maven Dependencies

| Dependency | Why |
| --- | --- |
| `spring-boot-starter-data-jpa` | JPA / Hibernate support |
| `org.postgresql:postgresql` (JDBC driver) | PostgreSQL connectivity (swap for MySQL's driver if using MySQL) |

- Skip the `<version>` tag. Spring Boot's parent manages versions.
- Reload Maven after adding them.

### 5.4 Database Setup (PostgreSQL via pgAdmin)

1. Create a new database (the transcript spells it "Tesco 1", likely a caption error).
2. Open the **Query Tool** and run:

```sql
CREATE TABLE users (
    id       INT PRIMARY KEY,
    username TEXT,
    password TEXT
);

INSERT INTO users VALUES (1, 'naveen', 'n@123');
INSERT INTO users VALUES (2, 'sushil', 's@123');  -- second user; exact name unclear in transcript

SELECT * FROM users;
```

**Why `users` and not `user`?**

> **⚠ Fact-check:** The speaker guesses `user` clashes with a built-in PostgreSQL table. More precisely, `user` is a **reserved keyword** in PostgreSQL, so it needs quoting. `users` avoids the problem.

### 5.5 Connection Properties (`application.properties`)

| Property | Value / Pattern |
| --- | --- |
| `spring.datasource.url` | `jdbc:postgresql://localhost:5432/<db_name>` |
| `spring.datasource.username` | `postgres` (on the speaker's machine) |
| `spring.datasource.password` | Your DB password |

**Default ports by DBMS:**

| DBMS | JDBC prefix | Default port |
| --- | --- | --- |
| PostgreSQL | `jdbc:postgresql` | **5432** |
| MySQL | `jdbc:mysql` | **3306** |

### 5.6 The Entity: `Users`

```java
@Entity
public class Users {
    @Id
    private int id;
    private String username;
    private String password;
    // getters, setters, toString()
}
```

- `@Entity` maps the class to a table, here `users`.
- `@Id` marks the primary key.
- Getters/setters are required. Add `toString()` for printing.

> **Note:** With Spring Boot's default naming strategy, entity `Users` maps to table `users`. If your names differ, add `@Table(name = "...")`.

### 5.7 The Repository: `UserRepo`

```java
public interface UserRepo extends JpaRepository<Users, Integer> {
    Users findByUsername(String username);
}
```

- `JpaRepository<EntityClass, PrimaryKeyType>`. The second argument is the primary key type (here `Integer`).
- `findByUsername` is a **derived query method**. Spring Data generates the SQL from the method name (the "DSL" the speaker mentions).

> **Tip:** Returning `Optional<Users>` is a cleaner modern alternative to checking for `null`.

### 5.8 The Service: `MyUserDetailsService`

```java
@Service
public class MyUserDetailsService implements UserDetailsService {

    @Autowired
    private UserRepo repo;

    @Override
    public UserDetails loadUserByUsername(String username)
            throws UsernameNotFoundException {

        Users user = repo.findByUsername(username);

        if (user == null) {
            System.out.println("User not found");
            throw new UsernameNotFoundException("User not found");
        }
        return new UserPrincipal(user);
    }
}
```

**Logic:**

1. Receive a username.
2. Query the DB through the repo.
3. If absent, throw an exception (**401**).
4. If present, wrap it in a `UserDetails` object and return it.

> **⚠ Fact-check:** The exception is **`UsernameNotFoundException`**. The transcript's "User Not Found Exception" is a loose paraphrase.

### 5.9 The Principal: `UserPrincipal`

`UserDetails` is an interface, so you implement your own class that wraps the `Users` entity.

| Method | Implementation in video |
| --- | --- |
| `getUsername()` | `user.getUsername()` |
| `getPassword()` | `user.getPassword()` |
| `getAuthorities()` | `Collections.singleton(new SimpleGrantedAuthority("USER"))` (static role for everyone) |
| `isAccountNonExpired()` | `true` |
| `isAccountNonLocked()` | `true` |
| `isCredentialsNonExpired()` | `true` |
| `isEnabled()` | `true` |

```java
public class UserPrincipal implements UserDetails {
    private Users user;

    public UserPrincipal(Users user) {
        this.user = user;
    }
    // ... methods as in the table above
}
```

- **Pattern:** the constructor receives the `Users` entity and delegates getters to it.
- `getAuthorities()` must return a `Collection`. `Collections.singleton(...)` is a quick way to return one item.
- Roles aren't stored in the DB yet, so everyone gets `USER` for now.

> **⚠ Fact-check (important):** `.roles("USER")` in the builder automatically adds the **`ROLE_`** prefix (`ROLE_USER`). `new SimpleGrantedAuthority("USER")` does **not**. If you later use `hasRole("USER")`, it checks for `ROLE_USER` and will fail unless you write `new SimpleGrantedAuthority("ROLE_USER")` (or use `hasAuthority("USER")`). In recent Spring Security versions the four `isXxx()` flag methods have default implementations returning `true`, but overriding them explicitly is fine.

### 5.10 The Provider Bean (Security Config)

```java
@Bean
public AuthenticationProvider authenticationProvider() {
    DaoAuthenticationProvider provider = new DaoAuthenticationProvider();
    provider.setPasswordEncoder(NoOpPasswordEncoder.getInstance());
    provider.setUserDetailsService(userDetailsService);   // injected MyUserDetailsService
    return provider;
}
```

| Setting | Value used | Meaning |
| --- | --- | --- |
| Password encoder | `NoOpPasswordEncoder.getInstance()` | No hashing, plain-text comparison (**temporary**) |
| UserDetailsService | Custom `MyUserDetailsService` | Loads users from the DB |

- `DaoAuthenticationProvider` extends `AbstractUserDetailsAuthenticationProvider`, which implements `AuthenticationProvider`.
- Autowire your `UserDetailsService` into the config class so you can pass it in.

> **⚠ Fact-check:** `NoOpPasswordEncoder` is deprecated and unsafe for production. Newer Spring Security versions are moving toward constructor-based configuration of `DaoAuthenticationProvider`, so check your version's Javadoc if the setter shows as deprecated.

## 6. Interface to Implementation Quick Reference

| Interface | Why needed | Implementation used |
| --- | --- | --- |
| `UserDetailsService` | Load user by username | `InMemoryUserDetailsManager` (demo), then **`MyUserDetailsService`** (DB) |
| `UserDetails` | Represent a user | `User` (demo), then **`UserPrincipal`** (DB) |
| `AuthenticationProvider` | Validate credentials | **`DaoAuthenticationProvider`** |
| `PasswordEncoder` | Compare/encode passwords | `NoOpPasswordEncoder` (temporary) |
| `GrantedAuthority` | Represent a role | `SimpleGrantedAuthority` |

> **Golden rule:** If Spring gives you an interface, you either use a built-in implementing class or write your own. Writing your own is a one-time cost.

## 7. End-to-End Test (Postman)

| Test | Expected | Observed |
| --- | --- | --- |
| Old in-memory user (`kiran`/`hush`) | Not in DB, so fails | ❌ 401 |
| DB user, wrong password | Fails | ❌ 401 |
| DB user, correct password | Success | ✅ 200 |
| Unknown user | Console prints "User not found" | ❌ 401 |

> **Result:** Authentication now reads from PostgreSQL. **Remaining gap:** passwords are stored in plain text. Encoding comes in the next video.

## 8. Complete Step Checklist

1. ☐ Add `spring-boot-starter-data-jpa` and the PostgreSQL driver to `pom.xml`, then reload Maven
2. ☐ Create the DB and a `users` table, then insert test rows
3. ☐ Add `spring.datasource.url`, `username` and `password` to `application.properties`
4. ☐ Create packages: `config`, `controller`, `service`, `repo`, `model`
5. ☐ Create the `Users` entity (`@Entity`, `@Id`, getters/setters)
6. ☐ Create `UserRepo extends JpaRepository<Users, Integer>` with `findByUsername`
7. ☐ Create `UserPrincipal implements UserDetails`
8. ☐ Create `MyUserDetailsService implements UserDetailsService` (`@Service`)
9. ☐ Define the `DaoAuthenticationProvider` bean with an encoder and the custom service
10. ☐ Remove or comment out the in-memory `UserDetailsService` bean
11. ☐ Test with Postman

## 9. Consolidated Fact-Check Summary

| # | Transcript statement | Verdict |
| --- | --- | --- |
| 1 | Filter → UserDetailsService directly | ⚠ Simplified. Real chain: filter → AuthenticationManager → AuthenticationProvider → UserDetailsService |
| 2 | `UserDetailsService` is an interface with `loadUserByUsername` | ✅ Correct |
| 3 | `InMemoryUserDetailsManager` indirectly implements `UserDetailsService` | ✅ Correct (via `UserDetailsManager`) |
| 4 | `User.withDefaultPasswordEncoder()` is not for production | ✅ Correct (deprecated) |
| 5 | `UserNotFoundException` | ⚠ Actual class: `UsernameNotFoundException` |
| 6 | `DaoAuthenticationProvider` implements `AuthenticationProvider` | ✅ Correct (indirectly) |
| 7 | PostgreSQL port 5432, MySQL 3306 | ✅ Correct |
| 8 | `user` table conflicts with a PostgreSQL built-in | ⚠ It's a **reserved keyword**; `users` is the safe name |
| 9 | `NoOpPasswordEncoder` = plain text | ✅ Correct (deprecated, insecure) |
| 10 | `SimpleGrantedAuthority("USER")` equals the `.roles("USER")` role | ⚠ `.roles()` adds `ROLE_`; this doesn't |
| 11 | Skip the Maven `<version>` because the parent manages it | ✅ Correct for Spring Boot starters |
| 12 | `findByUsername` works without implementation | ✅ Correct (Spring Data derived queries) |

## 10. Review Questions

1. Why did the old `application.properties` login stop working after defining a `UserDetailsService` bean?
2. What is the difference between `UserDetailsService` and `AuthenticationProvider`?
3. Why can't you write `new UserDetails()`, and what does the video do instead?
4. What does `Collections.singleton()` accomplish in `getAuthorities()`?
5. Why is `NoOpPasswordEncoder` unacceptable in production, and what's the planned next step?
6. What goes wrong if `SimpleGrantedAuthority("USER")` is combined with `hasRole("USER")`?
