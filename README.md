# ANSIBLE ASSIGNMENT-1
# UserManager Project Management System

---

# 📌 Objective

Create a comprehensive user and project management system using only Ansible ad-hoc commands.

This assignment includes:

- Team and group creation
- Advanced user management
- Password policy enforcement
- Sudo access management
- Directory structure creation
- Security & permission matrix
- ACL-based permission control

---

# 👥 Team Structure

| Team | Members | Shell | Sudo Access |
|------|---------|-------|-------------|
| dev-team | dev1, dev2, dev3 | /bin/bash | No |
| devops-team | ops1, ops2, ops3 | /bin/sh | Limited Sudo |
| admin-group | admin1, admin2, admin3 | /bin/bash | Full Sudo |

---

# 📂 Project Structure

```bash
/
├── home/
│   ├── dev1/workspace/
│   ├── dev2/workspace/
│   ├── dev3/workspace/
│   ├── ops1/workspace/
│   ├── ops2/workspace/
│   ├── ops3/workspace/
│   ├── admin1/workspace/
│   ├── admin2/workspace/
│   └── admin3/workspace/
├── teams/
│   ├── dev-team/
│   └── devops-team/
├── projects/
│   ├── WebApp/
│   ├── API/
│   └── Mobile/
├── shared/
├── archive/
└── admin/
```

---

# 👤 User Details

| User | UID | Group | Shell |
|------|-----|-------------|-------------|
| dev1 | 2001 | dev-team | /bin/bash |
| dev2 | 2002 | dev-team | /bin/bash |
| dev3 | 2003 | dev-team | /bin/bash |
| ops1 | 2004 | devops-team | /bin/sh |
| ops2 | 2005 | devops-team | /bin/sh |
| ops3 | 2006 | devops-team | /bin/sh |
| admin1 | 2007 | admin-group | /bin/bash |
| admin2 | 2008 | admin-group | /bin/bash |
| admin3 | 2009 | admin-group | /bin/bash |

---

# 🔐 Password Policies

| Team | Max Days | Min Days | Warning Days |
|------|----------|----------|--------------|
| dev-team | 30 | 1 | 5 |
| devops-team | 20 | 1 | 7 |
| admin-group | 15 | 1 | 10 |

---

# 🔒 Security and Permission Matrix

| Directory | Owner | Assigned Team | Other Teams | Others |
|-----------|--------|---------------|--------------|---------|
| `/home/*/workspace` | Owner User | Read + Execute (5) | No Access (0) | No Access (0) |
| `/teams/dev-team` | root | Full (7) | Read + Execute (5) | No Access (0) |
| `/teams/devops-team` | root | Full (7) | Read + Execute (5) | No Access (0) |
| `/projects/WebApp` | dev1 | Read/Write/Execute (7) | Read + Execute (5) | Read + Execute (5) |
| `/projects/API` | ops1 | Read/Write/Execute (7) | Read + Execute (5) | Read + Execute (5) |
| `/projects/Mobile` | dev2 | Read/Write/Execute (7) | Read + Execute (5) | Read + Execute (5) |
| `/shared` | root | Full (7) | Full (7) | Full (7) |
| `/archive` | root | Read + Execute (5) | Read + Execute (5) | Read + Execute (5) |
| `/admin` | root | Full (7) *(admin-group only)* | No Access (0) | No Access (0) |

---

# 🖥️ Inventory Configuration

## inventory

```ini
[servers]
web_server ansible_host=65.2.140.43

[all:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=/home/cls/ansible.pem
```

---

# 👥 1. Create Groups

## Create Development Team

```bash
ansible all -i inventory -m shell -a "getent group dev-team || groupadd dev-team" --become
```
---

## Create DevOps Team

```bash
ansible all -i inventory -m shell -a "getent group devops-team || groupadd devops-team" --become
```
---

## Create Admin Group

```bash
ansible all -i inventory -m shell -a "getent group admin-group || groupadd admin-group" --become
```

📸 Screenshot
<img width="745" height="166" alt="image" src="https://github.com/user-attachments/assets/5550c8ef-2877-41a9-911f-69a5309efb93" />


---

# 👤 2. Create Users

## Development Team Users

```bash
ansible all -i inventory -m user -a "name=dev1 uid=2001 group=dev-team shell=/bin/bash create_home=yes" --become

ansible all -i inventory -m user -a "name=dev2 uid=2002 group=dev-team shell=/bin/bash create_home=yes" --become

ansible all -i inventory -m user -a "name=dev3 uid=2003 group=dev-team shell=/bin/bash create_home=yes" --become
```

