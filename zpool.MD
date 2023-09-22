1. First of all we will check the list of pool on our machine 

```
root@solaris:~# zpool list
NAME    SIZE  ALLOC   FREE  CAP  DEDUP  HEALTH  ALTROOT
rpool  15.6G  5.05G  10.6G  32%  1.00x  ONLINE  -
```

Over here we can see only one pool that is default rpool.

2. Now we will check availabe free disks to create new pools.
If we don't have free disks then we have to run a command to generate the WWN ( world wide name ) and after generating WWN number we will provide it to DB team and they will provide disk ID to us.

```
root@solaris:~# fcinfo hba-port
No Adapters Found.
```

Now we are getting "No Adepters Found" as output because we are not working on actual server but on actual server it will generate WWN number.

On our local machine we can add multiple local disks from our virtual machine's setting.

3. After adding disks we have to refresh the disks.

```
root@solaris:~# devfsadm
```

If this command doesn't work then we have to use another command to refresh the disks.

```
root@solaris:~# cfgadm -al
Ap_Id                          Type         Receptacle   Occupant     Condition
c2                             scsi-bus     connected    configured   unknown
c2::dsk/c2t0d0                 disk         connected    configured   unknown
c2::dsk/c2t1d0                 disk         connected    configured   unknown
```

But in some cases these both command won't work to refresh newly added disks, then we have to reboot the system.

4. Now we can list our all disks after refreshing.

```
root@solaris:~# echo | format
Searching for disks...done

AVAILABLE DISK SELECTIONS:
       0. c2t0d0 <VMware,-VMware Virtual S-1.0-16.00GB>
          /pci@0,0/pci15ad,1976@10/sd@0,0
       1. c2t1d0 <VMware,-VMware Virtual S-1.0 cyl 1022 alt 2 hd 64 sec 32>
          /pci@0,0/pci15ad,1976@10/sd@1,0
       2. c2t2d0 <VMware,-VMware Virtual S-1.0 cyl 1022 alt 2 hd 64 sec 32>
          /pci@0,0/pci15ad,1976@10/sd@2,0
       3. c2t3d0 <VMware,-VMware Virtual S-1.0 cyl 1022 alt 2 hd 64 sec 32>
          /pci@0,0/pci15ad,1976@10/sd@3,0
```

After checking list of disks we will get disk Id.

5. Now we will create a normal with a single disk.
To create a normal pool with a single disk we will use command 
```
zpool create <pool_name> <disk_Id>
```

```
root@solaris:~# zpool create pool_1 c2t1d0
```

To crate offline pool 
```
zfs offline <pool_name> <disk_Id>
```

6. After creating this pool we will check pool list again using command 

```
root@solaris:~# zpool list
NAME     SIZE  ALLOC   FREE  CAP  DEDUP  HEALTH  ALTROOT
pool_1  1008M    85K  1008M   0%  1.00x  ONLINE  -
rpool   15.6G  5.05G  10.6G  32%  1.00x  ONLINE  -
```

Over here we can see a newly added pool and its size, health also.

7. To check more detials about a pool we will use a command 
```
zpool status <pool_name>
```

```
root@solaris:~# zpool status pool_1
  pool: pool_1
 state: ONLINE
  scan: none requested
config:

        NAME      STATE     READ WRITE CKSUM
        pool_1    ONLINE       0     0     0
          c2t1d0  ONLINE       0     0     0

errors: No known data errors
```

This command is showing status, configuration, errors, name, size, health and used disks etc.

By these simple stapes we have successfully creaed a normal pool with a single disk.

8. To check status of all pools we will use command 
```
zpool status
```

```
root@solaris:~# zpool status
  pool: pool_1
 state: ONLINE
  scan: none requested
config:

        NAME      STATE     READ WRITE CKSUM
        pool_1    ONLINE       0     0     0
          c2t1d0  ONLINE       0     0     0
          c2t2d0  ONLINE       0     0     0

errors: No known data errors

  pool: rpool
 state: ONLINE
  scan: none requested
config:

        NAME      STATE     READ WRITE CKSUM
        rpool     ONLINE       0     0     0
          c2t0d0  ONLINE       0     0     0

errors: No known data errors
```

9. Now if we want to destroy this created pool then we will use a command 
```
zpool destroy <pool_name>
```

```
root@solaris:~# zpool destroy pool_1
```

Now this pool won't be visible in zpool list.

10. To list down all destroied pools we will use command 
```
root@solaris:~# zpool import -D
  pool: pool_1
    id: 2408904489532631474
 state: ONLINE (DESTROYED)
action: The pool can be imported using its name or numeric identifier.
config:

        pool_1    ONLINE
          c2t1d0  ONLINE
```

This will show us all destroied pools with their name, id and status with disk Id.

11. To import back a destroied pool we will use command 
```
zpool import -D <pool_name>
or
zpool import -D <pool_Id>
```

```
root@solaris:~# zpool import -D pool_1
```

Now if we will list down all pools then this pool will be visible over there as before.

12. To export a pool we will use command 
```
zpool export <pool_name>
```

```
root@solaris:~# zpool export pool_1
```

Now this pool will not be visible in pools list after exported.

13. To list down all the exported pools we will use the command 
```
zpool import
```

```
root@solaris:~# zpool import
  pool: pool_1
    id: 2408904489532631474
 state: ONLINE
action: The pool can be imported using its name or numeric identifier.
config:

        pool_1    ONLINE
          c2t1d0  ONLINE
```

