# Jarkom-Modul-3-IT30-2023

### 1. Refaldi
### 2. Anisa Ghina Salsabila 5027211062

## Topologi
![Uploading Screenshot 2023-11-20 183345.png…]()

## Config
Statistic:

* Himmel (DHCP Server)
```
auto eth0
iface eth0 inet static
    address 192.248.2.1
    netmask 255.255.255.0
    gateway 192.248.2.0
```
* Heither (DNS Server)

```
auto eth0
iface eth0 inet static
    address 192.248.2.2
    netmask 255.255.255.0
    gateway 192.248.2.0
```
* Denken (Database Server)
```
auto eth0
iface eth0 inet static
    address 192.248.3.1
    netmask 255.255.255.0
    gateway 192.248.3.0
```
* Eisen (Load Balancer)

```
auto eth0
iface eth0 inet static
    address 192.248.3.2
    netmask 255.255.255.0
    gateway 192.248.3.0
```
* Frieren (Laravel Worker)
```
auto eth0
iface eth0 inet static
    address 192.248.1.3
    netmask 255.255.255.0
    gateway 192.248.1.0
```
* Flamme
```
auto eth0
iface eth0 inet static
    address 192.248.1.4
    netmask 255.255.255.0
    gateway 192.248.1.0
```
* Fern (Laravel Worker)
```
auto eth0
iface eth0 inet static
    address 192.248.1.5
    netmask 255.255.255.0
    gateway 192.248.1.0
```
* Lawine (PHP Worker)
```
auto eth0
iface eth0 inet static
    address 192.248.4.3
    netmask 255.255.255.0
    gateway 192.248.4.0
```
* Linie (PHP Worker)
```
auto eth0
iface eth0 inet static
    address 192.248.4.4
    netmask 255.255.255.0
    gateway 192.248.4.0
```
* Lugner (PHP Worker)
```
auto eth0
iface eth0 inet static
    address 192.248.4.5
    netmask 255.255.255.0
    gateway 192.248.4.0
```
Dynamic: 
* Aura  (DHCP Relay)
```
auto eth0
iface eth0 inet dhcp
up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.248.0.0/16

auto eth1
iface eth1 inet static
    address 192.248.1.0
    netmask 255.255.255.0

auto eth2
iface eth2 inet static
    address 192.248.2.0
    netmask 255.255.255.0

auto eth3
iface eth3 inet static
    address 192.248.3.0
    netmask 255.255.255.0

auto eth4
iface eth4 inet static
    address 192.248.4.0
    netmask 255.255.255.0
```
* evolte, Richter, Sein, dan Stark (Client)
```
auto eth0
iface eth0 inet dhcp
```
## Soal 0
Melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP [prefix IP].x.1.
Heiter : 
```
apt update
apt install bind9 dnsutils -y

echo '
zone "riegel.canyon.it30.com" {
    	type master;
    	notify yes;
    	file "/etc/bind/jarkom/riegel.canyon.it30.com";
};

zone "granz.channel.it30.com" {
	type master;
	notify yes;
	file "/etc/bind/jarkom/granz.channel.IT30.com";
};
' > /etc/bind/named.conf.local
cp /etc/bind/db.local /etc/bind/jarkom/riegel.canyon.it30.com
cp /etc/bind/db.local /etc/bind/jarkom/granz.channel.it30.com

echo '
;
; BIND data file for local loopback interface
;
$TTL	604800
@   	IN  	SOA 	riegel.canyon.it30.com. root.riegel.canyon.it30.com. (
                          	2     	; Serial
                     	604800     	; Refresh
                      	86400     	; Retry
                    	2419200     	; Expire
                     	604800 )   	; Negative Cache TTL
;
@   	IN  	NS  	riegel.canyon.it30.com.
@   	IN  	A   	192.248.1.3#frieren
www 	IN  	CNAME   riegel.canyon.it30.com.
' > /etc/bind/jarkom/riegel.canyon.it30.com

cat /etc/bind/jarkom/riegel.canyon.it30.com

echo '
;
; BIND data file for local loopback interface
;
$TTL	604800
@   	IN  	SOA 	granz.channel.it30.com. root.granz.channel.it30.com. (
                          	2     	; Serial
                     	604800     	; Refresh
                      	86400     	; Retry
                    	2419200     	; Expire
                     	604800 )   	; Negative Cache TTL
;
@   	IN  	NS  	granz.channel.it30.com.
@   	IN  	A   	192.248.4.3 #lawiner
www 	IN  	CNAME   granz.channel.it30.com.
' > /etc/bind/jarkom/granz.channel.it30.com

service bind9 restart
service bind9 status


```
## Soal 1
Melakukan konfigurasi sesuai dengan topologi dan config
## Soal 2
Semua CLIENT harus menggunakan konfigurasi dari DHCP Server. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80
Aura sebagai DHCP Relay

