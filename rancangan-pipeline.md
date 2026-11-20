# Rancangan Pipeline CI/CD

**Nama Proyek:** Aplikasi Toko Online Sederhana

## Trigger Pipeline
- **Event Pemicu:** `push` dan `pull_request`
- **Branch Target:** `main` (atau `master`)

---

## Tahapan (Stages) Pipeline

### 1. Source Control
- **Alat / Layanan:** Git & GitHub
- **Perintah / Aktivitas:**
  - Mengambil kode terbaru dari repository menggunakan `actions/checkout@v4` atau `git pull origin main`.

### 2. Build
- **Alat / Layanan:** Docker & Composer
- **Perintah / Aktivitas:**
  - Menginstal dependensi aplikasi: `composer install --no-dev --optimize-autoloader`
  - Membangun container image aplikasi: `docker build -t toko-online:latest .`

### 3. Test
- **Alat / Layanan:** PHPUnit / Linter
- **Perintah / Aktivitas:**
  - Memeriksa sintaks kode: `php -l index.php`
  - Menjalankan pengujian otomatis: `./vendor/bin/phpunit`

### 4. Security Scanning
- **Alat / Layanan:** Trivy / Composer Audit
- **Perintah / Aktivitas:**
  - Memindai kerentanan paket dependensi: `composer audit`
  - Memindai kerentanan keamanan pada image container: `trivy image toko-online:latest`

### 5. Artifact Generation
- **Alat / Layanan:** Docker Hub / GitHub Container Registry (GHCR) / GitHub Artifacts
- **Perintah / Aktivitas:**
  - Mengemas hasil build atau mengunggah image siap pakai: `docker push username/toko-online:latest` atau `actions/upload-artifact@v4`

### 6. Deployment
- **Alat / Layanan:** Docker Compose & Self-Hosted Runner
- **Perintah / Aktivitas:**
  - Menjalankan ulang layanan aplikasi di server tujuan: `docker compose up -d --build`
