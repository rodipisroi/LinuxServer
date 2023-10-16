<h1>INSTALASI DAN KONFIGURASI DNS SERTA DHCP SERVER</h1>

DNS (Domain Name System) server adalah komputer atau server yang bertugas menerjemahkan nama domain seperti "www.example.com" ke alamat IP yang sesuai. DNS server memainkan peran kunci dalam infrastruktur internet dengan menghubungkan nama domain yang mudah diingat dengan alamat IP yang digunakan untuk mengidentifikasi server dan perangkat di internet. Dengan cara ini, DNS server memungkinkan perangkat untuk menemukan dan berkomunikasi satu sama lain di jaringan internet dengan menggunakan nama domain yang lebih mudah diingat daripada alamat IP numerik.

DHCP (Dynamic Host Configuration Protocol) server adalah server yang digunakan untuk mengotomatisasi pengaturan jaringan dalam sebuah jaringan komputer. DHCP server memberikan alamat IP, informasi subnet mask, alamat gateway, dan alamat server DNS secara dinamis kepada perangkat yang terhubung ke jaringan. Ini memungkinkan perangkat untuk secara otomatis mengkonfigurasi diri mereka sendiri tanpa perlu pengaturan manual, memudahkan administrasi jaringan yang lebih efisien.

## Paket yang digunakan adalah: `dnsmasq`.

<h2>Instalasi dan Konfigurasi DNS Server Menggunakan dnsmasq</h2>

Langkah-langkahnya meliputi:
1. Melakukan instalasi paket ssh server yaitu ntp
   ```sh
   apt install dnsmasq
   ```

2. Buka file <i>dnsmasq.conf</i> yang merupakan file konfigurasi dnsmasq menggunakan text editor. Pathnya terletak di _/etc/dnsmasq.conf_
   ```sh
   nano /etc/dnsmasq.conf
   ```

3. Pada baris ke 19, Hilangkan tanda pagar (#) pada baris kode _domain-needed_. Baris _domain-needed_ memastikan bahwa dnsmasq hanya akan menjawab permintaan DNS yang mencakup domain yang valid<br>

4. Hilangkan tanda pagar (#) pada baris kode _bogus-priv_. Dengan pengaturan ini, dnsmasq akan mengabaikan permintaan DNS yang mencoba mengakses alamat IP dalam rentang IP privat atau lokal.<br>
 
   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/3c786b47-a026-4b27-9488-f1f5e274c02b)

5. [OPSIONAL]Hilangkan tanda pagar (#) pada baris kode _strict-order_. Opsi strict-order dalam dnsmasq adalah aturan yang membuat server DNS tersebut hanya menggunakan daftar server DNS dalam urutan yang telah ditetapkan dalam konfigurasi. Artinya, jika server DNS pertama dalam daftar tidak merespons, maka dnsmasq tidak akan mencoba server DNS kedua, ketiga, dan seterusnya.<br>

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/bfa50849-fd2d-413b-8b8a-0fed0ef4f3af)

6. [OPSIONAL]Pada baris ke 67, tambahkan opsi query untuk spesifik domain dan spesifik dns server. Contohnya seperti pada gambar dibawah. Ini berarti bahwa ketika perangkat dalam jaringan mencoba mengakses domain "local.rodipisroi.com", dnsmasq akan mengirimkan permintaan DNS ke DNS server lain dengan alamat IP "192.168.76.11" untuk mendapatkan resolusi nama ke alamat IP yang sesuai.

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/78514df3-afcd-43e0-8a77-d7af418f26c8)

7. [OPSIONAL]Pada baris ke 106 dan 124. hilangkan tanda pagar(#) serta isi interface yang diinginkan untuk merespon permintaan DNS. Opsi _bind-interfaces_ dalam konfigurasi dnsmasq adalah opsi yang memerintahkan dnsmasq untuk hanya mendengarkan permintaan DNS pada interface yang telah ditentukan.

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/65e0450b-13b8-4198-a3a5-d8035e0fa7aa)

8. [OPSIONAL]Pada baris ke 135 dan 145, hilangkan tanda pagar (#) untuk mengkonversi ketikkan nama hostname menjadi domain.   

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/3f760c5d-1e6c-4c09-8737-3d5948a7cea3)

9. Simpan dan Keluar

10. Buat symbolic link/rujukan dari file _/run/systemd/resolve/resolv.conf_ merujuk ke _/etc/resolv.conf_ 

   ```sh
   ln -fs /run/systemd/resolve/resolv.conf /etc/resolv.conf
   ```
11. Restart dnsmasq service dengan perintah

    ```sh
    systemctl restart dnsmasq
    ```

    Note: apabila terdapat error, gunakan perintah _journalctl -xe_ untuk mencari letak errornya.

    
## Menambahkan query DNS

1. Buka file _/etc/hosts_

   ```sh
   nano /etc/hosts
   ```

2. Tambahkan entri DNS dengan format

   ```sh
   ip_server    nama_domain   hostname(opsional)
   ```

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/6dcf4b5c-dfd6-48ad-b491-0c097d4b3af2)

3. Simpan dan Keluar. Restart dnsmasq untuk merefresh daftar hosts.

   ```sh
   systemctl restart dnsmasq
   ```

## Pengecekan query DNS pada Client

1. Menggunakan perintah _nslookup_

   ```sh
   nslookup nama_domain/ip_address
   ```

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/fe992bb5-2e05-4495-9979-6dc82f19eb15)

2. Menggunakan perintah _dig_

   ```sh
   dig nama_domain/ip_address
   ```

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/f756b31f-3f36-41a5-9cee-79a0453fcd0b)
