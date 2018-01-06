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

> Cacti di Server Monitoring

Cacti adalah tool network monitoring system (NMS) opensource dan berbasis web yang dirancang sebagai aplikasi front-end dari RRDTool yang berfungsi untuk menyimpan informasi kedalam database MySQL kemudian menampilkannya kedalam bentuk grafik

Dengan cacti, kita bisa memonitoring semua resource pada perangkat jaringan baik itu server, router, switch maupun perangkat network lainnya. Umumnya cacti digunakan untuk me-monitoring CPU load, Memory Usage dan juga penggunaan bandwidth yang ada pada jaringan.

Cacti menggunakan protocol SNMP (Simple Network Management Protocol) untuk mengambil dan mengumpulkan informasi tersebut kemudian menyimpannya kedalam database MySQL, setelah itu semua informasi yang sudah dikumpulkan akan ditampilkan pada web browser menggunakan bahasa pemrograman PHP.

Sebelumnya kita harus menginstall LAMP (Linux Apache2, MySQL dan PHP) pada sistem server yang akan kita jadikan server monitoringnya disini kita akan gunakan Ubuntu Server 16.04 LTS

> Persyaratan Installasi

Untuk menginstall NMS Cacti, ada beberapa package yang harus diinstall terlebih dahulu agar cacti NM dapat berfungsi dengan baik, diantaranya adalah sebagai berikut:

* LAMP Server
* SNMP, SNMPD dan RRDTool

> Install Web Server

Lihat link berikut: 

```java
https://github.com/latehero/u-need-a-webserver-for-develop
```

> Installasi SNMP, SNMPD dan RRDTool


```java
root@server:/home/pahlawankesiangan# apt-get install snmp snmpd rrdtool
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  fontconfig libcairo2 libdatrie1 libdbi1 libgraphite2-3 libharfbuzz0b
  libpango-1.0-0 libpangocairo-1.0-0 libpangoft2-1.0-0 libpixman-1-0 librrd4
  libsensors4 libsnmp-base libsnmp30 libthai-data libthai0 libxcb-render0
  libxcb-shm0 libxrender1
Suggested packages:
  lm-sensors snmp-mibs-downloader librrds-perl snmptrapd
The following NEW packages will be installed:
  fontconfig libcairo2 libdatrie1 libdbi1 libgraphite2-3 libharfbuzz0b
  libpango-1.0-0 libpangocairo-1.0-0 libpangoft2-1.0-0 libpixman-1-0 librrd4
  libsensors4 libsnmp-base libsnmp30 libthai-data libthai0 libxcb-render0
  libxcb-shm0 libxrender1 rrdtool snmp snmpd
0 upgraded, 22 newly installed, 0 to remove and 0 not upgraded.
Need to get 3419 kB of archives.
After this operation, 11.9 MB of additional disk space will be used.
Do you want to continue? [Y/n]
```

> Install Cacti Spine

```java
root@server:/home/pahlawankesiangan# apt-get install cacti cacti-spine
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional packages will be installed:
  libjs-jquery-cookie libjs-jquery-ui libjs-jquery-ui-theme-ui-lightness
  libmysqlclient20 libphp-adodb php-mysql php-snmp php7.0-snmp
Suggested packages:
  php-ldap moreutils snmp-mibs-downloader libjs-jquery-ui-docs php-adodb
The following NEW packages will be installed:
  cacti cacti-spine libjs-jquery-cookie libjs-jquery-ui
  libjs-jquery-ui-theme-ui-lightness libmysqlclient20 libphp-adodb php-mysql
  php-snmp php7.0-snmp
0 upgraded, 10 newly installed, 0 to remove and 0 not upgraded.
Need to get 3506 kB of archives.
After this operation, 14.4 MB of additional disk space will be used.
Do you want to continue? [Y/n]
```

Ketika proses installasi cacti akan ada beberapa pertanyaan untuk memilih web server yang diguanakan seperti gambar dibawah ini:

<img src="https://github.com/latehero/monitoring-system-cacti/blob/master/picture/Screenshot%20from%202018-01-06%2013-36-24.png">

selanjutnya akan diminta memasukkan password mysql pas ketika nginstall MySQL Servernya, seperti dibawah

<img src="https://github.com/latehero/monitoring-system-cacti/blob/master/picture/Screenshot%20from%202018-01-06%2013-37-37.png">

lalu kemudian konfirmasi password

<img src="https://github.com/latehero/monitoring-system-cacti/blob/master/picture/Screenshot%20from%202018-01-06%2013-40-17.png">

lalu kemudian pilih web server

<img src="https://github.com/latehero/monitoring-system-cacti/blob/master/picture/Screenshot%20from%202018-01-06%2013-44-50.png">

Tunggu hingga proses installasi selesai seperti dibawah ini

<img src="https://github.com/latehero/monitoring-system-cacti/blob/master/picture/Screenshot%20from%202018-01-06%2013-47-33.png">

> Restarting Service SNMP and See the Status

sebelum mengkonfigurasi SNMPD alangkah lebih baiknya kita restart terlebih dahulu SNMPD servicenya seperti berikut:


```java
root@server:/home/pahlawankesiangan# sudo /etc/init.d/snmpd restart
[ ok ] Restarting snmpd (via systemctl): snmpd.service.
root@server:/home/pahlawankesiangan# sudo /etc/init.d/snmpd status
● snmpd.service - LSB: SNMP agents
   Loaded: loaded (/etc/init.d/snmpd; bad; vendor preset: enabled)
   Active: active (running) since Sat 2018-01-06 13:51:50 WIB; 4s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 22444 ExecStop=/etc/init.d/snmpd stop (code=exited, status=0/SUCCESS)
  Process: 22453 ExecStart=/etc/init.d/snmpd start (code=exited, status=0/SUCCESS)
    Tasks: 1
   Memory: 4.5M
      CPU: 20ms
   CGroup: /system.slice/snmpd.service
           └─22470 /usr/sbin/snmpd -Lsd -Lf /dev/null -u snmp -g snmp -I -smu...

Jan 06 13:51:50 server systemd[1]: Stopped LSB: SNMP agents.
Jan 06 13:51:50 server systemd[1]: Starting LSB: SNMP agents...
Jan 06 13:51:50 server snmpd[22453]:  * Starting SNMP services:
Jan 06 13:51:50 server snmpd[22461]: /etc/snmp/snmpd.conf: line 145: Warnin...s.
Jan 06 13:51:50 server snmpd[22461]: /etc/snmp/snmpd.conf: line 147: Warnin...s.
Jan 06 13:51:50 server snmpd[22461]: Turning on AgentX master support.
Jan 06 13:51:50 server systemd[1]: Started LSB: SNMP agents.
Jan 06 13:51:50 server snmpd[22470]: NET-SNMP version 5.7.3
Hint: Some lines were ellipsized, use -l to show in full.
root@server:/home/pahlawankesiangan# 
```

