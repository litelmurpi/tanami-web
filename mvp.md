<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# bantu aku menyusun ini

Baik, aku bantu menyusun dokumen sesuai format tugas FP (Final Project) kamu. Berdasarkan gambar, ada 4 poin yang harus diselesakan (minimal 4 entitas untuk poin 4). Berikut susunannya:

***

# LAPORAN FINAL PROJECT

## Smart Urban Farming Marketplace


***

## 1. Kelompok dan Tema Project

**Kelompok:**

- [Nama Anggota 1] - [NIM]
- [Nama Anggota 2] - [NIM] (jika ada)
- [Nama Anggota 3] - [NIM] (jika ada)

**Tema Project:** Smart Urban Farming Marketplace (Platform Pre-Order Produk Organik Lokal)

**Sinkronisasi dengan Tugas Pemrograman Web Lanjut:**
Project ini merupakan kelanjutan dari tugas Pemrograman Web Lanjut yang mengimplementasikan sistem marketplace berbasis Laravel 12 dengan fokus pada transaksi jual-beli produk urban farming. Sistem ini mengintegrasikan konsep advanced web development dan database management untuk menciptakan platform e-commerce yang kompleks dan fungsional.

***

## 2. Abstraksi Sistem Informasi

### Deskripsi Sistem

Smart Urban Farming Marketplace adalah platform digital yang menghubungkan petani urban (urban farmers) dengan konsumen yang mencari produk organik segar dan lokal. Sistem ini menggunakan mekanisme pre-order untuk memastikan kesegaran produk dan mengurangi food waste, dimana konsumen dapat memesan produk sebelum masa panen tiba.

### Ruang Lingkup Proses

**Proses Utama:**

1. **Manajemen Pengguna Multi-Role**
    - Registrasi dan verifikasi farmer oleh admin
    - Autentikasi berbasis role (Customer, Farmer, Admin)
    - Manajemen profil dan preferensi pengguna
2. **Manajemen Produk dan Jadwal Panen**
    - Farmer membuat jadwal panen (harvest schedule) dengan informasi produk, quantity, tanggal panen, dan deadline pre-order
    - Upload foto produk dan deskripsi detail
    - Sistem subscription (Free vs Premium) untuk batasan jumlah produk
3. **Sistem Pre-Order dan Transaksi**
    - Customer browse produk berdasarkan harvest batch yang tersedia
    - Proses pre-order dengan validasi periode dan stok
    - Shopping cart dengan grouping berdasarkan farmer
    - Integrasi payment gateway (Midtrans) untuk pembayaran digital
    - Sistem escrow untuk hold payment hingga order selesai
4. **Manajemen Pengiriman**
    - Opsi self-pickup dengan time slot scheduling
    - Opsi delivery dengan kalkulasi ongkir berdasarkan area
    - Tracking status pengiriman real-time
5. **Sistem Keuangan dan Wallet**
    - Farmer wallet untuk menampung hasil penjualan
    - Auto-deduct commission platform (10%)
    - Withdrawal request dengan approval admin
    - Generate invoice dan sales report otomatis
6. **Review dan Rating**
    - Customer review produk setelah delivery confirmed
    - Rating system untuk farmer dan produk
    - Farmer response mechanism
7. **Community Engagement**
    - Farmer feed untuk share update dan tips
    - Follow system untuk notifikasi harvest baru
    - Comment dan interaction

### Tujuan Sistem

**Untuk Customer:**

- Mendapatkan produk organik segar langsung dari petani lokal
- Transparansi sumber makanan dan proses pertanian
- Fleksibilitas pilihan pickup atau delivery
- Support petani lokal dan ekonomi berkelanjutan

**Untuk Farmer:**

- Platform untuk menjual hasil panen dengan jangkauan lebih luas
- Sistem pre-order mengurangi risiko food waste
- Manajemen keuangan terintegrasi dengan wallet system
- Analytics untuk memahami customer behavior dan optimize production

**Untuk Platform:**

- Monetisasi melalui transaction fee dan premium subscription
- Membangun ekosistem pertanian urban yang sustainable
- Data-driven insights untuk pengembangan bisnis

***

## 3. Identifikasi Pengguna dan Kebutuhannya

### A. Customer (Buyer)

**Karakteristik:**

- Masyarakat urban yang peduli kesehatan dan sustainability
- Usia 25-45 tahun dengan daya beli menengah ke atas
- Familiar dengan teknologi dan online shopping

