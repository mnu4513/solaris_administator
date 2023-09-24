# Archive

## TAR

### 1. To compress file in a tar file 
To compress a single file in a tar file 
```
tar -cvf <new_tar_file_name> <file_name>
```

To compress multiple files in a tar file 
```
tar -cvf <new_tar_file_name> <file1> <file2> <file3>
```
When we compress files in a tar file then it won't change existing file's extension, it will create a new tar file and original files will remain same.

### 2. To see files inside a ter file 
```
tar -tvf <tar_file_name>
```

### 3. To extrect files from a tar file 
```
tar -xvf <tar_file_name>
```
After extracting the files from a tar file, tar file will remain same.

## ZIP

### 1. To compress file in a gzip file 
To compress a single file in gzip file 
```
gzip <file_name>
```

To compress multiple files in gzip files at a time 
```
gzip <file1> <file2> <file3>
```
When we compress files in a gzip file, then it doesn't create a new file. It converts original file in a gzip file and changes its extension into .gz format.

### 2. To read content of a gzip file 
```
gzcat <gzip_file_name>
```

### 3. To extract file from a gzip file
```
gunzip <gzip_file_name>
```