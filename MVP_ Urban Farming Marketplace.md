<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# MVP: Urban Farming Marketplace (Tanpa IoT)

## 🎯 Konsep Bisnis

**Platform marketplace yang menghubungkan urban farmers dengan konsumen**, fokus pada produk organik lokal dengan sistem pre-order untuk menjamin kesegaran dan mengurangi food waste.

## 💼 Proses Bisnis

### User Roles

1. **Farmer (Seller)** - Pemilik urban farm/hidroponik
2. **Customer (Buyer)** - Konsumen produk organik
3. **Admin** - Platform operator

### Business Flow

**Phase 1: Farmer Onboarding**

1. Farmer register → upload dokumen verifikasi (foto farm, sertifikat organik jika ada)
2. Admin review \& approve farmer account
3. Farmer setup toko: nama farm, lokasi, bio, foto farm
4. Farmer pilih subscription package

**Phase 2: Pre-Order System**

1. Farmer create "Harvest Schedule" (tanaman apa, estimasi panen kapan, quantity available)
2. System publish ke marketplace sebagai "Pre-Order Available"
3. Customer browse → pre-order produk (H-7 sampai H-1 sebelum panen)
4. Customer bayar dimuka via payment gateway
5. System kumpulkan semua pre-order untuk 1 harvest batch

**Phase 3: Harvest \& Fulfillment**

1. Harvest day: Farmer harvest sesuai total pre-order
2. Farmer pack produk → update "Ready to Ship"
3. **Opsi A**: Self-pickup di farm location (customer ambil sendiri)
4. **Opsi B**: Delivery via kurir/gojek (farmer arrange)
5. Customer confirm receipt
6. System release payment ke farmer wallet (minus commission)

**Phase 4: Community Building**

1. Customer review produk + foto
2. Farmer share farming tips \& update di feed
3. Customer bisa "subscribe" ke farmer favorit (auto-notify harvest baru)

## 🚀 MVP Features

### Module 1: User Management

**Database Tables:**

- `users` - Multi-role authentication
- `farmer_profiles` - Farm info, verification status, bank account
- `customer_addresses` - Multiple shipping addresses
- `subscriptions` - Package billing

**Features:**

- Register/Login dengan role selection
- Email verification
- Farmer profile: nama farm, bio, lokasi (Google Maps), foto farm (multiple)
- Farmer verification by admin (approve/reject)
- Subscription: Free (max 5 products) vs Premium Rp 75k/bulan (unlimited + analytics)


### Module 2: Product \& Harvest Management

**Database Tables:**

- `categories` - Sayuran, Buah, Microgreens, Herbs
- `products` - Master product (Bayam, Tomat, dll)
- `harvest_batches` - Scheduled harvest dengan pre-order window
- `product_images` - Product photos

**Features:**

- **Farmer Side**:
    - Create harvest schedule: pilih product, quantity, harvest date, pre-order deadline
    - Set pricing per kg/bunch/pack
    - Upload product photos
    - Manage harvest batch (edit, cancel)
    - Mark batch as "Harvested" → "Ready for Pickup/Delivery"
- **Customer Side**:
    - Browse upcoming harvest batches
    - Filter by category, location (nearby farms), harvest date
    - View product details + farm profile
    - Pre-order countdown timer


### Module 3: Pre-Order \& Transaction System

**Database Tables:**

- `carts` - Shopping cart
- `orders` - Master order
- `order_items` - Order detail (bisa dari multiple harvest batches)
- `transactions` - Payment records
- `payment_methods` - Bank Transfer, E-Wallet, QRIS

**Features:**

- Add to cart (validasi: masih dalam pre-order period?)
- Cart summary dengan grouping by farmer
- Checkout flow:
    - Pilih shipping address atau pickup
    - Pilih delivery method per farmer
    - Payment method selection
    - Order summary + total
- Payment gateway integration (Midtrans)
- Auto-update order status via webhook
- Order tracking: Pending Payment → Paid → Harvested → Ready → Completed


### Module 4: Delivery Management

**Database Tables:**

- `delivery_options` - Self-pickup, Farmer Delivery, Courier
- `delivery_schedules` - Pickup time slots
- `shipping_costs` - Per area/distance

**Features:**

- **Self-Pickup**:
    - Customer pilih pickup time slot
    - Farmer set available pickup hours
    - System generate pickup code (QR/PIN)
