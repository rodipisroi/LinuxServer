<h1>Hardening SSH</h1>
Langkah-langkah yang harus dicek antara lain:
1. Cek permissions file _sshd_config_. File /etc/ssh/sshd_config perlu dilindungi dari perubahan yang tidak sah dengan pengguna yang tidak memiliki hak istimewa.
Untuk mengecek permissions file _sshd_config_, gunakan perintah
```sh
stat /etc/ssh/sshd_config
```
Langkah-langkahnya meliputi:
1. Melakukan instalasi paket ssh server yaitu openssh
   ```sh
   apt get install openssh
   ```

3. Buka file <i>sshd_config</i> menggunakan text editor. Pathnya terletak di _/etc/ssh/sshd_config_
   ```sh
   nano /etc/ssh/sshd_config
   ```
