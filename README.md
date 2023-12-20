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
```sh
cek 123
```
# 2. INSTALLASI COWRIE HONEYPOT SSH
### 2.1 Install Dependency
```sh
apt-get install git python3-virtualenv libssl-dev libffi-dev build-essential libpython3-dev python3-minimal authbind virtualenv
```
### 2.2 Create a user account
```sh
sudo adduser --disabled-password cowrie
```
### 2.3 Checkout the code
```sh
git clone http://github.com/cowrie/cowrie
```
### 2.4 Setup Virtual Environment
```sh
python -m venv cowrie-env
```
### 2.5 activate virtual environment
```sh
source cowrie-env/bin/activate
```
### 2.6 Starting Cowrie
```sh
bin/cowrie start
```
Setelah Run, Untuk melihat log dari aktifitas honeypotnya secara real time bisa dengan command :
```sh
tail -f var/log/cowrie/cowrie.log
```


