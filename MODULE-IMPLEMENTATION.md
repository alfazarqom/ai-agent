# Module — Implementation

## Prasyarat

Implementasi hanya boleh dimulai jika:
1. Audit yang relevan telah selesai atau keterbatasannya dinyatakan.
2. Requirement dan acceptance criteria jelas.
3. Ada CHANGE PLAN terperinci dengan ID/versi, target, file/objek, operasi, exclusions, risiko, rollback, dan test.
4. Pengguna menyetujui rencana tersebut secara eksplisit.
5. Tool memiliki akses dan perubahan dapat dibatasi ke scope yang disetujui.

Jika ada syarat yang tidak terpenuhi, tetap di mode read-only dan minta klarifikasi/approval yang tepat.

## Implementasi repository

- Buat branch kerja terpisah dari base branch yang diverifikasi; jangan langsung commit ke main jika workflow branch tersedia.
- Periksa ulang branch dan commit sebelum menulis.
- Terapkan perubahan terkecil yang memenuhi requirement.
- Jangan overwrite file yang ada tanpa membaca versi aktif dan memeriksa diff.
- Jangan menyelipkan refactor, dependency update, formatting massal, atau fitur lain yang tidak ada dalam rencana.
- Jangan commit .env, secret, token, kredensial, data pelanggan, hasil dump database, atau artefak build yang tidak diminta.
- Pertahankan stack, baseline UI, dan pola arsitektur yang telah dikunci kecuali rencana menyetujui perubahan.
- Laporkan setiap file baru, diubah, atau dihapus beserta alasan.

## Implementasi Supabase

- Verifikasi project ref dan environment setiap sesi sebelum tindakan.
- Audit schema aktif dan migration historis sebelum menyusun/menerapkan migration.
- SQL migration yang akan dijalankan harus ditampilkan dan direview dalam CHANGE PLAN, dengan objek, grants, RLS, dependencies, dan rollback.
- Persetujuan untuk membuat file migration tidak sama dengan persetujuan menjalankannya.
- Jangan menjalankan SQL, migration, RPC mutatif, deploy function, atau mengubah secrets kecuali setiap tindakan itu tercantum dan disetujui secara eksplisit.
- Jangan drop/rename objek, mengubah RLS/grants, atau menonaktifkan fungsi existing sebagai efek samping yang tidak direncanakan.
- Untuk perubahan Edge Function, dokumentasikan auth boundary, input validation, signature validation, idempotency, database transaction/atomicity, logging redaction, error behavior, dan backward compatibility.
- Gunakan environment sandbox/non-production bila memungkinkan. Jangan menganggap project ref saja membuktikan environment aman.

## Integrasi Duitku POP

- Verifikasi kontrak resmi sebelum menulis kode. Simpan rujukan halaman resmi dan tanggal akses di dokumentasi.
- Request signature dan callback signature harus diimplementasikan persis menurut dokumentasi resmi yang berlaku; jangan menebak algoritme, urutan string, atau field.
- Merchant code dan API key dibaca hanya melalui secret/config yang disetujui; nilai tidak boleh dicetak ke log atau respons.
- Validasi referensi order, amount, currency, status, signature, dan idempotency di server sesuai kontrak resmi serta desain proyek.
- Jangan mempercayai redirect, query string, browser callback, atau input frontend sebagai bukti pembayaran final.
- Callback harus menangani duplicate/retry secara aman dan mencatat event tanpa menyimpan secret atau data berlebihan.
- Rekonsiliasi status harus dapat diaudit dan tidak boleh mengarang status sukses.
- Pembayaran nyata, refund, atau perubahan merchant production membutuhkan persetujuan khusus.

## Frontend security and UX

- Validasi server-side tetap wajib meskipun validasi UI tersedia.
- Jangan hardcode secret di bundle browser. Variabel frontend publik harus diperlakukan sebagai publik.
- Jangan biarkan client menentukan harga final, status payment, atau status order tepercaya.
- Tampilkan error yang aman dan berguna; detail internal/credential tidak boleh dikirim ke UI.
- Pertahankan mobile-first baseline, navigasi, aksesibilitas dasar, loading/empty/error state, dan kompatibilitas endpoint.

## Perubahan di luar rencana

Jika ada file/objek tambahan, kebutuhan migration, dependency, perubahan arsitektur, atau perbedaan requirement:
1. Hentikan perubahan yang terdampak.
2. Jangan memperluas scope sendiri.
3. Dokumentasikan alasan dan bukti.
4. Ajukan CHANGE PLAN revisi dengan acceptance criteria baru.
5. Tunggu approval baru.

## Rollback dan closeout

Sebelum perubahan, tentukan cara mengembalikan perubahan sesuai sifatnya. Jangan menjanjikan rollback otomatis untuk data/payment eksternal. Setelah implementasi:
- Periksa diff dan file list.
- Pastikan hanya file/objek yang disetujui berubah.
- Jalankan hanya test yang disetujui dan aman.
- Laporkan commit/branch/PR bila benar-benar dibuat.
- Nyatakan dengan jelas apakah database, secrets, push, merge, dan deployment berubah.
