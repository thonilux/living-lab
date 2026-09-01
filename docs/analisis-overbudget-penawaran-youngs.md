# Analisis Overbudget — Surat Penawaran Youngs Computer

Sumber: [SURAT_PENAWARAN_yound.pdf](SURAT_PENAWARAN_yound.pdf) — No. 01/PWRN/IX/2026, CV. Jaya Sejahtera Tekno (Youngs Computer), tanggal 1 September 2026.

## Ringkasan

| | Nilai |
|---|---|
| Budget cap yang disepakati | Rp40.000.000 |
| Total penawaran resmi (harga di PDF, belum PPN, belum ongkir) | Rp40.606.000 |
| **Total + PPN 11% (belum ongkir)** | **Rp45.072.660** |
| **Selisih dari cap (dengan PPN)** | **+Rp5.072.660** — lewat cap ~13% |
| Racikan draft sebelumnya (estimasi, spec.md) | Rp38.466.000 |

Belum termasuk ongkos kirim — total riil bisa lebih tinggi lagi dari Rp45.072.660.

**PPN dikonfirmasi belum termasuk di harga penawaran.** Marketing bilang barang-barang di penawaran "berppn semua" (bisa terbit faktur pajak) — tapi PDF penawaran eksplisit nyebut *"Apabila transaksi memerlukan penerbitan Faktur Pajak, mohon segera informasikan kepada tim marketing kami untuk kami lakukan penyesuaian harga"*. Artinya harga Rp40.606.000 itu harga **di luar PPN**; begitu institusi minta faktur pajak (yang **wajib** buat SPJ P2M — lihat [legal-living-lab-spj-p2m-2026.md](legal-living-lab-spj-p2m-2026.md)), harga naik 11%.

## Breakdown selisih per komponen (resmi vs draft)

| Komponen | Draft (spec.md) | Penawaran resmi | Selisih | % dari total selisih |
|---|---|---|---|---|
| Storage NVMe 1TB | ADATA Legend 900, Rp3.012.000 | Samsung 990 PRO w/ Heatsink, Rp3.990.000 | **+Rp978.000** | 46% |
| Casing + Cooler | Estimasi kasar, Rp1.500.000 | ASUS ProArt PA401 + Deepcool AK700, Rp2.115.000 | **+Rp615.000** | 29% |
| GPU RTX 5060 Ti | Colorful Gaming Duo, Rp13.596.000 | Colorful Gaming Duo, Rp13.905.000 | **+Rp309.000** | 14% |
| Motherboard | ASROCK X870 Challenger WIFI, Rp4.450.000 | ASUS X870 MAX Gaming WIFI7, Rp4.725.000 | **+Rp275.000** | 13% |
| RAM Kingston 32GB | Rp7.972.000 | Rp7.972.000 | 0 | — |
| CPU Ryzen 7 9700X Tray | Rp5.379.000 | Rp5.379.000 | 0 | — |
| PSU 1000W | MSI MAG A1000GL, Rp2.557.000 | MSI MAG A1000GLS (Silent), Rp2.520.000 | -Rp37.000 | (mengurangi selisih) |
| **Total selisih** | | | **+Rp2.140.000** | 100% |

## Kenapa overbudget — 3 penyebab

### 1. Estimasi casing dari awal terlalu kasar (29% dari selisih)

Draft `spec.md` cuma pakai angka estimasi Rp1.500.000 buat casing — **belum pernah discrape harga real**, karena waktu itu fokus riset ke komponen inti (CPU/mobo/RAM/GPU). Realisasinya, casing ASUS ProArt PA401 + cooler CPU Deepcool AK700 (paket dari toko) = Rp2.115.000. Ini bukan kenaikan harga, tapi **draft dari awal ga akurat** — gap Rp615rb ini seharusnya sudah diantisipasi.

### 2. Substitusi komponen ke SKU lebih mahal (42% dari selisih — storage + mobo)

