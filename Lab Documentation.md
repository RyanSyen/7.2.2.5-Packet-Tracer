# 7.2.2.5-Packet-Tracer
W9 7.2.2.5 Lab - Configuring a Point-to-Point GRE VPN Tunnel

Table contents
--------------
1. Part 1: Configure Basic Device Settings
- step 1
- step 2
- step 3
- step 4
- step 5
- step 6
- step 7
3. Part 2: Configure a GRE Tunnel
- step 1
- step 2
5. Part 3: Enable Routing over the GRE Tunnel
- step 1
- step 2
- step 3

### Topology
![topology](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/topology.png)

### Addressing Table
![addressing table](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/addressing%20table.png)

### Objectives
![objectives](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/obj.png)

### Background / Scenario
![Background](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/background.png)

### Required Resources
![resources](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/resources.png)

## Part 1: Configure Basic Device Settings

### Step 1:	Cable the network as shown in the topology.

### Step 2:	Initialize and reload the routers and switches.

### Step 3:	Configure basic settings for each router.
  a. Disable DNS lookup.
  ``` 
  no ip domain-lookup 
  ```
  ![Disable DNS lookup](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/1.png)
  
  b. Configure the device names.
  ``` 
  Router(config)#h WEST
  WEST(config)#
  ```
  ``` 
  Router(config)#h ISP
  ISP(config)#
  ```
  ``` 
  Router(config)#h EAST
  EAST(config)#
  ```
  ![Configure the device names](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/2.png)
  
  c. Encrypt plain text passwords.
  ``` 
  ISP(config)#service password-encryption
  ```
  ``` 
  EAST(config)#service password-encryption
  ```
  ``` 
  WEST(config)#service password-encryption
  ```
  d. Create a message of the day (MOTD) banner warning users that unauthorized access is prohibited.
``` 
ISP(config)#banner motd $ Unauthorized access is prohibited $  
```
``` 
EAST(config)#banner motd $ Unauthorized access is prohibited $  
```
``` 
WEST(config)#banner motd $ Unauthorized access is prohibited $  
```
e.	Assign class as the encrypted privileged EXEC mode password.
``` 
ISP(config)#enable password class
```
``` 
EAST(config)#enable password class
```
``` 
WEST(config)#enable password class
```
f. Assign cisco as the console and vty password and enable login.
``` 
ISP(config)#line vty 0 15
ISP(config-line)#password cisco
ISP(config-line)#login
ISP(config-line)#exit
```
``` 
EAST(config)#line vty 0 15
EAST(config-line)#password cisco
EAST(config-line)#login
EAST(config-line)#exit
```
``` 
WEST(config)#line vty 0 15
WEST(config-line)#password cisco
WEST(config-line)#login
WEST(config-line)#exit
```
g. Set console logging to synchronous mode.
``` 
ISP(config)#line console 0
ISP(config-line)#logging synchronous
```
``` 
EAST(config)#line console 0
EAST(config-line)#logging synchronous
```
``` 
WEST(config)#line console 0
WEST(config-line)#logging synchronous
```
h. Apply IP addresses to Serial and Gigabit Ethernet interfaces according to the Addressing Table and activate the physical interfaces. Do NOT configure the Tunnel0 interfaces at this time.
**[serial port may differ]**
``` 
ISP(config)#int s0/1/1
ISP(config-if)#ip add 10.1.1.2 255.255.255.252
ISP(config-if)#no shut
ISP(config-if)#int s0/1/0
ISP(config-if)#ip add 10.2.2.2 255.255.255.252
ISP(config-if)#no shut
```
``` 
WEST(config)#int s0/1/1
WEST(config-if)#ip add 10.1.1.1 255.255.255.252
WEST(config)#no shut
WEST(config-if)#int g0/0/1
WEST(config-if)#ip add 172.16.1.1 255.255.255.0
WEST(config)#no shut
```
``` 
EAST(config)#int s0/1/0
EAST(config-if)#ip add 10.2.2.1 255.255.255.252
EAST(config)#no shut
EAST(config-if)#int g0/0/1
EAST(config-if)#ip add 172.16.2.1 255.255.255.0
EAST(config)#no shut
```
i. Set the clock rate to 128000 for DCE serial interfaces.
``` 
ISP(config)#int s0/1/0
ISP(config-if)#clock rate 128000
```
``` 
WEST(config)#int s0/1/1
WEST(config)#clock rate 128000
```
### Step 4:	Configure default routes to the ISP router.
``` 
WEST(config)# ip route 0.0.0.0 0.0.0.0 10.1.1.2
```
``` 
EAST(config)# ip route 0.0.0.0 0.0.0.0 10.2.2.2
```
### Step 5:	Configure the PCs.
| PC-A | PC-C |
| ---- | ---- |
|   ![configure pc-a](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/3.png) | ![configure pc-c](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/4.png) |

### Step 6:	Verify connectivity.
| PC-A | PC-C |
| ---- | ---- |
| ping 172.16.1.1 | ping 172.16.2.1 |
|   ![configure pc-a](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/5.png) | ![configure pc-c](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/6.png) |

