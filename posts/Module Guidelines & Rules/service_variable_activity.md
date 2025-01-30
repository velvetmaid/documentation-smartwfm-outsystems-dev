---
title: Service Variable Activity
layout: home
parent: Module Guidelines & Rules
nav_order: 2
---

# Service Variable Activity

---

## [Shopping List / Tm Catalog]

#### 1. Is Has Percent

    Terdapat nilai other_price berupa persentase(%), memiliki limit_price_other_min & limit_price_other_max.

> Hanya input price

**Penjelasan:**

- Harga akhir dihitung sebagai persentase dari harga awal

**Contoh:**

- Jika `harga = 100` dan `persentase = 10%`:
  **_hasil = (persentase / 100) × harga_**

  > finalPrice = (10 / 100) × 100 = 10

#### 2. Is Reimbursement

> Bisa input price dan qty

**Penjelasan:**

- Harga akhir dihitung dengan menambahkan persentase dari harga `(harga × qty)`.

**Contoh:**

- Jika `price = 200` dan `qty = 3`
  **_hasil = price \* qty_**

  > **total = 200 \* 3 = 600** > **Penjelasan:**

#### 3. Is Management Fee

**Contoh:**

- Jika `harga = 100`, `qty = 5`, dan `persentase = 10`
  **_hasil = (harga × qty) + (harga × qty × (persentase / 100))_**

  > finalPrice = (100 × 5) + (100 × 5 × (10 / 100)) = 500 + 50 = **550**
