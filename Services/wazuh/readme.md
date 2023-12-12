
<h1>Instalasi dan Konfigurasi Wazuh sebagai Tools SIEM</h1>

Wazuh merupakan salah satu aplikasi open source yang berfungsi sebagai sistem deteksi berbasis host (endpoint). Wazuh melakukan analisis log, pemeriksaan integritas, pemantauan registry Windows, deteksi rootkit, peringatan berbasis waktu, dan respons aktif. Wazuh terdiri dari dua bagian, yaitu Wazuh-Server dan Wazuh-Agent. Wazuh-Server merupakan perangkat yang digunakan sebagai manajemen agen dan dashboard sistem monitoring baik file integrity, intrusion, maupun log. Sedangkan, Wazuh-Agent merupakan perangkat yang diinstal pada perangkat endpoint untuk melakukan pembacaan sistem, pengumpulan log, dan mengirimkan ke Wazuh-Server.

![1610636377991](https://github.com/rodipisroi/LinuxServer/assets/104636035/cdbc64e1-beda-448c-9f09-63e3aec8b3a0)

## Persiapan yang dibutuhkan

- 1 Virtual Machine Pusat sebagai Wazuh Server
- 1 Virtual Machine Host yang terinstall Wazuh Agent

## Skema Topologi

![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/6a87cb9c-85c8-4f05-8ab7-21e0e4fb8582)


<h2>Instalasi Wazuh Indexer, Wazuh Server, Wazuh Dashboard pada Wazuh Server</h2>

##Wazuh Indexer

Pada Virtual Machine pusat, langkah langkahnya meliputi:

1. Download file konfigurasi
   ```sh
   curl -sO https://packages.wazuh.com/4.7/wazuh-certs-tool.sh
   curl -sO https://packages.wazuh.com/4.7/config.yml
   ```
   
2. Edit file _config.yml_. Isikan alamat IP Wazuh Server.

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/fc0bef0a-1106-4c05-ac1f-54b7adc7b7ab)

3. Jalankan _./wazuh-certs-tool.sh_ untuk membuat certificate
   ```sh
   bash ./wazuh-certs-tool.sh -A
   ```

4. Kompress ke format .tar. 
   ```sh
   tar -cvf ./wazuh-certificates.tar -C ./wazuh-certificates/ .
   rm -rf ./wazuh-certificates
   ```


5. Install paket 
   ```sh
   apt-get install debconf adduser procps
   apt-get install gnupg apt-transport-https
   ```

6. Dapatkan GPG key
   ```sh
    curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && chmod 644 /usr/share/keyrings/wazuh.gpg
   ```


7. Menambahkan Repository
   ```sh
   echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list
   ```

8. Update Repository
   ```sh
   apt-get update
   ```

9. Install wazuh indexer
    ```sh
    apt-get -y install wazuh-indexer
    ```

10. Edit file _/etc/wazuh-indexer/opensearch.yml_. Ubah network host menjadi IP Wazuh

    ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/8717d038-6851-4eb4-9590-f22b8dd82823)

11. Deploy Certificate. Ubah kata (_$NODE_NAME_) menjadi nama node sesuai _config.yml_. Misal: _node-1_
    ```sh
    mkdir /etc/wazuh-indexer/certs
    tar -xf ./wazuh-certificates.tar -C /etc/wazuh-indexer/certs/ ./$NODE_NAME.pem ./$NODE_NAME-key.pem ./admin.pem ./admin-key.pem ./root-ca.pem
    mv -n /etc/wazuh-indexer/certs/$NODE_NAME.pem /etc/wazuh-indexer/certs/indexer.pem
    mv -n /etc/wazuh-indexer/certs/$NODE_NAME-key.pem /etc/wazuh-indexer/certs/indexer-key.pem
    chmod 500 /etc/wazuh-indexer/certs
    chmod 400 /etc/wazuh-indexer/certs/*
    chown -R wazuh-indexer:wazuh-indexer /etc/wazuh-indexer/certs
    ```
    
12. Start wazuh-indexer
    ```sh
    systemctl daemon-reload
    systemctl enable wazuh-indexer
    systemctl start wazuh-indexer
    ```
