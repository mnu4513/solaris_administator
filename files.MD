# Files 

## To create a file 
```
mkfile <file_size> <file_name>
```
With this command we can create a file with specific size.

```
touch <file_name>
```
With this command we can create an empty file.

```
vi <file_name>
```
With this command we can create a text file and this file will open in VI editor at creation time.

## To read a file 
To read whole content of a file
```
cat <file_name>
```

To read file's content head to tail
```
cat <file_name> | more 
```

To read file's content tail to head 
```
cat <file_name> | less
```

## To list files 
To list all normal files 
```
ls 
```

To list all files including hidden files 
```
ls -la
```

To list all files with permission, size 
```
ls -latrh
```

## To replace content of a text file
```
cat > <file_name>
```

## To add content in a text file with removing previous content
```
cat >> <file_name>
```

## To remove a file or more files 
To remove a file normally 
```
rm <file_name>
```

To remove a file forcefully 
```
rm -rf <file_name>
```

To remove multiple files at a time 
```
rm -rf <file1> <file2> <file3>
```

## To copy a file 
```
cp <source> <destination>
```

## To move a file 
```
mv <source> <destination>
```

## To rename a file 
```
mv <file_name> <new_name>
```