<h1>Hardening SSH</h1>

Langkah-langkah yang harus dicek antara lain:

1. Cek permissions file _sshd_config_. Pastikan hanya user _root_ yang memiliki akses.

   File /etc/ssh/sshd_config perlu dilindungi dari perubahan yang tidak sah dengan pengguna yang tidak memiliki hak istimewa.
  Untuk mengecek permissions file _sshd_config_, gunakan perintah

    ```sh
    stat /etc/ssh/sshd_config
    ```

