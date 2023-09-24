# Network Configuration 

## Creating / deleting simple links or IP addresses 

1. To check IP address 
```
root@solaris:~# ipadm
NAME              CLASS/TYPE STATE        UNDER      ADDR
ipmp1             ipmp       down         --         --
lo0               loopback   ok           --         --
   lo0/v4         static     ok           --         127.0.0.1/8
   lo0/v6         static     ok           --         ::1/128
net0              ip         ok           --         --
   net0/v4        dhcp       ok           --         192.168.10.130/24
   net0/v6        addrconf   ok           --         fe80::20c:29ff:fee7:1dee/10
```
Or
```
root@solaris:~# ifconfig -a
lo0: flags=2001000849<UP,LOOPBACK,RUNNING,MULTICAST,IPv4,VIRTUAL> mtu 8232 index 1
        inet 127.0.0.1 netmask ff000000
net0: flags=100001004843<UP,BROADCAST,RUNNING,MULTICAST,DHCP,IPv4,PHYSRUNNING> mtu 1500 index 17
        inet 192.168.10.130 netmask ffffff00 broadcast 192.168.10.255
        ether 0:c:29:e7:1d:ee
```
Or 
```
root@solaris:~# ipadm show-addr
ADDROBJ           TYPE     STATE        ADDR
lo0/v4            static   ok           127.0.0.1/8
net0/v4           dhcp     ok           192.168.10.130/24
lo0/v6            static   ok           ::1/128
net0/v6           addrconf ok           fe80::20c:29ff:fee7:1dee/10
```

2. To check availavle links 
```
root@solaris:~# dladm show-link
LINK                CLASS     MTU    STATE    OVER
net0                phys      1500   up       --
net4                phys      1500   up       --
net3                phys      1500   up       --
net2                phys      1500   up       --
net1                phys      1500   up       --
```

3. To check physical links 
```
root@solaris:~# dladm show-phys
LINK              MEDIA                STATE      SPEED  DUPLEX    DEVICE
net0              Ethernet             up         1000   full      e1000g0
net4              Ethernet             up         1000   full      e1000g4
net3              Ethernet             up         1000   full      e1000g3
net2              Ethernet             up         1000   full      e1000g2
net1              Ethernet             up         1000   full      e1000g1
```

4. To check mac address of links 
```
root@solaris:~# dladm show-phys -m
LINK                SLOT     ADDRESS            INUSE CLIENT
net0                primary  0:c:29:e7:1d:ee    yes   net0
net4                primary  0:c:29:e7:1d:2     no    --
net3                primary  0:c:29:e7:1d:c     no    --
net2                primary  0:c:29:e7:1d:f8    yes   net2
net1                primary  0:c:29:e7:1d:16    yes   net1
```

5. To create a new link 
```
ipadm create-ip <link_name>
```
```
root@solaris:~# ipadm create-ip net1
```
After creating new link, this link be in down state untill we don't add an IP address with this link.

6. To add an IP address to the link
```
ipadm create-addr -T <type_of_address> -a <IP> <link_name>
```
```
root@solaris:~# ipadm create-addr -T static -a 192.168.10.131 net1
net1/v4
```

Now state of newly created link will be OK after adding an IP address to it.

7. To delete an IP address 
```
ipadm delete-addr <address_name>
```
```
root@solaris:~# ipadm delete-addr net1/v4
```

8. To delete a link 
```
ipadm delete-ip <link_name>
```
```
root@solaris:~# ipadm delete-ip net1
```


## Creating / modifying / deleting of IPMP group 

1. To create an IPMP group 
```
ipadm create-ipmp <IPMP_group_name>
```
```
root@solaris:~# ipadm create-ipmp ipmp_1
```
After creating of IPMP group this will be in failed state by default.

2. To add links in an IPMP group 
```
ipadm add-ipmp -i <first_link> -i <second_link> <IPMP_group_name>
```
```
root@solaris:~# ipadm add-ipmp -i net1 -i net2 ipmp_1
```
Now state of this IPMP group will be changed to down form failed.

3. To assign an IP address to a newly created IPMP group 
```
ipadm create-addr -T <type_of_address> -a <IP_address> <IPMP_group_name>
```

```
root@solaris:~# ipadm create-addr -T static -a 192.168.10.131 ipmp_1
```
Now after assigning an IP address to this IPMP group, its status will be changed to OK from down.

4. To check group status 
```
root@solaris:~# ipmpstat -g
GROUP       GROUPNAME   STATE     FDT       INTERFACES
ipmp_1      ipmp_1      ok        --        net2 net1
```

5. To check interface, active status, group, flags and state of links those added in an IPMP group 
```
root@solaris:~# ipmpstat -i
INTERFACE   ACTIVE  GROUP       FLAGS     LINK      PROBE     STATE
net2        yes     ipmp_1      -------   up        disabled  ok
net1        yes     ipmp_1      --mbM--   up        disabled  ok
```

6. To disable a link in an IPMP group 
```
ipadm if_mpadm <link_name>
```
```
root@solaris:~# if_mpadm -d net1
```
Output will look like 
```
root@solaris:~# ipmpstat -i
INTERFACE   ACTIVE  GROUP       FLAGS     LINK      PROBE     STATE
net2        yes     ipmp_1      --mbM--   up        disabled  ok
net1        no      ipmp_1      -----d-   up        disabled  offline
```
After disabling a link link will be offline and other link will use for address.

7. Enable a link after disabling 
```
if_mpadm -r <link_name>
```
```
root@solaris:~# if_mpadm -r net1
```
Output will look like 
```
root@solaris:~# ipmpstat -i
INTERFACE   ACTIVE  GROUP       FLAGS     LINK      PROBE     STATE
net2        yes     ipmp_1      --mbM--   up        disabled  ok
net1        yes     ipmp_1      -------   up        disabled  ok
```

8. To delete an IPMP group 
```
ipadm delete-ipmp <IPMP_group_name>
```
Group must be empty before deleting.
```
root@solaris:~# ipadm delete-ipmp ipmp_1
ipadm: cannot delete IPMP interface ipmp_1: IPMP group is not empty
```

## Creating virtual network interface card (VNIC)

1. To create a VNIC 
```
dladm create-vnic -l <physical_link_name> <VINC_name>
```
```
root@solaris:~# dladm create-vnic -l net0 vnic1
```

2. To check created VNIC
```
root@solaris:~# dladm show-link
LINK                CLASS     MTU    STATE    OVER
net0                phys      1500   up       --
net1                phys      1500   up       --
```

3. To bring it in down state 
```
ipadm create-ip <VNIC_name>
```
```
root@solaris:~# ipadm create-ip vnic1
```

4. To delete VNIC 
Before deleting a VNIC it must be empay and must not be in down or in OK state.
```
dladm delete-vnic <VNIC_name>
```
```
root@solaris:~# dladm delete-vnic vnic1
dladm: vnic deletion failed: link busy
```