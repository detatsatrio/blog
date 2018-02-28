---
layout: post
title:  Cara Mereset Password MySQL di Ubuntu Server 14.04
date:   2018-02-27 20:50:20 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
img: separate-database.png # Add image post (optional)
tags: [Blog, MySQL, Ubuntu Server]
author: Deta Teguh Satrio # Add name author (optional)
---
Minggu lalu saya mengalami laporan dari rekan kerja persoalan aplikasi yang tidak bisa berjalan secara normal (buggy), Setelah saya server ternyata API yang digunakan mati dan beberapa komponen tidak berjalan. Dan dengan jurus andalan logging, yaitu melihat aktivitas server di log server saya menemukan hal aneh karna ditemukan error MySQL. Dan benar saja ketika saya coba masuk SQL Server sama sekali tidak mau masuk. di restart pun begitu.

Akhirnya saya memutuskan untuk mereset password user MySQL. Bagi kalian yang mengalami nasib yang sama berikut langkah-langkah untuk mereset password user MySQL dengan menggunakan SSH.

> Catatan :
> Password MySQL berbeda dengan Password Server, Root user memiliki hak akses yang penuh untuk mengakses server. Akseslah server menggunakan kredential user root.

## 1. Koneksikan SSH ke Server disini saya menggunakan [Putty](https://www.putty.org/).

## 2. Matikan service MySQL
untuk server Ubuntu and Debian, gunakan:
```bash
sudo /etc/init.d/mysql stop
```

sedangkan server CentOS, Fedora, and Red Hat Enterprise Linux, gunakan:
```bash
sudo /etc/init.d/mysqld stop
```

## 3. Jalankan MySQL tanpa password

```bash
sudo mysqld_safe --skip-grant-tables &
```

## 4. Buka session SSH baru aplikasi putty. dan masuk sebagai root dengan perintah:

```bash
mysql -uroot
```

## 5. Update kredential user root.
```bash
use mysql;

update user set password=PASSWORD("mynewpassword") where User='root';

flush privileges;

quit
```

## 6. Stop dan Jalankan service MySQL.

untuk server Ubuntu and Debian, gunakan:
```bash
sudo /etc/init.d/mysql stop
...
sudo /etc/init.d/mysql start
```

sedangkan server CentOS, Fedora, and Red Hat Enterprise Linux, gunakan:
```bash
sudo /etc/init.d/mysqld stop
...
sudo /etc/init.d/mysqld start
```

## 7. Tes kredential baru dengan mencoba masuk ke database
```bash
mysql -u root -p
```

Semoga bermanfaat
