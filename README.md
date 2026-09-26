# Dokploy AI Teams Templates

Katalog template resmi Dokploy dari **WednesDev** untuk orkestrasi Autonomous AI Departments, Tim Agen AI, dan Gateway LLM hemat token.

---

## 📦 Blueprints yang Tersedia

| Template ID | Komponen | Deskripsi |
|---|---|---|
| **`paperclip-ai-teams`** | Paperclip + 9Router + Headroom + PostgreSQL 17 | **Flagship All-in-One AI Department**: Org chart agen, heartbeat ticketing, multi-account routing otomatis, dan token cache proxy. |
| **`paperclip`** | Paperclip + PostgreSQL 17 | Standalone Paperclip control plane untuk manajemen tugas & agen AI. |
| **`9router`** | 9Router + Headroom | Standalone AI Model Router yang merotasi 40+ provider AI dengan auto-fallback. |

---

## 🚀 Cara Pemasangan di Dokploy

### Opsi A: Tambah sebagai Custom Template Repository

1. Buka dashboard **Dokploy** Anda.
2. Masuk ke menu **Settings** → **Templates**.
3. Masukkan URL template repository:
   ```text
   https://github.com/wednesdev-id/dockploy-ai-teams-templates
   ```
4. Klik **Save** / **Sync**. Seluruh blueprint akan muncul di galeri template Dokploy.

### Opsi B: Deploy Langsung via Docker Compose

1. Di Dokploy, pilih **Projects** → **Add Service** → **Compose**.
2. Salin isi file `docker-compose.yml` dari folder blueprint yang diinginkan (misal `blueprints/paperclip-ai-teams/docker-compose.yml`).
3. Konfigurasikan Environment Variables pada tab **Environment**:
   - `PAPERCLIP_DOMAIN` = `ai.domainanda.com`
   - `ROUTER_HOST` = `router.domainanda.com`
   - `POSTGRES_PASSWORD` = `<password_db_acak>`
   - `BETTER_AUTH_SECRET` = `<secret_64_karakter>`
   - `JWT_SECRET` = `<secret_64_karakter>`
   - `INITIAL_PASSWORD` = `<password_dashboard_9router>`
   - `API_KEY_SECRET` = `<secret_64_karakter>`
   - `MACHINE_ID_SALT` = `<salt_32_karakter>`
4. Klik **Deploy**.

---

## 🔒 Strategi Backup Restic & Disaster Recovery

Seluruh data transaksi agen, tiket, log approval, dan sesi akun AI tersimpan di named volumes:
- `paperclip-pgdata`: Database PostgreSQL 17
- `9router-data`: Sesi cookie & akun AI
- `paperclip-data`: Konfigurasi & workspace Paperclip

Gunakan script cold backup Restic yang tersedia di WednesDev host infrastructure:
```bash
/data/scripts/ai-team-backup.sh <CLIENT_ID>
```
Script ini akan:
1. Membekukan database sementara (downtime < 4 detik) untuk mencegah korupsi data disk.
2. Melakukan deduplikasi blok & enkripsi AES-256 via Restic.
3. Mengunggah snapshot ke Cloudflare R2 Cloud Vault.
4. Menyalakan database kembali secara otomatis.

Untuk restore:
```bash
/data/scripts/ai-team-restore.sh <CLIENT_ID> latest
```

---

## 📄 Lisensi
MIT License © 2026 WednesDev Cloud Infrastructure.