Dua komponen diganti toko ke merk/varian berbeda dari yang diminta di draft:
- **Storage**: ADATA Legend 900 1TB → Samsung 990 PRO 1TB w/ Heatsink (+Rp978rb, kelas jauh lebih atas)
- **Motherboard**: ASROCK X870 Challenger WIFI → ASUS X870 MAX Gaming WIFI7 (+Rp275rb)

Pola ini konsisten dengan **kemungkinan stok kosong** di SKU yang diminta — toko substitusi otomatis ke varian yang tersedia, biasanya kelas setara-atau-lebih-tinggi (bukan lebih rendah), makanya harga naik. Belum dikonfirmasi ke toko apakah ini beneran karena stok atau pilihan sepihak.

### 3. Fluktuasi harga wajar (14% dari selisih — GPU)

RTX 5060 Ti naik Rp309rb dari draft (Rp13.596.000 → Rp13.905.000) — ini range wajar untuk fluktuasi harga pasar antar waktu (draft dicatat beberapa hari sebelum penawaran resmi terbit).

## Implikasi

- **Overbudget dengan PPN jauh lebih signifikan** (~Rp5,07jt dari cap 40jt, ~13%) — bukan lagi selisih kecil. Sebelum PPN dihitung, kesannya cuma meleset Rp606rb (1,5%); begitu PPN 11% masuk, gap-nya melompat jadi >5jt.
- **RAM dan CPU harganya presisi sama** dengan draft (di luar PPN) — validasi bahwa riset harga awal untuk 2 komponen itu akurat, tapi ini juga belum termasuk PPN, jadi validasinya cuma berlaku ke harga dasar.
- Kalau storage & mobo dikembalikan ke SKU awal (ADATA Legend 900 + ASROCK X870 Challenger), selisih harga dasar turun Rp1.253.000 (jadi ~Rp39.353.000 sebelum PPN) — tapi **setelah PPN 11%, itu tetap jadi ~Rp43,68jt, masih lewat cap ~Rp3,68jt**. Substitusi komponen bukan penyebab utama overbudget lagi begitu PPN diperhitungkan — PPN sendiri (~Rp4,47jt) jauh lebih besar dari total selisih substitusi (Rp1,25jt).

## Perbandingan harga: Youngs (Surat Penawaran) vs Starcomp

Sumber Starcomp: [katalog/starcomp-origin.md](katalog/starcomp-origin.md) (gabungan cabang Solo + Origin). Catatan: sebagian item beda SKU/merk persis, dibandingkan sebagai alternatif setara, bukan produk identik. Kolom "After PPN" ikut tier pajak dari [skema-pemisahan-pembelian-workstation.md](skema-pemisahan-pembelian-workstation.md) (WA LPPM): ≤2jt & 2-5jt = bebas pajak (marketplace), 5-10jt & >10jt = +PPN 11%.

| Komponen | Tier | Youngs (harga dasar) | Youngs (after PPN) | Starcomp Solo (harga dasar) | Starcomp Solo (after PPN) | Termurah after PPN |
|---|---|---|---|---|---|---|
| CPU Ryzen 7 9700X Tray | 5-10jt, +PPN | Rp5.379.000 | **Rp5.970.690** | tidak ada Tray | — | **Youngs** |
| Mobo X870 (beda merk: ASUS vs ASROCK) | 2-5jt, bebas pajak | **Rp4.725.000** | **Rp4.725.000** | Rp4.450.000 | Rp4.450.000 | **✅ Youngs — DIPILIH** (prioritas WiFi7/ekosistem/after-sales, selisih Rp275rb dianggap worth it) |
| RAM DDR5 32GB 5600MHz | 5-10jt, +PPN | Rp7.972.000 | Rp8.848.920 | Rp7.350.000 | **Rp8.158.500** | **✅ Solo, -Rp690.420 — DIPILIH** |
| GPU RTX 5060 Ti 16GB | >10jt, +PPN | Rp13.905.000 | **Rp15.434.550** | tidak tersedia | — | **Youngs (satu-satunya)** |
| Storage NVMe 1TB | 2-5jt, bebas pajak | Rp3.990.000 | Rp3.990.000 | Rp4.700.000 | Rp4.700.000 | **Youngs, -Rp710.000** |
| PSU 1000W | 2-5jt, bebas pajak | **Rp2.520.000** | **Rp2.520.000** | Rp2.250.000 | Rp2.250.000 | **✅ Youngs — DIPILIH** (MSI lebih branded/established dibanding FSP, selisih Rp270rb dianggap worth it) |

