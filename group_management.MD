# Group management 

To know the all groups details 
```
cat /etc/group
```
This file contains 4 fields about the groups.
1. Group name
2. Password 
3. Group ID 
4. User list

* One user can be added in 16 groups, over there one will be primary group and others will be secondary groups.

## To create a group with default configuration 
```
groupadd <group_name>
```

## To create a group with custom group ID
```
groupadd -g <GID> <group_name>
```

## To modify GID for a group 
```
groupmod -g <new_GID> <group_name>
```

## To modify a group name
```
groupmod -n <group_new_name> <group_name>
```

## To delete a group 
```
groupdel <group_name>
```
