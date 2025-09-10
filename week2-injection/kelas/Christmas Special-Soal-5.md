# Juice-Shop Write-up: Christmas Special
Tujuan soal ini adalah mengeksploitasi SQL Injection untuk mengakses produk Christmas offer tersembunyi yang kemungkinan dihapus atau disembunyikan dari daftar produk normal (ditandai sebagai deleted dalam database).

## Tools yang Digunakan
- Web Browser: Untuk berinteraksi dengan antarmuka web Juice Shop.
- BurpSuite: Untuk memonitor dan memodifikasi traffic network.

## Metodologi dan Solusi

1. Menggunakan Pencarian Produk:
   - Fungsi pencarian di `127.0.0.1:3000/rest/products/search?q=payload` diidentifikasi sebagai titik SQL injection, mirip dengan tantangan sebelumnya yang melibatkan ekstraksi data user.

2. Mengeksploitasi Fitur Pencarian:
   - Memulai SQL injection dengan query `test')) UNION SELECT * FROM PRODUCTS WHERE deletedAt IS NOT NULL--`.

   ![Christmas_Special_1](https://cdn.discordapp.com/attachments/1031076369930649622/1415315081872408776/christmas_special_1.png?ex=68c2c262&is=68c170e2&hm=340a13a861290a6fff93d559681eb4940b88e70a2774c86698e39e492c6c0076&)

   - Ini didasarkan pada hipotesis bahwa produk tersembunyi atau spesial seperti Christmas offer mungkin ditandai sebagai deleted dalam sistem.

3. Mengekstrak Detail Produk Tersembunyi:
   - SQL Injection berhasil mengembalikan detail produk yang tidak terlihat dalam daftar produk reguler, termasuk Christmas offer.
   - `productId` untuk Christmas offer ditemukan adalah `10`.

4. **Manipulating the Cart**:
   - Menambahkan produk ke keranjang belanja dan menangkap request menggunakan BurpSuite:
     
   ![Christmas_Special_2](https://cdn.discordapp.com/attachments/1031076369930649622/1415315082404958259/christmas_special_2.png?ex=68c2c262&is=68c170e2&hm=a4f2acb9b0fab644c8182857f825546816bcb357ac57d4803c59a7d5e7bcd46c&)

   - Kemudian menambahkan produk spesial ke keranjang belanja dengan mengatur `ProductId` menjadi `10` dalam POST request:

   ![Christmas_Special_3](https://cdn.discordapp.com/attachments/1031076369930649622/1415315083541610517/christmas_special_3.png?ex=68c2c263&is=68c170e3&hm=1598933fffb0ad9f4d99c611dbd3ed3fc7647ae9b270d47706c1f0bf430b4c6d&)

## Penjelasan
Soal ini diselesaikan dengan memanfaatkan SQL Injection dalam API pencarian produk untuk mengungkap produk tersembunyi. Dengan menyuntikkan SQL yang query untuk produk dengan field `deletedAt` yang tidak null, produk Christmas offer berhasil terungkap. `productId` yang diperoleh melalui injection ini kemudian digunakan untuk menambahkan produk ke keranjang belanja, melewati mekanisme browsing dan seleksi normal.
