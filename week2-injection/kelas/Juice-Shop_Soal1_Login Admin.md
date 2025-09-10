# Juice-Shop Write-up: Login Admin
Challenge ini mengharuskan kita untuk mengeksploitasi kerentanan SQL Injection dalam proses autentikasi sebuah aplikasi web untuk mendapatkan akses tidak sah ke akun administrator.

## Tools yang Digunakan
- Web Browser: Untuk berinteraksi dengan interface login aplikasinya.
- Burp Suite: Digunakan untuk menangkap dan memodifikasi HTTP request supaya bisa menyuntikkan kode SQL.

## Metodologi
Setelah menganalisis mekanisme login, ternyata ketika memasukkan karakter khusus seperti tanda kutip (') ke dalam field input, muncul error SQL yang menunjukkan adanya potensi serangan SQL Injection.

#### Langkah-langkah yang dilakukan untuk menyelesaikan soal:
1. Meng-Intercept Login Request
   Menggunakan Burp Suite, HTTP POST request yang dikirim saat percobaan login normal berhasil ditangkap. Request ini berisi kredensial user dalam bentuk data JSON.
   ![login_admin_1](https://cdn.discordapp.com/attachments/1031076369930649622/1415301315139207168/login_admin_1.png?ex=68c2b590&is=68c16410&hm=d696824ddb717fb5b00e5023410458141f69c2f8300115009bec63df4caecfb3&)

2. Memodifikasi Query SQL:
   Field email dalam JSON payload diubah menjadi `admin@juice-sh.op' OR '1'='1' --`, yang efektifnya mengubah perintah SQL menjadi statement yang selalu
   mengembalikan nilai true, sehingga bypass kebutuhan password.
   ![login_admin_2](https://cdn.discordapp.com/attachments/1031076369930649622/1415301418285269093/login_admin_2.png?ex=68c2b5a9&is=68c16429&hm=0c5c1469b33852ccd1f05e401773d3f82bf1097e8c8f6440f870944b1bbe402b&)

3. Menjalankan Request yang Sudah Diinject:
   Request yang sudah dimodifikasi dikirim ke server. Karena kode SQL yang diinjeksi, kondisi `OR '1'='1'` selalu bernilai true, sehingga memungkinkan akses tidak sah ke akun administrator.

## Solusi
SQL Injection berhasil dilakukan karena input sanitization yang tidak proper dan tidak adanya prepared statements dalam menangani query SQL. Kode yang diinjeksi mengubah struktur logis dari perintah SQL, sehingga bypass autentikasi dan berhasil login sebagai administrator tanpa perlu password yang sebenarnya.
   
