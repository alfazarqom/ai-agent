# Module — Audit (Read-only)

## Tujuan dan batas

Mendapatkan gambaran akurat tentang kondisi GitHub, frontend, Supabase, dan Duitku POP sebelum merekomendasikan perubahan. Audit tidak mengubah file, database, secrets, permissions, deployment, atau status pembayaran.

Catat repository, branch, commit SHA, project ref, environment, dan waktu pemeriksaan. Jika akses tidak tersedia, tulis `BLOCKED` atau `UNKNOWN`; jangan menyimpulkan objek tidak ada hanya karena tool tidak dapat mengaksesnya.

## 1. Audit GitHub dan frontend

Periksa:
- Repository, branch default, status kerja yang terlihat, struktur folder, commit terkini, dan perubahan lokal/remote yang dapat diperiksa.
- `package.json`, lockfile, `vite.config.*`, TypeScript config, router, layout, page/component, state management, API client, styles, tests, lint/build scripts.
- Pemisahan MainPages/StandalonePages dan MainLayout/StandaloneLayout bila memang ada.
- Alur katalog, kategori, detail produk, cart, checkout, ongkir manual, invoice, callback/return UI, status pembayaran.
- Pemanggilan Edge Functions, CORS, error handling, loading/empty states, validasi form, penyimpanan data browser, dan kemungkinan kebocoran secret.
- Kecocokan frontend dengan field API dan database; jangan menyimpulkan berdasarkan nama file saja.

Output wajib: path yang diperiksa, commit/branch, bukti spesifik, mismatch, risiko, file yang tidak dapat diakses, dan rekomendasi bernomor. Jangan edit file selama audit.

## 2. Audit Supabase read-only

Verifikasi project ref dan environment sebelum query. Periksa objek yang relevan:
- Tabel, kolom, tipe, default, nullability, constraints, foreign keys, indexes.
- RLS enablement dan policies, grants, role access, ownership, views, triggers, functions/RPC, function search_path, security definer/invoker, dan transaction semantics.
- Edge Functions yang relevan, konfigurasi non-secret, status deploy/version jika tersedia, dan logs yang tidak mengungkap secret.
- Hubungan orders, order_items, payments, callback_events, inventory_reservations, dan products.
- Jalur pembuatan order/reservasi, release/expiry, penetapan ongkir, pembuatan invoice, callback, reconciliation, dan transisi status.
- Atomicity stok, kondisi race/concurrency, idempotency, unique constraints, duplicate callbacks, dan pembayaran sukses setelah reservasi expired.
- Drift antara skema aktif, migration historis, repository, dan dokumentasi.

Jangan menjalankan DDL/DML, migration, RPC yang mengubah data, deploy, atau perintah cleanup. Query yang tampak read-only pun harus ditinjau agar tidak memicu fungsi dengan efek samping.

## 3. Audit Duitku POP

Gunakan dokumentasi resmi terkini: https://docs.duitku.com/pop/id/. Periksa halaman spesifik yang relevan dan catat tanggal akses. Dokumentasikan:
- Alur create invoice dan payment URL/redirect.
- Request/response dan header yang benar-benar ditentukan vendor.
- Signature request dan callback: bahan pembentuk, urutan, algoritme, encoding, dan cara validasi.
- Kontrak callback, status transaksi, dan perilaku retry/duplikasi bila didokumentasikan.
- Sandbox vs production, secret names/config, timeout, serta error handling.
- Perbedaan kontrak resmi, kode aktual, dan rancangan yang belum diterapkan.

Jangan menyalin nilai secret ke output. Jangan menganggap frontend return URL atau callback JavaScript sebagai bukti pembayaran final.

## 4. Audit integritas bisnis

Petakan state machine dan invariants:
- Order dan payment status harus konsisten, tetapi tetap merupakan domain status terpisah.
- Amount, currency, merchant reference, dan order reference harus terikat ke order yang benar.
- Callback harus tervalidasi dan idempotent.
- Pembaruan status pembayaran dan event callback harus tahan duplikasi dan konkurensi.
- Stok/reservasi harus konsisten secara atomik dengan aturan yang telah disetujui.
- Jika pembayaran berhasil setelah reservasi kedaluwarsa, ikuti kebijakan rekonsiliasi dan penanganan admin yang disetujui; jangan otomatis menjanjikan stok.
- Pengiriman dan nomor resi manual harus dipisahkan dari status pembayaran.
- Browser tidak boleh menjadi sumber kebenaran status pembayaran atau harga.

## 5. Format temuan

| Field | Isi |
|---|---|
| ID | AUD-001, AUD-002, ... |
| Severity | Critical / High / Medium / Low / Info |
| Status | Confirmed / Suspected / Unknown / Blocked |
| Area | GitHub / Frontend / Supabase / Duitku / Operations |
| Bukti | Path + baris/commit, nama objek, atau URL sumber |
| Kondisi aktual | Fakta tanpa asumsi |
| Dampak | Risiko teknis/bisnis |
| Rekomendasi | Perubahan yang diusulkan, belum dilakukan |
| Verifikasi lanjutan | Bukti/test yang dibutuhkan |

Severity ditentukan oleh dampak dan kemungkinan; jelaskan alasan, jangan menggunakan label untuk memberi kesan tanpa bukti.

## 6. Audit closeout

Sebelum menyimpulkan:
- Pisahkan fakta, keputusan pengguna, rekomendasi, asumsi, dan unknown.
- Sebutkan keterbatasan akses atau sumber.
- Pastikan tidak ada operasi write yang dijalankan.
- Buat daftar rekomendasi bernomor dan urutkan berdasarkan risiko/dependensi.
- Perubahan apa pun setelah audit memerlukan CHANGE PLAN dan approval baru.
