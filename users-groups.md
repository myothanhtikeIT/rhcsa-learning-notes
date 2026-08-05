# User & Group Management
- Three big categories in Linux system, or Linux has three fundamental security concepts:
    - users
    - groups
    - permissions

### What is a User?
- A user is simply an identity.
- Every users has IDs, Linux OS itself doens't care about names, it cares about ID, such as UID 1000, root. would be UID 0. Groups also has number (aka IDs), developers GID 1050. 

Users are stored in ```/etc/passwd```
Example : ```john:x:1000:1000:John Smit:/home/john:/bin/bash```
* Everything about a user lives here.
Passwords used to stored in ```/etc/passwd```, Nowadays they're stored in ```/etc/shadow```

### What is a Group?
- If a company has like departments such as IT, HR, Finance, Marketing, instead of giving permission to users like John, Jack, Maria etc, one by one, could be assigned relvant users to their each departments with their access level and permissions. 

For example, ```/shared/projects``` belongs to Groups of Devs, anyone inside that Dev group can access it, no longer needed to configred 50 users individually. 
Groups live here ```/etc/group```
Example : ```developers:x:1050:david,john,sarah```

### What is a root?
- UID = 0
Root can
- delete anything
- modify anything
- kill any process
- create users
- change passwords
- mount disks
- install software

* Root ignores almost every permission check.
### Linux doesn't give permissions directly to users very often. It usually gives permissions to groups, then puts users into those groups.
---
# Lab-Users, Depts and permissions
## Hospital
---PENDING---
---
