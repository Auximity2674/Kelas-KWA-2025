# Juice-Shop Write-up: Login Bender
Soal ini fokus pada eksploitasi kerentanan SQL injection untuk bypass mekanisme autentikasi dan mendapatkan akses tidak sah ke akun user, khususnya menargetkan akun yang terkait dengan "Bender" dalam aplikasi.

## Tools yang Digunakan
- Web browser

## Metodologi

### Mencari Informasi Target
1. Mengidentifikasi Email Bender:
   - Login ke aplikasi sebagai admin untuk mengakses fungsi administratif.
   - Menemukan email Bender (`bender@juice-sh.op`) dalam panel admin.
     ![login_bender_1](https://cdn.discordapp.com/attachments/1031076369930649622/1415304292713234512/login_bender_1.png?ex=68c2b856&is=68c166d6&hm=44db38a3028f71f5197ef7bdaf3e2a4e4791471e87dffa1d3a86dbf7e14fcd72&)

### Melakukan SQL Injection
1. Mencoba Login sebagai Bender:
   - Navigasi ke halaman login aplikasi.
   - Memasukkan email Bender di field email.

2. Menyusun SQL Injection:
   - Memodifikasi entry di input field email untuk menyertakan payload SQL injection: `bender@juice-sh.op' AND '1'='1' --`.
   - Payload ini mencoba memanipulasi query SQL di balik form login dengan menutup string email (menggunakan single quote), menambahkan kondisi yang selalu bernilai true (`AND '1'='1'`), dan kemudian meng-comment sisa perintah SQL dengan `--`.
  
### Mengamati Hasilnya
1. Mengamati Bypass Autentikasi:
   - Dengan payload yang disubmit, query SQL yang diubah oleh injection akan salah mengautentikasi sesi sebagai Bender tanpa memerlukan password.
   - Berhasil login sebagai Bender, menunjukkan eksploitasi kerentanan SQL injection.
     ![login_bender_2](https://cdn.discordapp.com/attachments/1031076369930649622/1415304293191389204/login_bender_2.png?ex=68c2b856&is=68c166d6&hm=151ce1f5185f7d62171982fe197cec8129f11d8b6ac62d04c1d393203c82e9b6&)

## Solusi
Soal ini diselesaikan dengan mengeksploitasi kerentanan SQL injection klasik di field email form login. Kerentanan spesifik ini memungkinkan manipulasi query SQL yang mendasarinya, sehingga memungkinkan akses tidak sah ke akun Bender dengan selalu membuat bagian kondisional dari query bernilai true dan bypass verifikasi password.
