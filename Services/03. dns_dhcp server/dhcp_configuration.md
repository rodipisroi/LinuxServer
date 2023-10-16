<h1>KONFIGURASI DHCP SERVER</h1>

DHCP (Dynamic Host Configuration Protocol) server adalah server yang digunakan untuk mengotomatisasi pengaturan jaringan dalam sebuah jaringan komputer. DHCP server memberikan alamat IP, informasi subnet mask, alamat gateway, dan alamat server DNS secara dinamis kepada perangkat yang terhubung ke jaringan. Ini memungkinkan perangkat untuk secara otomatis mengkonfigurasi diri mereka sendiri tanpa perlu pengaturan manual, memudahkan administrasi jaringan yang lebih efisien.

## Paket yang digunakan adalah: `dnsmasq`.

<h2>Konfigurasi DHCP Server Menggunakan dnsmasq</h2>

Langkah-langkahnya meliputi:

1. Buka file <i>dnsmasq.conf</i> yang merupakan file konfigurasi dnsmasq menggunakan text editor. Pathnya terletak di _/etc/dnsmasq.conf_
   
   ```sh
   nano /etc/dnsmasq.conf
   ```

2. Pada baris ke 158, Hilangkan tanda pagar (#) serta atur konfigurasi range IP address yang akan diberikan ke client serta waktu peminjaman.

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/5e158505-fbc3-4661-be9e-16db0cb9c958)

   Pada gambar tersebut, rentangnya adalah 192.168.76.100 sampai 192.168.76.199, waktu peminjaman adalah 12 jam.
   
3. Pada baris ke 335, Hilangkan tanda pagar (#) serta atur default gateway untuk client.<br>
 
   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/9bc851b4-cb2a-4a5f-bc35-4684c72dab8a)

4. Pada baris ke 344, Hilangkan tanda pagaar (#) serta atur ntp server, dns server serta netmask untuk DHCP client.<br>

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/43af8e30-6d93-4473-824b-411d29d13b36)
    
## Pengujian pada mendapatkan IP DHCP pada client

1. Pengujian pada Windows

   Pergi ke _Control Panel\Network and Internet\Network Connections_. Pilih interface yang terhubung pada DHCP Server. Klik kanan. Klik Properties. Double click pada IPv4. Ubah settingannya menjadi DHCP. Klik OK.

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/cf9798f7-9881-41cc-b699-d2f92ca7b076)