- **Delivery**:
    - Farmer input shipping cost per area
    - Customer input address → auto-calculate ongkir
    - Manual tracking update by farmer


### Module 5: Financial Management

**Database Tables:**

- `wallets` - Farmer balance
- `wallet_transactions` - Transaction logs (credit/debit)
- `withdrawals` - Withdrawal requests
- `platform_fees` - Commission configuration

**Features:**

- **Escrow System**:
    - Hold customer payment until order completed
    - Auto-release after customer confirm receipt (atau auto-confirm H+3)
- **Farmer Wallet**:
    - Real-time balance
    - Transaction history (earnings, withdrawals, fees)
    - Withdrawal request → admin approval → transfer
- **Commission System**:
    - Platform fee: 10% per transaction
    - Auto-deduct saat release payment
- **Invoice \& Reports**:
    - Auto-generate invoice PDF
    - Sales report (daily/weekly/monthly)
    - Tax report (for compliance)


### Module 6: Review \& Rating

**Database Tables:**

- `reviews` - Product reviews dengan photo
- `farmer_ratings` - Overall farmer rating

**Features:**

- Customer review after delivery confirmed (1-5 stars + text + photo)
- Farmer response to reviews
- Average rating display di product \& farmer profile
- "Verified Purchase" badge


### Module 7: Community \& Content

**Database Tables:**

- `posts` - Farmer updates/tips
- `post_comments` - Customer engagement
- `follows` - Customer follow farmers

**Features:**

- **Farmer Feed**: Share farming updates, tips, harvest photos
- **Customer Engagement**: Like, comment, share
- **Follow System**: Get notified saat farmer favorit buka pre-order baru
- **Farming Tips Library**: Kategorisasi tips by topic


### Module 8: Notification System

**Database Tables:**

- `notifications` - In-app notifications
- `email_logs` - Email history

**Features:**

- **Email Notifications**:
    - Order confirmation
    - Payment received
    - Harvest ready reminder
    - Pickup/delivery schedule
- **In-App Notifications**:
    - New pre-order available (dari followed farmers)
    - Order status updates
    - Review notification untuk farmer
- **Optional WhatsApp** (via API):
    - Urgent updates (harvest delay, dll)


### Module 9: Admin Panel

**Database Tables:**

- `admin_logs` - Audit trail

**Features:**

- Dashboard analytics (GMV, total orders, active users, revenue)
- User management (approve farmers, suspend accounts)
- Withdrawal approval
- Transaction monitoring \& dispute resolution
- Platform settings (commission rate, payment methods)
- Content moderation (reviews, posts)


## 📊 Database Advanced Features

### 1. Stored Procedures

```sql
-- Calculate total dengan commission
CREATE PROCEDURE calculate_order_total(order_id)

-- Process farmer withdrawal
CREATE PROCEDURE process_withdrawal(withdrawal_id)

-- Auto-complete orders setelah 3 hari
CREATE PROCEDURE auto_complete_old_orders()

-- Generate sales report
CREATE PROCEDURE generate_farmer_report(farmer_id, start_date, end_date)
```


### 2. Database Triggers

```sql
-- Auto-create wallet saat farmer approved
CREATE TRIGGER after_farmer_approved

-- Auto-update harvest batch quantity saat ada order
CREATE TRIGGER after_order_created

-- Auto-credit wallet saat order completed
CREATE TRIGGER after_order_completed

-- Send notification saat withdrawal approved
CREATE TRIGGER after_withdrawal_approved
```


### 3. Views

```sql
-- Best selling products
CREATE VIEW vw_trending_products

-- Top rated farmers
CREATE VIEW vw_top_farmers

-- Available harvest batches (dengan validasi tanggal)
CREATE VIEW vw_available_harvests

-- Farmer earnings summary
CREATE VIEW vw_farmer_earnings
```


### 4. Complex Queries dengan Joins

```sql
-- Order dengan multiple tables join
SELECT o.*, oi.*, p.*, f.farm_name, u.name as customer_name
FROM orders o
JOIN order_items oi ON o.id = oi.order_id
JOIN harvest_batches hb ON oi.harvest_batch_id = hb.id
JOIN products p ON hb.product_id = p.id
JOIN farmer_profiles f ON hb.farmer_id = f.id
JOIN users u ON o.customer_id = u.id
WHERE o.status = 'paid'
```


