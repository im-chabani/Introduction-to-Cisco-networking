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

(on going chapter)
<br>

## 2.2. Exploring the function of routing

### `Description`
. Routers are required to reach hosts that are not in the local network. They use routing table to route between networks.
<br>

### `CLI commands`

(enable) Display known ip routes:
```
show ip route 
```

(enable) Show interfaces states:	
```
Show ip interface brief
```

(enable) Show interfaces details:	
```
Show interfaces
```

(enable) CDP: Cisco Discovering Neighbors. Show characteristics of directly connected devices:
```
show cdp neighbors
```

(enable) same thing with more depth:
```
show cdp neighbors detail
```

(config-if) Add a descriptive text to an interface:	
```
interface serial 0/0/0
description Link to isp
```
<br>

## 2.3. Configuring static routing

### `Comparison between static and dynamic routes`

. Static routes: 
-	A network administrator manually enters static routes into the router.
-	A network <b>physical changes</b> requires a manual update to the route.
-	Routing behavior can be precisely controlled.
-	We use static routes when:
    o	It’s a small network.
    o	It’s a hub-and-spoke network topology (star topology).
    o	When you want to create a quick ad-hoc route.

. Dynamic routes:
-	Adjusts dynamically routes when the topology or the traffic changes.
-	Routers learn and maintain routes to the remote destinations by exchanging routing updates.
-	Routers discover new networks by sharing routing table information.
-	We use dynamic routes when :
    o	It’s a large network.
    o	The network is expected to scale.
<br>

### `Configure a static route`
<p align="center">
  <img src="https://user-images.githubusercontent.com/51119025/58522379-65045f00-81c0-11e9-932e-5a4782103089.png" alt="cisco image">
</p>
<br>

. Static route is unidirectional. Which means, we have to configure it to and from a stub (destination) network to allow communication to occur.
. Steps to configure a static route:
-	Define a path to an IP destination network (172.16.1.0 255.255.255.0).
-	Use the IP address of the <b>next-hop router interface</b> (172.16.2.1). OR (less recommanded), use the outbound interface of the <b>local router</b> (serial 0/0/0).

Note :You have to specify routes to <b>all networks</b> that you have to attend on each router.
-	To expand the network outside the local area (eg: internet), We just have to make a default all 0 route (0.0.0.0) that literlly express : all possible routes. 
<br>

### `CLI commands`

R1 (config) Where we want to go + mask + from where should we enter on R2:
```
ip route 172.16.1.0 255.255.255.0 172.16.2.1
```

R1 (config) Gives a default route through the serial port 0/0/1 (concerned router). This method is not recommended:
```
ip route 0.0.0.0  0.0.0.0 serial 0/0/1
```

R1 (config) Gives a default route through the next hop router for any address (0.0.0.0 mask 0.0.0.0). Ex: it’s used for internet:
``` 	
ip route 0.0.0.0  0.0.0.0 172.16.2.1
```

(config) Show our actual static routes:	
```
show ip route
```

<br>

## 2.4. Managing traffic using ACLs

### `Global description`
. ACL are rules (succession of statements) to filter the traffics. It acts like a firewall to the network. They can be applied <b>in</b>bound (going in the router) or <b>out</b>bound (going out the router).
<br>
. The wildcard mask is calculated by: 
-	Putting important bits to 0.
-	Putting unimportant bits to 1.
<br>
<img src="https://user-images.githubusercontent.com/51119025/58551716-00252500-8211-11e9-9da3-c6785eba5481.png" alt="ACL">
<br>
. ACL are read and executed from the 1st to the last statement, so be careful how sorting them! That's being said, there’s always a <b>deny any</b> (implicit deny) at the end of an ACL, which means in other words: every <b>not</b> mentioned permit case will be denied.
<br>
. The advantage of named ACL is that you can modify statements order by adding or removing them. By default, the range between every statement is 10. 
<br>
.Note that <b>Only one ACL per protocol, per direction AND per interface is allowed</b>.

<br>

### `Standard ACL`
### a. `Description`

. With standard ACL we can deny <b>source IP addresses</b> from crossing from a router to another.
<br>
. Standard ACL goes from “1 to 99”, then from “1300 to 1999”.
<br>
. Standard ACL are configured <b>closest to the destination</b> (closest router and closest interface. Which means, mostly outbound). Otherwise, if it is applied closer to the destination, it will block the source from reaching all the networks (not only the selected one).
<br>

### b.1 `CLI commands (std ACL)`

(enable) show configured access-lists:	
```
show access-lists 
```

(config t) Putting our ACL statements. On the below ACL, all 192.168.2.X addresses are denied (sources addresses):
```
access-list 1 deny 192.168.2.0 0.0.0.255
```

(config t) Those two commands are equivalent: 
```
access-list 1 deny 192.168.2.101 0.0.0.0
	
access-list 1 deny host 192.168.2.101
```

(config t) Those two commands are equialent:
```
access-list permit 0.0.0.0 255.255.255.255
	
access-list 1 permit any
```

(config-if) Applying the ACL on the interface. Be careful, out means out the ROUTER.
```
interface fa 0/1
ip access-group 1 out
```

(config-line) Configuring the interface on a LINE not interface, so don’t confuse "class" with “group”.
```
line vty 0 4
ip access-class 1 out
```
<br>

### b.2 `CLI commands (std named ACL)`

(config t) create a named acl OR enter modification mode to the existing named ACL: 
``` 		
ip access-list standard nameACL
```

(config-std-nacl) We add statements only when we enter the named ACL config mode:
```		
deny 192.168.2.101 0.0.0.0
permit 192.168.2.0 0.0.0.255
```

(config-std-nacl) Add a statement between existing statements by adding the number of is line:
```		
9 permit host 192.168.2.3 
```

(config-std-nacl) remove the statement number 10:
```		
no 10
```

(config t) re attribute the sequence numbers to the statements starting from 10 and incrementing by 20:
```
ip access-list resequence nameACL 10 20
```
Example :
```
It goes from this :

9 deny …
10 permit …
20 permit …

to this: 

10 deny …
30 permit …
50 permit …		
```
<br>

## 2.5. Establishing INTERNET connection
