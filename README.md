# 🎮 Guess Royal — Laravel Second-Order SQL Injection Challenge

## 🧩 Overview

**Guess Royal** is a Laravel-based guessing game that hides a **second-order SQL injection** within its codebase.  
This challenge showcases how dangerous unsanitized data persistence can be — especially when user-controlled input is stored and later reused in raw SQL queries.

> 💡 Difficulty: **Hard**  

---

##Code

Since this CTF has been submitted to Hack The Box, I’m unable to share the code publicly.

---

## 🚀 Environment

- **Framework:** Laravel + [Octane](https://laravel.com/docs/octane) (FrankenPHP backend)
- **Containerization:** [Laravel Sail](https://laravel.com/docs/sail) for Dockerized deployment
- **Dependencies:** Managed via Composer and Node — initialized through `./vendor/bin/sail`

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
