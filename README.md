
Repositori Jarkom Modul 3 kelompok E06

<details><summary>Anggota kelompok(click to show)</summary>
<p>

### Kelompok E06 :

1. Billy Brianto 5025201080
2. Atha Dzaky Hidayanto 5025201269
3. Naily Khairiya 5025201244
</p>
</details>


### TOPOLOGI
Membuat topologi sesuai perminataan soal.
<img width="659" alt="image" src="https://user-images.githubusercontent.com/72675854/201386369-f1631958-0b8b-4fcc-8593-8839e88a6a0d.png">

### SOAL 1
### Loid bersama Franky berencana membuat peta tersebut dengan kriteria WISE sebagai DNS Server, Westalis sebagai DHCP Server, Berlint sebagai Proxy Server
Kemudian setting network masing-masing node dengan fitur `edit network configuration` yang ada di menu `Configure`.
Berikut merupakan konfogurasi yang digunakan:

- Ostania (Router dan DHCP Relay)
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.195.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.195.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.195.3.1
	netmask 255.255.255.0
```
- SSS (Client)
```
auto eth0
iface eth0 inet dhcp
```

- Garden (Client)
```
auto eth0
iface eth0 inet dhcp
```

- Wise (DNS Server)
```
auto eth0
iface eth0 inet static
	address 192.195.2.2
	netmask 255.255.255.0
	gateway 192.195.2.1
```

- Berlint (Proxy Server)
```
auto eth0
iface eth0 inet static
	address 192.195.2.3
	netmask 255.255.255.0
	gateway 192.195.2.1
```


- Westalis (DHCP Server)
```
auto eth0
iface eth0 inet static
	address 192.195.2.4
	netmask 255.255.255.0
	gateway 192.195.2.1
```


- Eden (Client)
```
auto eth0
iface eth0 inet dhcp
```

- NewstonCastle (Client)
```
auto eth0
iface eth0 inet dhcp
```

- KemonoPark (Client)
```
auto eth0
iface eth0 inet dhcp
```

Jalankan command berikut pada Router Ostania untuk pengaturan lalu lintas komputer.
`iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.195.0.0/16`
Dengan 192.195 merupakan prefix IP Kelompok E06.

- Pada Wise
Jalankan command:
```
apt-get install bind9 -y
service bind9 start
```

- Pada Westalis
Instalasi server
```
apt-get install isc-dhcp-server-y
dhcp --version
```

Edit konfigurasi
```
echo '
INTERFACES="eth0"
' > /etc/default/isc-dhcp-server
```

- Pada Berlint
Instalasi squid
```
apt-get install squid -y
service squid start
```


### SOAL 2
### dan Ostania sebagai DHCP Relay. Loid dan Franky menyusun peta tersebut dengan hati-hati dan teliti.
- Pada Ostania Melakukan instalasi DHCP Relay sebagai berikut.
```
apt-get install isc-dhcp-relay -y
```

Edit konfigurasi pada `/etc/default/isc-dhcp-relay` sebagai berikut

```
echo '
# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="192.195.2.4"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
' > /etc/default/isc-dhcp-relay

```


### SOAL 3
### Ada beberapa kriteria yang ingin dibuat oleh Loid dan Franky, yaitu: Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server. Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.50 - [prefix IP].1.88 dan [prefix IP].1.120 - [prefix IP].1.155 

- Pada Westalis
Edit konfigurasi berikut sesuai dengan permintaan soal.
```
echo '
subnet 192.195.28.1.0 netmask 255.255.255.0 {
    range 192.295.28.1.50 192.195.1.88;
    range 192.195.1.120 192.195.1.155;
    option routers 192.195.1.1;
    option broadcast-address 192.195.1.255;
    option domain-name-servers 192.195.2.2;
}
subnet 192.195.2.0 netmask 255.255.255.0{
}

' > /etc/dhcp/dhcpd.conf
```

Restart DHCP Server
```
service isc-dhcp-server restart
service isc-dhcp-server status
```

- Pada SSS
edit konfigurasi pada /etc/network/interfaces
```
echo '
#auto eth0
#iface eth0 inet static
#	address 192.195.1.2
#	netmask 255.255.255.0
#	gateway 192.195.1.1
auto eth0
iface eth0 inet dhcp
' > /etc/network/interfaces
```

- Pada Garden
edit konfigurasi pada /etc/network/interfaces
```
#auto eth0
#iface eth0 inet static
#	address 192.195.1.3
#	netmask 255.255.255.0
#	gateway 192.195.1.1
auto eth0
iface eth0 inet dhcp
' > /etc/network/interfaces
```

### SOAL 4
### Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.10 - [prefix IP].3.30 dan [prefix IP].3.60 - [prefix IP].3.85 
- Pada Westalis
Tambahkan konfigurasi berikut ke `/etc/dhcp/dhcpd.conf`
```
subnet 192.195.3.0 netmask 255.255.255.0 {
    range 192.195.3.10 192.195.3.30;
    range 192.195.3.60 192.195.3.85;
    option routers 192.195.3.1;
    option broadcast-address 192.195.3.255;
    option domain-name-servers 192.195.2.2;
    default-lease-time 600;
    max-lease-time 6900;
}

