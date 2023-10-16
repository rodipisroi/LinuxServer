<h1>INSTALASI SSH SERVER</h1>

SSH (Secure Shell) server adalah perangkat lunak yang digunakan untuk memberikan layanan akses jarak jauh yang aman ke sebuah sistem atau komputer melalui protokol SSH. Ini memungkinkan pengguna untuk melakukan koneksi aman dan mengendalikan sistem dari jarak jauh dengan enkripsi yang kuat, yang menjaga data dan akses dari potensi serangan dan pengawasan yang tidak diinginkan. SSH sering digunakan untuk mengelola server, mentransfer file, dan menjalankan perintah pada mesin jarak jauh.

## Paket yang digunakan adalah: `openssh`.

Langkah-langkahnya meliputi:
1. Melakukan instalasi paket ssh server yaitu openssh
   ```sh
   apt get install openssh
   ```

3. Buka file <i>sshd_config</i> menggunakan text editor. Pathnya terletak di _/etc/ssh/sshd_config_
   ```sh
   nano /etc/ssh/sshd_config
   ```

5. [OPSIONAL]Hilangkan tanda pagar (#) pada baris _Passwordauthentication = Yes_ untuk mengizinkan client login menggunakan password sebagai metode autentikasi<br>

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/87daf103-c8a7-40e4-a8e8-d96a448564fe)

6. [OPSIONAL]Hilangkan tandda pagar (#) pada baris _Permitrootlogin yes_ untuk mengizinkan client login sebagai root<br>
 
   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/c32ec440-8378-45c7-9bab-3b2742214f4d)

7. Simpan menggunakan shortcut _CTRL+O_ apabila menggunakan text editor nano atau command _:w_ apabila menggunakan vim
8. Lakukan ujicoba pada client
   
   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/538245ae-8b2e-428f-aa56-e165f45406d5)


