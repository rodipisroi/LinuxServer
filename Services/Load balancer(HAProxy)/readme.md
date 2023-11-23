<h1>Instalasi dan Konfigurasi HAProxy sebagai Load Balancer</h1>

HAProxy, atau High Availability Proxy, adalah perangkat lunak load balancer dan proxy open-source yang dirancang untuk mendistribusikan lalu lintas jaringan dengan efisien dan mengoptimalkan kinerja serta ketersediaan aplikasi. Berfungsi sebagai reverse proxy, HAProxy dapat memutuskan rute lalu lintas berdasarkan berbagai kriteria seperti beban server, kesehatan server, dan algoritma balancing yang dapat disesuaikan. Selain itu, HAProxy mendukung SSL termination, memungkinkan enkripsi lalu lintas di lapisan aplikasi. Dengan kemampuan monitoring dan manajemen yang canggih, HAProxy menjadi solusi populer untuk meningkatkan skalabilitas, kehandalan, dan kinerja aplikasi web di lingkungan produksi.

![1610636377991](https://github.com/rodipisroi/LinuxServer/assets/104636035/cdbc64e1-beda-448c-9f09-63e3aec8b3a0)

## Persiapan yang dibutuhkan

- 1 Virtual Machine Pusat sebagai Load Balancer yang terinstall HAProxy
- 2 Virtual Machine sebagai Webserver yang identik

## Paket yang digunakan adalah: `haproxy`.

<h2>Instalasi Load Balancer Menggunakan HAProxy</h2>

Langkah-langkahnya meliputi:

Virtual Machine Pusat:

1. Melakukan instalasi paket haproxy pada
   ```sh
   apt install haproxy
   ```
