### Link 
Link is a shortcut to access any file.

Link has two types 
1. Soft link
2. Hard link

### Difference between soft and hard link
1. If original file is deleted then soft link won't work but in such hard link will work.
2. Soft link different INODE number, while hard link has same INODE number.

## Soft link creation 

```
ln -s <source> <destination>
```
```
root@solaris:~# ln -s test/file.sh link_1
```

## Hard link creation 
```
ln <source> <destination>
```
```
root@solaris:~# ln test/file.sh link_1
```

## To unlink a link

```
unlink <source>
```
```
root@solaris:~# unlink link_1
```