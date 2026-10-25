# Module — Testing & Verification

## Prinsip

Pengujian harus membuktikan acceptance criteria, bukan hanya menunjukkan bahwa perintah selesai. Sebelum menjalankan test, identifikasi target environment, side effects, data yang digunakan, biaya/operasi eksternal, dan apakah test termasuk dalam CHANGE PLAN yang disetujui.

Jangan menjalankan pembayaran nyata, refund nyata, migrasi production, pengiriman pesan pelanggan, atau operasi eksternal berbiaya tanpa persetujuan eksplisit yang terpisah.

## Status hasil

Gunakan salah satu status untuk setiap test/acceptance criterion:
- **PASS** — test dijalankan dan hasil membuktikan kriteria.
- **FAIL** — test dijalankan dan kriteria tidak terpenuhi.
- **NOT RUN** — belum dijalankan.
- **BLOCKED** — tidak dapat dijalankan karena akses/dependensi/lingkungan.
- **UNKNOWN** — bukti tidak cukup untuk menentukan hasil.
- **NOT APPLICABLE** — tidak relevan, dengan alasan.

Jangan mengubah NOT RUN/BLOCKED menjadi PASS berdasarkan review statis saja.

## Frontend checks

Jika tersedia dan disetujui:
- Install reproducible sesuai lockfile.
- Type check.
- Lint.
- Unit/component tests.
- Build production bundle.
- Smoke test navigasi, katalog, kategori, detail, cart, checkout, pembayaran/redirect, halaman akun, loading/error/empty state.
- Uji layout mobile-first dan perilaku responsive sesuai baseline.
- Verifikasi API contract, error handling, loading state, dan tidak ada secret di bundle.
- Periksa diff agar tidak ada file/generated asset yang tidak diharapkan.

Catat perintah persis, exit code, ringkasan hasil, environment, dan keterbatasan. Jangan mengklaim browser/UI test bila hanya menjalankan build.

## Supabase/database checks

Untuk audit read-only:
- Verifikasi keberadaan objek, constraint, foreign key, indexes, RLS, policies, grants, triggers, RPC/functions, dan status deployment yang tersedia.
- Cocokkan schema aktif dengan migration dan kode.
- Tinjau transaksi/atomicity dan kondisi concurrency secara statis; jangan mengklaim uji race tanpa test yang benar-benar dijalankan.
- Gunakan test DB atau environment terisolasi untuk uji mutasi bila disetujui.
- Jangan menguji lewat fungsi/RPC yang dapat memodifikasi data tanpa memastikan efek sampingnya.
- Jangan jalankan migration atau DML hanya untuk “melihat apakah berhasil” tanpa approval spesifik.

## Duitku POP sandbox scenarios

Kontrak setiap test harus lebih dahulu diverifikasi terhadap dokumentasi resmi Duitku yang berlaku. Gunakan sandbox dan data sintetis bila tersedia.

Skenario yang perlu dipetakan jika relevan:
- Invoice valid menghasilkan response sesuai kontrak resmi.
- Input wajib hilang/salah ditolak dengan aman.
- Signature request/callback valid diterima.
- Signature invalid ditolak.
- Amount, currency, merchant reference, atau order reference tidak cocok ditolak atau dikarantina sesuai desain.
- Callback duplikat/retry tidak menggandakan efek.
- Callback tidak valid tidak mengubah order/payment menjadi sukses.
- Event gagal diproses dapat diobservasi dan direkonsiliasi.
- Transisi status tidak sah ditolak.
- Timeout/network failure tidak menghasilkan status sukses palsu.
- Pembayaran berhasil setelah reservasi expired masuk ke jalur reconciliation/admin yang disetujui, bukan otomatis menjamin stok.
- Secret tidak muncul pada logs, response, test output, atau commit.

Jangan mengarang status test yang didukung sandbox atau memalsukan response provider. Catat test yang tidak dapat dilakukan dan alasannya.

## Acceptance criteria

Setiap CHANGE PLAN harus memiliki kriteria yang dapat diperiksa. Contoh:
- Hanya file yang terdaftar dalam rencana yang berubah.
- Signature contract cocok dengan sumber resmi yang dicatat.
- Callback invalid/duplicate tidak menghasilkan efek pembayaran ganda.
- Status final hanya berubah melalui jalur backend yang tervalidasi.
- Constraint/RLS/grants sesuai rencana dan dapat diverifikasi.
- Type check/lint/test/build sesuai perintah yang tersedia.
- Tidak ada secret/data sensitif di diff atau log.
- Rollback dan risiko residual terdokumentasi.

Jangan menggunakan contoh ini sebagai klaim bahwa implementasi telah memenuhi kriteria.

## Template laporan akhir

```text
# Verification Report

Plan ID/version:
Tanggal dan timezone:
Repository / branch / commit:
Supabase project ref / environment:
Ringkasan:
Status akhir: PASS / PARTIAL / FAIL / BLOCKED

## Scope
- Scope yang disetujui:
- Scope yang dikecualikan:
- Deviasi (jika ada):

## Perubahan aktual
- File/path:
- Objek database:
- Commit/PR:
- Secrets/config:
- Push/merge/deployment:
- Operasi eksternal/pembayaran nyata:

## Test matrix
| ID | Kriteria/test | Perintah/metode | Status | Bukti | Catatan |
|----|---------------|-----------------|--------|-------|---------|

## Keamanan
- Signature/input validation:
- Idempotency/replay:
- RLS/grants/auth boundary:
- Secret/log redaction:
- Amount/order/status consistency:
- Stock/reservation consistency:

## Risiko residual dan keterbatasan
- Risiko:
- Mitigasi:
- Hal yang belum diverifikasi:

## Rollback
- Cara rollback:
- Batas rollback/efek yang tidak dapat dibatalkan:

## Rekomendasi langkah berikutnya
1.
2.
3.
```

Laporan harus menyatakan secara eksplisit bila database, secrets, deployment, merge, atau pembayaran nyata tidak disentuh.