**Kebutuhan Fungsional:**

- Browse dan search produk berdasarkan kategori, lokasi, dan jadwal panen
- Pre-order produk dengan pembayaran digital yang aman
- Track status pesanan real-time dari harvest hingga delivery
- Pilihan self-pickup atau delivery sesuai preferensi
- Review dan rating untuk feedback
- Follow farmer favorit dan notifikasi harvest baru
- Akses order history dan invoice digital

**Kebutuhan Non-Fungsional:**

- Interface yang user-friendly dan responsive (mobile \& desktop)
- Proses checkout yang cepat (< 3 menit)
- Keamanan data pribadi dan payment information
- Notifikasi real-time via email/WhatsApp


### B. Farmer (Seller)

**Karakteristik:**

- Pemilik urban farm, hidroponik, atau vertical farming
- Produksi skala kecil-menengah (5-100 kg per harvest)
- Membutuhkan channel distribusi langsung ke konsumen

**Kebutuhan Fungsional:**

- Membuat dan manage harvest schedule dengan mudah
- Upload produk dengan multiple images
- Manage incoming orders dan update status fulfillment
- Monitor wallet balance dan transaction history
- Request withdrawal ke rekening bank
- View sales analytics (best products, revenue trends)
- Posting update dan engage dengan customers
- Respond to reviews

**Kebutuhan Non-Fungsional:**

- Dashboard yang informatif dengan visualisasi data
- Notifikasi instant saat ada order baru
- Export data untuk keperluan tax reporting
- Mobile-friendly untuk manage on-the-go


### C. Admin (Platform Operator)

**Karakteristik:**

- Tim internal platform
- Bertanggung jawab atas operasional dan quality control

**Kebutuhan Fungsional:**

- Verifikasi dan approve farmer registration
- Monitor semua transaksi di platform
- Approve/reject withdrawal requests
- Manage user accounts (suspend, delete)
- View platform analytics (GMV, revenue, active users)
- Moderate content (reviews, posts)
- Configure platform settings (commission rate, payment methods)

**Kebutuhan Non-Fungsional:**

- Real-time dashboard dengan KPI metrics
- Audit trail untuk semua admin actions
- Bulk operation tools untuk efisiensi
- Secure access dengan multi-factor authentication

***

## 4. Desain Sistem Informasi

### A. Alur Proses/Activity Diagram

**1. Proses Pre-Order (Customer)**

```
Start → Browse Marketplace → Filter/Search Produk → View Product Detail → 
Check Harvest Schedule → Add to Cart → Continue Shopping / Checkout → 
Input Shipping Address → Choose Delivery Method → Select Payment Method → 
Review Order Summary → Pay via Midtrans → Receive Order Confirmation → 
Track Order Status → Receive Product → Confirm Receipt → Write Review → End
```

**2. Proses Fulfillment (Farmer)**

```
Start → Login Dashboard → View New Orders → Check Order Details → 
Harvest on Schedule → Pack Products → Update Status "Ready" → 
(If Pickup) Set Pickup Schedule / (If Delivery) Arrange Courier → 
Update Tracking → Wait Customer Confirmation → Receive Payment in Wallet → 
View Transaction → Request Withdrawal → Wait Admin Approval → 
Receive Bank Transfer → End
```

**3. Proses Harvest Scheduling (Farmer)**

```
Start → Login Dashboard → Navigate to Harvest Management → 
Create New Batch → Select Product → Set Quantity Available → 
Set Harvest Date → Set Pre-order Deadline → Upload Photos → 
Set Price → Save & Publish → System Validate → Notify Followers → 
Monitor Pre-orders → Close Pre-order on Deadline → End
```


### B. Rule Bisnis

1. **Pre-Order Rules:**
    - Customer hanya bisa pre-order selama periode: (harvest_date - 7 hari) sampai pre_order_deadline
    - Quantity pre-order tidak boleh melebihi quantity_available di harvest batch
    - Satu customer bisa pre-order max 10 items per harvest batch
    - Pre-order otomatis close saat mencapai deadline atau stok habis
2. **Payment Rules:**
    - Payment harus completed dalam 24 jam setelah order created, jika tidak order auto-cancel
    - System menggunakan escrow: hold payment hingga customer confirm receipt
    - Auto-release payment ke farmer wallet H+3 setelah status "shipped" jika customer belum confirm