```
apt update

apt-get install isc-dhcp-relay -y

echo "# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS=\"192.248.2.2\"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES=\"eth1 eth2 eth3 eth4\"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=\"\"" >/etc/default/isc-dhcp-relay

sed -i 's/#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/g' /etc/sysctl.conf
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.248.0.0/16
service isc-dhcp-relay restart
service isc-dhcp-relay status
```
Himmel sebagai DHCP Server
```
apt update
apt install isc-dhcp-server -y

echo "# Defaults for isc-dhcp-server (sourced by /etc/init.d/isc-dhcp-server)
# Path to dhcpd's config file (default: /etc/dhcp/dhcpd.conf).
#DHCPDv4_CONF=/etc/dhcp/dhcpd.conf
#DHCPDv6_CONF=/etc/dhcp/dhcpd6.conf
# Path to dhcpd's PID file (default: /var/run/dhcpd.pid).
#DHCPDv4_PID=/var/run/dhcpd.pid
#DHCPDv6_PID=/var/run/dhcpd6.pid
# Additional options to start dhcpd with.
#       Don't use options -cf or -pf here; use DHCPD_CONF/ DHCPD_PID instead
#OPTIONS=\"\"
# On what interfaces should the DHCP server (dhcpd) serve DHCP requests?
#       Separate multiple interfaces with spaces, e.g. \"eth0 eth1\".
INTERFACESv4=\"eth0\"
INTERFACESv6=\"\"" >/etc/default/isc-dhcp-server

echo "subnet 192.248.2.0 netmask 255.255.255.0 {
    option routers 192.248.2.254;
    option broadcast-address 192.248.2.255;
}

subnet 192.248.3.0 netmask 255.255.255.0 {
    option routers 192.248.3.254;
    option broadcast-address 192.248.3.255;
}

subnet 192.248.4.0 netmask 255.255.255.0 {
    range 192.248.4.16 192.248.4.32;
    range 192.248.4.64 192.248.4.80;
    option routers 192.248.4.0;
    option broadcast-address 192.248.4.255;
    option domain-name-servers 192.248.2.2;
}" >/etc/dhcp/dhcpd.conf

rm /var/run/dhcpd.pid

service isc-dhcp-server restart

service isc-dhcp-server status

```

## Soal 3

Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168
Disini karena ditpologi Switch4 berada pada 192.248.1.0
```
subnet 192.248.1.0 netmask 255.255.255.0 {
    range 192.248.1.12 192.248.1.20;
    range 192.248.1.160 192.248.1.168;
    option routers 192.248.1.254;
    option broadcast-address 192.248.1.255;
    option domain-name-servers 192.248.2.2;
}
```
## Soal 4
Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut
Pngaturan domain name servers pada Himmel sebagai DHCP Server
```
subnet 192.248.4.0 netmask 255.255.255.0 {
    range 192.248.4.16 192.248.4.32;
    range 192.248.4.64 192.248.4.80;
    option routers 192.248.4.0;
    option broadcast-address 192.248.4.255;
    option domain-name-servers 192.248.2.2;
}

subnet 192.248.1.0 netmask 255.255.255.0 {
    range 192.248.1.12 192.248.1.20;
    range 192.248.1.160 192.248.1.168;
    option routers 192.248.1.254;
    option broadcast-address 192.248.1.255;
    option domain-name-servers 192.248.2.2;
}
```
## Soal 5
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit
Pengaturan domain name servers pada Himmel sebagai DHCP Server
```
subnet 192.248.4.0 netmask 255.255.255.0 {
	range 192.248.4.16 192.248.4.32;
	range 192.248.4.64 192.248.4.80;
	option routers 192.248.4.0;
	option broadcast-address 192.248.4.255;
	option domain-name-servers 192.248.2.2;#heiter
	default-lease-time 180;
	max-lease-time 5760; # 96 minutes
}

subnet 192.248.1.0 netmask 255.255.255.0 {
	range 192.248.1.12 192.248.1.20;
	range 192.248.1.160 192.248.1.168;
	option routers 192.248.1.254;
	option broadcast-address 192.248.1.255; 
	option domain-name-servers 192.248.2.2; #heiter
	default-lease-time 720; # 12 minutes
	max-lease-time 5760; # 96 minutes
}

```
![Uploading Screenshot 2023-11-20 193153.png…]()


![Uploading Screenshot 2023-11-20 193342.png…]()