```

- Pada Eden
```
echo '
#auto eth0
#iface eth0 inet static
#	address 192.195.3.2
#	netmask 255.255.255.0
#	gateway 192.195.3.1
auto eth0
iface eth0 inet dhcp
' > /etc/network/interfaces
```

- Pada NewstonCastle
```
#auto eth0
#iface eth0 inet static
#	address 192.195.3.3
#	netmask 255.255.255.0
#	gateway 192.195.3.1
auto eth0
iface eth0 inet dhcp
' > /etc/network/interfaces
```

- Pada KemonoPark
```
#auto eth0
#iface eth0 inet static
#	address 192.195.3.4
#	netmask 255.255.255.0
#	gateway 192.195.3.1
auto eth0
iface eth0 inet dhcp
' > /etc/network/interfaces
```


### SOAL 5
### Client mendapatkan DNS dari WISE dan client dapat terhubung dengan internet melalui DNS tersebut. 
- Pada WISE
lakukan konfigurasi berikut pada `/etc/bind/named.conf.options`
```
options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
```
restart bind9
```
service bind9 restart
```

Lakukan restart pada DHPCP server dan DHCP relay

- Pada Westalis
```
service isc-dhcp-server restart
```
- Pada Ostania
```
service isc-dhcp-relay restart
```
- Pada Client
```
cat /etc/resolv.conf
```


### SOAL 6
### Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit sedangkan pada client yang melalui Switch3 selama 10 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit. 
- Pada Westalis
Untuk Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 5 menit:
`default-lease-time 300;` tambahkan pada `/etc/dhcp/dhcpd.conf`

sedangkan yang melalui Switch 2 10 menit:
``default-lease-time 600;` tambahkan pada `/etc/dhcp/dhcpd.conf`

Konfigurasi untuk mengatur waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 115 menit. 
`max-lease-time 6900;`

	
### SOAL 7
### Loid dan Franky berencana menjadikan Eden sebagai server untuk pertukaran informasi dengan alamat IP yang tetap dengan IP [prefix IP].3.13
- Di Eden
cek ip terlebih dahulu
```
ip a
```

- Di Westalis

Buka file lagi, yaitu /etc/dhcp/dhcpd.conf dan edit seperti konfigurasi berikut.
```
host Eden {
	hardware ethernet 36:76:4e:b9:de:3f
	fixed-address 192.195.3.13
}
```

- Di Ostania
restart DHCP Relay
```
service isc-dhcp-relay restart
```

- Di Eden
perbarui konfigurasi di eden
```
echo '
#auto eth0
#iface eth0 inet static
#	address 192.195.3.2
#	netmask 255.255.255.0
#	gateway 192.195.3.1
auto eth0
iface eth0 inet dhcp

hwaddress ether 36:76:4e:b9:de:3f

' > /etc/network/interfaces

```




### SOAL 1 PROXY
### Setiap client harus menggunakan Berlint sebagai HTTP & HTTPS proxy. Adapun kriteria pengaturannya adalah sebagai berikut:
### Client hanya dapat mengakses internet diluar (selain) hari & jam kerja (senin-jumat 08.00 - 17.00) dan hari libur (dapat mengakses 24 jam penuh)
- Di Berlint
```
echo '
acl WORKING time MTWHF 08:00-17:00

' > /etc/squid/acl.conf 

echo '
include /etc/squid/acl.conf

http_port 8080
http_access deny WORKING
http_access allow all 
visible_hostname Berlint

' > /etc/squid/squid.conf

service squid restart
```


### SOAL 2 PROXY
### Adapun pada hari dan jam kerja sesuai nomor (1), client hanya dapat mengakses domain loid-work.com dan franky-work.com (IP tujuan domain dibebaskan)
```
echo '
liod-work.com
franky-work.com

' > /etc/squid/work-sites.acl

echo '
include /etc/squid/acl.conf

http_port 8080
visible_hostname Berlint

acl WORKSITES dstdomain "/etc/squid/work-sites.acl"
http_access allow WORKSITES
http_access deny WORKING
http_access allow all

' > /etc/squid/squid.conf

```



### SOAL 3 PROXY
### Saat akses internet dibuka, client dilarang untuk mengakses web tanpa HTTPS. (Contoh web HTTP: http://example.com)

- di Berlint
Tambahkan pada `/etc/squid/squid.conf`
```
http_access deny !SSL_ports
```

### SOAL 4 PROXY
### Agar menghemat penggunaan, akses internet dibatasi dengan kecepatan maksimum 128 Kbps pada setiap host (Kbps = kilobit per second; lakukan pengecekan pada tiap host, ketika 2 host akses internet pada saat bersamaan, keduanya mendapatkan speed maksimal yaitu 128 Kbps)
- Di Berlint
```
echo '
include /etc/squid/acl.conf

http_port 8080
visible_hostname Berlint

acl SSL_ports port 443
acl WORKSITES dstdomain "/etc/squid/work-sites.acl"
#http_access deny !SSL_ports
http_access allow WORKSITES
#http_access deny WORKING
http_access allow all

delay_pools 1
delay_class 1 2
delay_access 1 allow all
delay_parameters 1 none 16000/16000
' > /etc/squid/squid.conf

service squid restart
```

### SOAL 5 PROXY
### Setelah diterapkan, ternyata peraturan nomor (4) mengganggu produktifitas saat hari kerja, dengan demikian pembatasan kecepatan hanya diberlakukan untuk pengaksesan internet pada hari libur
```
echo '
include /etc/squid/acl.conf

http_port 8080
visible_hostname Berlint

acl SSL_ports port 443
acl WORKSITES dstdomain "/etc/squid/work-sites.acl"
#http_access deny !SSL_ports
http_access allow WORKSITES
#http_access deny WORKING
http_access allow all

acl OPEN_TIME time MTWHF
delay_pools 1
delay_class 1 2
delay_access 1 allow !OPEN_TIME
delay_parameters 1 none 16000/16000
' > /etc/squid/squid.conf

service squid restart
```
