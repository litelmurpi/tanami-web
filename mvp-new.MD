### Deskripsi Sistem
Smart Urban Farming Marketplace adalah platform jual-beli produk pertanian urban yang menghubungkan petani lokal dengan konsumen. Sistem ini dirancang untuk menangani transaksi semi-otomatis dimana sistem mampu menghitung total belanjaan secara cerdas (Harga Produk + Ongkir Spesifik Kota - Diskon Kupon), namun pembayaran tetap divalidasi secara manual melalui upload bukti transfer untuk kemudahan implementasi.

### Ruang Lingkup Proses
1.  **Manajemen Katalog (Petani):** Pengelolaan data produk sayur/buah dengan status stok *ready*.
2.  **Manajemen Wilayah & Tarif (Admin):** Pengaturan database kota tujuan pengiriman beserta tarif ongkos kirim tetap (*flat rate*) per kota.
3.  **Manajemen Promosi (Admin):** Pembuatan kode voucher/kupon diskon dengan batasan waktu (*expired date*).
4.  **Keranjang & Checkout:** Sistem kalkulasi total bayar yang dinamis.
5.  **Verifikasi Pesanan:** Validasi pembayaran manual (cek mutasi & bukti foto) oleh petani/admin.
6.  **Ulasan Produk:** Pembeli dapat memberikan rating dan komentar setelah transaksi selesai.

### Tujuan Sistem
* **Bagi Petani:** Memperluas pasar dengan sistem pencatatan pesanan yang rapi.
* **Bagi Pembeli:** Mendapatkan transparansi biaya kirim dan keuntungan dari promo/diskon.
* **Bagi Pengembang:** Mengimplementasikan relasi database majemuk (*Many-to-Many* dan logika bisnis transaksional).

---

## 3. Identifikasi Pengguna dan Kebutuhannya

### A. Pembeli (Customer)
**Kebutuhan:**
* Melihat katalog produk berdasarkan kategori.
* Menambahkan produk ke keranjang belanja (*Cart*).
* **Penting:** Memilih kota pengiriman saat checkout agar biaya ongkir muncul otomatis.
* **Penting:** Memasukkan kode kupon (jika ada) untuk mendapatkan potongan harga.
* Mengunggah bukti pembayaran (gambar/foto).
* Melacak status pesanan (Pending -> Paid -> Shipped -> Completed).
* Memberikan ulasan (*Review*) pada produk yang telah dibeli.

### B. Petani (Seller)
**Kebutuhan:**
* Menambah, mengedit, dan menghapus data produk (CRUD).
* Melihat daftar pesanan masuk (*Incoming Orders*).
* Memverifikasi bukti transfer pembeli.
* Mengupdate status pesanan menjadi "Dikirim".
* Merespons ulasan dari pembeli.

### C. Admin (Platform Owner)
**Kebutuhan:**
* Mengelola Data Kota & Tarif Ongkir (Master Data).
* Mengelola Data Kupon Diskon (Kode, Jumlah Potongan, Masa Berlaku).

---

## 4. Desain Sistem Informasi

### A. Alur Proses (Activity Diagram Summary)

**Alur Transaksi Utama:**
```mermaid
graph TD
    A[Start] --> B[User Login & Pilih Produk];
    B --> C[Masuk Keranjang];
    C --> D[Checkout];
    D --> E{Pilih Kota Tujuan};
    E --> F[Sistem Tarik Biaya Ongkir dari DB];
    F --> G{Input Kode Kupon?};
    G -- Ya --> H[Validasi & Potong Harga];
    G -- Tidak --> I[Hitung Grand Total Normal];
    H --> I;
    I --> J[User Transfer & Upload Bukti];
    J --> K[Petani Verifikasi & Kirim Barang];
    K --> L[User Terima & Review];
    L --> M[End];

### B. Rule Bisnis
1.  **Logika Ongkir:** User wajib memilih kota tujuan. Sistem mengambil harga ongkir dari tabel `cities` berdasarkan pilihan tersebut.
2.  **Logika Kupon:** Kupon hanya valid jika kode sesuai dan belum melewati tanggal kadaluarsa (*expired date*). Kupon mengurangi *Subtotal*, bukan mengurangi ongkir.
3.  **Logika Stok:** Stok produk berkurang otomatis saat checkout berhasil atau saat pembayaran diverifikasi.
4.  **Logika Review:** User hanya diperbolehkan memberi ulasan satu kali per item dalam transaksi yang berstatus *Completed*.

### C. Rancangan Database (ERD) & Struktur Data
*Sistem menggunakan 9 Entitas Utama.*

#### 1. Tabel Master & User
* **`users`**: Menyimpan data login (Admin, Farmer, Customer).
    * *(id, name, email, password, role, address, phone)*
* **`banks`**: Menyimpan info rekening tujuan transfer.
    * *(id, bank_name, account_number, account_name)*

#### 2. Tabel Produk & Wilayah
* **`categories`**: Kategori produk (Sayur, Buah).
    * *(id, name, slug)*
* **`products`**: Data barang dagangan.
    * *(id, farmer_id, category_id, name, price, stock, description, image)*
* **`cities`** (Fitur Ongkir): Daftar kota dan tarifnya.
    * *(id, city_name, shipping_cost)*

#### 3. Tabel Transaksi & Promosi
* **`coupons`** (Fitur Diskon): Data kode voucher.
    * *(id, code, discount_amount, valid_until, is_active)*
* **`orders`**: Header transaksi.
    * *(id, user_id, shipping_city_id, subtotal, shipping_cost, discount_amount, grand_total, status, payment_proof)*
* **`order_items`**: Detail barang yang dibeli per transaksi.
    * *(id, order_id, product_id, quantity, price)*

#### 4. Tabel Interaksi
* **`reviews`**: Ulasan pembeli.
    * *(id, order_id, product_id, user_id, rating, comment)*

### D. Sampling Data (Sesuai Struktur)
Berikut simulasi perhitungan data pada tabel `orders` untuk menunjukkan logika bisnis berjalan:

**Skenario:**
User membeli **2 kg Wortel** (@Rp 10.000) dikirim ke **Bogor** (Ongkir Rp 15.000) menggunakan kupon **PROMO5** (Diskon Rp 5.000).

| Tabel | Kolom | Nilai Data | Keterangan |
| :--- | :--- | :--- | :--- |
| `order_items` | quantity | 2 | 2 item |
| `order_items` | price | 10000 | Harga per item |
| **`orders`** | **subtotal** | **20000** | (2 * 10.000) |
| `cities` | shipping_cost | 15000 | Tarif Kota Bogor |
| **`orders`** | **shipping_cost** | **15000** | Masuk ke order |
| `coupons` | discount_amount | 5000 | Nilai kupon |
| **`orders`** | **discount_amount**| **5000** | Potongan |
| **`orders`** | **grand_total** | **30000** | (20rb + 15rb) - 5rb |

*Total yang harus ditransfer user adalah **Rp 30.000**.*
