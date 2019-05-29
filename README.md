<p align="center">
  <img src="https://user-images.githubusercontent.com/51119025/58493148-99e9c500-8172-11e9-8f64-2a95425e1feb.png" alt="cisco image">
</p>

## Table of Contents
  1. [Building a simple network](#1-Building-a-simple-network)   
    1.1. [Switch configuration](#11-Switch-configuration)    
    1.2. [Memory management](#12-memory-management)  
    
  2. [Establishing Internet Connectivity](#2-establishing-internet-connectivity)      
    2.1. [Understanding TCP/IP transport layer](#21-understanding-tcp-ip-transport-layer)          
    2.2. [Exploring the function of routing](#22-Exploring-the-function-of-routing)     
    2.3. [Configuring static routing](#23-configuring-static-routing)      
    2.4. [Managing traffic using ACLs](#24-managing-traffic-using-acls)      
    2.5. [Establishing INTERNET connection](#25-Establishing-internet-connection)      
    
    
 <br>
# 1. Building a simple network

. Networks are crucial to connect materials in a local or a worldwide area. In order to deploy a network, some notions are required. Therefore, we'll be using Cisco CLI command line as an example (I recomand using cisco's materials or packet tracer).

. Before starting the course, those general CLI commands might be helpful:

(enable) Show the 50 last executed commands:
```	
terminal history size 50 
``````

(enable) Show the last commands:	
```
show history 
```

(enable) Number of lines in the current terminal screen:
```	
terminal length 100
```

(enable) Show only the Y found on X:
(example : X running-configuration , Y interface)
```	
show X | include Y
```

(enable) Show the whole section in X that contains Y (doesn't work on the actual packet tracer):
```	
show X | section Y
```

<br>

## 1.1. Switch configuration
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

<br>

## 1.2. Memory management
### `Description`
. There are 2 types of configuration:
-	Running configuration : 
    o	Saved on <b>RAM</b> (not permanent, disappear on reload). 
    o	Reflects the current configuration.

-	Startup configuration : 
    o	Saved on <b>NVRAM</b> (permanently, reloads after a reboot).

- If there’s no configuration yet, the device will enter the setup mode or load a blank configuration from the <b>flash memory</b>.

- The last type of memories is the <b>ROM memory</b>. It stores the bootstrap program that is used to initialize the boot process.


### `CLI commands`

(user) Show the actual config:	
```
Show running-config
``` 

(user) Show the after reload config:
```
Show startup-config
```

(enable) Save the actual config permanently on the NVRAM:
```
copy running-config startup-config
```

(enable) Save the actual config permanently on the TFTP server:
```
copy running-config tftp
```

(enable) Write in the NVRAM memory:	
```
write 
```
Or
```
write memory
```

<br>
# 2. Establishing Internet Connectivity     
## 2.1. Understanding TCP/IP transport layer     
(on going)
<br>

## 2.2. Exploring the function of routing     
## 2.3. Configuring static routing      
## 2.4. Managing traffic using ACLs      
## 2.5. Establishing INTERNET connection     

