# Process management 

## 1. To list all process 
```
ps -ef
```

## 2. To list all search matched process 
```
ps -ef | grep -i <process_name>
```
Or 
```
ps -ef | grep -i <process_Id>
```
Or
```
ps -ef | grep -i <user_name>
```

## 3. To kill a process 
To kill a process normally 
```
kill <PID>
```

To kill a process forcefully 
```
kill -9 <PID>
```

## 4. To get the childern process and PID of any process 
```
ptree <PID>
```