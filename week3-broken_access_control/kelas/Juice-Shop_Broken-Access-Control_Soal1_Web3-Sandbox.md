# Juice-Shop Write-up: Web3 Sandbox
Challenge ini mengharuskan penemuan code sandbox Web3 yang secara tidak sengaja di-deploy dan memungkinkan penulisan serta testing smart contract secara on-the-fly. Ini mengeksplorasi konsep broken access control dalam aplikasi web, khususnya di lingkungan yang melibatkan teknologi Web3.

## Tools yang Digunakan
- Web browser

## Metodologi dan Solusi
Proses untuk menyelesaikan tantangan Web3 Sandbox ini relatif mudah:
1. Mulai dengan mengeksplorasi URL yang berbeda terkait fungsi Web3 pada aplikasi, menduga bahwa sandbox mungkin berada di path yang agak tersembunyi atau kurang obvious.
   
2. Menggunakan pendekatan tebakan sederhana, terinspirasi dari nama direktori umum yang terkait dengan testing atau development environment seperti `/sandbox`, `/test`, `/dev`, atau serupa. Dalam kasus ini, mencoba variasi hingga mencapai URL yang benar: `http://127.0.0.1:3000/#/web3-sandbox`.

3. Setelah navigasi ke URL yang benar, menemukan environment Web3 code sandbox yang berfungsi penuh. Sandbox ini mencakup fitur untuk editing, compiling, dan deploying smart contract Ethereum langsung dari browser.

![web3_sandbox_1](https://cdn.discordapp.com/attachments/1031076369930649622/1415592554900099163/web3_interface_1.png?ex=68c3c4cd&is=68c2734d&hm=c651804eb7ddacf5b0f64d99a0805f4b5874a8d533c3aa19171ad79d8d8d7756&)

## Penjelasan
Soal ini mengeksploitasi kelalaian keamanan yang tipikal di mana development tools atau fitur eksperimental secara tidak sengaja dibiarkan dapat diakses di production environment. Mengakses URL secara langsung memberikan akses tanpa batasan ke fungsionalitas yang powerful yang dimaksudkan untuk penggunaan terkontrol, menimbulkan risiko keamanan yang signifikan.