📸 Screenshot:
<img width="751" height="198" alt="image" src="https://github.com/user-attachments/assets/9b72f671-cdec-48d5-b368-f02c4b832279" />
<img width="767" height="324" alt="image" src="https://github.com/user-attachments/assets/2cef19dd-709e-4ac7-9b8b-6625600a16da" />


---

## DevOps Team Users

```bash
ansible all -i inventory -m user -a "name=ops1 uid=2004 group=devops-team shell=/bin/sh create_home=yes" --become

ansible all -i inventory -m user -a "name=ops2 uid=2005 group=devops-team shell=/bin/sh create_home=yes" --become

ansible all -i inventory -m user -a "name=ops3 uid=2006 group=devops-team shell=/bin/sh create_home=yes" --become
```

📸 Screenshot:
<img width="751" height="366" alt="image" src="https://github.com/user-attachments/assets/377c988d-1f75-4cfe-98da-307868caabec" />
<img width="733" height="211" alt="image" src="https://github.com/user-attachments/assets/4362005a-6597-4ad9-9316-7d7753fd4867" />


---

## Admin Team Users

```bash
ansible all -i inventory -m user -a "name=admin1 uid=2007 group=admin-group shell=/bin/bash create_home=yes" --become

ansible all -i inventory -m user -a "name=admin2 uid=2008 group=admin-group shell=/bin/bash create_home=yes" --become

ansible all -i inventory -m user -a "name=admin3 uid=2009 group=admin-group shell=/bin/bash create_home=yes" --become
```

📸 Screenshot:

---

# 🔐 3. Password Policy Configuration

## Development Team Password Policy

```bash
ansible all -i inventory -m shell -a "chage -M 30 -m 1 -W 5 dev1" --become

ansible all -i inventory -m shell -a "chage -M 30 -m 1 -W 5 dev2" --become

ansible all -i inventory -m shell -a "chage -M 30 -m 1 -W 5 dev3" --become
```

📸 Screenshot:

---

## DevOps Team Password Policy

```bash
ansible all -i inventory -m shell -a "chage -M 20 -m 1 -W 7 ops1" --become

ansible all -i inventory -m shell -a "chage -M 20 -m 1 -W 7 ops2" --become

ansible all -i inventory -m shell -a "chage -M 20 -m 1 -W 7 ops3" --become
```

📸 Screenshot:

---

## Admin Team Password Policy

```bash
ansible all -i inventory -m shell -a "chage -M 15 -m 1 -W 10 admin1" --become

ansible all -i inventory -m shell -a "chage -M 15 -m 1 -W 10 admin2" --become

ansible all -i inventory -m shell -a "chage -M 15 -m 1 -W 10 admin3" --become
```

📸 Screenshot:

---

# 🛡️ 4. Configure Sudo Access

## Admin Group Full Sudo Access

```bash
ansible all -i inventory -m copy -a "dest=/etc/sudoers.d/admin-group content='%admin-group ALL=(ALL) NOPASSWD:ALL' mode=0440" --become
```

📸 Screenshot:

---

## DevOps Team Limited Sudo Access

```bash
ansible all -i inventory -m copy -a "dest=/etc/sudoers.d/devops-team content='%devops-team ALL=(ALL) /usr/bin/systemctl' mode=0440" --become
```

📸 Screenshot:

---

# 📁 5. Create Directory Structure

## Create Root-Level Directories

```bash
ansible all -i inventory -m file -a "path=/teams state=directory mode=0755" --become

ansible all -i inventory -m file -a "path=/projects state=directory mode=0755" --become

ansible all -i inventory -m file -a "path=/shared state=directory mode=0777" --become

ansible all -i inventory -m file -a "path=/archive state=directory mode=0555" --become

ansible all -i inventory -m file -a "path=/admin state=directory owner=root group=admin-group mode=0770" --become
```

📸 Screenshot:

---

## Create Team Directories

```bash
ansible all -i inventory -m file -a "path=/teams/dev-team state=directory owner=root group=dev-team mode=0770" --become

ansible all -i inventory -m file -a "path=/teams/devops-team state=directory owner=root group=devops-team mode=0770" --become
```

📸 Screenshot:

---

## Create Project Directories

```bash
ansible all -i inventory -m file -a "path=/projects/WebApp state=directory owner=dev1 group=dev-team mode=0775" --become

ansible all -i inventory -m file -a "path=/projects/API state=directory owner=ops1 group=devops-team mode=0775" --become

ansible all -i inventory -m file -a "path=/projects/Mobile state=directory owner=dev2 group=dev-team mode=0775" --become
```

📸 Screenshot:

---

## Create Personal Workspaces

