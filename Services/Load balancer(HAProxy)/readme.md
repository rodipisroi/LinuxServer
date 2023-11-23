<h1>Instalasi dan Konfigurasi HAProxy sebagai Load Balancer</h1>

HAProxy, atau High Availability Proxy, adalah perangkat lunak load balancer dan proxy open-source yang dirancang untuk mendistribusikan lalu lintas jaringan dengan efisien dan mengoptimalkan kinerja serta ketersediaan aplikasi. Berfungsi sebagai reverse proxy, HAProxy dapat memutuskan rute lalu lintas berdasarkan berbagai kriteria seperti beban server, kesehatan server, dan algoritma balancing yang dapat disesuaikan. Selain itu, HAProxy mendukung SSL termination, memungkinkan enkripsi lalu lintas di lapisan aplikasi. Dengan kemampuan monitoring dan manajemen yang canggih, HAProxy menjadi solusi populer untuk meningkatkan skalabilitas, kehandalan, dan kinerja aplikasi web di lingkungan produksi.

![1610636377991](https://github.com/rodipisroi/LinuxServer/assets/104636035/cdbc64e1-beda-448c-9f09-63e3aec8b3a0)

## Persiapan yang dibutuhkan

- 1 Virtual Machine Pusat sebagai Load Balancer yang terinstall HAProxy
- 2 Virtual Machine sebagai Webserver yang identik

## Skema Topologi

![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/438b4236-8eaa-45c8-95b9-cbc829213992)


## Paket yang digunakan adalah: `haproxy`.

<h2>Instalasi Load Balancer Menggunakan HAProxy</h2>

Pada Virtual Machine pusat, langkah langkahnya meliputi:

1. Melakukan instalasi paket haproxy pada
   ```sh
   apt install haproxy
   ```
2. Menambahkan IP Address dari masing masing host/server
   ```sh
   nano /etc/hosts
   ```

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/3ee3b982-e12b-4f26-b731-4d8ffb8c162b)


3. Langkah selanjutnya, melakukan konfigurasi load balancer pada file haproxy.cfg
   ```sh
   nano /etc/haproxy/haproxy.cfg
   ```

4. Selanjutnya, menambahkan konfigurasi frontend untuk selalu listen port 80 (HTTP) pada baris paling bawah. 
   ```sh
   frontend front-loadbalance
    bind alamat IP ke client:80
    mode http
    default_backend back-loadbalance
   ```

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/1a6dc9ae-74b3-413a-89a7-8c49d1995a4d)

5. Tambahkan konfigurasi untuk backend pada bagian bawah. Untuk metode loadbalancernya menggunakan metode round robin
   ```sh
   backend back-loadbalance
    balance roundrobin
    server web-server1 IPAddress_webserver1 check port 80
    server web-server2 IPAddress_webserver2 check port 80
   ```

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/ed28265d-8160-42dd-92bb-cba94e10f233)


6. Tambahkan konfigurasi listen stats untuk mengaktifkan modul statistik pada HAProxy.
   ```sh
   listen stats
    bind ipaddress:8080
    stats enable
    stats hide-version
    stats refresh 30s
    stats show-node
    stats auth username:password
    stats uri /stats
   ```

   Berikut adalah arti dari setiap baris konfigurasi:
   - **listen stat**s: Menunjukkan bahwa konfigurasi ini terkait dengan mendengarkan (listen) statistik.
   - **bind ipaddress:8080**: Mengikat HAProxy untuk mendengarkan alamat IP Server dan port 8080.
   - **stats enable**: Mengaktifkan modul statistik.
   - **stats hide-version**: Menyembunyikan versi HAProxy dari output statistik untuk keamanan.
   - **stats refresh 30**s: Menentukan interval penyegaran statistik, dalam hal ini, setiap 30 detik.
   - **stats show-node**: Menampilkan informasi tentang node (server backend) dalam statistik.
   - **stats auth username:password**: Menetapkan nama pengguna dan kata sandi yang diperlukan untuk mengakses halaman statistik.
   - **stats uri /stats**: Menentukan URI atau path tempat statistik akan diakses, dalam hal ini, /stats.
7. 
