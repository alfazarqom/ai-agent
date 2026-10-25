# Asri Collection Full Engineering Agent

Versi: 1.0.0  
Bahasa kerja: Bahasa Indonesia  
Zona waktu operasional: Asia/Makassar

Paket instruksi agent engineering untuk proyek **Asri Collection Online Store**. Paket ini memisahkan prompt utama dan modul dokumentasi, audit, implementasi, serta testing agar setiap perubahan dapat ditelusuri, dibatasi, dan diverifikasi.

## Dokumen

1. [PROMPT.md](./PROMPT.md) — instruksi utama, konfigurasi, workflow, approval, dan batas kewenangan.
2. [MODULE-DOCUMENTATION.md](./MODULE-DOCUMENTATION.md) — aturan dokumentasi dan source of truth.
3. [MODULE-AUDIT.md](./MODULE-AUDIT.md) — audit read-only GitHub, frontend, Supabase, dan Duitku POP.
4. [MODULE-IMPLEMENTATION.md](./MODULE-IMPLEMENTATION.md) — implementasi berbasis rencana yang disetujui.
5. [MODULE-TESTING.md](./MODULE-TESTING.md) — strategi dan kriteria verifikasi.

## Prinsip inti

- Audit read-only terlebih dahulu.
- Bedakan fakta terverifikasi, temuan, asumsi, rekomendasi, dan keputusan proyek.
- Persetujuan berlaku hanya untuk **satu rencana perubahan terperinci**; tidak memberi izin untuk pekerjaan di luar rencana tersebut.
- Perubahan repository, database, secrets, deployment, merge, dan pengujian pembayaran nyata memiliki batas otorisasi masing-masing.
- Dokumentasi resmi Duitku POP adalah source of truth untuk kontrak integrasi Duitku. Verifikasi sumber resmi sebelum menetapkan spesifikasi.
- Jangan pernah menaruh nilai secret, kredensial, token, atau data pelanggan sensitif di dokumentasi, log, commit, atau laporan.

## Status

Dokumen ini adalah baseline instruksi agent v1.0.0. Konfigurasi yang belum dikonfirmasi ditandai `UNKNOWN`; daftar tabel/fungsi target tidak berarti objek tersebut sudah ada atau aktif.
