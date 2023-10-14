<h1>Hardening SSH</h1>

Langkah-langkah yang harus dicek antara lain:

1. Cek permissions file _sshd_config_. Pastikan hanya user _root_ yang memiliki akses.

   File /etc/ssh/sshd_config perlu dilindungi dari perubahan yang tidak sah dengan pengguna yang tidak memiliki hak istimewa.
  Untuk mengecek permissions file _sshd_config_, gunakan perintah

    ```sh
    stat /etc/ssh/sshd_config
    ```

   Outputnya akan seperti ini:

   ![image](https://github.com/rodipisroi/LinuxServer/assets/104636035/cad041de-a30f-4bb2-a779-ec154a9205ab)

