<h1>INSTALASI DAN KONFIGURASI NTP SERVER</h1>

## Paket yang digunakan adalah: `ntp` atau `chrony`.

<h2>Instalasi dan Konfigurasi NTP Server Menggunakan NTP</h2>

Langkah-langkahnya meliputi:
1. Melakukan instalasi paket ssh server yaitu ntp
   ```sh
   apt install ntp
   ```

3. Buka file <i>ntp.conf</i> yang merupakan file konfigurasi NTP menggunakan text editor. Pathnya terletak di _/etc/ntp.conf_
   ```sh
   nano /etc/ntp.conf
   ```

5. [OPSIONAL]Ubah lokasi pool NTP Server ke lokasi terdekat untuk mengurangi latensi/delay. Daftar pool dapat dilihat [disini](https://support.ntp.org/Servers/NTPPoolServers)<br>

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/0f876066-289a-4355-bea7-2abbe1115c68)

6. [OPSIONAL]Hilangkan tandda pagar (#) pada baris _Permitrootlogin yes_ untuk mengizinkan client login sebagai root<br>
 
   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/c32ec440-8378-45c7-9bab-3b2742214f4d)

7. Simpan menggunakan shortcut _CTRL+O_ apabila menggunakan text editor nano atau command _:w_ apabila menggunakan vim
8. Lakukan ujicoba pada client
   
   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/538245ae-8b2e-428f-aa56-e165f45406d5)


