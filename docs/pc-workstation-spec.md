# PC Workstation — Brainstorming Spek

Status: brainstorming, belum final beli. Dicatat dari diskusi 2026-08-26.

## Catatan legal/SPJ P2M 2026 - Codex

**Status legal sementara: HOLD untuk pembelian sampai sumber dana dan skema aset dikunci.**

Berdasarkan Panduan Pertanggungjawaban Anggaran P2M UNS Tahun 2026, PC workstation ini harus diperlakukan sebagai **aset tetap/peralatan yang berpotensi menjadi aset**, bukan belanja bahan habis pakai. Konsekuensinya:

- Jika memakai **dana penelitian bersumber APBN**, pembelian ini berisiko **tidak diperbolehkan**, karena panduan melarang pembelian alat seperti mesin, peralatan laboratorium, atau peralatan lain yang berpotensi menjadi aset.
- Jika memakai **dana Non-APBN** atau sumber dana lain yang memperbolehkan belanja aset tetap, pembelian dapat diproses dengan syarat dicatat sebagai **Barang Milik Universitas (BMU)** pada fakultas/unit kerja/lab yang menguasai barang.
- Jika workstation ini dimaksudkan sebagai luaran yang diserahkan kepada mitra/masyarakat, maka harus ada dasar program yang jelas, **BAST penyerahan**, dokumentasi serah terima, dan data penerima. Untuk use-case dokumen ini, asumsi legal yang paling aman adalah workstation tetap dikuasai UNS sebagai BMU.

Ambang pengadaan juga wajib diperhatikan. Karena total estimasi berada sekitar Rp38.200.000 sampai Rp52.500.000, pengadaan ini masuk kategori **di atas Rp10.000.000**. Untuk skenario sampai Rp50.000.000, dokumen harus mengetahui Pejabat Pengadaan Bidang I dan **wajib menggunakan penyedia PKP**. Jika realisasi melewati Rp50.000.000, proses harus melalui Pejabat Pengadaan Bidang I/UKPBJ dengan dokumen lengkap.

Checklist legal sebelum final beli:

- [ ] Konfirmasi sumber dana: APBN penelitian, APBN pengabdian, Non-APBN, kerja sama, atau sumber lain.
- [ ] Konfirmasi apakah belanja aset tetap diperbolehkan oleh sumber dana tersebut.
- [ ] Tetapkan unit pemilik/penguasa barang untuk pencatatan BMU.
- [ ] Siapkan BAST aset tetap sebagai BMU kepada fakultas/unit kerja/prodi/lab.
- [ ] Gunakan penyedia PKP jika nilai paket pengadaan di atas Rp10.000.000.
- [ ] Siapkan kuitansi/invoice, faktur pajak, surat pesanan, surat kesanggupan, surat jalan, bukti pembayaran, foto barang, rincian spesifikasi, jumlah, dan nilai perolehan.
- [ ] Dokumen pembayaran di atas Rp5.000.000 wajib bermeterai Rp10.000.
- [ ] Jangan memecah paket pembelian hanya untuk menghindari ambang pengadaan.

**Red flag tambahan:** muncul opsi "satu part satu invoice" untuk mencari celah aturan. Dari sisi legal/SPJ, opsi ini **tidak direkomendasikan dan harus ditolak** jika substansi pembeliannya adalah satu paket kebutuhan PC workstation. Pemecahan invoice per part dapat dibaca sebagai upaya menghindari ambang pengadaan Rp10.000.000/Rp50.000.000, terutama karena seluruh komponen dirancang sebagai satu unit aset yang sama. Jalur aman adalah menetapkan paket pengadaan workstation secara utuh, lalu mengikuti ambang pengadaan berdasarkan nilai total paket.

