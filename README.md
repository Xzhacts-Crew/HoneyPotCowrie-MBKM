# HoneyPotCowrie-MBKM
LIST NAMA ANGGOTA KELOMPOK
- Mohamad Farhan Palilati 23.66.0020 (Ketua) - System Security Administrator
- Siti Marwa 23.66.0024 (Anggota) - Network Administrator
- Moh Wahyu A.Saini 23.66.0034 (Anggota) - Konfigurasi DNS Server
- Ilun Nambo 23.66.0032 (Anggota) - Konfigurasi Web Server
- Amin Nusi 23.66.0033 (Anggota) - Konfigurasi Database server

Cowrie adalah salah satu proyek open-source yang bertujuan untuk meniru layanan Secure Shell (SSH) dan bertindak sebagai honeypot. Honeypot adalah sistem atau perangkat lunak yang dirancang untuk menarik dan menangkap serangan siber dengan cara meniru sebagai target yang mudah diserang. Cowrie digunakan khusus untuk menarik serangan terhadap layanan SSH.

Berikut adalah beberapa poin utama tentang Cowrie SSH honeypot:
- SSH Honeypot
- Logging dan Analisis
- Intelligence Gathering
- Penggunaan untuk Penelitian Keamanan
- Konfigurasi Fleksibel
- Open Source
  
