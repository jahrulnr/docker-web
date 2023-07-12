# Docker - Nginx, PHP 7.2-fpm, dan MariaDB

## Deskripsi

Proyek ini berisi konfigurasi Docker untuk menjalankan aplikasi web menggunakan Nginx sebagai server web, PHP 7.2-fpm sebagai pengolah PHP, dan MariaDB sebagai database.

## Persyaratan

Pastikan Anda telah menginstal Docker dan Docker Compose sebelum melanjutkan.

- Docker: [https://docs.docker.com/install/](https://docs.docker.com/install/)
- Docker Compose: [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/)

## Penggunaan

1. Clone repository:
   `git clone https://github.com/jahrulnr/docker-web.git`
2. Pindah ke direktori proyek:
   `cd docker-web`
3. Jalankan container menggunakan Docker Compose:
   `docker-compose up -d`
4. Setelah selesai, Anda dapat mengakses aplikasi web melalui `http://localhost` pada browser Anda.

## Konfigurasi

- **Nginx**: File konfigurasi Nginx dapat ditemukan di direktori `nginx/conf.d`. Anda dapat mengubah konfigurasi server Nginx sesuai kebutuhan.

- **PHP**: File konfigurasi PHP-FPM dapat ditemukan di direktori `php`. Anda dapat menyesuaikan konfigurasi PHP sesuai kebutuhan.

- **MariaDB**: Konfigurasi MariaDB dapat diperbarui melalui file `docker-compose.yml`. Anda dapat menyesuaikan variabel lingkungan yang terkait dengan layanan MariaDB.

## Lisensi

Proyek ini dilisensikan di bawah Lisensi MIT. Silakan lihat berkas [LICENSE](LICENSE) untuk informasi lebih lanjut.

## Referensi

- [https://github.com/markhilton/docker-php-fpm](https://github.com/markhilton/docker-php-fpm)
