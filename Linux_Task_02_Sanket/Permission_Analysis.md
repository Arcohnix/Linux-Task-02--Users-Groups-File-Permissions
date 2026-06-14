# Linux Task 02 — Part E: Permission Analysis

Linux numeric permissions are made of three digits — **Owner / Group / Others** — where each digit is the sum of:
**read (r) = 4, write (w) = 2, execute (x) = 1.**

So `7 = rwx`, `6 = rw-`, `5 = r-x`, `4 = r--`, `0 = ---`.

---

## 755 — `rwxr-xr-x`
| Class | Rights |
|-------|--------|
| Owner | read, write, execute (7) |
| Group | read, execute (5) |
| Others | read, execute (5) |

**Real-world use case:** Executables, scripts, and directories that everyone needs to access/run but only the owner should modify — e.g. `/usr/bin` programs, web directories, shared scripts.

---

## 644 — `rw-r--r--`
| Class | Rights |
|-------|--------|
| Owner | read, write (6) |
| Group | read (4) |
| Others | read (4) |

**Real-world use case:** Normal data/config files and documents. Owner can edit, everyone else can only read — the default for most created files (e.g. HTML files, README files, config that isn't secret).

---

## 777 — `rwxrwxrwx`
| Class | Rights |
|-------|--------|
| Owner | read, write, execute (7) |
| Group | read, write, execute (7) |
| Others | read, write, execute (7) |

**Real-world use case:** Almost none in production — it gives **everyone** full control, including the ability to modify or delete the file. Considered a serious security risk. Occasionally seen as a lazy "fix" in temporary test environments, but should be avoided.

---

## 600 — `rw-------`
| Class | Rights |
|-------|--------|
| Owner | read, write (6) |
| Group | none (0) |
| Others | none (0) |

**Real-world use case:** Private/sensitive files only the owner should touch — SSH private keys (`~/.ssh/id_rsa`), password files, credential stores, personal config containing secrets.

---

## 700 — `rwx------`
| Class | Rights |
|-------|--------|
| Owner | read, write, execute (7) |
| Group | none (0) |
| Others | none (0) |

**Real-world use case:** Private directories and personal scripts only the owner may enter/run — e.g. `~/.ssh` directory, a user's private home subfolder, or a personal executable that must stay confidential.

---

### Quick reference
| Numeric | Symbolic | Owner | Group | Others |
|---------|----------|-------|-------|--------|
| 755 | rwxr-xr-x | rwx | r-x | r-x |
| 644 | rw-r--r-- | rw- | r-- | r-- |
| 777 | rwxrwxrwx | rwx | rwx | rwx |
| 600 | rw------- | rw- | --- | --- |
| 700 | rwx------ | rwx | --- | --- |
