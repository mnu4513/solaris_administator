# Service management

## 1. To list down all services 
```
svcs -a
```

## 2. To search any service using service name 
```
svcs -a | grep <service_name>
```

## 3. To enable any service 
```
svcs enable <service_name>
```

## 4. To disable any service 
```
svcadm disable <service_name>
```

## 5. To refresh any service 
```
svcadm refresh <service_name>
```

## 6. To restart any service 
```
svcadm restart <service_name>
```

## 7. To list down maintinance services 
```
svcs -xv
```

## 8. To clear any service from the maintinance 
```
svcadm clear <service_name>
```
