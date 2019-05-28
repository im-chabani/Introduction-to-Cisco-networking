<p align="center">
  <img src="https://user-images.githubusercontent.com/51119025/58493148-99e9c500-8172-11e9-8f64-2a95425e1feb.png" alt="cisco image">
</p>

## Table of Contents
  1. [Basic networking notions](#1-basic-netwoking-notions)  
    1.1. [Switch](#11-switch)    
    1.2. [Router](#12-router)  
    
    
# 1. Basic networking notions

. Networks are crucial to connect materials in a local or a worldwide area. In order to deploy a network, some notions are required. Therefore, we'll be using Cisco CLI command line as an example (I recomand using cisco's materials or packet tracer).


## 1.1. Switch
### `Description`
. Switches are intelligent devices that connect many machines under the same network. It works on OSI layer 2 (link layer) using mac addresses. It forward, filter or flood (broadcast) <b>frames based on MAC table entries</b>.
 . A switch have many full duplex ports (generally 24 ports) and allows various port speed.

. We can: 
-	Give it a hostname.
-	Give it an ip address and default gateway through VLANs (vlan1 is up by default on all interfaces).
-	Give a password (clear, encrypted or secret) to :

    o	EXEC user mode.
    
    o	EXEC privileged mode.
    
    o	Line VTY from 0 to 15 (allows SSH and telnet communication for remote control).
    
    o	Line console.
-	Show : 

o	Device’s properties.

o	Interface’s (physical and virtual) properties.


### `CLI commands`

(enable) Display configuration and properties of the device:
``` 
show version
```

(enable) Display fa0/1 configuration:
```
show interfaces fastethernet 0/1
```

(config t) Change the switch’s name to name:
```
hostname name
```

(config t) Gives the default gateway of a connected router:
```
ip default-gateway A.B.C.D
```

(config t) Assign an ip address and a subnet mask for the switch trough vlan:
```
interface vlan 1
ip address A.B.C.D 255.255.255.0
no shutdown
```

(config t) Create a user so he can enter the switch (EXEC user mode only):
```
username user1 password cisc0
```

(config t) Create a user so he can enter the switch (EXEC privileged mode): 
```
username user1 privilege 15 secret cisc0
```

(config t) Create a secret secure password for the exec privileged mode:
```
enable secret cisc0

```
(config t) Gives a password to the line console 0 (the line from which we can modify directly the switch):
```
line console 0
password cisc0
login
exit

```

(config t) Gives a password from vty line 0 to vty line 15 :
```
line vty 0 15
password cisc0
login
exit
```

(config t) Crypt all passwords with a weak hash :
```
service password-encryption
 ```

(config-line) It will stop the sessions after 1mnt and 13 seconds. Note if we put 0 0, it will be stopped immediately.
```
exec-timeout 1 13
````