**Catatan penting**: setelah PPN masuk, selisih RAM justru melebar (Rp622rb → Rp690.420) karena base harganya beda dan sama-sama kena pajak 11% — makin menguatkan alasan pindah ke Solo. Mobo dan PSU (tier bebas pajak) selisihnya tetap sama persis dengan harga dasar, karena tidak kena PPN sama sekali. PSU dan Mobo sama-sama akhirnya dipilih dari Youngs (brand/ekosistem lebih dipercaya), sementara RAM tetap dari Solo (spek setara, selisih harga signifikan).

**Casing & Cooler dikeluarkan dari perbandingan vendor** — Youngs pakai varian premium (ASUS ProArt PA401 Rp1.213.000 + Deepcool AK700 Rp902.000 = Rp2.115.000) yang bukan kebutuhan pokok racikan ini. Ganti ke opsi murah generik (casing entry ATX/mATX ~Rp400-500rb, cooler stock/air cooler dasar ~Rp150-300rb) — estimasi **~Rp700.000 total**, hemat ~Rp1.415.000 dari harga Youngs. Ga perlu dipatok ke merk/toko tertentu — casing dan cooler dasar sudah cukup, ga ada requirement teknis khusus buat 2 komponen ini di racikan.

### Perbandingan spek: ASUS X870 Max Gaming WIFI7 vs ASROCK X870 Challenger WIFI

| Aspek | ASUS X870 Max Gaming WIFI7 (Youngs) | ASROCK X870 Challenger WIFI (Starcomp Solo) |
|---|---|---|
| Chipset | X870 — PCIe 5.0 wajib di slot GPU utama | X870 — sama, standar PCIe identik |
| VRM | Kelas ASUS mid-range biasanya solid, power stage matang | Kelas ASROCK setara, historisnya cukup buat CPU non-flagship (9700X bukan 9950X3D) |
| WiFi | **WiFi7** — lebih baru, future-proof | Kemungkinan **WiFi6E** (nama "Challenger WIFI" biasanya bukan WiFi7 — perlu verifikasi spek sheet resmi) |
| BIOS/software | AI Suite/Armoury Crate — ekosistem lebih matang, update BIOS rutin | ASROCK App Shop — fungsional, ekosistem kurang semapan ASUS |
| Garansi & after-sales | Jaringan servis luas di Indonesia, reputasi RMA baik | Garansi resmi ada, jaringan servis lebih terbatas dibanding ASUS |
| Slot PCIe x16 kedua (dual-VGA) | **Belum diverifikasi manual** apakah x8/x4 elektrik atau cuma fisik | **Belum diverifikasi manual** — sama-sama belum ada kepastian |
| Harga | Rp4.725.000 | Rp4.450.000 (-Rp275.000) |

**Poin krusial**: requirement dual-VGA slot (syarat utama racikan dari awal) — **dua-duanya sama-sama belum terverifikasi** soal slot kedua beneran punya lane atau tidak, jadi bukan faktor pembeda di keputusan ini.

**Keputusan: tetap ASUS X870 Max Gaming WIFI7 (Youngs)**. Prioritas WiFi7 (future-proof), ekosistem software lebih matang, dan jaringan after-sales/garansi ASUS lebih luas di Indonesia — selisih Rp275rb terhadap ASROCK dianggap sepadan dengan itu. Slot PCIe x16 kedua tetap perlu diverifikasi manual sebelum beli, terlepas dari merk mana yang dipilih.

### Perbandingan spek: Kingston Fury Beast vs ADATA XPG Lancer Blade (DDR5 32GB 5600MHz)