```bash
ansible all -i inventory -m file -a "path=/home/dev1/workspace state=directory owner=dev1 group=dev-team mode=0750" --become

ansible all -i inventory -m file -a "path=/home/dev2/workspace state=directory owner=dev2 group=dev-team mode=0750" --become

ansible all -i inventory -m file -a "path=/home/dev3/workspace state=directory owner=dev3 group=dev-team mode=0750" --become

ansible all -i inventory -m file -a "path=/home/ops1/workspace state=directory owner=ops1 group=devops-team mode=0750" --become

ansible all -i inventory -m file -a "path=/home/ops2/workspace state=directory owner=ops2 group=devops-team mode=0750" --become

ansible all -i inventory -m file -a "path=/home/ops3/workspace state=directory owner=ops3 group=devops-team mode=0750" --become

ansible all -i inventory -m file -a "path=/home/admin1/workspace state=directory owner=admin1 group=admin-group mode=0750" --become

ansible all -i inventory -m file -a "path=/home/admin2/workspace state=directory owner=admin2 group=admin-group mode=0750" --become

ansible all -i inventory -m file -a "path=/home/admin3/workspace state=directory owner=admin3 group=admin-group mode=0750" --become
```

📸 Screenshot:

---

# 🔐 6. Configure ACL Permissions

## Install ACL Package

```bash
ansible all -i inventory -m package -a "name=acl state=present" --become
```

📸 Screenshot:

---

## Personal Workspace ACL

```bash
ansible all -i inventory -m shell -a "setfacl -m g:dev-team:rx /home/dev1/workspace" --become
```

> Similar ACL commands were executed for all remaining user workspaces.

📸 Screenshot:

---

## Team Directory ACL

```bash
ansible all -i inventory -m shell -a "setfacl -m g:devops-team:rx /teams/dev-team" --become

ansible all -i inventory -m shell -a "setfacl -m g:dev-team:rx /teams/devops-team" --become
```

📸 Screenshot:

---

## Project Directory ACL

```bash
ansible all -i inventory -m shell -a "setfacl -m g:dev-team:rwx /projects/WebApp" --become

ansible all -i inventory -m shell -a "setfacl -m g:devops-team:rx /projects/WebApp" --become

ansible all -i inventory -m shell -a "setfacl -m g:devops-team:rwx /projects/API" --become

ansible all -i inventory -m shell -a "setfacl -m g:dev-team:rx /projects/API" --become

ansible all -i inventory -m shell -a "setfacl -m g:dev-team:rwx /projects/Mobile" --become

ansible all -i inventory -m shell -a "setfacl -m g:devops-team:rx /projects/Mobile" --become
```

📸 Screenshot:

---

## Shared Resources ACL

```bash
ansible all -i inventory -m shell -a "setfacl -m g:dev-team:rwx /shared" --become

ansible all -i inventory -m shell -a "setfacl -m g:devops-team:rwx /shared" --become

ansible all -i inventory -m shell -a "setfacl -m g:admin-group:rwx /shared" --become
```

📸 Screenshot:

---

## Archive ACL

```bash
ansible all -i inventory -m shell -a "setfacl -m g:dev-team:rx /archive" --become

ansible all -i inventory -m shell -a "setfacl -m g:devops-team:rx /archive" --become

ansible all -i inventory -m shell -a "setfacl -m g:admin-group:rx /archive" --become
```

📸 Screenshot:

---

## Admin Area ACL Cleanup

```bash
ansible all -i inventory -m shell -a "setfacl -b /admin" --become
```

📸 Screenshot:

---

# ✅ 7. Verification Commands

## Check User Information

```bash
ansible all -i inventory -m shell -a "id dev1"
```

📸 Screenshot:

---

## Check Directory Permissions

```bash
ansible all -i inventory -m shell -a "ls -ld /teams /projects /shared /archive /admin"
```

📸 Screenshot:

---

## Check ACL Permissions

```bash
ansible all -i inventory -m shell -a "getfacl /teams/dev-team" --become
```

📸 Screenshot:

---

## Verify Sudo Access

```bash
ansible all -i inventory -m shell -a "sudo -l -U admin1" --become
```

📸 Screenshot:

---

# 🎯 Conclusion

Successfully implemented a complete User and Project Management System using Ansible ad-hoc commands with:

- Team and user management
- Password policies
- Sudo access control
- Structured directory hierarchy
- ACL-based permission management
- Secure access control matrix

This assignment demonstrates practical Linux administration and Ansible automation concepts used in real-world DevOps environments.

---

# 👨‍💻 Author

**Name:** Your Name  
**Project:** UserManager Project Management System  
**Technology:** Ansible Ad-Hoc Commands  
**Assignment:** Ansible Assignment-1
