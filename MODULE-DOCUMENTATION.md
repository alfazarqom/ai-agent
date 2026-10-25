# Module — Documentation

## Tujuan

Menghasilkan dokumentasi yang benar, dapat diaudit, versioned, dan selaras dengan implementasi aktual untuk frontend, Supabase, dan Duitku POP.

## Source of truth

1. **Duitku POP:** https://docs.duitku.com/pop/id/ adalah sumber utama untuk kontrak POP. Gunakan https://docs.duitku.com/api/id/ sebagai sumber pendukung hanya jika relevan terhadap integrasi yang sedang dibahas.
2. **Repository aktif:** kode dan konfigurasi yang benar-benar ada pada branch/commit yang diperiksa.
3. **Supabase aktif:** skema, grants, RLS, functions/RPC, triggers, indexes, dan konfigurasi aktif yang berhasil diperiksa.
4. **Migration historis:** bukti niat/perubahan masa lalu, bukan bukti tunggal bahwa skema aktif sama.
5. **Keputusan pengguna:** sumber untuk kebutuhan bisnis yang sudah disetujui; pisahkan dari fakta implementasi.

Jika sumber saling bertentangan, dokumentasikan konflik dan jangan menebak. Dokumentasi vendor dapat berubah; catat URL spesifik, judul halaman, tanggal akses, dan versi/tanggal dokumen bila tersedia. Jangan menyatakan kontrak resmi terverifikasi tanpa benar-benar memeriksa sumber tersebut.

## Kategori pernyataan

Setiap spesifikasi teknis harus membedakan:
- **Kontrak resmi vendor** — dinyatakan eksplisit oleh dokumentasi resmi.
- **Kebutuhan proyek** — kebutuhan bisnis atau keputusan pengguna.
- **Kondisi implementasi** — perilaku yang dibuktikan dari kode/skema aktif.
- **Rekomendasi keamanan/engineering** — kontrol yang disarankan, bukan klaim vendor.
- **Belum diverifikasi** — bukti tidak tersedia atau ambigu.

Jangan mengarang endpoint, method, header, nama field, tipe data, aturan signature, response code, payment status, batas waktu, atau kebijakan retry. Untuk signature, tulis input yang tepat, urutan concatenation, encoding, algoritme, dan validasi hanya jika dapat diverifikasi dari dokumentasi resmi terkini. Jika contoh vendor dan implementasi berbeda, laporkan perbedaannya.

## Struktur dokumen

Gunakan versi eksplisit, misalnya:
- `docs/duitku/DUITKU-POP-v1.md`
- `docs/duitku/DUITKU-POP-v2.md`
- `docs/architecture/ARCHITECTURE-v1.md`
- `docs/database/SCHEMA-BASELINE-v1.md`
- `docs/decisions/ADR-0001-<slug>.md`

Jangan membuat salinan versi baru tanpa alasan. Versi lama tetap menjadi riwayat; versi baru mencatat perubahan dan alasan. Hindari mengubah dokumen sumber yang disetujui tanpa perubahan versi atau jejak revisi.

## Template spesifikasi endpoint

Untuk setiap Edge Function/API, dokumentasikan:
1. Nama dan tujuan.
2. Status verifikasi: tersedia / belum tersedia / belum diverifikasi.
3. Environment dan URL hanya jika telah dikonfirmasi.
4. Auth model dan izin pemanggil.
5. Request: method, header, body, required/optional, tipe, batasan, validasi.
6. Response sukses dan error berdasarkan bukti.
7. Kontrak signature: sumber resmi, input persis, algoritme, encoding, perbandingan aman jika relevan.
8. Urutan operasi dan transaksi database.
9. Idempotency, replay protection, duplicate handling, concurrency.
10. Data yang disimpan dan redaction pada log.
11. Timeout, retry, rate limiting, dan failure handling.
12. Security controls, observability, dan alerting.
13. Test case dan acceptance criteria.
14. Sumber resmi, path kode, schema/RPC terkait, tanggal audit.
15. Pertanyaan terbuka dan hal yang belum diverifikasi.

## Template data model

Untuk tiap tabel/kolom/RPC/trigger/policy:
- Nama objek dan schema.
- Keberadaan di database aktif dan waktu audit.
- Migration historis yang terkait.
- Tipe, nullability, default, constraint, index, foreign key.
- RLS dan grants.
- Penulis/pembaca data dan boundary kepercayaan.
- Invariant bisnis (stok, order, pembayaran, callback).
- Risiko kompatibilitas dan migrasi.
- Bukti dan status verifikasi.

Jangan menyimpulkan RLS aktif hanya karena policy ada; periksa grants, ownership, fungsi security definer/invoker, dan jalur akses yang relevan.

## Change log dan acceptance

Setiap dokumen versioned harus mencantumkan:
- Versi, tanggal, status (draft/reviewed/approved/superseded).
- Tujuan perubahan dan dokumen sebelumnya.
- Bukti sumber.
- Bagian berubah.
- Reviewer/approval jika tersedia.
- Pertanyaan terbuka.
- Acceptance criteria.

Dokumen dianggap siap setelah fakta vendor diverifikasi, fakta proyek dicocokkan dengan kode/skema aktif, asumsi diberi label, link sumber dapat ditelusuri, dan tidak ada secret/data sensitif.
