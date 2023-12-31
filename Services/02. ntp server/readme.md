<h1>INSTALASI DAN KONFIGURASI NTP SERVER</h1>

NTP (Network Time Protocol) server adalah sebuah server yang digunakan untuk menyediakan referensi waktu yang akurat dan sinkronisasi waktu dalam jaringan komputer. NTP server memungkinkan perangkat dalam jaringan untuk menjaga waktu yang konsisten dengan standar waktu global, seperti UTC (Coordinated Universal Time), sehingga memungkinkan berbagai perangkat di jaringan untuk bekerja dengan waktu yang tepat dan seragam.

## Paket yang digunakan adalah: `ntp` atau `chrony`.

<h2>Instalasi dan Konfigurasi NTP Server Menggunakan NTP</h2>

Langkah-langkahnya meliputi:
1. Melakukan instalasi paket ssh server yaitu ntp
   ```sh
   apt install ntp
   ```

2. Buka file <i>ntp.conf</i> yang merupakan file konfigurasi NTP menggunakan text editor. Pathnya terletak di _/etc/ntp.conf_
   ```sh
   nano /etc/ntp.conf
   ```

3. [OPSIONAL]Ubah lokasi pool NTP Server ke lokasi terdekat untuk mengurangi latensi/delay. Daftar pool dapat dilihat [disini](https://support.ntp.org/Servers/NTPPoolServers/)<br>

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/0f876066-289a-4355-bea7-2abbe1115c68)

   Apabila menggunakan server Indonesia, ubah menjadi:

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/f0edfd86-44f6-41f6-9641-e479410e6ab1)


4. [OPSIONAL]Ini adalah baris server cadanagan apabila server utama mati<br>
 
   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/6cf1baa8-49f6-43d7-afb3-960de9d7d3a4)

5. Restart service NTP menggunakan perintah:

   ```sh
   systemctl restart ntp
   ```
   
6. [OPSIONAL]Check service NTP apakah sudah berjalan atau belum dengan perintah:

   ```sh
   ntpq -p
   ```

   Apabila berhasil, maka hasilnya adalah seperti berikut:

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/1779454c-a51e-4def-bf12-c43781092854)

## Konfigurasi NTP CLient pada Windows

1. Pergi ke _Control Panel\Clock and Region\Date and Time\Internet Time_

2. Ubah Servernya sesuai dengan IP Server yang telah terinstall NTP Server

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/e52b7f1d-437a-4fab-bdda-b56e6a245f54)

## Konfigurasi NTP Client pada Linux

1. Buka file _timesyncd.conf_ menggunakan text editor.

   ```sh
   nano /etc/systemd/timesyncd.conf
   ```

2. Hilangkan tanda pagar (#) pada baris _#NTP=_. Isi dengan alamat server Linux yang telah terinstall NTP.

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/c777d3dd-7fd6-4d31-bd18-f4efea11287e)

3. Restart waktu dengan perintah

   ```sh
   sudo systemctl restart systemd-timesyncd.service
   ```
   
4. Cek apakah client sudah berhasil mendapatkan/sinkronisasi waktu dari server dengan perintah

   ```sh
   timedatectl timesync-status
   ```

   Hasilnya adalah:

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/3b3ec4a0-e7bc-4f91-acf0-512a2e31d816)




