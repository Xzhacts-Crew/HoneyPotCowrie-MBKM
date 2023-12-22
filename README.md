# HoneyPotCowrie-MBKM
LIST NAMA ANGGOTA KELOMPOK
- Mohamad Farhan Palilati 23.66.0020 (Ketua) - System Security Administrator
- Siti Marwa 23.66.0024 (Anggota) - Network Administrator

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
test
