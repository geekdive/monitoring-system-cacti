# Monitoring System Cacti
Monitoring System with Cacti


> Persiapkan Jaringan Seperti Berikut

<img src="https://github.com/latehero/monitoring-system-cacti/blob/master/picture/Screenshot%20from%202018-01-06%2001-27-35.png">



> Set IP DHCP Client


```java
[admin@MikroTik] > ip dhcp-client add interface=ether1 disabled=no
[admin@MikroTik] > ip dhcp-client print detail
RESULT
interface=ether1 add-default-route=yes default-route-distance=1
use-peer-dns=yes use-peer-ntp=yes dhcp-options=hostname,clientid
status=bound address=192.168.2.19/24 gateway=192.168.2.1
dhcp-server=192.168.2.1 primary-dns=192.168.2.1 expires-after=23h59m54s
```


> Add IP Address for Ether 3


```java
[admin@MikroTik] /ip address> add address=192.168.10.1/24 interface=ether3
[admin@MikroTik] /ip address> print
RESULT  
0 D 192.168.2.19/24    192.168.2.0     ether1                          
1   192.168.10.1/24    192.168.10.0    ether3
```

 
 > Set DHCP-Server


```java
[admin@MikroTik] > ip dhcp-server
[admin@MikroTik] /ip dhcp-server> setup
Select interface to run DHCP server on 

dhcp server interface: ether3 
Select network for DHCP addresses

dhcp address space: 192.168.10.0/24 
Select gateway for given network 

gateway for dhcp network: 192.168.10.1 
Select pool of ip addresses given out by DHCP server  

addresses to give out: 192.168.10.2-192.168.10.254 
Select DNS servers 

dns servers: 192.168.2.1 
Select lease time 

lease time: 10m 
[admin@MikroTik] /ip dhcp-server> print 
RESULT
0   dhcp1    ether3                        dhcp_pool1       10m
```


> Set Firewall NAT


```java
[admin@MikroTik] > ip firewall nat
[admin@MikroTik] /ip firewall nat> add chain=srcnat action=masquerade out-interface=ether1 
[admin@MikroTik] /ip firewall nat> print 
RESULT
0    chain=srcnat action=masquerade out-interface=ether1 log=no log-prefix=""
```

sumber:
```java
https://wiki.mikrotik.com/wiki/Manual:IP/DHCP_Client
https://wiki.mikrotik.com/wiki/Manual:IP/Address
https://wiki.mikrotik.com/wiki/Manual:IP/DHCP_Server
https://wiki.mikrotik.com/wiki/Manual:IP/Firewall/NAT
```


> Saya Asumsikan ada Client komputer di bawah switch

<img src="https://github.com/latehero/monitoring-system-cacti/blob/master/picture/Screenshot%20from%202018-01-06%2006-54-28.png">


> Persiapkan Server Monitoring 

<img src="https://github.com/latehero/monitoring-system-cacti/blob/master/picture/Screenshot%20from%202018-01-06%2011-56-45.png">

ipserver: 192.168.10.21
access: ssh -l pangerankesiangan 192.168.10.30

Install Web Server
