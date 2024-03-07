# Deploy Zookeeper dengan Docker Compose

## Overview

Zookeeper adalah layanan terdistribusi yang digunakan untuk menyimpan konfigurasi, melakukan sinkronisasi dalam sistem terdistribusi, dan menyediakan grup layanan. Zookeeper sangat penting dalam ekosistem Apache Kafka untuk mengelola metadata dan bertindak sebagai koordinator cluster.

## Requirements

- VM dengan Docker dan Docker Compose terinstal.
- VM harus memiliki setidaknya 1GB RAM untuk memenuhi memory limit yang ditentukan.
- Firewall pada VM harus mengizinkan traffic ingress pada port `12181` (atau port yang ditentukan dalam file `docker-compose.yaml`) untuk memungkinkan komunikasi dengan Zookeeper.

## Deployment Steps

1. **Persiapan VM**

   - Pastikan Docker dan Docker Compose sudah terinstal pada VM.
   - Buka port `12181` pada firewall VM untuk memungkinkan akses ke Zookeeper.

2. **Konfigurasi Zookeeper**

   - Unduh atau siapkan file `docker-compose.yaml`.
   - Sesuaikan nilai environment variable `ZOOKEEPER_SERVER_ID` jika Anda berencana mendeploy lebih dari 1 replica Zookeeper. Setiap replica harus memiliki ID yang unik.

3. **Menjalankan Zookeeper**

   - Navigasikan ke direktori tempat file `docker-compose.yaml` berada.
   - Jalankan perintah berikut untuk mendeploy Zookeeper:

     ```sh
     docker-compose up -d
     ```

   - Periksa status container untuk memastikan Zookeeper berjalan dengan baik:

     ```sh
     docker-compose ps
     ```

## Mendeploy Lebih dari 1 Zookeeper

Untuk mendeploy lebih dari satu instance Zookeeper menggunakan Docker Compose, Anda perlu menyesuaikan konfigurasi `docker-compose.yaml` dengan menambahkan lebih banyak service yang masing-masing memiliki `ZOOKEEPER_SERVER_ID` unik dan port yang sesuai. Berikut adalah langkah-langkahnya:

1. **Salin dan Sesuaikan Konfigurasi**: Duplikasi bagian service `zookeeper1` dalam file `docker-compose.yaml`. Untuk setiap replica baru, ubah nilai `ZOOKEEPER_SERVER_ID` dan sesuaikan port mapping-nya untuk menghindari konflik. Tambahkan konfigurasi `ZOOKEEPER_SERVERS`.

2. **Contoh Konfigurasi untuk Replica Kedua**:

   - Ubah `container_name` menjadi unik, misalnya `zookeeper2`.
   - Sesuaikan `ZOOKEEPER_SERVER_ID` menjadi `2`.
   - Ubah port mapping menjadi port yang belum digunakan, misalnya `12182:2181`.
   - Lebih lengkapnya bisa dilihat pada file `docker-compose.double.yaml`

3. Hubungkan dengan Zookeeper lain.

   - Isi `ZOOKEEPER_SERVERS` seperti pada file `docker-compose.double.yaml`
   - Pastikan VM membuka port 2888 dan 3888.

4. **Jalankan Docker Compose**:
   - Gunakan perintah `docker-compose up -d` untuk menjalankan semua instance Zookeeper yang telah dikonfigurasi.

Pastikan untuk mengupdate firewall dan konfigurasi jaringan VM sesuai dengan port yang digunakan oleh setiap replica Zookeeper. Ini memastikan bahwa setiap instance dapat berkomunikasi dengan benar baik di dalam cluster maupun dengan aplikasi eksternal yang membutuhkannya.

## Catatan
- Jika ingin membuat cluster zookeeeper, jumlah zookeeper harus ganjil
- Pastikan untuk membuka port 2888 dan 3888 pada docker beserta vm jika ingin menghubungkan dengan zookeeper lain di luar vm yang berbeda.
