## VI editor

### To open a file in VI editor
```
vi <file_name>
```
```
root@solaris:~# vi file.txt
```
If file alredy exits then it will open in VI editor else new file will be created with same name.
We can also provide file's path rather than file's name.

```
vi <file_path>
```
```
root@solaris:~# vi /root/file.txt
```

### To enter in insert mode 
To simply enter in insert we will press "i".
```
i
```

### To enter in insert mode after currnet cursor 
To enter in insert mode after present character we will press "a".
```
a
```

### To enter in insert mode in a new line 
To enter in insert mode in a new line we will press "o".
```
o
```

### To come out of insert mode 
To come out of the insert mode we will press "Esc" key.
```
Esc
```

### To delete a character 
To delete a character we will press "x" after coming out of insert mode. 
```
Esc + x
```

### To delete a line 
To delete a line we will press "dd" after coming out of insert mode.
```
Esc + dd
```

### To exit the VI editor without saving changes 
To exit the VI editor without saving the changes we will press "Esc" + ":!" keys.
```
Esc + :!
```

### To save changes 
To save the changes in a file we will press "Esc" + ":wq" keys.
```
Esc + wq
```
Or we can use "Esc" + ":x" keys to save changes in a file.
```
Esc + :x
```

### To save changes forcefully in a read only file
To save changes forcefully in a read only file we will press "Esc" + ":wq!" keys.
```
Esc + :wq!
```