14. To import a alreday exported pool we will use command 
```
zpool import <pool_name>
or 
zpool import <pool_Id>
```

After importing a pool we can see it in zpool list.

15. To add a new disk in a already created pool we will use command 
```
zpool add <pool_name> <new_disk_Id>
```
```
root@solaris:~# zpool add pool_1 c2t2d0
```

Now will check status of pool after adding a new disk and we will find disk's detials in pool's status.

```
root@solaris:~# zpool status pool_1
  pool: pool_1
 state: ONLINE
  scan: none requested
config:

        NAME      STATE     READ WRITE CKSUM
        pool_1    ONLINE       0     0     0
          c2t1d0  ONLINE       0     0     0
          c2t2d0  ONLINE       0     0     0

errors: No known data errors
```

Now we can see there are two disk added with this pool and the pool size will be increased.

16. If we want to create a normal pool with multiple disks then we have to use command 

```
zpool create <pool_name> <disk1_id> <disk2_id>
```

```
root@solaris:~# zpool create pool_2 c2t1d0 c2t2d0
'pool_2' successfully created, but with no redundancy; failure of one
device will cause loss of the pool
```

Pool will be created successfully, but it will show a redundancy warning.
Poll size will be the sum of all added disks size.

17. If we want to create a mirror pool then we have provide two disks at the time of creation of the pool
```
zpool create <pool_name> mirror <disk1_id> <disk2_id>
```

```
root@solaris:~# zpool create pool_3 mirror c2t1d0 c2t2d0
```

In this case 2nd disk's sapce must be equal or more than 1st disk.

Pool size will be equal to 1st disk's space.

To check the pool's details we will see pool's status.

```
root@solaris:~# zpool status pool_3
  pool: pool_3
 state: ONLINE
  scan: none requested
config:

        NAME        STATE     READ WRITE CKSUM
        pool_3      ONLINE       0     0     0
          mirror-0  ONLINE       0     0     0
            c2t1d0  ONLINE       0     0     0
            c2t2d0  ONLINE       0     0     0

errors: No known data errors
```

Over here we can see in its status, this is a mirror pool.
By this simple step we have sucessfully created a mirror pool.

18. To convert a normal pool into a mirror pool we have to attach disk with that pool 
```
zpool attach <pool_name> <primary_disk_id> <new_disk_id>
```

```
root@solaris:~# zpool attach pool_1 c2t1d0 c2t2d0
```

Over here new disk's size must be equal or more then the primary disk.

19. To detach or to convert a mirror pool into a normal pool we have detach a disk from the pool.
we can detach primary or secondary disk.
```
zpool detach <pool_name> <disk_id>
```
```
root@solaris:~# zpool detach pool_1 c2t1d0
```

After detaching of disk pool will be converted into a normal pool.

20. If we get an error in a pool and we want to clear the error from the pool then we will use command 
```
zpool clear <pool_name>
```

```
root@solaris:~# zpool clear pool_1
```

21. To get the history of the pools we will use the command 
```
zpool history 
```
```
root@solaris:~# zpool history
History for 'pool_1':
2023-09-22.22:16:37 zpool create pool_1 c2t1d0
2023-09-22.22:17:04 zpool attach pool_1 c2t1d0 c2t2d0
2023-09-22.22:19:42 zpool detach pool_1 c2t1d0
2023-09-22.22:28:02 zpool clear pool_1

History for 'rpool':
2023-09-22.09:39:44 zpool create -f -B rpool c2t0d0
2023-09-22.09:39:47 zfs create -p -V 1024.0m rpool/dump
2023-09-22.09:39:48 zfs create -p -V 1024.0m rpool/swap
2023-09-22.09:39:53 zfs set primarycache=metadata rpool/swap
2023-09-22.10:06:16 zfs set primarycache=metadata rpool/swap
2023-09-22.10:06:16 zfs create -o mountpoint=/system/zones rpool/VARSHARE/zones
```

This will show all pools history.

If we want to check history of a particular pool then we have to provide pool's name with above command 
```
zpool history <pool_name>
```

```
root@solaris:~# zpool history pool_1
History for 'pool_1':
2023-09-22.22:16:37 zpool create pool_1 c2t1d0
2023-09-22.22:17:04 zpool attach pool_1 c2t1d0 c2t2d0
```

22. To take the pool's intigirity 
```
zpool scrub <pool_name>
```

```
root@solaris:~# zpool scrub pool_1
```

23. To check the pools status ( health )
```
root@solaris:~# zpool status -xv
all pools are healthy
```

24. If we want to create a ZFS file system then first of all we have to create a pool.
This will create a file system by default.
Now we can check its data set name, mountpoint, size, etc. using the command 
``` 
root@solaris:~# df -h
Filesystem             Size   Used  Available Capacity  Mounted on
rpool/ROOT/solaris      15G   2.8G        10G    22%    /
/devices                 0K     0K         0K     0%    /devices
/dev                     0K     0K         0K     0%    /dev
pool_1                 976M    31K       976M     1%    /pool_1
```

25. if we want to create childern file systems then we have to use command 
```
zfs create <pool_name>/<child_name>
```
```
root@solaris:~# zfs create pool_1/child1
```

Now we can check this newly created child file system's detials using that smae command 
```
df -h
```

26. If we want to change mountpoint name then we will use the command 
```
zfs set mountpoint=/<new_mountpoint_name> <data_set_name>
```

```
root@solaris:~# zfs set mountpoint=/child1 pool_1/child1
```