| Aspek | Kingston Fury Beast Black EXPO | ADATA XPG Lancer Blade |
|---|---|---|
| Kapasitas & speed | 32GB (2x16GB), 5600MHz | 32GB (2x16GB), 5600MHz — sama |
| CAS Latency | CL36 | CL36-40 tergantung varian (perlu cek SKU exact) |
| Profil overclock | EXPO (AMD-native) | Umumnya XMP + EXPO (dual-support) |
| Heatsink | Low-profile aluminium, minimalis | Lebih tebal, gaming-oriented |
| Garansi | Lifetime, jaringan servis luas di Indonesia | Lifetime, jaringan servis sedikit lebih terbatas |
| Reputasi utk beban 24/7 | Rekam jejak lebih panjang di server/workstation | Solid, tapi kurang teruji utk use-case ini |

**Keputusan: pindah ke ADATA Lancer Blade (Starcomp Solo)**. Selisih performa nyata minim (sama-sama 5600MHz, kapasitas sama) — beda harga Rp622rb lebih ke premium brand-trust Kingston, bukan gap performa. RAM sama-sama masuk tier kena PPN (5-10jt) di kedua vendor, jadi pindah vendor **tidak mengubah beban pajak**, murni menekan harga dasar sebelum pajak.

### Skenario Mix — Mobo & PSU tetap Youngs, cuma RAM dari Starcomp Solo + casing/cooler murah

Ganti RAM saja ke SKU Starcomp Solo yang lebih murah. Mobo (ASUS X870 Max Gaming) dan PSU (MSI MAG A1000GLS) tetap dari Youngs — prioritas brand/ekosistem/after-sales lebih dipercaya untuk 2 komponen ini. Casing+Cooler ke opsi generik murah (bukan lagi ASUS ProArt/Deepcool AK700 dari Youngs):

**Sebelum PPN**: Rp5.379.000 (CPU, Youngs) + Rp4.725.000 (Mobo, Youngs) + Rp7.350.000 (RAM, Solo) + Rp13.905.000 (GPU, Youngs) + Rp3.990.000 (Storage, Youngs) + Rp2.520.000 (PSU, Youngs) + ~Rp700.000 (Casing+Cooler, generik murah) = **~Rp38.569.000**

**Setelah PPN per tier** (CPU, RAM, GPU kena +11%; Mobo, Storage, PSU, Casing+Cooler ≤5jt/item bebas pajak): Rp5.970.690 (CPU) + Rp4.725.000 (Mobo) + Rp8.158.500 (RAM) + Rp15.434.550 (GPU) + Rp3.990.000 (Storage) + Rp2.520.000 (PSU) + Rp700.000 (Casing+Cooler) = **~Rp41.498.740**

**Ini masih lewat cap 40jt setelah PPN** (~Rp1.498.740), tapi jauh lebih dekat dibanding skenario "full Youngs + PPN" (Rp45,07jt) — gap-nya turun dari ~Rp5,07jt jadi ~Rp1,5jt. Sisa gap ini bisa ditutup dengan sedikit lagi penghematan (nego harga, atau turun 1 tingkat spek storage/casing), atau approval tambahan budget kecil. Hanya 2 vendor dipakai (Youngs + Starcomp Solo untuk RAM saja) — kompleksitas administrasi lebih ringan dibanding skenario Mix sebelumnya (3 komponen dari Solo).

**Trade-off**: belanja dari 2 vendor beda (Youngs + Starcomp Solo) menambah kompleksitas — 2 invoice terpisah minimal, dan berdasar diskusi legal sebelumnya, pembelian dari vendor berbeda untuk 1 unit PC yang sama tetap dinilai auditor sebagai 1 paket pengadaan (bukan celah untuk menghindari ambang). Kalau dipilih, dokumentasikan alasannya (harga lebih murah per komponen, bukan menghindari prosedur).

### Skenario Full Youngs, tanpa Casing & Cooler

Ambil seluruh paket sesuai surat penawaran resmi (CPU, Mobo, RAM Kingston, GPU, Storage, PSU — semua dari Youngs), tapi **keluarkan Casing ASUS ProArt (Rp1.213.000) dan Cooler Deepcool AK700 (Rp902.000)** dari pembelian. Casing/cooler dicari terpisah (misal opsi generik murah, atau pakai casing/cooler yang sudah ada).