### 5. Indexing Strategy

- Composite index: `harvest_batches(farmer_id, harvest_date, status)`
- Index: `orders(customer_id, status, created_at)` untuk order history
- Full-text index: `products(name, description)` untuk search
- Unique index: `transactions(order_id, payment_ref)` prevent duplicate


### 6. Database Transactions (ACID)

```sql
-- Checkout transaction
BEGIN TRANSACTION;
  INSERT INTO orders (...);
  INSERT INTO order_items (...);
  UPDATE harvest_batches SET quantity_sold = quantity_sold + ?;
  INSERT INTO transactions (...);
COMMIT;

-- Withdrawal transaction
BEGIN TRANSACTION;
  UPDATE wallets SET balance = balance - amount;
  INSERT INTO wallet_transactions (...);
  UPDATE withdrawals SET status = 'approved';
COMMIT;
```


### 7. Data Integrity \& Constraints

- Foreign key constraints dengan ON DELETE CASCADE/RESTRICT
- CHECK constraint: `harvest_batches.quantity_available >= 0`
- CHECK constraint: `harvest_batches.harvest_date > pre_order_deadline`
- UNIQUE constraint: prevent duplicate active harvest


## 🎨 Key Pages

### Public

- Landing page (value proposition, how it works, featured farmers)
- Marketplace (grid/list harvest batches dengan filter)
- Product detail + farmer profile preview
- Farmer profile page (semua products + feed + reviews)
- About, How It Works, FAQ


### Customer Dashboard

- My Orders (dengan filter status)
- Order detail \& tracking
- My Addresses
- Following (list followed farmers + their updates)
- My Reviews
- Account settings


### Farmer Dashboard

- **Overview**: Today's stats, pending orders, wallet balance
- **Harvest Management**: Create/edit batches, calendar view
- **Orders**: List orders by status, bulk update
- **Products**: CRUD products
- **Wallet**: Balance, transaction history, request withdrawal
- **Analytics**: Sales chart, best products, customer insights
- **Feed**: Create posts, engagement metrics
- **Reviews**: View \& respond


### Admin Dashboard

- Analytics dashboard (GMV, graphs, KPIs)
- User management (farmers, customers)
- Transaction monitoring
- Withdrawal approval queue
- Platform settings
- Content moderation


## 🔒 Advanced Web Features

1. **Queue System (Laravel Jobs)**:
    - Send email notifications (background)
    - Generate PDF invoices
    - Process bulk operations
    - Auto-complete old orders (scheduled job)
2. **Payment Integration**:
    - Midtrans Snap untuk multiple payment methods
    - Webhook handler untuk auto-update status
    - Retry mechanism untuk failed payments
3. **File Upload \& Management**:
    - Image optimization (compress, resize, watermark)
    - Multiple image upload dengan drag-drop
    - Storage management (local/S3)
4. **Search \& Filter**:
    - Advanced search dengan Eloquent scopes
    - Filter: category, location, price range, harvest date
    - Sort: newest, price, rating, distance
5. **Security**:
    - CSRF protection
    - Rate limiting untuk API
    - Input validation \& sanitization
    - Role-based authorization (Policy)
6. **Caching**:
    - Cache popular queries (trending products, top farmers)
    - Session-based cart
    - Redis untuk performance
7. **API Development** (Optional):
    - RESTful API untuk mobile app
    - API authentication dengan Sanctum
    - API documentation (Swagger)

## 💰 Revenue Model

- **Transaction Fee**: 10% dari setiap penjualan
- **Premium Subscription**: Rp 75k/bulan (unlimited products + priority listing + analytics)
- **Featured Listing**: Rp 50k/minggu untuk highlight di homepage
- **Advertisement**: Banner space untuk suppliers (seeds, fertilizer, tools)


## 🎯 Unique Value Proposition

1. **Fresh Guarantee**: Pre-order system = harvest on demand, produk lebih segar dari supermarket
2. **Zero Food Waste**: Farmer harvest sesuai demand, no excess produce
3. **Local \& Organic**: Support urban farmers lokal, transparansi sumber makanan
4. **Community**: Direct connection farmer-consumer, edukasi, storytelling
5. **Flexible**: Self-pickup (hemat ongkir) atau delivery

Ini MVP yang **tetap complex \& showcase advanced concepts** tanpa perlu hardware IoT, pure software dengan business logic yang kuat!

