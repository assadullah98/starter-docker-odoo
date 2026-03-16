# Odoo Starter Pack

Repository ini untuk development Odoo secara lokal menggunakan Docker.

---

## Prasyarat

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) sudah terinstall

---

## Setup Awal

1. Clone repository ini
2. Salin file environment:
   ```bash
   cp .env.example .env
   ```
3. Edit `.env` dan ganti password sesuai kebutuhan:
   ```
   POSTGRES_PASSWORD=password_kuat_anda
   ODOO_ADMIN_PASSWORD=password_kuat_anda
   ```
4. Jalankan Docker Compose:
   ```bash
   docker compose up -d
   ```
5. Buka browser dan akses http://localhost:10018

---

## Custom Module

Letakkan folder module kustom di dalam direktori `addons/`. Struktur minimal sebuah module:

```
addons/
└── nama_module/
    ├── __init__.py
    ├── __manifest__.py
    └── models/
        └── __init__.py
```

Contoh `__manifest__.py`:
```python
{
    'name': 'Nama Module',
    'version': '18.0.1.0.0',
    'category': 'Uncategorized',
    'depends': ['base'],
    'installable': True,
}
```

Setelah menambahkan module baru, update module list di Odoo:
- Aktifkan **Developer Mode** (Settings > Activate Developer Mode)
- Buka **Apps > Update Apps List**

---

## Perintah Berguna

**Jalankan container:**
```bash
docker compose up -d
```

**Hentikan container:**
```bash
docker compose down
```

**Lihat log Odoo:**
```bash
docker compose logs -f odoo18
```

**Install module via CLI:**
```bash
docker compose exec odoo18 odoo -d nama_database -i nama_module --stop-after-init
```

**Update module via CLI:**
```bash
docker compose exec odoo18 odoo -d nama_database -u nama_module --stop-after-init
```

**Masuk ke shell container Odoo:**
```bash
docker compose exec odoo18 bash
```

---

## Backup & Restore Database

**Backup:**
```bash
docker compose exec db pg_dump -U odoo nama_database > backup.sql
```

**Restore:**
```bash
docker compose exec -T db psql -U odoo nama_database < backup.sql
```

---

## Struktur Direktori

```
.
├── addons/          # Custom modules Odoo
├── etc/
│   └── odoo/
│       └── odoo.conf    # Konfigurasi Odoo
├── postgresql/      # Data PostgreSQL (di-ignore oleh git)
├── .env             # Environment variables (di-ignore oleh git)
├── .env.example     # Template environment variables
└── docker-compose.yml
```
