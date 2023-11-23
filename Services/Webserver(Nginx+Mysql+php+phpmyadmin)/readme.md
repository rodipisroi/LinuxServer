<h1>INSTALASI DAN KONFIGURASI WEBSERVER</h1>

Nginx merupakan sebuah webserver yang menyajikan solusi yang efisien, ringan, dan cepat untuk mengelola permintaan HTTP. Dengan kemampuan sebagai web server, reverse proxy, dan load balancer, Nginx memberikan performa optimal dalam menangani lalu lintas web. Keunggulannya tidak hanya terletak pada kemampuan untuk menangani banyak koneksi secara bersamaan dengan sumber daya yang efisien, tetapi juga dalam fleksibilitas konfigurasinya yang memungkinkan pengguna untuk mengatasi tugas-tugas kompleks dengan mudah. Selain itu, Nginx mendukung enkripsi SSL/TLS, berfungsi sebagai proxy untuk server aplikasi, dan dapat diandalkan sebagai bagian integral dari arsitektur yang terdistribusi. Dengan modularitas dan ekstensibilitasnya, Nginx menjadi pilihan utama untuk mengoptimalkan kinerja web server dan menangani berbagai kebutuhan di berbagai lingkungan pengembangan dan produksi.

## Paket yang digunakan adalah: `nginx, mysql-server, php-fpm, phpmyadmin`.

<h2>Instalasi WEB Server Menggunakan nginx</h2>

Langkah-langkahnya meliputi:
1. Melakukan instalasi paket web server yaitu nginx
   ```sh
   apt install nginx
   ```
   
2. Start nginx service dengan perintah

    ```sh
    systemctl start nginx
    ```
3. Ujicoba pada browser client dengan mengakses IP Address Webserver

    ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/acf7d28d-7029-4a7c-b3bd-74c12e8f2944)