**Red flag klasifikasi BHP:** mengubah judul belanja menjadi **BHP/bahan habis pakai** tidak mengubah karakter hukum barang. Komponen seperti CPU, motherboard, RAM, GPU, SSD/NVMe, PSU, casing, cooler, dan monitor/periferal utama adalah barang berumur manfaat lebih dari satu periode kegiatan dan membentuk satu aset workstation. Karena itu, klasifikasi sebagai BHP berisiko dianggap salah akun/salah klasifikasi belanja jika tujuannya menutupi pengadaan aset. BHP hanya layak untuk item yang benar-benar habis pakai dalam pelaksanaan kegiatan, misalnya kabel kecil tertentu, thermal paste, consumable instalasi, label, atau material pendukung yang tidak menjadi aset utama.

Catatan ini disisipkan oleh Codex sebagai tim legal agar Claude dan tim teknis memahami batas SPJ sebelum spesifikasi difinalkan.

Tanda tangan: **Codex - Legal/SPJ Review, 2026-08-26**

## Fungsi yang dibutuhkan

1. Simulasi software berlisensi (MATLAB, dll)
2. Server IoT: InfluxDB, MQTT, Grafana
3. AI/ML training (model-training layer project ini)
4. Bisa diremote dari mana saja

## Arsitektur yang disarankan

Host: **Proxmox VE** — supaya tiap workload terpisah (VM), gampang dikelola, gampang isolasi resource.

- **VM Ubuntu** — AI/ML, Docker, InfluxDB, MQTT, Grafana, JupyterLab
- **VM Windows** — MATLAB dan software berlisensi lain

### Remote access

- Tailscale / WireGuard VPN — akses jaringan dari mana saja
- Proxmox Web UI — manage host/VM
- Browser — akses Grafana & JupyterLab langsung
- MATLAB via browser pakai Guacamole/noVNC (ga harus RDP)

## Upgradeability — pertimbangan socket (must-have)

Requirement baru: **upgradeable is must**. Socket mobo nentuin umur platform sebelum kudu ganti mobo+CPU total.

| Socket | Platform | Status 2026 | Jalur upgrade CPU | Catatan |
|---|---|---|---|---|
| LGA1700 | Intel i7-13700/14700 (Opsi 1 & 2 lama) | **Mati** — Intel udah pindah ke LGA1851 | Mentok, generasi terakhir di socket ini | Beli sekarang = ga ada CPU next-gen buat upgrade tanpa ganti mobo |
| LGA1851 | Intel Core Ultra 200S (current-gen) | Aktif | Ga pasti — Intel historis ganti socket tiap ~2 generasi | Kemungkinan next-gen (Panther Lake desktop) masih kompatibel, belum dikonfirmasi Intel |
| **AM5** | AMD Ryzen 7000/9000 | Aktif | **Resmi disupport AMD sampai 2027+** | Jalur upgrade paling jelas — Ryzen 7000 → 9000 → next-gen tanpa ganti mobo/cooler. DDR5 + PCIe 5.0 native |
| Xeon Scalable (server) | Intel Xeon Gold (Opsi 3) | Aktif, siklus lambat | Upgrade generasi jarang, tapi ekspansi RAM/PCIe jauh lebih luas | Cocok kalau prioritas expandability jangka panjang > upgrade CPU sering |

**Kesimpulan socket**: kalau upgradeable CPU tanpa ganti mobo itu prioritas, **AM5 (AMD)** paling unggul. LGA1700 (opsi lama) harus dicoret — itu socket mati, beli sekarang langsung mentok. LGA1851 masih opsi tapi riwayat Intel bikin jalur upgrade-nya ga sepasti AM5.

## Pilihan GPU — RTX 3090 24GB vs RTX 5060 Ti 16GB → **FINAL: RTX 5060 Ti**

Dipertanyakan: kenapa harus 3090, kenapa ga 5060 Ti aja? Trade-off dibahas, **keputusan final jatuh ke RTX 5060 Ti 16GB**.