**Sebelum PPN**: Rp40.606.000 (total penawaran resmi) − Rp2.115.000 (Casing+Cooler) = **Rp38.491.000**

**Setelah PPN per tier**: Casing dan Cooler masing-masing di tier ≤2jt (bebas pajak), jadi mengeluarkannya tidak mengubah PPN dari komponen lain — cuma menghapus base price keduanya.

| Komponen | Harga dasar | After PPN |
|---|---|---|
| CPU Ryzen 7 9700X Tray | Rp5.379.000 | Rp5.970.690 |
| Mobo ASUS X870 Max Gaming | Rp4.725.000 | Rp4.725.000 (bebas pajak) |
| RAM Kingston Fury Beast 32GB | Rp7.972.000 | Rp8.848.920 |
| GPU RTX 5060 Ti Colorful | Rp13.905.000 | Rp15.434.550 |
| Storage Samsung 990 PRO 1TB | Rp3.990.000 | Rp3.990.000 (bebas pajak) |
| PSU MSI MAG A1000GLS | Rp2.520.000 | Rp2.520.000 (bebas pajak) |
| **Total** | **Rp38.491.000** | **~Rp41.489.760** |

**Masih lewat cap 40jt setelah PPN** (~Rp1.489.760) — hampir sama persis dengan Skenario Mix (Rp41.498.740, RAM dari Solo). Bedanya: skenario ini **satu vendor saja** (Youngs), jadi jauh lebih simpel dari sisi administrasi/invoice — tapi tidak dapat penghematan RAM dari Solo (~Rp690rb after-PPN). Casing dan cooler harus dicari sendiri di luar paket ini (opsi generik murah, atau reuse yang sudah ada) sebelum unit bisa dirakit.

**Perbandingan 2 opsi realistis:**

| | Skenario Mix (RAM dari Solo) | Skenario Full Youngs (tanpa casing/cooler) |
|---|---|---|
| Vendor | 2 (Youngs + Starcomp Solo) | 1 (Youngs saja) |
| Total after-PPN (komponen inti) | Rp40.798.740 | Rp41.489.760 |
| Casing+Cooler | Sudah termasuk (generik ~Rp700rb) | Belum termasuk — perlu dicari terpisah |
| Kompleksitas administrasi | Lebih tinggi (2 invoice vendor beda) | Lebih rendah (1 vendor, 1 hubungan procurement) |

Catatan: kalau casing+cooler generik (~Rp700rb) ditambahkan ke Skenario Full Youngs, totalnya jadi ~Rp42.189.760 — lebih mahal dari Skenario Mix. Skenario Full Youngs lebih murah HANYA jika casing/cooler didapat dari sumber lain (reuse barang existing, hibah, dll) dengan biaya nol.

## Opsi tindak lanjut (belum diputuskan)

1. **Konfirmasi ke toko**: harga final SETELAH faktur pajak terbit — pastikan PPN 11% itu angka final, bukan ada komponen lain (PPh, dst) yang nambah lagi
2. **Ajukan revisi anggaran** — gap ~Rp5jt dari cap 40jt bukan nilai kecil, kemungkinan perlu approval tambahan resmi dari sisi institusi/sumber dana, bukan cuma keputusan teknis
3. **Substitusi RAM saja ke Starcomp Solo (Mobo & PSU tetap Youngs) + turunkan Casing/Cooler ke generik murah** — hemat ~Rp2.037.000 sebelum PPN (lihat Skenario Mix di atas), cuma nambah 1 vendor buat 1 komponen (RAM)
4. **Turun spek GPU atau RAM** kalau anggaran benar-benar mentok di 40jt termasuk PPN — GPU (Rp13,9jt) dan RAM (Rp7,97jt) porsinya besar, satu-satunya jalan realistis buat motong ~Rp5jt tanpa nambah budget
5. **Cek apakah cap 40jt itu sudah termasuk PPN atau belum** dari sisi kebijakan institusi — kalau cap dari awal dimaksudkan "harga barang saja, PPN di luar itu", maka Rp45jt+ mungkin masih dalam toleransi resmi