3. **Commission Rules:**
    - Platform fee: 10% dari total order (exclude ongkir)
    - Premium subscription farmer: Rp 75.000/bulan (unlimited products + analytics)
    - Free farmer: max 5 active harvest batches
4. **Withdrawal Rules:**
    - Minimum withdrawal: Rp 100.000
    - Maximum withdrawal: tidak melebihi available wallet balance
    - Admin harus approve withdrawal dalam 3 hari kerja
    - Setelah approval, transfer dilakukan dalam 1-2 hari kerja
5. **Review Rules:**
    - Customer hanya bisa review setelah order status "completed"
    - Satu order hanya bisa di-review 1 kali
    - Review tidak bisa di-edit setelah 7 hari
    - Farmer bisa respond ke review
6. **Product Rules:**
    - Product harus belong to valid category
    - Harvest date harus > today + 1 hari (minimum)
    - Pre-order deadline harus < harvest date - 1 hari

### C. Rancangan Database (ERD)

**Entitas Minimal (4+ Entitas):**

#### 1. **users**

- id (PK)
- name
- email (unique)
- password
- role (enum: customer, farmer, admin)
- email_verified_at
- created_at, updated_at


#### 2. **farmer_profiles**

- id (PK)
- user_id (FK to users)
- farm_name
- bio (text)
- location (text)
- latitude, longitude
- verification_status (enum: pending, approved, rejected)
- bank_account_name
- bank_account_number
- bank_name
- subscription_type (enum: free, premium)
- subscription_expires_at
- created_at, updated_at


#### 3. **customer_addresses**

- id (PK)
- user_id (FK to users)
- label (enum: home, office, other)
- recipient_name
- phone_number
- full_address (text)
- city
- postal_code
- is_default (boolean)
- created_at, updated_at


#### 4. **categories**

- id (PK)
- name (Sayuran, Buah, Microgreens, Herbs)
- slug
- description
- icon (optional)
- created_at, updated_at


#### 5. **products**

- id (PK)
- farmer_id (FK to users)
- category_id (FK to categories)
- name
- description (text)
- unit (enum: kg, bunch, pack, pcs)
- created_at, updated_at


#### 6. **product_images**

- id (PK)
- product_id (FK to products)
- image_path
- is_primary (boolean)
- order (int)
- created_at, updated_at


#### 7. **harvest_batches**

- id (PK)
- farmer_id (FK to users)
- product_id (FK to products)
- quantity_available (decimal)
- quantity_sold (decimal, default 0)
- price_per_unit (decimal)
- harvest_date (date)
- pre_order_deadline (date)
- status (enum: active, closed, harvested, completed, cancelled)
- created_at, updated_at


#### 8. **orders**

- id (PK)
- order_number (unique, auto-generated)
- customer_id (FK to users)
- total_amount (decimal)
- shipping_cost (decimal)
- platform_fee (decimal)
- grand_total (decimal)
- status (enum: pending_payment, paid, processing, shipped, completed, cancelled)
- payment_status (enum: pending, paid, failed, refunded)
- delivery_method (enum: pickup, delivery)
- shipping_address_id (FK to customer_addresses, nullable)
- notes (text, nullable)
- created_at, updated_at


#### 9. **order_items**

- id (PK)
- order_id (FK to orders)
- harvest_batch_id (FK to harvest_batches)
- product_name (snapshot)
- farmer_name (snapshot)
- quantity (decimal)
- price_per_unit (snapshot)
- subtotal (decimal)
- created_at, updated_at


#### 10. **transactions**

- id (PK)
- order_id (FK to orders, unique)
- payment_method (enum: bank_transfer, e_wallet, qris, credit_card)
- payment_ref (external reference dari Midtrans)
- amount (decimal)
- status (enum: pending, success, failed, expired)
- payment_url (nullable)
- paid_at (nullable)
- created_at, updated_at


#### 11. **wallets**

- id (PK)
- farmer_id (FK to users, unique)
- balance (decimal, default 0)
- total_earned (decimal, default 0)
- total_withdrawn (decimal, default 0)
- created_at, updated_at


#### 12. **wallet_transactions**

- id (PK)
- wallet_id (FK to wallets)
- type (enum: credit, debit)
- amount (decimal)
- description (text)
- reference_type (order, withdrawal, subscription)
- reference_id (polymorphic)
- balance_after (decimal)
- created_at, updated_at


#### 13. **withdrawals**

