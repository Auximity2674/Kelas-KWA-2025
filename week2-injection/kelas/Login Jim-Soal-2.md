# Juice-Shop Write-up: Login Jim
Soal ini membutuhkan eksploitasi kerentanan SQL Injection untuk bypass mekanisme login dan mendapatkan akses tidak sah ke akun user, khususnya akun Jim.

## Tools yang Digunakan
- **Web Browser**: Untuk berinteraksi dengan interface login aplikasi.

## Metodologi dan Solusi
1. Percobaan Login Awal:
   - Seperti tantangan akun admin sebelumnya, langkah pertama melibatkan pengecekan form login untuk kerentanan SQL Injection. Field input tempat kredensial user dimasukkan adalah titik eksploitasi yang umum.

2. Menyusun Query Injection:
   - Memasukkan perintah SQL yang sudah disusun ke dalam field username atau password yang mengubah query SQL backend agar selalu mengembalikan nilai true. Payload SQLi umum melibatkan penggunaan `' OR '1'='1' --` untuk memanipulasi query SQL.
   - Untuk Jim, payload akan dimasukkan di field username atau password: `' OR '1'='1' --`. Payload ini secara efektif membuat query SQL di balik form autentikasi mengembalikan nilai true, sehingga bypass kebutuhan username dan password yang benar.

   ![login_jim_1](https://cdn.discordapp.com/attachments/1031076369930649622/1415312010207432904/login_jim_1.png?ex=68c2bf86&is=68c16e06&hm=f285f7b50be03c3874b5bc0627ed6c4c04c201d473867b836ae12c49af7eb129&)

3. Mengakses Akun Jim:
   - Setelah submit payload SQLi melalui form login, aplikasi memproses query SQL yang sudah diubah, yang salah mengautentikasi sesi sebagai Jim karena query mengembalikan nilai true.
   - Berhasil bypass autentikasi, mendapatkan akses ke akun Jim tanpa mengetahui kredensial yang sebenarnya.

### Penjelasan
Soal diselesaikan dengan mengeksploitasi kerentanan SQL Injection dalam proses login. Jenis kerentanan ini muncul ketika input tidak disanitasi dengan proper, memungkinkan penyerang menyuntikkan kode SQL berbahaya.
- **Regular Security Audits and Testing**: Conduct vulnerability assessments and security audits regularly. Use tools and techniques to detect SQL injection vulnerabilities before attackers do.
- **Error Handling**: Do not display database error messages to users. Use generic error messages to avoid giving attackers hints about the database structure.
