# Linux Task 02 — Command Outputs

> Environment: Kali Linux | User: `kali` (uid=1000)

---

## Part A — Understanding Users

```bash
whoami
id
cat /etc/passwd
```

**`whoami`** → current logged-in user:
```
kali
```

**`id`** → user identity, UID, GID, and group memberships:
```
uid=1000(kali) gid=1000(kali) groups=1000(kali),4(adm),20(dialout),24(cdrom),
25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),100(users),
106(netdev),118(wireshark),121(bluetooth),134(scanner),141(kaboxer)
```

**`cat /etc/passwd`** (key entries) → one line per account on the system:
```
root:x:0:0:root:/root:/usr/bin/zsh
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
mysql:x:100:107:MySQL Server,,,:/nonexistent:/bin/false
postgres:x:122:127:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
sshd:x:113:65534::/run/sshd:/usr/sbin/nologin
kali:x:1000:1000:kali,,,:/home/kali:/usr/bin/zsh
```

### Answers
| Question | Answer |
|----------|--------|
| **Current username** | `kali` |
| **What is UID?** | User ID — a unique number identifying each user. `kali` = 1000. Regular users start at 1000; root = 0. |
| **What is GID?** | Group ID — the unique number of the user's primary group. `kali` = 1000. |
| **What does `/etc/passwd` contain?** | One line per account with 7 colon-separated fields: `username : password-placeholder(x) : UID : GID : comment/GECOS : home directory : login shell`. The `x` means the actual password hash is stored in `/etc/shadow`. |

**Field breakdown of the `kali` line** `kali:x:1000:1000:kali,,,:/home/kali:/usr/bin/zsh`:
- `kali` → username
- `x` → password stored in /etc/shadow
- `1000` → UID
- `1000` → GID
- `kali,,,` → comment / full name (GECOS)
- `/home/kali` → home directory
- `/usr/bin/zsh` → login shell

---

## Part B — Create Users & Groups

```bash
sudo groupadd interns
sudo groupadd cyberteam
sudo useradd -m student1
sudo useradd -m student2
sudo useradd -m student3
sudo usermod -aG interns student1
sudo usermod -aG interns student2
sudo usermod -aG cyberteam student3
groups
id student1
id student2
id student3
```

Verification output:
```
groups (for kali):
kali adm dialout cdrom floppy sudo audio dip video plugdev users netdev wireshark bluetooth scanner kaboxer

id student1
uid=1001(student1) gid=1003(student1) groups=1003(student1),1001(interns)

id student2
uid=1002(student2) gid=1004(student2) groups=1004(student2),1001(interns)

id student3
uid=1003(student3) gid=1005(student3) groups=1005(student3),1002(cyberteam)
```

| User | UID | Primary group | Added to |
|------|-----|---------------|----------|
| student1 | 1001 | student1 (1003) | interns (1001) |
| student2 | 1002 | student2 (1004) | interns (1001) |
| student3 | 1003 | student3 (1005) | cyberteam (1002) |

> Note: `useradd -m` creates the user with a home directory; `usermod -aG` adds a user to a supplementary group (the `-a` is important — it *appends* instead of replacing existing groups).

---

## Part C — File Ownership

```bash
mkdir CyberSecurity_Project
cd CyberSecurity_Project
touch report.txt notes.txt credentials.txt
ls -l
sudo chown student1 credentials.txt
ls -l
```

Before `chown`:
```
total 0
-rw-r--r-- 1 kali kali 0 Jun 13 22:09 credentials.txt
-rw-r--r-- 1 kali kali 0 Jun 13 22:09 notes.txt
-rw-r--r-- 1 kali kali 0 Jun 13 22:09 report.txt
```

After `chown student1 credentials.txt`:
```
total 0
-rw-r--r-- 1 student1 kali 0 Jun 13 22:09 credentials.txt
-rw-r--r-- 1 kali     kali 0 Jun 13 22:09 notes.txt
-rw-r--r-- 1 kali     kali 0 Jun 13 22:09 report.txt
```

| Field | Value |
|-------|-------|
| File | credentials.txt |
| Original Owner | kali |
| New Owner | student1 |
| Command Used | `sudo chown student1 credentials.txt` |

---

## Part D — File Permissions

```bash
touch security_policy.txt
ls -l security_policy.txt
chmod 444 security_policy.txt   # Read only
ls -l security_policy.txt
chmod 664 security_policy.txt   # Read & Write (owner+group)
ls -l security_policy.txt
chmod 777 security_policy.txt   # Full access
ls -l security_policy.txt
```

Output at each stage:
```
-rw-r--r-- 1 kali kali 0 Jun 13 22:09 security_policy.txt   # default 644
-r--r--r-- 1 kali kali 0 Jun 13 22:09 security_policy.txt   # 444 read-only
-rw-rw-r-- 1 kali kali 0 Jun 13 22:09 security_policy.txt   # 664 read+write
-rwxrwxrwx 1 kali kali 0 Jun 13 22:09 security_policy.txt   # 777 full access
```

| chmod | Symbolic | Meaning |
|-------|----------|---------|
| 444 | r--r--r-- | Everyone read-only |
| 664 | rw-rw-r-- | Owner & group read+write, others read |
| 777 | rwxrwxrwx | Everyone full read+write+execute (insecure) |