### Step 7:	Save your running configuration.
``` 
ISP#copy running-config startup-config
```
``` 
EAST#copy running-config startup-config
```
``` 
WEST#copy running-config startup-config
```
## Part 2: Configure a GRE Tunnel
### Step 1:	Configure the GRE tunnel interface.
a. Configure the tunnel interface on the WEST router. Use S0/0/0 on WEST as the tunnel source interface and 10.2.2.1 as the tunnel destination on the EAST router.
``` 
WEST(config)# interface tunnel 0
WEST(config-if)# ip address 172.16.12.1 255.255.255.252
WEST(config-if)# tunnel source s0/1/1
WEST(config-if)# tunnel destination 10.2.2.1
```
b. Configure the tunnel interface on the EAST router. Use S0/0/1 on EAST as the tunnel source interface and 10.1.1.1 as the tunnel destination on the WEST router.
``` 
EAST(config)# interface tunnel 0
EAST(config-if)# ip address 172.16.12.2 255.255.255.252
EAST(config-if)# tunnel source s0/1/0
EAST(config-if)# tunnel destination 10.1.1.1
```
### Step 2:	Verify that the GRE tunnel is functional.
a. Verify the status of the tunnel interface on the WEST and EAST routers.
![configure pc-c](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/2a.png)

b. Issue the show interfaces tunnel 0 command to verify the tunneling protocol, tunnel source, and tunnel destination used in this tunnel.
<!--<details>
  <summary>Question: What is the tunneling protocol used? What are the tunnel source and destination IP addresses associated with GRE tunnel on each router?</summary>
  <p>
   ```ruby
   puts "Hello World"
  ```
  </p>
</details>
-->
Question: What is the tunneling protocol used? What are the tunnel source and destination IP addresses associated with GRE tunnel on each router?

| WEST | EAST |
| ---- | ---- |
| GRE tunnel protocol, Tunnel source 10.1.1.1 (Serial0/1/1), destination 10.2.2.1 | GRE tunnel protocol, Tunnel source 10.2.2.1 (Serial0/1/0), destination 10.1.1.1 |
|   ![configure WEST](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/7.png) | ![configure EAST](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/8.png) |

c. Ping across the tunnel from the WEST router to the EAST router using the IP address of the tunnel interface.
![ping tunnel from WEST to EAST](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/2b.png)
d. Use the traceroute command on the WEST to determine the path to the tunnel interface on the EAST router. 

Question: What is the path to the EAST router? 
```
Direct path (172.16.12.1 to 172.16.12.2)
```
![ping tunnel from WEST to EAST](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/2c.png)

e. Ping and trace the route across the tunnel from the EAST router to the WEST router using the IP address of the tunnel interface.

Question: What is the path to the WEST router from the EAST router? 
```
Direct path (172.16.12.2 to 172.16.12.1)
```
![ping tunnel from EAST to WEST](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/2d.png)

Question: With which interfaces are these IP addresses associated? Why? 
```
The tunnel0 interfaces on both WEST and EAST routers. The traffic is using the tunnel.
```
f. The ping and traceroute commands should be successful. If not, troubleshoot before continuing to the next part.

## Part 3: Enable Routing over the GRE Tunnel
### Step 1:	Configure OSPF routing for area 0 over the tunnel.
a. Configure OSPF process ID 1 using area 0 on the WEST router for the 172.16.1.0/24 and 172.16.12.0/24 networks.
```
WEST(config)# router ospf 1
WEST(config-router)# network 172.16.1.0 0.0.0.255 area 0
WEST(config-router)# network 172.16.12.0 0.0.0.3 area 0
```
b. Configure OSPF process ID 1 using area 0 on the EAST router for the 172.16.2.0/24 and 172.16.12.0/24 networks.
```
EAST(config)# router ospf 1
EAST(config-router)# network 172.16.2.0 0.0.0.255 area 0
EAST(config-router)# network 172.16.12.0 0.0.0.3 area 0
```
### Step 2:	Verify OSPF routing.
a.	From the WEST router, issue the show ip route command to verify the route to 172.16.2.0/24 LAN on the EAST router.


![show ip route WEST](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/10.png)

Question: What is the exit interface and IP address to reach the 172.16.2.0/24 network?
```
The tunnel 0 interface with an IP address of 172.16.12.2 is used to reach 172.16.2.0/24.
```
b.	From the EAST router issue the command to verify the route to 172.16.1.0/24 LAN on the WEST router.

Question: What is the exit interface and IP address to reach the 172.16.1.0/24 network?
```
The tunnel 0 interface with an IP address of 172.16.12.1 is used to reach 172.16.1.0/24.
```
### Step 3:	Verify end-to-end connectivity.
a.	Ping from PC-A to PC-C. It should be successful. If not, troubleshoot until you have end-to-end connectivity.
b.	Traceroute from PC-A to PC-C. What is the path from PC-A to PC-C?
```
172.16.1.1 > 172.16.12.2(Tunnel interface on the EAST router)> 172.16.2.3
```
![Traceroute from PC-A to PC-C](https://github.com/RyanSyen/imagesStorage/blob/main/7.2.2.5%20packet%20tracer%20Images/9.png)

## Reflection
1. What other configurations are needed to create a secured GRE tunnel?
```
IPsec can be configured to encrypt the data for a secured GRE tunnel.
```
3. If you added more LANs to the WEST or EAST router, what would you need to do so that the network will use the GRE tunnel for traffic?
```
The new networks would need to be added to the same routing protocols as the tunnel interface.
```
