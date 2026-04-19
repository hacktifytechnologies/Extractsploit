# Assessment — Extractsploit: PHP extract() Variable Injection

---

## Question 1 — MCQ

**Which PHP function is at the root of the authentication vulnerability?**

- A) `eval()`
- B) `include()`
- C) `preg_replace()`
- D) **`extract()`** ✅

> **Answer:** D — PHP's `extract()` takes an associative array and creates local variables from it. Called on unsanitised JSON input, attacker-supplied keys become server-side variables.

---

## Question 2 — MCQ

**Which JSON key-value pair must be injected into the login body to gain admin privileges?**

- A) `"role": "admin"`
- B) `"is_admin": 1`
- C) **`"admin": "true"`** ✅
- D) `"privilege": "elevated"`

> **Answer:** C — `extract()` creates `$admin = "true"` from this key. If the authorization check evaluates `$admin == "true"`, the attacker gains elevated access.

---

## Question 3 — MCQ

**What OWASP category best describes this vulnerability?**

- A) A03: Injection
- B) A07: Identification and Authentication Failures
- C) **A01: Broken Access Control — privilege escalation via variable injection** ✅
- D) A05: Security Misconfiguration

> **Answer:** C — The vulnerability allows a normal user to inject a variable that grants admin-level access, bypassing the intended authorization model.

---

## Question 4 — Fill in the Blank

**What PHP function called on unsanitised user input creates the `$admin` variable from the request body?**

**Answer:** `extract`

> PHP's `extract()` converts each key in the input array into a local variable. When called on unsanitised POST/JSON data, adding `"admin": "true"` to the request body creates `$admin = "true"` in the script scope, satisfying the authorization check.

---

## Question 5 — Fill in the Blank

**What JSON key must be added to the login request body to set `$admin = "true"` server-side via the vulnerable `extract()` call?**

**Answer:** `admin`

> Adding `"admin": "true"` to the JSON login payload causes `extract()` to create the PHP variable `$admin` with value `"true"`. The authorization logic checks this variable and grants admin access without any actual credential verification.

---

*Lab target:* `http://localhost:8095`
