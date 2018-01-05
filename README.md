# Monitoring System Cacti
Monitoring System with Cacti


> Persiapkan Jaringan Seperti Berikut

<img src="https://github.com/latehero/monitoring-system-cacti/blob/master/picture/Screenshot%20from%202018-01-06%2001-27-35.png">



> Set IP DHCP Client

[admin@MikroTik] > **ip dhcp-client add interface=ether1 disabled=no <br>**
[admin@MikroTik] > **ip dhcp-client print detail <br>**
**RESULT** <br>
**interface=ether1 add-default-route=yes default-route-distance=1 <br>
use-peer-dns=yes use-peer-ntp=yes dhcp-options=hostname,clientid <br>
status=bound address=192.168.2.19/24 gateway=192.168.2.1 <br>
dhcp-server=192.168.2.1 primary-dns=192.168.2.1 expires-after=23h59m54s <br>**

> Add IP Address for Ether 3

[admin@MikroTik] **/ip address> add address=192.168.10.1/24 interface=ether3 <br>**
[admin@MikroTik] **/ip address> print <br>**
**RESULT** <br>                
**0 D 192.168.2.19/24    192.168.2.0     ether1 **                            
**1   192.168.10.1/24    192.168.10.0    ether3**
 
 > Set DHCP-Server

[admin@MikroTik] > **ip dhcp-server <br>**
[admin@MikroTik] /ip dhcp-server> **setup <br>**
Select interface to run DHCP server on <br>

dhcp server interface: **ether3 <br>**
Select network for DHCP addresses <br>

dhcp address space: **192.168.10.0/24 <br>**
Select gateway for given network <br>

gateway for dhcp network: **192.168.10.1 <br>**
Select pool of ip addresses given out by DHCP server  <br>

addresses to give out: **192.168.10.2-192.168.10.254 <br>**
Select DNS servers <br>

dns servers: **192.168.2.1 <br>**
Select lease time <br>

lease time: **10m <br>**
[admin@MikroTik] /ip dhcp-server> **print <br>**
**RESULT** <br>
** 0   dhcp1    ether3                        dhcp_pool1       10m <br>**

> Set Firewall NAT

[admin@MikroTik] > **ip firewall nat <br>**

[admin@MikroTik] /ip firewall nat> **add chain=srcnat action=masquerade out-interface=ether1 <br>**
[admin@MikroTik] /ip firewall nat> **print <br>**
**RESULT** <br>
** 0    chain=srcnat action=masquerade out-interface=ether1 log=no log-prefix=""** 

