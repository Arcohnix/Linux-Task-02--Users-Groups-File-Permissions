# Linux Task 02 — Part F: Security Challenge & Part G: Research

## Part F — Recommended Permissions Table

As a Linux Administrator, here are the recommended permissions for each file, with reasoning:

| File | Recommended Permission | Reason |
|------|------------------------|--------|
| **password_backup.txt** | `600` (rw-------) | Contains sensitive credentials. Only the owner should read/write it; group and others must have zero access to prevent leakage. |
| **public_notice.txt** | `644` (rw-r--r--) | Meant to be read by everyone, but only the owner should edit it. Read access for all is appropriate for a public notice. |
| **system_log.txt** | `640` (rw-r-----) | Logs may hold sensitive system/security info. Owner (often root/admin) reads & writes, a trusted group (e.g. admins/auditors) can read, others get nothing. |
| **personal_notes.txt** | `600` (rw-------) | Private to the owner. No one else needs access, so restrict fully to the owner. |

### Reasoning summary
The guiding principle is **least privilege**: give each file the minimum access needed for its purpose.
- Secrets (passwords, personal notes) → `600`, owner-only.
- Audit/log files → `640`, readable by a trusted group but not the world.
- Public content → `644`, world-readable but owner-editable only.
Never use `777` for any of these — it would let any user tamper with credentials or logs.

---

## Part G — Linux Security Research

### 1. Why are file permissions important?
File permissions control **who can read, modify, or execute** each file and directory. They are the foundation of Linux access control and multi-user security. Proper permissions prevent unauthorized users from viewing sensitive data, altering critical system files, or running malicious code. Without them, any user could read everyone else's data or break the system.

### 2. What happens if sensitive files are given 777 permissions?
`777` grants **every user** full read, write, and execute rights. For a sensitive file this means:
- Any user (or a compromised process) can **read** the secrets inside.
- Anyone can **modify or corrupt** the file — e.g. inject malicious content into a script.
- Anyone can **delete** it.
- For executables/scripts, an attacker could replace the contents and have it run with the next user's privileges — a classic **privilege-escalation** vector.
In short, `777` removes all protection and is one of the most common real-world misconfigurations leading to breaches.

### 3. What is the Principle of Least Privilege?
The Principle of Least Privilege (PoLP) states that every user, process, or program should be granted **only the minimum access needed to perform its task — and nothing more**. Applied to files, that means a file gets the most restrictive permissions that still allow legitimate use (e.g. credentials at `600`, not `644`). This limits the damage if an account or process is compromised, because the attacker inherits only those minimal rights.

### 4. Why do organizations restrict user access?
- **Protect confidential data** — limit who can view financial, personal, or security-sensitive information.
- **Reduce attack surface** — fewer privileges per account means a compromised account causes less damage.
- **Prevent accidental/malicious changes** — restricting write access stops users from breaking shared systems.
- **Accountability & auditing** — controlled access makes it possible to trace who did what.
- **Regulatory compliance** — laws and standards (GDPR, ISO 27001, HIPAA, etc.) require access control over sensitive data.
- **Containment** — segmentation of access stops a single breach from spreading across the whole environment.
