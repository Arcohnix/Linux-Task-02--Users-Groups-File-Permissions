# Linux Task 02 — Users, Groups & File Permissions

**Author:** Sanket
**Environment:** Kali Linux | User: kali (uid=1000)

## Objective
Understand Linux user management, groups, ownership, and file permissions — the foundation of Linux security and system administration.

## Contents
```
Linux_Task_02_Sanket/
├── README.md
├── Permission_Analysis.md                  # Part E (755/644/777/600/700)
├── Security_Challenge_and_Research.md       # Parts F & G
├── Command_Outputs/
│   └── command_outputs.md                   # Parts A–D
└── Screenshots/                             # (add your screenshots here)
```

## Tasks Completed
- **Part A** — Users explained: `whoami`, `id`, `/etc/passwd` (UID, GID, passwd fields)
- **Part B** — Created groups `interns`, `cyberteam`; users `student1/2/3`; added to groups
- **Part C** — File ownership: created `CyberSecurity_Project`, changed owner of `credentials.txt` to `student1`
- **Part D** — Permissions: `444` → `664` → `777` on `security_policy.txt`
- **Part E** — Analysis of 755, 644, 777, 600, 700
- **Part F** — Security challenge: recommended permissions table
- **Part G** — Security research answers

## Key Results
| User | UID | Groups |
|------|-----|--------|
| student1 | 1001 | student1, interns |
| student2 | 1002 | student2, interns |
| student3 | 1003 | student3, cyberteam |

`credentials.txt` owner changed: **kali → student1**

## Screenshots To Add
1. partA_whoami_id_passwd.png
2. partB_users_groups.png
3. partC_ownership.png
4. partD_permissions.png
