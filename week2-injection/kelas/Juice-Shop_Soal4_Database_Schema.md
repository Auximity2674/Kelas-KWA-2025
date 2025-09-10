# Juice-Shop Write-up: Database Schema
Soal ini menguji kemampuan dalam mengekstrak skema database melalui SQL injection, yang bisa mengungkap detail struktural sensitif dari database backend.

## Tools yang Digunakan
- Web Browser: Untuk mengakses aplikasi web.
- HTTP Interception Tool (contoh: Burp Suite): Untuk menangkap dan memodifikasi HTTP request untuk SQL injection.

## Metodologi dan Solusi

### Mengidentifikasi Titik Injection
1. Menemukan Endpoint yang Rentan:
   - Mengidentifikasi parameter dalam aplikasi yang rentan terhadap SQL injection. Untuk tantangan ini, fungsi pencarian produk di `127.0.0.1:3000/rest/products/search?q=payload` digunakan, di mana parameter `q` adalah yang rentan.

### Menyusun Payload Injection
2. SQL Injection untuk Ekstraksi Skema:
   - Menggunakan operator SQL UNION untuk menyuntikkan query yang akan mengungkap skema database. Karena hasil pencarian produk ditampilkan dalam format terstruktur, dan diketahui bahwa entri produk biasanya memiliki beberapa atribut (kolom), payload SQL injection harus menyesuaikan dengan jumlah kolom yang benar.
   - Menggunakan payload: `test')) UNION SELECT 1, 2, 3, 4, 5, 6, 7, 8, sql FROM sqlite_schema--` untuk menyesuaikan dengan jumlah kolom yang diharapkan dalam query asli dan mengekstrak informasi skema dari tabel `sqlite_schema`.
   ![database schema_1](https://cdn.discordapp.com/attachments/1031076369930649622/1415310442946887681/database_schema_1.png?ex=68c2be10&is=68c16c90&hm=355cb27ee95a016f0b3d7b3627e98ed8c4284c5dd04ba89e089e766b36beb840&)

### Mengekstrak Schema
3. Menjalankan Injection:
   - Submit payload yang sudah disusun melalui parameter yang rentan. Jika jumlah kolom dan tipe data yang diharapkan oleh query SQL asli benar-benar tercermin dalam injection UNION SELECT, aplikasi akan menampilkan isi skema database dalam hasil pencarian.
   - Hasilnya mencakup informasi kritis tentang struktur tabel, detail kolom, dan kadang bahkan petunjuk tentang relasi dan constraint.

### Mendokumentasikan Hasil
4. Merekam Informasi Schema:
   - Capture dan dokumentasikan output dari SQL injection, yang merinci skema database. Informasi ini sangat berharga untuk memahami layout database untuk eksploitasi atau analisis lebih lanjut.

### Penjelasan
Soal ini diselesaikan dengan mengeksploitasi kerentanan SQL injection untuk mengekstrak skema database, langkah kritis dalam memahami struktur dasar mekanisme penyimpanan data aplikasi. Ini tidak hanya menunjukkan kelemahan keamanan yang signifikan tapi juga mengekspos informasi sensitif yang berpotensi bisa digunakan untuk eskalasi serangan atau pelanggaran data.
