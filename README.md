# MarketKita — Aplikasi Web Marketplace Sederhana

Aplikasi Web Marketplace berbasis **PHP Native (PHP 8.x)**, **MySQLi (Prepared Statements & Transaction)**, dan **Bootstrap 5.3** yang mempertemukan **Penjual** dan **Pembeli**.

---

## Fitur Utama

1. **Menampilkan Daftar Produk (`index.php`)** — pencarian kata kunci, filter per kategori, pengurutan harga/terbaru/nama, dan pagination.
2. **Menampilkan Detail Produk (`product.php`)** — foto produk, harga, stok real-time, jumlah terjual, profil toko penjual, deskripsi, dan produk terkait.
3. **Mengelola Data Produk / CRUD (`dashboard/products.php`, `product_form.php`, `product_delete.php`)** — tambah, lihat, ubah, dan hapus produk beserta unggah gambar (JPG/PNG/WEBP/GIF maks. 2 MB).
4. **Keranjang Belanja (`cart.php`, `cart_action.php`)** — tambah ke keranjang (UPSERT), ubah kuantitas dengan validasi stok, hapus item, kosongkan keranjang, serta tombol **Beli Sekarang**.
5. **Transaksi Pemesanan (`checkout.php`, `orders.php`, `order_detail.php`, `dashboard/orders.php`)** — checkout atomik menggunakan **Database Transaction (`BEGIN` / `COMMIT` / `ROLLBACK`)** dan row locking (`SELECT ... FOR UPDATE`), pengurangan stok otomatis, kode invoice unik (`INV-YYYYMMDD-XXXXXX`), pembatalan pesanan (stok dikembalikan), serta pembaruan status pesanan oleh penjual/admin.
6. **Multi-Role Pengguna (`login.php`, `register.php`, `profile.php`)** — **Admin**, **Penjual (Seller)**, dan **Pembeli (Buyer)** dengan password di-hash (`password_hash` Bcrypt) dan proteksi CSRF di seluruh form POST.

---

## Struktur Direktori

```text
web-marketplace/
├── assets/
│   ├── css/style.css              # Custom stylesheet
│   ├── js/app.js                  # Interaksi UI (qty +/-, preview gambar, konfirmasi)
│   └── img/
│       ├── products/              # Foto produk bawaan (seed)
│       ├── favicon.svg
│       └── no-image.svg
├── config/
│   ├── .htaccess                  # Blokir akses langsung dari browser
│   ├── config.example.php         # Template konfigurasi (di-commit ke Git)
│   └── config.php                 # Konfigurasi aktif (DI-IGNORE oleh Git)
├── dashboard/
│   ├── index.php                  # Ringkasan statistik penjual / admin
│   ├── products.php               # Daftar produk (Read)
│   ├── product_form.php           # Tambah & edit produk (Create & Update)
│   ├── product_delete.php         # Hapus produk (Delete)
│   ├── orders.php                 # Daftar pesanan masuk
│   ├── order_detail.php           # Detail & ubah status pesanan
│   ├── categories.php             # CRUD kategori (Admin)
│   └── users.php                  # Manajemen pengguna (Admin)
├── database/
│   ├── .htaccess
│   └── marketplace.sql            # Skema 6 tabel + Foreign Key + data awal (seed)
├── docs/
│   └── erd_dan_mockup.html        # Diagram ERD & Mockup UI siap cetak
├── includes/
│   ├── init.php                   # Bootstrap aplikasi, session, error handler
│   ├── db.php                     # Koneksi MySQLi & helper prepared statement
│   ├── functions.php              # Helper keamanan (CSRF, XSS), auth, upload, format
│   ├── header.php                 # Template navbar
│   ├── footer.php                 # Template footer
│   └── dashboard_sidebar.php      # Sidebar dashboard
├── uploads/products/              # Tujuan upload foto produk baru
├── index.php                      # Halaman daftar produk (beranda)
├── product.php                    # Halaman detail produk
├── cart.php                       # Halaman keranjang belanja
├── cart_action.php                # Endpoint aksi keranjang (POST)
├── checkout.php                   # Halaman checkout & simpan transaksi pesanan
├── orders.php                     # Riwayat pesanan pembeli
├── order_detail.php               # Detail pesanan pembeli
├── login.php                      # Halaman masuk
├── register.php                   # Halaman daftar akun
├── logout.php                     # Proses keluar (POST)
├── profile.php                    # Halaman profil & ganti password
├── .gitignore                     # Mencegah config.php & kredensial terunggah ke GitHub
└── .htaccess                      # Keamanan Apache
```

---

## Cara Menjalankan di Lokal (XAMPP)

1. Salin atau arahkan folder `web-marketplace` ke `C:\xampp\htdocs\web-marketplace`.
2. Jalankan **Apache** dan **MySQL** dari **XAMPP Control Panel**.
3. Buka `http://localhost/phpmyadmin`, buat database baru bernama **`marketplace_db`**, lalu **Import** file [`database/marketplace.sql`](database/marketplace.sql).
4. Salin `config/config.example.php` menjadi `config/config.php` dan pastikan isinya:
   ```php
   'db_host' => 'localhost',
   'db_user' => 'root',
   'db_pass' => '',
   'db_name' => 'marketplace_db',
   ```
5. Buka `http://localhost/web-marketplace` di browser.

### Akun Demo Bawaan

| Peran | Email | Password |
| :--- | :--- | :--- |
| **Pembeli (Buyer)** | `budi@marketplace.test` | `buyer123` |
| **Penjual (Seller)** | `toko.andi@marketplace.test` | `seller123` |
| **Penjual (Seller)** | `toko.sari@marketplace.test` | `seller123` |
| **Admin** | `admin@marketplace.test` | `admin123` |

> **Penting:** Segera ubah password akun demo melalui menu **Profil Saya** setelah aplikasi di-deploy ke hosting publik.

---

## Keamanan Repository Publik

File `config/config.php` sudah tercantum di [`.gitignore`](.gitignore) sehingga **password database dan kredensial hosting tidak akan terunggah ke GitHub**. Hanya `config/config.example.php` (berisi nilai kosong/placeholder) yang disertakan di dalam repository.
