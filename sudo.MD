## Sudo 
Sudo is a privilage or the type of accessability.
Sudo is use to give privilage to a normal user by which, normal user can perform same task as a root user.

## To give sudo privilage a normal user 
1. First of all we will create a normal user with user's home directory.
```
useradd -m -d /export/home/<dir_name> <user_name>
```

```
root@solaris:~# useradd -m -d /export/home/user_1 user_1
80 blocks
```

2. Now we can check user's enrty in /etc/passwd file 
```
root@solaris:~# cat /etc/passwd
user_1:x:100:10::/export/home/user_1:/usr/bin/bash
```

3. Now we will set newly created user's password 
```
passwd <user_name>
```
```
root@solaris:~# passwd user_1
New Password:
Re-enter new Password:
passwd: password successfully changed for user_1
```

4. Now we will make entry of user in sudo configuration file 
```
root@solaris:~# vi /etc/sudoers
```

5. Now we will switch user to normal user
```
su - <user_name>
```
```
root@solaris:~# su - user_1
```

6. After switching user we will provide sudo privilage to this normal user 
```
user_1@solaris:~$ sudo su -
Password:
```
Or
```
user_1@solaris:~$ sudo su -
Password:
```
After entering normal user's password this normal user will be a sudo user.

7. To become a normal user again 
```
root@solaris:~# exit
logout
```
User will be a normal user again.

## Default file of sudo configuration 
```
/etc/sudoers
```
To give permanent sudo privilage to a normal user we will make entry in this file same as root user.

When we will read this file then we will see 
```
root ALL=(ALL) ALL
```
Now in next line we have to make same entry for the normal user
```
<user_name> All=(ALL) ALL
```