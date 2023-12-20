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
- Heiter
- Frieren
- Himmel
- Fern

## Konfigurasi
- DHCP Server (Revolte)
- DHCP Relay (Fern - Heiter - Himmel - Aura - Frieren)
- DNS Server (Richter)

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
```

## No.8
Karena berbeda koalisi politik, maka subnet dengan masyarakat yang berada pada Revolte dilarang keras mengakses WebServer hingga masa pencoblosan pemilu kepala suku 2024 berakhir. Masa pemilu (hingga pemungutan dan penghitungan suara selesai) kepala suku bersamaan dengan masa pemilu Presiden dan Wakil Presiden Indonesia 2024.
```
```

## No 9
Sadar akan adanya potensial saling serang antar kubu politik, maka WebServer harus dapat secara otomatis memblokir alamat IP yang melakukan scanning port dalam jumlah banyak (maksimal 20 scan port) di dalam selang waktu 10 menit (clue: test dengan nmap).
```
```

## No.10
Karena kepala suku ingin tau paket apa saja yang di-drop, maka di setiap node server dan router ditambahkan logging paket yang di-drop dengan standard syslog level.
```
```







