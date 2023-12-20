# Jarkom-D25-modul-5

| **No** | **Nama** | **NRP** | 
| ------------- | ------------- | --------- |
| 1 | Andrian Tambunan  | 5025211018 | 
| 2 | Sandhika Surya Ardyanto | 5025211022 |

## Topologi Subnet
Setelah pandai mengatur jalur-jalur khusus, kalian diminta untuk membantu North Area menjaga wilayah mereka dan kalian dengan senang hati membantunya karena ini merupakan tugas terakhir. Untuk menghitung rute-rute yang diperlukan, gunakan perhitungan dengan metode VLSM. Buat juga pohonnya, dan lingkari subnet yang dilewati.
![image](https://github.com/AndrianTambunan/Jarkom-D25-modul-5/blob/main/subnet.png)
- Rute

| **Subnet** | **Rute** | **Jumlah IP** | **Netmask** |
| -------- | ------ | --------- | -------- |
| A1 | Fern-Switch2-Revolte | 2 | /30 |
| A2 | Fern-Richter | 2 | /30 | 
| A3 | Fern - Himmel - Switch1 - SchwerMountains | 66 | /25 |
| A4 | Himmel - LaubHills | 256 | /23 |
| A5 | Himmel - Frieren | 2 | /30 |
| A6 | Frieren - Stark | 2 | /30 |
| A7 | Frieren - Aura | 2 | /30 |
| A8 | Aura - Heiter | 2 | /30 |
| A9 | Heiter - TurkRegion | 1023 | /21 |
| A10 | Heiter - Switch3 - Sein - GrobeForest | 514 | /22 |
| Total |  | 1871 | /20 |

- Pembagian Subnet VLSM
  
  ![image](https://github.com/AndrianTambunan/Jarkom-D25-modul-5/blob/main/vlsm.png)

- Pembagian IP

![image](https://github.com/AndrianTambunan/Jarkom-D25-modul-5/assets/100081922/576c1eb7-1347-47b5-928d-84dfa3bc003c)

## Routing
- Aura
```
 up route add -net 10.34.14.128 netmask 255.255.255.252 gw 10.34.14.146
 up route add -net 10.34.14.132 netmask 255.255.255.252 gw 10.34.14.146
 up route add -net 10.34.14.0 netmask 255.255.255.128 gw 10.34.14.146
 up route add -net 10.34.12.0 netmask 255.255.254.0 gw 10.34.14.146
 up route add -net 10.34.14.136 netmask 255.255.255.252 gw 10.34.14.146
 up route add -net 10.34.14.140 netmask 255.255.255.252 gw 10.34.14.146
 up route add -net 10.34.14.144 netmask 255.255.255.252 gw 10.34.14.146
 up route add -net 10.34.0.0 netmask 255.255.248.0 gw 10.34.14.150
 up route add -net 10.34.8.0 netmask 255.255.252.0 gw 10.34.14.150
 up route add -net 10.34.14.148 netmask 255.255.255.252 gw 10.34.14.150
```
- Heiter
```
 route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.34.14.149
```
- Frieren
```
 up route add -net 10.34.14.128 netmask 255.255.255.252 gw 10.34.14.138
 up route add -net 10.34.14.132 netmask 255.255.255.252 gw 10.34.14.138
 up route add -net 10.34.14.136 netmask 255.255.255.252 gw 10.34.14.138
 up route add -net 10.34.14.0 netmask 255.255.255.128 gw 10.34.14.138
 up route add -net 10.34.12.0 netmask 255.255.255.0 gw 10.34.14.138
 route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.34.14.145
```
- Himmel
```
 up route add -net 10.34.14.0 netmask 255.255.255.128 gw 10.34.14.2
 up route add -net 10.34.14.128 netmask 255.255.255.252 gw 10.34.14.2
 up route add -net 10.34.14.132 netmask 255.255.255.252 gw 10.34.14.2
 route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.34.14.137
```
- Fern
```
route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.34.14.1
```

## Konfigurasi
- DHCP Relay (Aura)
```
 auto eth0
 iface eth0 inet dhcp

 auto eth1
 iface eth1 inet static
 	address 10.34.14.149
 	netmask 255.255.255.252

 auto eth2
 iface eth2 inet static
 	address 10.34.14.145
 	netmask 255.255.255.252
```

- DHCP Server (Revolte)
```
 auto eth0
 iface eth0 inet static
 	address 10.34.14.130
 	netmask 255.255.255.252
 	gateway 10.34.14.129

 up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- DNS Server (Richter)
```
 auto eth0
 iface eth0 inet static
 	address 10.34.14.134
 	netmask 255.255.255.252
 	gateway 10.34.14.133

 up echo nameserver 192.168.122.1 > /etc/resolv.conf
```

- Web Server (Stark)
```
 auto eth0
 iface eth0 inet static
 	address 10.34.14.142
 	netmask 255.255.255.252
 	gateway 10.34.14.141

 up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
- Web Server (Sein)
```
 auto eth0
 iface eth0 inet static
 	address 10.34.8.2
 	netmask 255.255.252.0
  gateway 10.34.8.1

 up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
- Heiter
```
 auto eth0
 iface eth0 inet static
 	address 10.34.14.150
 	netmask 255.255.255.252
 	gateway 10.10.14.149

 auto eth1
 iface eth1 inet static
 	address 10.34.8.1
 	netmask 255.255.252.0

 auto eth2
 iface eth2 inet static
 	address 10.34.0.1
 	netmask 255.255.248.0
```
- Himmel
```
 auto eth0
 iface eth0 inet static
 	address 10.34.14.138
 	netmask 255.255.255.252
 	gateway 10.34.14.137

 auto eth1
 iface eth1 inet static
 	address 10.34.14.1
 	netmask 255.255.255.128

 auto eth2
 iface eth2 inet static
 	address 10.34.12.1
 	netmask 255.255.254.0
```
- Frieren
```
 auto eth0
 iface eth0 inet static
 	address 10.34.14.146
 	netmask 255.255.255.252
 	gateway 10.34.14.145

 auto eth1
 iface eth1 inet static
 	address 10.34.14.137
 	netmask 255.255.255.252

 auto eth2
 iface eth2 inet static
 	address 10.34.14.141
 	netmask 255.255.255.252
```
- Fern
```
 auto eth0
 iface eth0 inet static
 	address 10.34.14.2
 	netmask 255.255.255.128
 	gateway 10.34.14.1

 auto eth1
 iface eth1 inet static
 	address 10.34.14.129
 	netmask 255.255.255.252

 auto eth2
 iface eth2 inet static
 	address 10.34.14.133
 	netmask 255.255.255.252
```
- TurkRegion
```
 auto eth0
 iface eth0 inet dhcp
```
- SchwerMountains
```
 auto eth0
 iface eth0 inet dhcp
```
- LaubHills
```
 auto eth0
 iface eth0 inet dhcp
```
- GrobeForest
```
 auto eth0
 iface eth0 inet dhcp
```
## No.1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Aura menggunakan iptables, tetapi tidak ingin menggunakan MASQUERADE. kita akan memasukkan codingan ini di node `Aura`
```
iptables -t nat -A POSTROUTING -s 10.34.0.0/16 -o eth0 -j SNAT --to-s 192.168.122.1
```

## No.2
Kalian diminta untuk melakukan drop semua TCP dan UDP kecuali port 8080 pada TCP.
```
apt install netcat
```
pada sender masukkan
```
nc -l -p 8080
```
pada receiver masukkan
```
iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
iptables -A INPUT -p tcp -j DROP
iptables -A INPUT -p udp -j DROP

nc <ip> 8080
```

## No.3
Kepala Suku North Area meminta kalian untuk membatasi DHCP dan DNS Server hanya dapat dilakukan ping oleh maksimal 3 device secara bersamaan, selebihnya akan di drop
```
iptables -I INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
iptables -I INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

## No 4
Lakukan pembatasan sehingga koneksi SSH pada Web Server hanya dapat dilakukan oleh masyarakat yang berada pada GrobeForest.
```
iptables -A INPUT -p tcp --dport 22 -s 10.34.8.0/22 -j ACCEPT

iptables -A INPUT -p tcp --dport 22 -j REJECT
```

## No.5
Selain itu, akses menuju WebServer hanya diperbolehkan saat jam kerja yaitu Senin-Jumat pada pukul 08.00-16.00.
```
iptables -A INPUT -m time --timestart 08:00 --timestop 16:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT

iptables -A INPUT -j REJECT
```

## No.6
Lalu, karena ternyata terdapat beberapa waktu di mana network administrator dari WebServer tidak bisa stand by, sehingga perlu ditambahkan rule bahwa akses pada hari Senin - Kamis pada jam 12.00 - 13.00 dilarang (istirahat maksi cuy) dan akses di hari Jumat pada jam 11.00 - 13.00 juga dilarang (maklum, Jumatan rek).
Di Web Server (Sein dan Stark)
```
iptables -A INPUT -p tcp --dport 80 -m time --timestart 13:01 --timestop 10:59 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -p tcp --dport 80 -m time --timestart 12:00 --timestop 13:00 --weekdays Mon,Tue,Wed,Thu -j DROP
iptables -A INPUT -p tcp --dport 80 -m time --timestart 11:00 --timestop 13:00 --weekdays Fri -j DROP
iptables -A INPUT -p tcp --dport 80 -j DROP
```

## No.7
Karena terdapat 2 WebServer, kalian diminta agar setiap client yang mengakses Sein dengan Port 80 akan didistribusikan secara bergantian pada Sein dan Stark secara berurutan dan request dari client yang mengakses Stark dengan port 443 akan didistribusikan secara bergantian pada Sein dan Stark secara berurutan.
```
iptables -A PREROUTING -t nat -p tcp --dport 80 -d 10.34.8.2 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.34.8.2
iptables -A PREROUTING -t nat -p tcp --dport 80 -d 10.34.8.2 -j DNAT --to-destination 10.34.14.142

iptables -A PREROUTING -t nat -p tcp --dport 443 -d 10.34.142 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.34.14.142
iptables -A PREROUTING -t nat -p tcp --dport 443 -d 10.34.14.142 -j DNAT --to-destination 10.34.8.2

```

## No.8
Karena berbeda koalisi politik, maka subnet dengan masyarakat yang berada pada Revolte dilarang keras mengakses WebServer hingga masa pencoblosan pemilu kepala suku 2024 berakhir. Masa pemilu (hingga pemungutan dan penghitungan suara selesai) kepala suku bersamaan dengan masa pemilu Presiden dan Wakil Presiden Indonesia 2024.
```
iptables -A INPUT -p tcp --dport 80 -m time --datestart 2024-02-14T00:00:00 --datestop 2024-03-20T23:59:59 -j DROP
```

## No 9
Sadar akan adanya potensial saling serang antar kubu politik, maka WebServer harus dapat secara otomatis memblokir alamat IP yang melakukan scanning port dalam jumlah banyak (maksimal 20 scan port) di dalam selang waktu 10 menit (clue: test dengan nmap).
```
```

## No.10
Karena kepala suku ingin tau paket apa saja yang di-drop, maka di setiap node server dan router ditambahkan logging paket yang di-drop dengan standard syslog level.
```
```







