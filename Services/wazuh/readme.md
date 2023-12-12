
<h1>Instalasi dan Konfigurasi Wazuh sebagai Tools SIEM</h1>

Wazuh merupakan salah satu aplikasi open source yang berfungsi sebagai sistem deteksi berbasis host (endpoint). Wazuh melakukan analisis log, pemeriksaan integritas, pemantauan registry Windows, deteksi rootkit, peringatan berbasis waktu, dan respons aktif. Wazuh terdiri dari dua bagian, yaitu Wazuh-Server dan Wazuh-Agent. Wazuh-Server merupakan perangkat yang digunakan sebagai manajemen agen dan dashboard sistem monitoring baik file integrity, intrusion, maupun log. Sedangkan, Wazuh-Agent merupakan perangkat yang diinstal pada perangkat endpoint untuk melakukan pembacaan sistem, pengumpulan log, dan mengirimkan ke Wazuh-Server.


## Persiapan yang dibutuhkan

- 1 Virtual Machine Pusat sebagai Wazuh Server
- 1 Virtual Machine Host yang terinstall Wazuh Agent

## Skema Topologi

![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/6a87cb9c-85c8-4f05-8ab7-21e0e4fb8582)


<h2>Instalasi Wazuh Indexer, Wazuh Server, Wazuh Dashboard pada Wazuh Server</h2>

## Wazuh Indexer

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

## Wazuh Manager

Langkah-langkahnya meliputi:

1. Install wazuh manager
   ```sh
   apt-get -y install wazuh-manager
   ```
   
2. Enable wazuh manager
   ```sh
   systemctl daemon-reload
   systemctl enable wazuh-manager
   systemctl start wazuh-manager
   ```
   
3. Cek status wazuh manager
   ```sh
   systemctl status wazuh-manager
   ```
   
4. Install filebeat
   ```sh
   apt-get -y install filebeat
   ```

5. Download filebeat config
   ```sh
   curl -so /etc/filebeat/filebeat.yml https://packages.wazuh.com/4.7/tpl/wazuh/filebeat/filebeat.yml
   ```

6. Edit file _/etc/filebeat/filebeat.yml_. Ubah host sesuai alamat IP Wazuh

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/dd432954-b221-4faf-ae05-58998344f3f1)

7. Create filebeat keystore
   ```sh
   filebeat keystore create
   ```

8. Tambahkan username dan password default admin:admin
   ```sh
   echo admin | filebeat keystore add username --stdin --force
   echo admin | filebeat keystore add password --stdin --force
   ```

9. Download alert template untuk wazuh indexer
   ```sh
   curl -so /etc/filebeat/wazuh-template.json https://raw.githubusercontent.com/wazuh/wazuh/v4.7.0/extensions/elasticsearch/7.x/wazuh-template.json
   chmod go+r /etc/filebeat/wazuh-template.json
   ```

10. Install wazuh module filebeat
    ```sh
    curl -s https://packages.wazuh.com/4.x/filebeat/wazuh-filebeat-0.3.tar.gz | tar -xvz -C /usr/share/filebeat/module
    ```

11. Deploy certificate. Ubah kata (_$NODE_NAME_) menjadi nama node sesuai _config.yml_. Misal: _node-1_
    ```sh
    mkdir /etc/filebeat/certs
    tar -xf ./wazuh-certificates.tar -C /etc/filebeat/certs/ ./$NODE_NAME.pem ./$NODE_NAME-key.pem ./root-ca.pem
    mv -n /etc/filebeat/certs/$NODE_NAME.pem /etc/filebeat/certs/filebeat.pem
    mv -n /etc/filebeat/certs/$NODE_NAME-key.pem /etc/filebeat/certs/filebeat-key.pem
    chmod 500 /etc/filebeat/certs
    chmod 400 /etc/filebeat/certs/*
    chown -R root:root /etc/filebeat/certs
    ```
   
12. Start filebeat
    ```sh
    systemctl daemon-reload
    systemctl enable filebeat
    systemctl start filebeat
    ```

13. Cek apakah konfigurasi filebeat berhasil
    ```sh
    filebeat test output
    ```

## Wazuh Dashboard

Langkah-langkahnya meliputi:

1. Install paket required
   ```sh
   apt-get install debhelper tar curl libcap2-bin #debhelper version 9 or later
   ```

2. Install wazuh dashboard
   ```sh
   apt-get -y install wazuh-dashboard
   ```

3. Edit file _/etc/wazuh-dashboard/open_dashboard.yml_. Ganti _opensearch_host_ sesuai IP Wazuh

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/2cdd629c-ceeb-47fd-a1f3-4484bad37274)

4. Deploy certificate. Ubah kata (_$NODE_NAME_) menjadi nama node sesuai _config.yml_. Misal: _node-1_
   ```sh
   mkdir /etc/wazuh-dashboard/certs
   tar -xf ./wazuh-certificates.tar -C /etc/wazuh-dashboard/certs/ ./$NODE_NAME.pem ./$NODE_NAME-key.pem ./root-ca.pem
   mv -n /etc/wazuh-dashboard/certs/$NODE_NAME.pem /etc/wazuh-dashboard/certs/dashboard.pem
   mv -n /etc/wazuh-dashboard/certs/$NODE_NAME-key.pem /etc/wazuh-dashboard/certs/dashboard-key.pem
   chmod 500 /etc/wazuh-dashboard/certs
   chmod 400 /etc/wazuh-dashboard/certs/*
   chown -R wazuh-dashboard:wazuh-dashboard /etc/wazuh-dashboard/certs
    ```

5. Start wazuh dashboard
   ```sh
   systemctl daemon-reload
   systemctl enable wazuh-dashboard
   systemctl start wazuh-dashboard
   ```

6. Akses dashboard wazuh dengan _https://ip-wazuh_. Default password admin:admin 

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/e9573887-ce10-4cf1-bdf8-aaeddb5e645d)

## Install Wazuh Agent pada Linux

Langkahnya adalah:

1. Install GPG-key
   ```sh
   curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && chmod 644 /usr/share/keyrings/wazuh.gpg
   ```

2. Menambahkan Repository
   ```sh
   echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list
   ```

3. Update Repository
   ```sh
   apt-get update
   ```
   
4. Install Wazuh Agent. Sesuaikan IP Address dengan IP Wazuh Server.
   ```sh
   WAZUH_MANAGER="10.0.0.2" apt-get install wazuh-agent
   ```

5. Cek wazuh agent di wazuh dashboard

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/5c3c1197-1346-44af-9fea-a4456d69489f)
