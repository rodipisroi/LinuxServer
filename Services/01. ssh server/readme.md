<h1>INSTALASI SSH SERVER</h1>

<h2>Paket yang digunakan adalah <i>openssh</i></h2>

Langkah-langkahnya meliputi:
1. Jalankan perintah <i>apt install openssh-server</i>
   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/7df5311a-27e4-43ae-b8a1-beb2ee6b560b)
2. Buka file <i>sshd_config</i> menggunakan text editor. Pathnya terletak di _/etc/ssh/sshd_config_
   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/84573851-b13d-4958-a640-07a5cd00174b)
3. [OPSIONAL]Hilangkan tanda pagar (#) pada baris _Passwordauthentication = Yes_ untuk mengizinkan client login menggunakan password sebagai metode autentikasi
   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/87daf103-c8a7-40e4-a8e8-d96a448564fe)
4. [OPSIONAL]Hilangkan tandda pagar (#) pada baris _Permitrootlogin yes_ untuk mengizinkan client login sebagai root
   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/c32ec440-8378-45c7-9bab-3b2742214f4d)


