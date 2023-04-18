---
title: Instalasi Minio Server Standalone
description: konfigurasi dan instalasi minio object storage single-node linux server
date: 2023-04-18
draft: false
tags: linux,minio,s3
---

Minio merupakan perangkat lunak penyedia layanan _object storage_ yang memiliki API kompatibel dengan 
[Amazon S3 Object Storage](https://aws.amazon.com/s3/). Minio bisa dijadikan solusi untuk menyediakan 
layanan _object storage_ untuk server privat. Minio dapat diinstal secara gratis untuk versi komunitas, dan memiliki
versi _enterprise_ jika perusahaan anda menginginkan _support enterprise_. Penulis pribadi menggunakan Minio untuk
kebutuhan lingkungan pengembangan aplikasi yang menggunakan _object storage_ sebagai komponen penyimpanan datanya. 
Pada artikel ini sendiri berisikan langkah-langkah _setup & deploy_ Minio untuk _environment_ spesifik saja.


# Environment 

- Linux Redhat Family

# Langkah-langkah konfigurasi dan menjalankan minio.

1. Instalasi Minio

    ```bash
    sudo dnf install https://dl.min.io/server/minio/release/linux-amd64/minio-20230413030807.0.0.x86_64.rpm
    ```

    > Note : \
    > Untuk beberapa versi OS Redhat family yang tidak memiliki package manager `dnf`, mereka dapat menggunakan package manager `yum`.

2. Membuat direktori untuk menyimpan data dari aplikasi Minio.

    ```bash
    sudo mkdir -p /data/minio
    ```

3. Menamambahkan user dan group untuk digunakan systemd sebagai identitas Minio.

    ```bash
    sudo groupadd -r minio-user
    sudo useradd -M -r -g minio-user minio-user
    sudo chown minio-user:minio-user /data/minio
    ```

4. Berikut ini konfigurasi minimal untuk minio pada file `/etc/default/minio`.

    ```bash
    MINIO_ROOT_USER="admin"
    MINIO_ROOT_PASSWORD="admin-password-123"
    MINIO_VOLUMES="/data/minio"
    MINIO_CONSOLE_ADDRESS="0.0.0.0:9080"
    ```

5. Menjalankan _service_ systemd Minio.

    ```bash
    sudo systemctl start minio
    ```

6. Memeriksa status _service_ systemd Minio.

    ```bash
    systemctl status minio

    # for debug log
    # journalctl -xeu minio
    ```

# Referensi

[Deploy MinIO: Single-Node Single-Drive](https://min.io/docs/minio/linux/operations/install-deploy-manage/deploy-minio-single-node-single-drive.html)

[Minio Resources](https://resources.min.io/l/library)