- id (PK)
- farmer_id (FK to users)
- amount (decimal)
- bank_account_name
- bank_account_number
- bank_name
- status (enum: pending, approved, rejected, completed)
- approved_by (FK to users, nullable)
- approved_at (nullable)
- rejection_reason (text, nullable)
- transferred_at (nullable)
- created_at, updated_at


#### 14. **reviews**

- id (PK)
- order_id (FK to orders)
- customer_id (FK to users)
- farmer_id (FK to users)
- product_id (FK to products)
- rating (int, 1-5)
- comment (text, nullable)
- farmer_response (text, nullable)
- responded_at (nullable)
- created_at, updated_at


#### 15. **review_images**

- id (PK)
- review_id (FK to reviews)
- image_path
- created_at, updated_at


#### 16. **posts** (Farmer Feed)

- id (PK)
- farmer_id (FK to users)
- content (text)
- image_path (nullable)
- likes_count (int, default 0)
- comments_count (int, default 0)
- created_at, updated_at


#### 17. **post_comments**

- id (PK)
- post_id (FK to posts)
- user_id (FK to users)
- comment (text)
- created_at, updated_at


#### 18. **follows**

- id (PK)
- customer_id (FK to users)
- farmer_id (FK to users)
- created_at, updated_at
- UNIQUE(customer_id, farmer_id)


#### 19. **notifications**

- id (PK)
- user_id (FK to users)
- type (string)
- title
- message (text)
- data (json, nullable)
- read_at (nullable)
- created_at, updated_at


### D. Struktur Database

**Relationships:**

- users (1) → (1) farmer_profiles
- users (1) → (M) customer_addresses
- users (1) → (M) products (as farmer)
- products (1) → (M) product_images
- products (M) → (1) categories
- products (1) → (M) harvest_batches
- users (1) → (M) harvest_batches (as farmer)
- users (1) → (M) orders (as customer)
- orders (1) → (M) order_items
- harvest_batches (1) → (M) order_items
- orders (1) → (1) transactions
- users (1) → (1) wallets (as farmer)
- wallets (1) → (M) wallet_transactions
- users (1) → (M) withdrawals (as farmer)
- orders (1) → (1) reviews
- reviews (1) → (M) review_images
- users (1) → (M) posts (as farmer)
- posts (1) → (M) post_comments
- users (M) → (M) users (follows - self-referencing many-to-many)

**Indexes:**

- Composite: `harvest_batches(farmer_id, harvest_date, status)`
- Composite: `orders(customer_id, status, created_at)`
- Full-text: `products(name, description)`
- Index: `order_items(harvest_batch_id)`
- Index: `wallet_transactions(wallet_id, created_at)`
- Unique: `transactions(order_id)`
- Unique: `transactions(payment_ref)`


### E. Sampling Data Sesuai Struktur

**Sample Data untuk Demo/Testing:**

**1. users (5 records)**

```
- id: 1, name: "Admin Platform", email: "admin@sfmp.com", role: "admin"
- id: 2, name: "Budi Tani", email: "budi@farm.com", role: "farmer"
- id: 3, name: "Siti Green", email: "siti@farm.com", role: "farmer"
- id: 4, name: "Ahmad Customer", email: "ahmad@mail.com", role: "customer"
- id: 5, name: "Rina Buyer", email: "rina@mail.com", role: "customer"
```

**2. farmer_profiles (2 records)**

```
- user_id: 2, farm_name: "Budi Urban Farm", location: "Jakarta Selatan", 
  verification_status: "approved", subscription_type: "premium"
  
- user_id: 3, farm_name: "Siti Hydroponic Garden", location: "Tangerang", 
  verification_status: "approved", subscription_type: "free"
```

**3. categories (4 records)**

```
- id: 1, name: "Sayuran", slug: "sayuran"
- id: 2, name: "Microgreens", slug: "microgreens"
- id: 3, name: "Herbs", slug: "herbs"
- id: 4, name: "Buah", slug: "buah"
```

**4. products (6 records)**

```
- id: 1, farmer_id: 2, category_id: 1, name: "Bayam Hijau Organik", unit: "bunch"
- id: 2, farmer_id: 2, category_id: 1, name: "Kangkung Organik", unit: "bunch"
- id: 3, farmer_id: 2, category_id: 2, name: "Microgreens Mix", unit: "pack"
- id: 4, farmer_id: 3, category_id: 1, name: "Selada Romaine", unit: "kg"
- id: 5, farmer_id: 3, category_id: 3, name: "Basil Fresh", unit: "pack"
- id: 6, farmer_id: 3, category_id: 4, name: "Tomat Cherry", unit: "kg"
```

