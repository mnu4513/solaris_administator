## Quota and Reservation 

### Difference between quota and reservation 
    In a very simple term quota is a maximum size can be used by a file system and if it has free space then other sibling or patent file system can use it's space too.
    Reservation is the reserved space for a particular file system, this file system can use its parent's or sibling's sapce if it is out of sapce, but its parent or siblings can't use its reserved space.

1. To understand quota and reservation we will create a pool using the command 
```
zpool create <pool_name>
```

After creating pool we will create 2 children file system in it using the command 
```
zfs create <pool_name>/<child_name>
```

We can check these newly created file systems using the command 
```
df -h
```

After creating two children file systems we will set their mount points and then we will start distributting quota and reservation.

2. To check quota details for a file system 
```
zfs get quota <data_set_name>
```

```
root@solaris:~# zfs get quota pool_1/child1
NAME           PROPERTY  VALUE  SOURCE
pool_1/child1  quota     none   local
```

If we haven't set quota for a particular file system then it will show none as value by default.

3. To set quota for a file system 
```
zfs set quota=<size> <data_set_name>
```

```
root@solaris:~# zfs set quota=200M pool_1/child1
```

After setting the quota for a file system if we will check the quota detials for that file system then will see the size of quota rahter than none value.

```
root@solaris:~# zfs get quota pool_1/child1
NAME           PROPERTY  VALUE  SOURCE
pool_1/child1  quota     200M   local
```

4. To unset the quota for a file system 
```
zfs set quota=none <data_set_name>
```

```
root@solaris:~# zfs set quota=none pool_1/child1
```

Now if we will check its quota details then will we get none as the quota value.

5. to check reservation details for a file system 
```
zfs get reservation <data_set_name>
```

```
root@solaris:~# zfs get reservation pool_1/child1
NAME           PROPERTY     VALUE  SOURCE
pool_1/child1  reservation  none   default
```

6. To set the reservation value for a file system 
```
zfs set reservation=<size> <data_set_name>
```

```
root@solaris:~# zfs set reservation=200M pool_1/child1
```

7. To unset the reservation for a file system 
```
zfs set reservation=none <data_set_name>
```

```
root@solaris:~# zfs set reservation=none pool_1/child1
```