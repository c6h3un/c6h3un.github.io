# [Ubuntu] User security
## Configurations
### Setup password expiration time and umask value for all users
```
$ sudo vim /etc/login.defs
UMASK		027
PASS_MAX_DAYS	90
PASS_MIN_DAYS	7
PASS_WARN_AGE	15
```
### Unlock user expiration time
- unlimit
```
$ sudo passwd -x -1 user
```
- setup min/max days
```
$ sudo passwd -n 7 -x 90 -w 15 user
# Or use chage
$ sudo chage -m 7 -M 90 -W 15 user
```

### Check user password info
```
$ sudo chage -l user
---
# Before
Last password change					: Jun 14, 2017
Password expires					: never
Password inactive					: never
Account expires						: never
Minimum number of days between password change		: 0
Maximum number of days between password change		: 99999
Number of days of warning before password expires	: 7
---
# After
sudo chage -l user
Last password change					: Jun 14, 2017
Password expires					: Sep 12, 2017
Password inactive					: never
Account expires						: never
Minimum number of days between password change		: 7
Maximum number of days between password change		: 90
Number of days of warning before password expires	: 15
---
```
#### check dir/files privileges
```
$ mkdir tmp
$ touch temp
$ ls -l
-rw-rw---- 1 helen helen        0 Feb 21 14:02 temp
drwxrwx--- 2 helen helen     4096 Feb 21 13:59 tmp
```

## Appendix - umask

| umask | directory | file |
|-------|-----------|------|
| 022   | 755       | 644  |
| 027   | 750       | 640  |
| 002   | 775       | 664  |
| 006   | 771       | 660  |
| 007   | 770       | 660  |

- if enable setting of the umask group to be the same as owner bits for non-root users, umask value will change from 022 -> 002, 027 -> 007.
```
$ cat /etc/login.defs |grep USERGROUPS_ENAB
USERGROUPS_ENAB yes
```