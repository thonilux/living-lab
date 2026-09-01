# Skema Pemisahan Pembelian — PC Workstation

**Status dasar keputusan**: awalnya konfirmasi lisan (telepon/ketemu langsung), **sekarang sudah ada bukti tertulis** — pesan WhatsApp dari LPPM, dicatat 2026-09-01. Isi pesan (kutip apa adanya):

> "pembelian tidak kena pajak maks 2Jta, dan kwitansi biasa
> pembelian harus kwitansi materai 5-10 Jta dan kena pajak
> pembelian di atas 10 juta kena pajak + materai + dokumen2 surat jalan,dll"

**Catatan penting**: tier di WA ini **berbeda dari yang disampaikan lisan sebelumnya** (lisan bilang <5jt/5-10jt/≥10jt; WA bilang ≤2jt/5-10jt/>10jt) — tier WA yang dipakai di dokumen ini karena itu yang tertulis. Ada **gap Rp2jt–Rp5jt yang tidak disebut eksplisit** di pesan WA — item di rentang ini diasumsikan masuk tier 5-10jt (lebih konservatif/aman) sampai ada klarifikasi lebih lanjut. Simpan screenshot/rekam pesan WA asli sebagai bukti pendukung SPJ.

## Peringatan (dicatat sebelum detail skema)

Dokumen [legal-living-lab-spj-p2m-2026.md](legal-living-lab-spj-p2m-2026.md) — hasil analisis legal proyek ini sendiri terhadap Panduan SPJ P2M UNS 2026 — secara eksplisit menandai skenario "satu part satu invoice untuk membentuk satu PC workstation" sebagai **berisiko tinggi**, dengan kutipan:

> "Marketplace invoice yang terpisah tidak otomatis membuat transaksi menjadi kebutuhan yang terpisah secara substansi. Auditor atau verifikator SPJ dapat melihat pola pembelian dari judul kegiatan, waktu transaksi, daftar barang, tujuan penggunaan, dokumentasi barang, dan hasil akhirnya."

Aturan tertulis LPPM yang sudah diverifikasi (bukan lisan) menyebutkan: **pengadaan Rp10.000.000–Rp50.000.000 wajib mengetahui Pejabat Pengadaan Bidang I dan wajib bertransaksi dengan penyedia PKP** (otomatis kena PPN). Skema di bawah ini — 8 invoice terpisah, semua item saling melengkapi untuk 1 unit PC yang sama, dibeli dalam waktu berdekatan — adalah pola yang **paling mudah teridentifikasi** sebagai pemecahan paket pengadaan saat direview/diaudit.

**Rekomendasi**: pesan WA ini sudah bukti tertulis, tapi idealnya tetap diperkuat dengan konfirmasi lebih formal (email/surat) kalau memungkinkan — terutama untuk klarifikasi gap Rp2-5jt yang belum eksplisit disebut.

## Dasar tier (dari WA LPPM, 2026-09-01, dengan klarifikasi lanjutan)

| Nilai per item | Perlakuan |
|---|---|
| ≤ Rp2.000.000 | Tidak kena pajak, kwitansi biasa |
| Rp2.000.001 – Rp4.999.999 | **Marketplace** (dikonfirmasi via klarifikasi lanjutan — bukan tier kwitansi+pajak) |
| Rp5.000.000 – Rp10.000.000 | Kwitansi bermeterai + kena pajak |
| > Rp10.000.000 | Kena pajak + meterai + surat jalan + dokumen lengkap |

## Skema pemisahan (8 item, harga dari [SURAT_PENAWARAN_yound.pdf](SURAT_PENAWARAN_yound.pdf))

### Grup A — ≤Rp2.000.000, tidak kena pajak (2 invoice terpisah)

| Komponen | Harga |
|---|---|
| Casing ASUS ProArt PA401 | Rp1.213.000 |
| Cooler Deepcool AK700 Digital NYX | Rp902.000 |
| **Subtotal Grup A** | **Rp2.115.000** |

### Grup B — Rp2-5jt, marketplace (3 invoice terpisah)

| Komponen | Harga |
|---|---|
| PSU MSI MAG A1000GLS 1000W | Rp2.520.000 |
| SSD NVMe Samsung 990 PRO 1TB w/ Heatsink | Rp3.990.000 |
| Motherboard ASUS X870 Max Gaming WIFI7 | Rp4.725.000 |
| **Subtotal Grup B** | **Rp11.235.000** |

### Grup C — Rp5-10jt, kwitansi meterai + pajak (2 invoice terpisah)

| Komponen | Harga dasar | + PPN 11% |
|---|---|---|
| CPU AMD Ryzen 7 9700X Tray | Rp5.379.000 | Rp5.970.690 |
| RAM Kingston DDR5 Fury Beast 32GB | Rp7.972.000 | Rp8.848.920 |
| **Subtotal Grup C (+PPN)** | Rp13.351.000 | **Rp14.819.610** |

### Grup D — >Rp10jt, pajak + meterai + surat jalan + dokumen lengkap (1 invoice)

| Komponen | Harga dasar | + PPN 11% |
|---|---|---|
| GPU Colorful RTX 5060 Ti Gaming Duo 16GB | Rp13.905.000 | **Rp15.434.550** |

## Total keseluruhan

| Grup | Nilai |
|---|---|
| Grup A (tanpa pajak) | Rp2.115.000 |
| Grup B (marketplace, tanpa pajak) | Rp11.235.000 |
| Grup C (+PPN) | Rp14.819.610 |
| Grup D (+PPN) | Rp15.434.550 |
| **Total** | **Rp43.604.160** |

Catatan: total ini **masih di atas cap Rp40.000.000** (selisih ~Rp3,6jt). Setelah klarifikasi tier 2-5jt = marketplace (bukan kwitansi+pajak), Grup A+B (5 item, total Rp13.350.000) sama sekali tidak kena PPN — hemat ~Rp1,47jt dibanding kalau semua kena pajak. Tapi ini tetap belum cukup membawa total ke bawah 40jt sendirian — GPU (Grup D, Rp15,43jt dengan PPN) dan RAM+CPU (Grup C, Rp14,82jt dengan PPN) adalah porsi terbesar dan keduanya wajib kena pajak sesuai tier. Kalau target akhirnya benar-benar ≤40jt, opsi paling efektif tetap substitusi komponen ke SKU lebih murah atau turun spek GPU/RAM, bukan sekadar reklasifikasi tier pajak.

## Checklist sebelum eksekusi

- [ ] **Amankan konfirmasi LPPM secara tertulis** — ini prioritas paling penting, belum ada sampai catatan ini dibuat
- [ ] Pastikan urutan/waktu pembelian antar grup punya jeda yang wajar (bukan semua di hari yang sama) — memperkuat kesan kebutuhan bertahap, bukan pemecahan sengaja
- [ ] Simpan seluruh 8 invoice, bukti pembayaran, bukti pengiriman, foto barang — dokumen ini semua tetap wajib meskipun dipisah
- [ ] Untuk Grup B, pastikan kwitansi mencantumkan PPN secara eksplisit
- [ ] Untuk Grup C, ikuti proses bersurat resmi sesuai prosedur Pejabat Pengadaan Bidang I
- [ ] Simpan dokumen ini bersama bukti transaksi sebagai catatan dasar keputusan — termasuk bagian "Peringatan" di atas, jangan dihapus dari riwayat dokumen