**5. harvest_batches (4 records)**

```
- id: 1, farmer_id: 2, product_id: 1, quantity_available: 50, quantity_sold: 20,
  price_per_unit: 15000, harvest_date: "2025-12-15", pre_order_deadline: "2025-12-13", status: "active"
  
- id: 2, farmer_id: 2, product_id: 3, quantity_available: 30, quantity_sold: 10,
  price_per_unit: 25000, harvest_date: "2025-12-14", pre_order_deadline: "2025-12-12", status: "active"
  
- id: 3, farmer_id: 3, product_id: 4, quantity_available: 20, quantity_sold: 15,
  price_per_unit: 35000, harvest_date: "2025-12-16", pre_order_deadline: "2025-12-14", status: "active"
  
- id: 4, farmer_id: 3, product_id: 6, quantity_available: 25, quantity_sold: 25,
  price_per_unit: 45000, harvest_date: "2025-12-10", pre_order_deadline: "2025-12-08", status: "closed"
```

**6. orders (3 records)**

```
- id: 1, order_number: "ORD-20251208-0001", customer_id: 4, 
  total_amount: 140000, shipping_cost: 15000, platform_fee: 14000, grand_total: 155000,
  status: "paid", payment_status: "paid", delivery_method: "delivery"
  
- id: 2, order_number: "ORD-20251208-0002", customer_id: 5,
  total_amount: 75000, shipping_cost: 0, platform_fee: 7500, grand_total: 75000,
  status: "processing", payment_status: "paid", delivery_method: "pickup"
  
- id: 3, order_number: "ORD-20251208-0003", customer_id: 4,
  total_amount: 90000, shipping_cost: 12000, platform_fee: 9000, grand_total: 102000,
  status: "completed", payment_status: "paid", delivery_method: "delivery"
```

**7. order_items (5 records)**

```
- order_id: 1, harvest_batch_id: 1, product_name: "Bayam Hijau Organik", 
  farmer_name: "Budi Urban Farm", quantity: 5, price_per_unit: 15000, subtotal: 75000
  
- order_id: 1, harvest_batch_id: 2, product_name: "Microgreens Mix",
  farmer_name: "Budi Urban Farm", quantity: 3, price_per_unit: 25000, subtotal: 75000
  
- order_id: 2, harvest_batch_id: 3, product_name: "Selada Romaine",
  farmer_name: "Siti Hydroponic Garden", quantity: 2, price_per_unit: 35000, subtotal: 70000
  
- order_id: 3, harvest_batch_id: 4, product_name: "Tomat Cherry",
  farmer_name: "Siti Hydroponic Garden", quantity: 2, price_per_unit: 45000, subtotal: 90000
```

**8. transactions (3 records)**

```
- order_id: 1, payment_method: "e_wallet", payment_ref: "MDT-20251208-001", 
  amount: 155000, status: "success", paid_at: "2025-12-08 10:30:00"
  
- order_id: 2, payment_method: "bank_transfer", payment_ref: "MDT-20251208-002",
  amount: 75000, status: "success", paid_at: "2025-12-08 14:15:00"
  
- order_id: 3, payment_method: "qris", payment_ref: "MDT-20251207-003",
  amount: 102000, status: "success", paid_at: "2025-12-07 16:20:00"
```

**9. wallets (2 records)**

```
- farmer_id: 2, balance: 135000, total_earned: 135000, total_withdrawn: 0
- farmer_id: 3, balance: 144000, total_earned: 244000, total_withdrawn: 100000
```

**10. reviews (2 records)**

```
- order_id: 3, customer_id: 4, farmer_id: 3, product_id: 6,
  rating: 5, comment: "Tomat sangat segar dan manis! Packing rapi.",
  farmer_response: "Terima kasih untuk reviewnya! Senang Anda suka 🍅"
```


***

Dokumen ini sudah mencakup semua 4 poin yang diminta dengan **minimal 4 entitas** (sebenarnya ada 19 entitas untuk project yang comprehensive). Semoga membantu untuk FP kamu! 🚀
<span style="display:none">[^1]</span>

<div align="center">⁂</div>

[^1]: image.jpg

