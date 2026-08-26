# Usulan PC Workstation — Ringkasan untuk Persetujuan

**Tanggal**: 26 Agustus 2026
**Status**: Usulan, menunggu persetujuan & finalisasi harga — **ada catatan legal yang wajib dibaca sebelum lanjut** (lihat bagian 1)

---

## 1. Status legal — wajib dibaca dulu

Sudah direview oleh tim legal/SPJ project (Codex) berdasarkan **Panduan Pertanggungjawaban Anggaran P2M UNS 2026**. Analisis lengkap: **[legal-living-lab-spj-p2m-2026.md](legal-living-lab-spj-p2m-2026.md)**.

Poin paling penting:

- ⚠️ **PC ini secara aturan dihitung sebagai aset tetap/peralatan, bukan barang habis pakai.** Kalau sumber dana adalah dana penelitian APBN, pembelian **berpotensi tidak diperbolehkan**. Kalau sumber dana Non-APBN (atau sumber lain yang mengizinkan belanja aset), pembelian bisa lanjut dengan syarat dicatat sebagai **Barang Milik Universitas (BMU)**.
- Karena nilai estimasi (Rp38,2 – 52,5 juta) di atas Rp10 juta, pengadaan **wajib** mengikuti prosedur resmi — sampai Rp50 juta harus diketahui Pejabat Pengadaan Bidang I dan pakai penyedia PKP.
- **Semua komponen (CPU, motherboard, RAM, GPU, dst) harus dibeli & dinilai sebagai SATU paket workstation** — tidak boleh dipecah jadi invoice terpisah-terpisah hanya supaya nilainya terlihat kecil dan lolos dari prosedur pengadaan.
- **Tidak boleh diberi label "bahan habis pakai (BHP)"** — klasifikasi ikut wujud barangnya (komponen PC = aset), bukan judul dokumennya.
- Dokumen pembayaran di atas Rp5 juta wajib bermeterai Rp10.000.

**Sebelum proposal ini bisa disetujui, perlu dipastikan dulu:**
1. Sumber dana yang dipakai mengizinkan belanja aset tetap
2. Unit/penanggung jawab penerima barang untuk pencatatan BMU sudah ditetapkan

## 2. Kenapa perlu PC ini?

Satu PC dipakai untuk 3 kebutuhan sekaligus:

1. **Menjalankan software simulasi berlisensi** (MATLAB, dll)
2. **Server untuk data IoT** — menyimpan & menampilkan data sensor project Living Lab (suhu, kelembaban, energi, dll) secara real-time
3. **Melatih model AI/ML** — untuk sistem kontrol otomatis project (prediksi kondisi ruangan, optimasi AC & lampu)

Bisa diakses dari mana saja lewat internet (remote), tidak harus di lokasi.

## 3. Ringkasan keputusan komponen

| Komponen | Pilihan | Kenapa |
|---|---|---|
| **Prosesor & motherboard** | AMD Ryzen 7 (platform AM5) | Platform yang jalur upgrade-nya paling terjamin — resmi didukung sampai 2027+, jadi tidak perlu ganti motherboard tiap upgrade prosesor |
| **Kartu grafis (GPU)** | RTX 5060 Ti 16GB (baru) | Harga jelas & bergaransi resmi, hemat listrik, cukup untuk kebutuhan AI training project saat ini. Kartu bekas yang lebih besar (RTX 3090) justru **lebih mahal** setelah dicek harga pasar |
| **RAM** | 32GB | Cukup untuk kebutuhan saat ini; motherboard disiapkan agar bisa upgrade ke 64GB/128GB nanti tanpa ganti board — harga RAM sedang tinggi di pasar global |
| **Penyimpanan (SSD)** | 1TB + 2TB (WD Black, garansi resmi) | Kelas menengah yang tetap tahan lama (durable) dan bergaransi, tidak perlu beli kelas tertinggi yang jauh lebih mahal tanpa manfaat tambahan untuk kebutuhan ini |
| **Motherboard — slot GPU ganda** | Wajib mendukung 2 kartu grafis | Supaya nanti bisa tambah GPU kedua tanpa bongkar-pasang ulang seluruh PC |

**Prinsip utama: semua komponen dipilih agar bisa di-upgrade bertahap ke depan**, tanpa harus ganti seluruh PC kalau kebutuhan bertambah.

Detail perbandingan teknis, tabel trade-off tiap komponen, dan seluruh sumber harga: **[pc-workstation-spec.md](pc-workstation-spec.md)**.

## 4. Perkiraan biaya

| | Estimasi biaya |
|---|---|
| **Skenario hemat** (semua komponen di harga termurah) | **± Rp38.200.000** |
| **Skenario di atas** (kalau pilih komponen kelas lebih tinggi) | ± Rp52.500.000 |
| **Batas anggaran yang disepakati** | **Rp40.000.000** |

Skenario hemat masih masuk anggaran, tapi mepet (sisa ± Rp1,8 juta). Ada 1 komponen (motherboard dengan slot GPU ganda) yang harganya belum final dicek — kemungkinan menggeser total sedikit ke atas.

**Catatan penting terkait legal (lihat bagian 1)**: karena nilai transaksi ini wajib dinilai sebagai satu paket workstation (bukan per-komponen), ambang pengadaan yang berlaku adalah **total di atas** — bukan nilai tiap part. Rincian harga per-komponen ada di [pc-workstation-spec.md](pc-workstation-spec.md).

## 5. Strategi pembelian

Karena harga beberapa komponen (terutama RAM) sedang naik-turun cukup signifikan di pasar, disarankan beli **bertahap**:

1. **Tahap 1 — beli dulu**: Prosesor, motherboard, RAM, penyimpanan, PSU (bisa langsung dipakai untuk server data IoT & persiapan sistem, tanpa perlu GPU)
2. **Tahap 2 — menyusul**: Kartu grafis (GPU), begitu tahap 1 siap. Harga GPU pilihan (baru, bergaransi) relatif stabil jadi tidak ada risiko besar menunggu.

Catatan legal: pembelian bertahap ini **tetap dinilai sebagai satu paket pengadaan** dari sisi administrasi (lihat bagian 1) — bukan alasan untuk memecah nilai transaksi supaya lolos ambang pengadaan.

## 6. Apa yang masih perlu dipastikan sebelum beli

**Teknis:**
- Harga final motherboard yang mendukung 2 slot GPU (belum dicek harga pasarnya)
- Toko/vendor rakitan yang dipakai
- Konfirmasi anggaran final kalau ada komponen yang perlu naik kelas

**Legal/administrasi** (detail: [legal-living-lab-spj-p2m-2026.md](legal-living-lab-spj-p2m-2026.md)):
- Sumber dana & dasar hukum yang mengizinkan belanja aset tetap
- Penetapan unit/penanggung jawab penerima barang untuk pencatatan BMU
- Kelengkapan dokumen SPJ (invoice, faktur pajak, bukti pembayaran, surat pesanan, BAST BMU, dll)

---

**Dokumen terkait:**
- Detail teknis & sumber harga: [pc-workstation-spec.md](pc-workstation-spec.md)
- Analisis legal lengkap: [legal-living-lab-spj-p2m-2026.md](legal-living-lab-spj-p2m-2026.md)