| Aspek | RTX 3090 24GB | RTX 5060 Ti 16GB |
|---|---|---|
| VRAM | 24GB | 16GB |
| Arsitektur | Ampere (2020) | Blackwell (current-gen, 2025) |
| Kondisi pasar | Second-hand only, EOL, ga ada garansi resmi | Baru, bergaransi resmi |
| Harga (Tokopedia, Agustus 2026) | Ga dapet angka pasti (listing ada, harga ga muncul di snippet) | **Rp13.150.000 – Rp19.116.000** tergantung merk/model — [Tokopedia](https://www.tokopedia.com/find/rtx-5060-ti) |
| Daya (TDP) | ~350W | ~180W |
| Compute raw (CUDA core) | Lebih banyak — kuat di beban kerja tertentu | Lebih sedikit, tapi arsitektur lebih efisien per-watt |
| Software/driver | CUDA lama, ga dapet fitur terbaru (FP8/FP4 native, dll) | Dukung fitur CUDA/driver terbaru |

**Kenapa VRAM (3090) penting buat use-case ini:**
- GPU **shared** — dipakai training AI/ML sekaligus kadang buat simulasi MATLAB (VM Windows). VRAM lebih besar = lebih longgar buat beban gabungan / model lebih besar / batch lebih besar
- Tapi: model-training layer project ini defaultnya **classical ML (XGBoost/sklearn), CPU-based** — GPU besar ga jadi kebutuhan pokok buat training model sekarang, cuma disiapin buat kalau nanti upgrade ke deep learning (LSTM/PyTorch)

**Kenapa 5060 Ti masuk akal:**
- Harga jelas & terjangkau (13–19jt), bergaransi, jauh lebih hemat daya (penting kalau PC nyala 24/7 buat server IoT)
- 16GB VRAM tetap cukup luas buat kebanyakan model ML/DL skala Living Lab project ini (data indoor/outdoor sensor, bukan LLM raksasa)
- Konsumsi daya rendah = biaya listrik jangka panjang lebih murah, penting buat operasi 24/7

**Keputusan final**: **RTX 5060 Ti 16GB**. 5060 Ti menang dari sisi harga-pasti, garansi, efisiensi daya, dan kebutuhan aktual project (classical ML dulu, GPU besar belum krusial) — plus dikonfirmasi harga scraping (section "Harga pasar real" di bawah) 3090 second malah lebih mahal dari 5060 Ti baru. 3090 dicoret dari opsi.

### Pain & Gain — pilih RTX 5060 Ti 16GB

**Gain:**
- Harga jelas & pasti: Rp13,15jt – 19,1jt, muat nyaman di budget 40jt (beda jauh dari 3090 yang harganya ga ketemu di scraping)
- Bergaransi resmi — beli baru, ga ada risiko kondisi fisik/umur pemakaian kayak barang second
- Daya jauh lebih hemat (~180W vs ~350W) — penting buat PC nyala 24/7 (server IoT + VM), hemat listrik & panas lebih rendah (PSU/cooling lebih ringan, casing bisa lebih kecil/murah)
- Arsitektur Blackwell (current-gen) — dukung fitur software/driver terbaru, umur dukungan driver lebih panjang ke depan dibanding Ampere (3090) yang makin tua
- Ready dipakai sekarang, ga perlu buru-buru cari listing second yang kondisinya ga pasti

**Pain:**
- VRAM cuma 16GB vs 24GB (3090) — headroom lebih kecil buat batch/model besar kalau nanti upgrade ke deep learning (LSTM/PyTorch), atau kalau training + MATLAB simulasi jalan bareng dan sama-sama butuh VRAM gede
- Compute raw (CUDA core count) lebih rendah dari 3090 — di beban kerja tertentu yang murni butuh raw compute (bukan efisiensi), 3090 masih bisa menang
- Kalau nanti data historis project udah besar dan mau ke deep learning serius, 16GB bisa jadi limit lebih cepat dibanding 24GB — mungkin perlu upgrade GPU lagi lebih awal dari rencana

**Ringkas**: 5060 Ti itu trade harga-pasti + garansi + efisiensi daya buat headroom VRAM. Buat kebutuhan project ini sekarang (classical ML, data belum besar, GPU shared tapi ga nonstop dipakai training berat), gain-nya lebih relevan daripada pain-nya. Pain baru kerasa kalau roadmap ke depan udah pasti arah ke deep learning skala besar.

## Opsi hardware (revisi — mempertimbangkan socket)

### Opsi A — AMD AM5 (rekomendasi upgradeability)

- AMD Ryzen 7 7700X / 9700X
- DDR5 **32GB** (turun lagi dari 64GB — budget hard cap 40jt, lihat breakdown di bawah)
- GPU: **RTX 5060 Ti 16GB** (final — lihat "Pilihan GPU" di atas)
- Storage: **NVMe Gen4 1TB (OS+VM) + 2TB (data/InfluxDB/model artifact)** — kriteria durable-bukan-top-tier: WD Black SN770 (gen4, garansi resmi) atau setara, bukan Gen5/PCIe 5.0 premium yang mahal & belum perlu
- PSU 1000W Gold
- Mobo AM5 dengan VRM kuat + slot RAM 4x (biar bisa upgrade ke 64GB/128GB+ nanti tanpa ganti mobo) + **dual VGA slot support** (2x PCIe x16, minimal x16 + x8/x4) buat upgrade tambah GPU kedua nanti

Kelebihan:
- Socket AM5 disupport resmi sampai 2027+ — upgrade CPU next-gen tanpa ganti mobo
- DDR5 native, PCIe 5.0 — bandwidth lebih tinggi buat GPU & NVMe
- Performa AI training/VM sebanding Intel LGA1700 lama
- RAM 32GB bisa diupgrade ke 64GB/128GB+ nanti kapan aja pas harga DDR5 turun atau budget nambah — mobo AM5 dgn 4 slot udah nyiapin jalur itu
- Dual VGA slot (2x PCIe x16) — GPU kedua bisa ditambah nanti tanpa ganti mobo, misal buat naikin total VRAM, dedicated GPU per VM (satu buat training Ubuntu, satu buat kebutuhan lain), atau upgrade tanpa buang 5060 Ti yang lama

Kekurangan:
- Harga mobo AM5 + DDR5 sedikit lebih mahal dari platform DDR4 lama
- Perlu pilih mobo bagus (bukan budget board) biar VRM/slot cukup buat upgrade jangka panjang
- Requirement dual VGA slot naikin harga mobo lebih jauh — kebanyakan board entry (B650M) cuma 1 slot x16 fisik, butuh naik ke board mid-atas (B650/X670) buat 2 slot PCIe x16 beneran (bukan cuma x16 fisik tapi x1 elektrik)
- PSU 1000W perlu dipastikan cukup buat 2 GPU nanti (5060 Ti ~180W x2 + CPU + komponen lain masih dalam batas, tapi kalau GPU kedua lebih besar/boros perlu dihitung ulang)
- 32GB RAM buat host Proxmox + 2 VM (Ubuntu server + Windows MATLAB) + training itu ketat — kemungkinan cuma bisa jalanin satu VM berat pada satu waktu, atau alokasi tiap VM dipangkas kecil. Upgrade RAM jadi prioritas pertama begitu budget/harga DDR5 mengizinkan

### Opsi B — Intel LGA1851 (Core Ultra 200S)

- Intel Core Ultra 9 / 7 (200S series)
- DDR5 96–128GB
- GPU: RTX 5060 Ti 16GB
- NVMe 1TB + SSD/NVMe 2TB

Kelebihan:
- Platform current-gen Intel
- Performa single-core kompetitif

Kekurangan:
- Jalur upgrade socket ga sepasti AM5 — riwayat Intel ganti socket tiap 2 generasi
- Ekosistem/harga board masih menyesuaikan (platform baru)

### Opsi C — Platform server (Xeon Scalable + ECC)

- Xeon Gold + ECC RAM
- GPU: RTX 5060 Ti 16GB

Kelebihan:
- ECC RAM
- Cocok buat banyak VM dan operasi 24/7
- Kapasitas RAM & ekspansi (PCIe lane, slot RAM) jauh lebih besar — upgradeability dari sisi expansion, bukan cuma ganti CPU
- Fitur server (IPMI, dll)

Kekurangan:
- Performa per-core lebih rendah
- Konsumsi daya lebih tinggi
- Biaya platform lebih mahal
- Siklus upgrade CPU generasi lambat

### Keputusan sementara

**Opsi A (AMD AM5)** condong jadi pilihan utama — satu-satunya platform dengan jaminan resmi upgrade path (AM5 sampai 2027+), plus DDR5/PCIe 5.0 native, harga sebanding LGA1851. Opsi C (Xeon/server) tetap dipertimbangkan kalau prioritas geser ke expandability RAM/PCIe lane besar buat banyak VM jangka panjang, bukan upgrade CPU per generasi.

Belum final — tunggu konfirmasi budget & ketersediaan mobo AM5 bagus di pasar lokal.

## Budget final: hard cap Rp40.000.000

Breakdown kasar Opsi A + RTX 5060 Ti (pakai harga real yang udah kekumpul, sisanya estimasi — perlu dicek manual):

Update pakai harga real hasil scraping Tokopedia/Shopee (RAM 32GB & PSU sudah dapet angka pasti, GPU dikonfirmasi 5060 Ti lebih murah dari 3090 second):

| Part | Estimasi (IDR) |
|---|---|
| Ryzen 7 9700X | 6.790.000 – 7.040.000 |
| Mobo AM5 (mid, 4 slot RAM, VRM layak) — **belum termasuk premium buat dual VGA slot, lihat catatan di bawah** | ~3.000.000 – 3.905.000 |
| RTX 5060 Ti 16GB | 13.150.000 – 19.116.000 |
| RAM DDR5 32GB kit 6000MHz (G.Skill Flare X5 – Corsair Dominator Titanium) | **6.500.000 – 12.043.200** |
| NVMe Gen4 1TB (WD Black SN770) + 2TB (WD Black SN850X) | **~4.775.000** |
| PSU 1000W 80+ Gold | **2.490.000 – 3.186.000** |
| Casing + fan/cooler | ~1.500.000 (estimasi, belum discrape) |
| **Total (skenario hemat)** | **~38.205.000** |
| **Total (skenario mahal)** | **~52.565.000** |

Skenario hemat (semua part termurah, storage sudah harga real) masih muat di bawah 40jt tapi mepet (~1,8jt sisa) — lebih ketat dari estimasi sebelumnya karena storage jadi ~4,78jt (sebelumnya diestimasi ~2,5jt). Skenario mahal jauh lewat cap — kalau kepepet harus pilih RAM G.Skill Flare X5 (bukan varian premium) dan GPU/mobo di rentang tengah-bawah biar tetap ≤40jt.

**Catatan mobo**: angka di atas masih pakai estimasi lama (entry-mid board, sebelum requirement dual VGA slot). Board B650/X670 dengan 2 slot PCIe x16 beneran kemungkinan lebih mahal dari ~3-3,9jt — perlu discrape ulang (lihat open questions), budget hemat di atas bisa makin mepet atau lewat cap begitu angka mobo dual-VGA masuk.

**RTX 3090 second dikonfirmasi bukan opsi hemat** — harga realistis 19-46,7jt, malah lebih mahal dari 5060 Ti (13,15-19,1jt) di titik manapun. Ini menguatkan keputusan pilih **RTX 5060 Ti** sbg default GPU (lihat "Pain & Gain" di atas), bukan cuma soal garansi/efisiensi tapi juga literally lebih murah.

RAM 32GB dipilih sebagai kompromi: 64GB terlalu berat di budget 40jt (DDR5 128GB kit ~29jt), 32GB (varian G.Skill ~6,5jt) cukup mepet buat 1 VM berat + host, upgrade jadi prioritas pertama begitu ada dana lebih atau harga DDR5 turun.

## Beli bertahap — GPU dulu atau CPU+RAM dulu?

Harga part lagi naik-turun signifikan (DDR5 mahal) — kemungkinan beli ga sekaligus. Pertimbangan urutan:

### Opsi 1 — GPU dulu

Alasan:
- RTX 5060 Ti komponen mahal & paling nentuin kapabilitas AI training (VRAM = batas ukuran model/batch)
- Harga baru+bergaransi relatif stabil (beda dari second-hand yang fluktuatif) — ga ada urgensi "harus buru-buru sebelum harga naik"
- GPU biasanya lebih awet dipakai lintas beberapa generasi platform

Risiko:
- GPU nganggur/ga ketes kalau platform (mobo+CPU+RAM) belum ada
- Kalau nunggu lama buat beli sisanya, GPU idle, ga produktif

### Opsi 2 — CPU+RAM (base platform) dulu

Alasan:
- Base platform (mobo+CPU+RAM) nentuin upgradeability jangka panjang — keputusan socket AM5 udah diambil, makin cepat platform ini ada, makin cepat bisa mulai setup Proxmox/VM/network meski GPU belum masuk
- Harga CPU+mobo AM5 relatif stabil dibanding RAM DDR5 yang lagi bergejolak — risiko "kemahalan karena nunggu" lebih kecil
- Bisa mulai kerja: setup OS, Proxmox, VM Ubuntu, InfluxDB/MQTT/Grafana, JupyterLab — semua ga butuh GPU buat jalan (baru training model butuh GPU)

Risiko:
- Tanpa GPU, training/simulasi berat (MATLAB GPU-accelerated, PyTorch CUDA) ga bisa jalan sama sekali sampai GPU nyusul

### Rekomendasi

**CPU+RAM (base platform) dulu**, GPU nyusul. Alasan utama: base platform bisa langsung dipakai buat kerjaan yang ga butuh GPU (server IoT — InfluxDB/MQTT/Grafana, setup Proxmox/VM, dashboard project ini). Model-training layer project ini juga defaultnya classical ML (CPU-based, lihat bagian dampak ke model-training di bawah) — jadi GPU belum jadi blocker buat mulai kerja riil. GPU (RTX 5060 Ti) baru krusial pas mulai butuh deep learning atau MATLAB simulasi berat, dan karena harganya baru+bergaransi (bukan second yang fluktuatif), ga ada tekanan buat buru-buru beli duluan.

## Harga pasar real (scraping, Agustus 2026)

Sumber: web search (Indonesia, snippet marketplace). **Catatan limitasi**: search engine yang dipakai US-only jadi hasil Indonesia terbatas ke apa yang ke-index; percobaan fetch langsung ke listing Tokopedia timeout (SPA berat, ga bisa di-scrape penuh). Angka di bawah level *estimasi dari snippet*, bukan harga per-unit presisi — cek ulang manual di marketplace sebelum beli.

| Part | Harga estimasi (IDR) | Sumber |
|---|---|---|
| AMD Ryzen 7 9700X | 6.790.000 – 7.040.000 | [Tokopedia](https://www.tokopedia.com/find/ryzen-9700x) |
| Motherboard AM5 (entry–mid) | 1.688.000 – 3.905.000 | ASRock B840M-HVS s/d B650I Lightning WiFi — [BigGo](https://biggo.id/s/motherboard%20atx%20ddr5%20am5) |
| RAM DDR5 128GB kit (Corsair Vengeance RGB 6400MHz) | ~29.000.000 | [telko.id](https://telko.id/trend-technology/harga-ram-ddr5-indonesia-2026/), [Jawa Pos](https://www.jawapos.com/teknologi/016918381/produsen-memori-berpaling-ke-ai-harga-ram-pc-di-2026-bakal-masih-selangit) |
| **RAM DDR5 32GB kit 6000MHz CL30** | **6.500.000 (G.Skill Flare X5) – 12.043.200 (Corsair Dominator Titanium)** | [Tokopedia](https://www.tokopedia.com/find/ddr5-32gb-6000-mhz-cl30) — varian lain (XPG/Klevv) tembus 40-60jt, kemungkinan salah kategori/listing custom, abaikan sbg outlier |
| Intel Core Ultra 7 270K Plus (LGA1851) | ~4.800.000 | [carisinyal.com](https://carisinyal.com/news/intel-core-ultra-200s-plus/) |
| Intel Core Ultra 5 250K Plus (LGA1851) | ~3.200.000 | [carisinyal.com](https://carisinyal.com/news/intel-core-ultra-200s-plus/) |
| PC server rakitan Xeon Gold 5416S + 128GB DDR5 ECC | 23.000.000 – 25.000.000 | [Tokopedia](https://www.tokopedia.com/find/pc-server-xeon) |
| **RTX 3090 24GB (second)** | **~19.000.000 (MSI Gaming) – 46.700.000 (ASUS ROG STRIX OC)** | [Tokopedia](https://www.tokopedia.com/find/3090-rtx) — ada listing Rp1.525.000 (Gigabyte Eagle) tapi kemungkinan bukan harga unit utuh/salah listing, diabaikan sbg outlier |
| **PSU 1000W 80+ Gold** | **2.490.000 – 3.186.000** | KYO SAMA ARMOR, NZXT C1000 GOLD, MONTECH TITAN GOLD — [Shopee](https://shopee.co.id/list/PSU/PC) |
| **NVMe Gen4 1TB — WD Black SN770** | **~1.558.000** | [Tokopedia](https://www.tokopedia.com/find/wd-black-sn770-1tb) — garansi resmi, durable, ga perlu top tier |
| **NVMe Gen4 2TB — WD Black SN850X (heatsink)** | **~3.217.000** | [Tokopedia](https://www.tokopedia.com/wd-official/ssd-wd-black-sn850x-1tb-2tb-ssd-m-2-nvme-pcie-gaming-1tb-heatsink) — WD Official Store |
| NVMe alternatif — Samsung 990 EVO Plus 1TB/2TB | 4.058.225 – 5.984.000 (1TB) / 6.219.000 – 7.699.000 (2TB) | [Tokopedia](https://www.tokopedia.com/find/samsung-990-evo-plus-2-tb) — gen4x4/gen5x2, lebih mahal dari WD Black, ga perlu buat kebutuhan sekarang |

**Storage terpilih**: WD Black SN770 1TB (~1,56jt) + SN850X 2TB (~3,2jt) = **~4,78jt total**. Kriteria "durable, gen terbaru (Gen4), garansi jelas, ga perlu top tier" terpenuhi — WD Black lini garansi resmi jelas (biasanya 5 tahun), Gen4 masih current (Gen5 baru dibutuhin kalau butuh bandwidth ekstrem, ga relevan buat beban kerja project ini), harga jauh di bawah Samsung 990 EVO Plus tanpa banyak kehilangan durability.

**Temuan kritis:**
- RAM DDR5 lagi mahal banget secara global — produsen alihin kapasitas produksi ke AI/data center, suplai konsumen ketat sejak akhir 2025, belum ada tanda turun di 2026. Kit 128GB (~29jt) sendirian udah nyaris makan seluruh budget awal. RAM 32GB (G.Skill Flare X5, ~6,5jt) jauh lebih terjangkau — cocok sama keputusan turun ke 32GB (lihat Budget final di bawah)
- **RTX 3090 second ternyata LEBIH MAHAL dari RTX 5060 Ti baru** (19-46,7jt vs 13,15-19,1jt) — makin nguatin rekomendasi 5060 Ti di section "Pilihan GPU" di atas. Second-hand 3090 ga otomatis lebih murah, malah listing termurah yang masuk akal (MSI Gaming ~19jt) itu di harga tertinggi 5060 Ti (~19,1jt) — 5060 Ti menang jelas dari sisi harga+garansi+efisiensi daya tanpa korban banyak buat kebutuhan classical ML sekarang

## Dampak ke model-training layer (project ini)

GPU RTX 5060 Ti 16GB tersedia tapi **shared** — dipakai juga buat simulasi MATLAB (VM Windows) dan training lain. Implikasi:

| Aspek | Keputusan |
|---|---|
| Lokasi jalan training | VM Ubuntu (Proxmox), akses via JupyterLab browser / Tailscale |
| Environment | Miniconda, env `livinglab-training` terpisah — biar ga bentrok CUDA/toolkit sama task lain |
| Framework utama | scikit-learn + XGBoost (CPU cukup, data awal masih dikit) |
| Framework opsional | PyTorch (CUDA) — disiapin tapi ga default dipakai sampe data historis cukup besar |
| Data source | InfluxDB (VM sama) — training script query langsung |
| Trigger | Manual/on-demand via notebook, bukan service always-on — jangan nyandera GPU shared lama-lama |
| Output | Model file (joblib/pickle) disalin ke `edge-intelligence/` (Raspberry Pi 5) |

Alasan classical ML tetap default: MOPSO butuh eval model berulang kali tiap iterasi optimasi — model ringan & cepat inference lebih aman, apalagi dieksekusi di Raspberry Pi 5 (bukan GPU workstation). Deep learning (LSTM/PyTorch) jadi opsi upgrade nanti kalau data historis sudah banyak dan classical ML mentok akurasi.

## Open questions

- [ ] GPU passthrough ke VM Ubuntu — konfirmasi Proxmox support passthrough GPU ke VM (bukan cuma host)
- [ ] Kalau VM Windows (MATLAB) & VM Ubuntu (training) butuh GPU bersamaan — perlu jadwal pemakaian atau vGPU split?
- [ ] Storage allocation per VM (NVMe 1TB + SSD/NVMe 2TB) — belum dibagi per fungsi
- [ ] Vendor/toko rakitan final
- [ ] Pilih mobo AM5 spesifik — cek VRM quality, jumlah slot RAM (4x biar bisa 64GB/128GB+), PCIe lane buat GPU + NVMe sekaligus
- [x] ~~Cek harga RTX 3090 24GB manual~~ — sudah dicek, 3090 second (19-46,7jt) ternyata lebih mahal dari 5060 Ti baru (13,15-19,1jt)
- [x] **GPU final: RTX 5060 Ti 16GB** — diputuskan, dicoret dari trade-off
- [x] ~~Cek harga RAM DDR5 32GB kit real~~ — sudah dicek, G.Skill Flare X5 ~6,5jt s/d Corsair Dominator Titanium ~12jt
- [ ] Pantau harga DDR5 — kalau turun atau budget nambah, upgrade 32GB → 64GB duluan sebelum GPU di-upgrade
- [ ] Cek harga storage (NVMe/SSD) & casing real — masih estimasi kasar, belum discrape
- [ ] Finalisasi varian RAM (pilih G.Skill, bukan varian premium) + mobo spesifik biar total tetap ≤40jt (skenario mahal saat ini ~50,3jt kalau semua part ambil termahal)
- [ ] Cari mobo AM5 B650/X670 dengan 2x slot PCIe x16 beneran (bukan cuma fisik) — cek harga real, kemungkinan naikin total budget dari estimasi mobo entry (~3-3,9jt) sebelumnya
- [ ] Hitung ulang kapasitas PSU 1000W buat skenario 2 GPU (5060 Ti x2 + CPU + komponen lain)
- [x] ~~Cek harga storage NVMe real~~ — sudah dicek. **Storage final: WD Black SN770 1TB (~1,56jt) + SN850X 2TB (~3,2jt) = ~4,78jt**, gen4, garansi resmi, ga perlu top tier (Gen5/990 Pro dilewatin, ga perlu buat kebutuhan sekarang)
- [ ] Cek harga casing real — masih estimasi kasar (~1,5jt), belum discrape
