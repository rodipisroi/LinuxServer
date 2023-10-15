<h1>Hardening SSH</h1>

Langkah-langkah yang harus dicek antara lain:

<h3>1. Cek permissions file _sshd_config_. Pastikan hanya user _root_ yang memiliki akses.</h3>

   File /etc/ssh/sshd_config perlu dilindungi dari perubahan yang tidak sah dengan pengguna yang tidak memiliki hak istimewa.
  Untuk mengecek permissions file _sshd_config_, gunakan perintah

    ```sh
    stat /etc/ssh/sshd_config
    ```

   Outputnya akan seperti ini:

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/cad041de-a30f-4bb2-a779-ec154a9205ab)

   Ubah izin hak aksesnya sehingga hanya user _root_ yang dapat memiliki izin. Perintahnya adalah:

   ```sh
   chmod 600 /etc/ssh/sshd_config
   ```

   Sehingga hasil akhirnya akan menjadi seperti berikut:

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/f6e1a845-e1cc-4af2-a7fe-562388c5c63c)

<h3>2. Batasi client yang dapat mengakses SSH Server.</h3>

   Pastikan untuk membatasi SSH Server agar hanya bisa diakses oleh client dengan alamat IP tertentu. Untuk melakukannya, tambahkan konfigurasi dibawah ini:

   ```sh
   AllowUsers your_username@ip_address
   ```

   example: 
   
   _AllowUsers root@192.168.1.200_ untuk bisa diakses dengan alamat ip spesifik

   _ALlowUsers guest@192.168.1.0/24_ untuk bisa diakses oleh user yang memiliki ip dengan network 192.168.1.0

   