### TOPOLOGI JARINGAN
![alt text](https://github.com/Xzhacts-Crew/HoneyPotCowrie-MBKM/blob/main/Topologi%20spj.png?raw=true)

# 1. KONFIGURASI JARINGAN VM
### 1.1 MENGECEK IP INTERFACES
pertama tama cek terlebih dahulu ip address dari kedua interfaces dengan command
```sh
ip addr
```
### 1.2 MENGECEK IP INTERFACES
setelah itu kalian pergi ke bagian config jaringan dengan command
```sh
nano /etc/network/interfaces
```
tambahkan konfigurasinya sebagai berikut
```sh
auto enp0s8
iface enp0s8 inet static
address 200.0.0.1/24
```
### 1.3 RESTART JARINGAN VM
agar komfigurasi bisa berjalan perlu di restart jaringan vm dengan command
```sh
/etc/init.d/networking restart
```
# 2. KONFIGURASI SSH
### 2.1 INSTALL SSH
untuk menginstall ssh pada vm ketikan command
```sh
apt install openssh-server -y
```
### 2.2 KONFIGURASI FILE SSH
untuk mengkonfigurasi ssh terlebih dahulu kita masuk ke foldernya yaitu
```sh
nano /etc/ssh/sshd_config
```
setelah terbuka, ubahah port yang awalnya 22 menjadi 7878
```sh
port 7878
```
setelah itu coba test koneksi ssh nya dengan menggunakan command
```sh
ssh amikomspj@200.0.0.1 -p 7878
```
# 3. INSTALLASI COWRIE HONEYPOT SSH
### 3.1 Install Dependency
```sh
apt-get install git python3-virtualenv libssl-dev libffi-dev build-essential libpython3-dev python3-minimal authbind virtualenv
```
### 3.2 Create a user account
```sh
sudo adduser --disabled-password cowrie
```
### 3.3 Checkout the code
```sh
git clone http://github.com/cowrie/cowrie
```
### 3.4 Setup Virtual Environment
```sh
python -m venv cowrie-env
```
### 3.5 activate virtual environment
```sh
source cowrie-env/bin/activate
```
### 3.6 Starting Cowrie
```sh
bin/cowrie start
```
Setelah Run, Untuk melihat log dari aktifitas honeypotnya secara real time bisa dengan command :
```sh
tail -f var/log/cowrie/cowrie.log
```
# 4. KONFIGURASI DNS SERVER
Ada dua domain yang akan dibuat yaitu:
www.amikomspj.com
www.dilarang.net
Dua domain ini berada dalam satu ip yaitu ip 200.0.0.1 ip server dan nantinya pada saat pengujian bisa ping pada komputer fisik kepada dua domain ini
baik itu adalah topologi yang akan dibangun atau jaringan yang akan dibangun,selanjutnya menginstall paket DNS Server yang bernama bind9 dan untuk melakukan pengujian memerlukan sebuah paket yang bernama dnsutils untuk melakukan nslookup 
Untuk mengkonfigurasi dns kita masuk di vm jika sudah berjalan langsung saja kita masuk sebagai root lalu minimaze dan buka cmd untuk melakukan konfigurasi via ssh lalu ketik ssh amikomspj@200.0.0.1 -p 7878 lalu masukan password
### 4.1 Install paket dns server bind9 dan dnsutils
```sh
apt install bind9 dnsutils -y
```
### 4.2 Edit file konfigurasi lalu masuk di file/folder konfigurasi 
```sh
cd /etc/bind
```
lalu ketik ls
```sh
ls
```
### 4.3 Edit file named.conf.local
```sh
nano.named.conf.local
```
### 4.4 Gandakan file db.local dan db.127
Jadi ini memerlukan tiga file db yaitu db.amikomspj,db.dilarang,db.200
```sh
cp db.local db.amikomspj
cp db.local db.dilarang
cp db.127 db.200
Enter lalu ketik
``
```sh
ls
```
### 4.5 Edit tiga file konfigurasi utama yaitu db.200,db.amikomspj,db.dilarang
```sh
nano db.200
nano db.amikomspj
nano db.dilarang
```
### 4.6 Edit file name server
```sh
nano /etc/resolv.conf
```
lalu ketik
```sh
nameserver 200.0.0.1
```
seluruh file konfigurasi di edit sekarang reserv service
### 4.7 Reserv service
```sh
systemctl restart bind9
```
```sh
Enter
```
### 4.8 Cek nslookup yang sudah dibuat
```sh
nslookup 200.0.0.1
nslookup www.amikomspj.com
nslookup www.dilarang.net
```
### 4.9 Pengujian atau ping
buka cmd di tab baru
lalu ping server
```sh
ping 200.0.0.1
```
lalu ping domain
```sh
ping www.amikomspj.com
ping www.dilarang.net
```
# 5 Konfigurasi Web Server
### 5.1 Pertama-tama menginstal paket web server dengan command
```sh
apt install apache2 -y
```
kemudian tekan ENTER.
### 5.2 Setelah terinstall, selanjutnya mengedit file konfigurasi apache2 pada folder
```sh
cd /etc/apache2/sites-available
```
kemudian ENTER, lalu ketik command:
```sh
ls
```
maka, akan muncul dua file, yaitu:
```sh
000-default.conf (http)
default-ssl.conf (https)
```
Karena kami hanya ingin membuat website yang berbasis http, maka kami menggunakan file 000-default.conf.
### 5.3 Membuat webiste berbasis http
Karena yang akan kami buat website berbasis http, maka masukkan command:
```sh
cp 000-default.conf amikomspj.conf
cp 000-default.conf dilarang.conf
```
Setelah itu, mengedit satu persatu websitenya dengan command:
```sh
nano amikomspj.conf
```
lalu tekan ENTER, setelah muncul jendela konfigurasinya servername diubah menjadi nama websitenya, begitupun pada ServerAdmin-nya dan DokumenRoot-nya menjadi:
```sh
ServerName www.amikomspj.com

ServerAdmin webmaster@amikomspj
DocumentRoot /var/www/amikomspj
```
lalu tekan CTRL+O > ENTER > CTRL+X

Kemudian mengedit website berikutnya, yakni dilarang.net yang diawali dengan command:
```sh
nano dilarang.conf
```
selanjutnya, ENTER. 
Setelah muncul jendela konfigurasinya, servername diubah menjadi nama websitenya, begitupun pada ServerAdmin-nya dan DokumenRoot-nya menjadi:
```sh
ServerName www.dilarang.net

ServerAdmin webmaster@dilarang
DocumentRoot /var/www/dilarang
```
lalu tekan CTRL+O > ENTER > CTRL+X
Setelah dikonfigurasi, beralih ke DokumenRoot-nya
### 5.4 Membuat folder di folder /var/www
Masukkan command:
```sh
cd /var/www
```
lalu ENTER, kemudian masukkan command:
```sh
ls
```
untuk melihat folder yang sudah ada.
Selanjutnya membuat direktori dengan command:
```sh
mkdir amikomspj dilarang
```
selanjutnya, mengganti hak akses direktori yang dibuat tadi menjadi full access (bisa dibaca, ditulis dan dieksekusi) dengan command:
```sh
chmod -R 777 amikomspj dilarang
```
lalu ENTER, maka dengan itu direktori menjadi full access.
### 5.5 Membuat halaman Website menggunakan bahasa pemrograman html yang sederhana pada direktori amikomspj dan juga dilarang
Pertama masuk pada folder amikomspj dengan command:
```sh
cd amikomspj
```
kemudian membuat halaman websitenya dengan command:
```sh
nano index.html
```
lalu ENTER.
Kemudian membuat halaman website sederhana seperti berikut:
```sh
<html>
<head>
      <title>Amikom SPJ</title>
</head>
<body>
      <h1>Amikom SPJ</h1>
Selamat datang di website Amikom SPJ.
</body>
</html>
```
kemudian tekan CTRL+O > ENTER > CTRL+X.
Selanjutnya copy file html yang diedit di folder amikomspj ke folder dilarang dengan command:
```sh
cp index.html /var/www/dilarang/index.html
```
lalu ENTER, selanjutnya masuk ke folder dilarang dengan command:
```sh
cd /var/www/dilarang
```
setelah masuk, maka lakukan edit dengan masuk ke:
```sh
nano index.html
```
selanjutnya mengeditnya menjadi:
```sh
<html>
<head>
      <title>Situs Terlarang</title>
</head>
<body>
      <h1>Mohon maaf, situs ini tidak dapat diakses!</h1>
Jangan panik, situs ini hanya bisa diakses oleh mahasiswa Amikom SPJ.
</body>
</html>
```
kemudian tekan CTRL+O > ENTER > CTRL+X.
### 5.6 Melakukan Enablesites pada webserver yang telah dikonfigurasi
Commandnya adalah:
```sh
a2ensite amikomspj.conf dilarang.conf
```
atau apabila command diatas tidak berfungsi, maka masukkan command berikut:
```sh
/usr/sbin/a2ensite amikomspj.conf dilarang.conf
```
lalu ENTER. Setelah itu akan muncul status 
Enabling site amikomspj.
Enabling site dilarang.
Maka, akan diperintahkan untuk me-reload apache2 dengan command:
```sh
systemctl reload apache2
```
ENTER, setelah itu me-restart dengan command:
```sh
systemctl restart apache2
```
lalu ENTER.
### 5.7 Pengujian pada website yang telah dibuat 
Buka web browser dan harus incognito/mode samaran, dengan mengakses:
```sh
www.amikomspj.com
```
```sh
www.dilarang.net
```
jika tidak berhasil, maka harus mengganti nama domainnya pada konfigurasi DNS Server dan dicoba kembali.
# 6 Penginstalan apk database
```sh
apt install phpmyadmin mariadb-server php-mysql php-json php-mbstring php-zip php-gd php-xml php-curl
```
### 6.1 Penginstalan Mysql (mariadb)
```sh
mysql_secure_instalation
```
### 6.2 Masuk ke konsol mysql
```sh
mysql -u root -p
```
### 6.3 Membuat database
```sh
create database ung;
```
### 6.4 Cara melihat database yang sudah dibuat
```sh
show databases;
```
### 6.5 Membuat user
```sh
create user 'ung'@'localhost' identified by '1';
```
### 6.6 Membuat privilage atau akses agar user ung hanya bisa mengakses database ung saja
```sh
 grant all privileges on tkj.sql to 'ung'@'localhost';
```
 ### 6.7 Setelah kita memberikan akses kita harus memasukan perintah lagi atau mengaktifkan konfigurasi yang ada di dalamnya
```sh
 flush privileges;
```
 ### 6.8 keluar dari konsol mysql
```sh
 quit
```
 ### cara mengujinya
```sh
 buka google chrome lalu ketik www.amikomspj.com/phpmyadmin
```








