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

<h2>Instalasi Database Server Menggunakan MySQL</h2>

Langkah-langkahnya meliputi:
1. Melakukan instalasi paket database server yaitu mysql-server
   ```sh
   apt install mysql-server
   ```
   
2. Masuk ke MySQL menggunakan root
   ```sh
   sudo mysql
   ```
   
3. Menambahkan password untuk root pada MySQL
   ```sh
   ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Password_anda!?3';
   ```

4. Menguji password tersebut dengan login ke mysql menggunakan user root
   ```sh
   mysql -u root -p
   ```
   Kemudian masukkan password, maka akan berhasil masuk

5. Keluar MySQL dengan perintah
   ```sh
   exit
   ```

<h2>Instalasi PHP-FPM</h2>

PHP-FPM (PHP FastCGI Process Manager) adalah komponen server web yang memisahkan dan mengelola proses PHP secara efisien, meningkatkan kinerja dan skalabilitas dengan memungkinkan konfigurasi dinamis dari pool proses PHP. Dengan memanfaatkan arsitektur FastCGI, PHP-FPM dapat berinteraksi dengan server web seperti Nginx atau Apache, memastikan penanganan permintaan PHP yang cepat dan terisolasi, serta memberikan kontrol yang lebih baik terhadap manajemen sumber daya dan kestabilan server.

Langkah-langkahnya meliputi:
1. Melakukan instalasi paket php-fpm versi terbaru
   ```sh
   apt install -y php-fpm php-mysql php-cli
   ```

<h2>Instalasi PHPmyadminL</h2>

Langkah-langkahnya meliputi:
1. Melakukan instalasi paket phpmyadmin
   ```sh
   apt install phpmyadmin
   ```
2. Pada saat proses instalasi akan terdapat pilihan untuk memilih webserver Apache atau Lighttpd, klik TAB untuk tidak memilih.
3. Ketika ada pilihan _allow dbconfig-common to install a database and configure_ Pilih Yes dan tekan ENTER.
4. Buat file baru untuk file konfigurasi phpmyadmin dengan perintah
   ```sh
   nano /etc/nginx/snippets/phpmyadmin.conf
   ```
5. Paste script dibawah ke dalam file phpmyadmin.conf
   ```sh
   location /phpmyadmin {
	    root /usr/share/;
	    index index.php index.html index.htm;
	    location ~ ^/phpmyadmin/(.+\.php)$ {
	        try_files $uri =404;
	        root /usr/share/;
	        fastcgi_pass unix:/run/php/php8.0(php ver name)-fpm.sock;
	        fastcgi_index index.php;
	        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
	        include /etc/nginx/fastcgi_params;
	    }

                  location ~* ^/phpmyadmin/(.+\.(jpg|jpeg|gif|css|png|js|ico|html|xml|txt))$ {
	        root /usr/share/;
	    }

	}```
6. Kemudian buka file _/etc/nginx/sites-available/default_ dengan perintah nano
   ```sh
   nano /etc/nginx/sites-available/default
   ```
7. Tambahkan konfigurasi _include snippets/phpmyadmin.conf;_ di dalam block server{}
   ```sh
   server {
		    . . .
              		           include snippets/phpmyadmin.conf;
		    . . .
		}
   ```
8. Restart nginx dengan perintah
   ```sh
   systemctl restart nginx
   ```
9. Ujicoba pada browser dengan membuka _http://ipserver/phpmyadmin_ kemudian masukkan user root dan password sesuai yang disetting pada saat konfigurasi MySQL.
    ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/d7e6e401-2739-4c06-aea6-06b62848bdad)

