# Belajar Proxmox VE — catatan buat workstation project ini

Status: catatan belajar, hardware sudah dibeli & dirakit (lihat [pc-workstation-spec.md](pc-workstation-spec.md)).

## Spek final aktual (dari surat penawaran Youngs Computer, Revisi 2, 03 Sep 2026)

Sumber: [SURAT_PENAWARAN_UNS_5Paket_Revisi_2.pdf](SURAT_PENAWARAN_UNS_5Paket_Revisi_2.pdf) — beda dari estimasi awal di pc-workstation-spec.md, catat di sini biar ga campur aduk sama angka lama.

| Paket | Komponen | Harga |
|---|---|---|
| 1 | AMD Ryzen 7 9700X Tray (No Fan) | Rp5.379.000 |
| 1 | Motherboard **MSI PRO X870E-S EVO WIFI** (beda dari rencana lama ASROCK X870 Challenger) | Rp4.734.000 |
| 2 | VGA COLORFUL iGame RTX 5060 Ti Ultra W DUO OC 16GB-V | Rp14.499.000 |
| 4 | ~~SSD NVMe Samsung 990 PRO 1TB with Heatsink~~ — **diubah ke WD Black 1TB Gen5** | (angka surat lama, harga final ikut item pengganti) |
| 5 | PSU MSI MAG A1000GLS 1000W 80+ Gold Full Modular | Rp2.520.000 |
| **Total (4 paket ini)** | | **Rp31.122.000** |

Catatan: nomor paket lompat dari 1,2 ke 4,5 — paket 3 (RAM) ga ikut di surat pengadaan ini, **dibeli terpisah**: RAM **XPG (ADATA) DDR5 32GB kit**, di luar paket resmi Youngs Computer.

Storage aktual = **1x NVMe 1TB WD Black, Gen5** — diganti dari Samsung 990 PRO (Gen4) yang tercantum di surat penawaran. Ini yang jadi acuan final, bukan angka Gen4 di surat.

## Spek final ringkas (gabungan surat + beli terpisah)

| Komponen | Detail |
|---|---|
| CPU | AMD Ryzen 7 9700X Tray (non-iGPU) |
| Motherboard | MSI PRO X870E-S EVO WIFI |
| RAM | XPG (ADATA) DDR5 32GB kit (beli terpisah) |
| GPU | Colorful iGame RTX 5060 Ti Ultra W DUO OC 16GB |
| Storage | WD Black 1TB NVMe Gen5 |
| PSU | MSI MAG A1000GLS 1000W 80+ Gold Full Modular |

## Apa itu Proxmox VE

Proxmox VE = OS khusus (berbasis Debian Linux) yang fungsinya jadi **hypervisor** — jalanin banyak komputer virtual di satu hardware fisik. Install langsung ke disk, gantiin posisi Windows/Linux biasa. Setelah install, kerja sehari-hari dilakukan lewat **Web UI** (browser) dari laptop/PC lain di jaringan yang sama, bukan ngetik langsung di layar host.

## Istilah dasar

| Istilah | Arti |
|---|---|
| **Host** | Mesin fisik hasil rakitan, yang jalanin Proxmox |
| **VM (Virtual Machine)** | Komputer virtual penuh, punya OS sendiri (Ubuntu, Windows) — seolah komputer terpisah |
| **LXC (Container)** | Versi ringan dari VM, share kernel Linux dgn host, lebih hemat resource — cocok buat servis Linux ringan |
| **Passthrough** | Meminjamkan hardware fisik (misal GPU) langsung ke satu VM tertentu, seolah nempel langsung di VM itu |

## Kenapa Proxmox cocok buat kebutuhan ini

Butuh 2 dunia beda sekaligus di satu PC: Linux (server IoT + training AI) dan Windows (MATLAB), tapi harus terisolasi biar ga saling ganggu. Proxmox yang atur pembagian resource (CPU, RAM, GPU) ke masing-masing VM.

## Rencana arsitektur (dari pc-workstation-spec.md)

- **VM Ubuntu** — Docker, InfluxDB, MQTT, Grafana, JupyterLab, training env (scikit-learn/XGBoost, opsional PyTorch)
- **VM Windows** — MATLAB dan software berlisensi lain
- **GPU passthrough** — RTX 5060 Ti dipakai gantian/split antara training (Ubuntu) dan simulasi MATLAB (Windows)
- **Remote access** — Tailscale/WireGuard buat akses jaringan dari mana aja, browser buat Grafana/JupyterLab, Guacamole/noVNC buat MATLAB tanpa RDP

## Alur besar instalasi (rencana, belum dieksekusi)

1. Install Proxmox VE ke PC (via USB installer) — jadi OS dasar host
2. Buka Web UI dari browser laptop lain (akses via IP host, port 8006)
3. Bikin VM Ubuntu → install Docker dkk di dalamnya
4. Bikin VM Windows → install MATLAB
5. Setting GPU passthrough ke VM yang lagi butuh

## Alokasi resource — RAM vs CPU vs GPU beda cara kerja

**RAM — di-limit tetap, ga bisa "joinan" pas 2 VM jalan bareng**
Tiap VM dikasih alokasi tetap (misal Ubuntu 12GB, Windows 16GB). Total alokasi ga boleh lebih dari RAM fisik host dikurangi jatah host sendiri (~2-4GB). Ada mode "ballooning" (RAM elastis, VM bisa minjem balik ke pool kalau nganggur) tapi tetap ada batas maksimal per VM, bukan gabungan bebas. RAM 32GB kerasa ketat kalau 2 VM jalan bareng — lihat catatan di [pc-workstation-spec.md](pc-workstation-spec.md).

**CPU — di-share, relatif fleksibel**
vCPU dialokasikan ke tiap VM, tapi Proxmox bisa overcommit — total vCPU yang dijatah ke semua VM boleh lebih banyak dari core fisik, karena dipakai gantian (time-slicing) kalau ga dipakai penuh bersamaan. Lebih "joinan" dibanding RAM, asal ga dua-duanya nge-full-load bersamaan.

**GPU — paling kaku, TIDAK bisa dibagi berbarengan dengan setup standar**
Passthrough GPU consumer (RTX 5060 Ti) sifatnya exclusive — begitu di-passthrough ke satu VM, GPU "hilang" dari host dan VM lain, cuma bisa dipakai VM yang lagi pegang dia. Opsi:
1. **Gantian manual** — assign GPU ke VM yang butuh, matiin VM itu, baru assign+nyalain VM lain
2. **vGPU/MIG (split beneran)** — butuh GPU kelas datacenter (NVIDIA A-series/Grid license), RTX 5060 Ti (consumer) ga resmi support ini
3. **GPU kedua** — pasang di slot PCIe kedua (mobo AM5 udah siapin dual-VGA slot), tiap VM pegang GPU sendiri

## Software VM Windows: CPU-bound atau butuh GPU?

**MATLAB** default CPU-bound (Simulink, signal processing, optimization, statistics jalan di CPU). GPU cuma kepake **kalau eksplisit dipakai**: Parallel Computing Toolbox, Deep Learning Toolbox, atau function `gpuArray`. Kalau simulasi ga pakai toolbox itu, GPU ga kepake sama sekali.

Kalau MATLAB di VM Windows ga pakai GPU-toolbox → CPU-bound semua, GPU passthrough ke Windows ga perlu, GPU bisa full-time di Ubuntu buat training. **Perlu dikonfirmasi**: MATLAB simulasi project ini nanti pakai GPU-accelerated toolbox apa ga.

## Catatan penting: CPU Ryzen 7 9700X = non-iGPU (versi "non-G")

CPU ini ga punya GPU terintegrasi (beda dari varian 9700G yang ada Radeon iGPU built-in). Konsekuensi:

- **GPU diskrit (RTX 5060 Ti) wajib terpasang buat ada display sama sekali** — CPU sendirian ga bisa render output ke monitor
- **Host butuh GPU available buat console lokal saat instalasi awal** — begitu GPU di-passthrough penuh ke VM, host kehilangan output display-nya. Biasanya ga masalah karena Proxmox dikelola headless lewat Web UI, tapi pas install awal / troubleshooting boot, GPU harus nancep & belum di-passthrough dulu
- **VM Windows tanpa GPU passthrough tetap dapat virtual display default** (SPICE/VNC) — cukup buat install OS, jalanin MATLAB CPU-only, remote lewat RDP/noVNC, cuma ga ada GPU acceleration

## Catatan: storage aktual cuma 1 NVMe 1TB Gen5

Beda dari rencana awal spec (1TB+2TB terpisah) — hardware jadi cuma **1 NVMe 1TB Gen5**. Konsekuensi: Proxmox host + VM Ubuntu + VM Windows + data InfluxDB + model files numpuk di 1 disk yang sama. Ga ada pemisahan fisik OS vs data, backup jadi lebih penting. Space & I/O dipakai bareng oleh semua VM.

## Scaffolding alokasi resource (starting point)

Asumsi hardware: Ryzen 7 9700X (8 core/16 thread), RAM 32GB, NVMe 1TB Gen5.

**CPU (16 thread total)**

| Alokasi | Thread |
|---|---|
| Host Proxmox | reserve ~2 (otomatis, ga usah dialokasi eksplisit) |
| VM Ubuntu | 8 vCPU |
| VM Windows | 6 vCPU |
| Total dialokasikan | 14 / 16 |

Overcommit dikit oke — CPU boleh "joinan", jarang dua-duanya full-load bersamaan.

**RAM (32GB)**

| Alokasi | RAM |
|---|---|
| Host Proxmox | ~2-3GB (reserve, ga usah alokasi manual) |
| VM Ubuntu | 16GB |
| VM Windows | 12GB |
| Total dialokasikan | 28GB / 32GB |

Ketat tapi cukup, asal ga training berat + MATLAB berat bersamaan. Ballooning bisa diaktifkan biar elastis dikit.

**Storage (NVMe 1TB Gen5)**

| Alokasi | Size |
|---|---|
| Proxmox host (root/local-lvm overhead) | ~30-40GB |
| VM Ubuntu disk (OS+Docker+Miniconda+InfluxDB+model files) | 200GB |
| VM Windows disk (OS+MATLAB) | 250GB |
| Sisa buffer/snapshot/growth | ~500-520GB |

Angka starting point, disesuaikan pas instalasi — disk image bisa di-resize kalau thin-provisioned.

## Tambahan HDD — buat data dingin, dampingi NVMe

NVMe 1TB kerasa ketat kalau semua numpuk (OS host + 2 VM disk + data aktif). Solusi: tambah **HDD internal** buat data yang jarang diakses cepat, biar NVMe lega buat data aktif.

**Prinsip pembagian**: NVMe = hot data (butuh speed, akses sering/random), HDD = cold data (jarang disentuh, butuh kapasitas besar & murah).

| Data | Taro di | Alasan |
|---|---|---|
| OS Proxmox host | NVMe (wajib) | Boot & operasi sistem butuh latency rendah |
| VM disk (OS Ubuntu, OS Windows, aplikasi) | NVMe (wajib) | Random I/O cepat, kalau di HDD kerasa lemot |
| InfluxDB aktif (data terbaru, di-query Grafana real-time) | NVMe | Query time-series butuh speed |
| Backup/snapshot VM | HDD | Tulis sekali, jarang dibaca — ga butuh speed |
| Data historis InfluxDB (retention lama, archive) | HDD | Jarang di-query, agak lambat masih oke |
| Dataset training mentah / model files arsip | HDD | Baca sesekali pas training ulang |
| Dokumentasi, log lama | HDD | Akses jarang |

**Cek katalog** [Youngs Componen Retail Price.xlsx](katalog/Youngs%20Componen%20Retail%20Price.xlsx) (sheet "Storage Int", kolom HDD 3.5"):

| Model | Kapasitas | Harga | Catatan |
|---|---|---|---|
| WD Red Plus 2TB | 2TB | Rp3.132.000 | **NAS-grade**, rated 24/7, garansi 3 tahun |
| WD Red Plus 4TB | 4TB | Rp4.190.000 | NAS-grade, kapasitas lebih lega — rekomendasi kalau budget cukup |
| Seagate 2TB 1 Tahun | 2TB | Rp1.147.000 | Desktop-grade, garansi 1 tahun — di bawah 2jt tapi ga didesain 24/7 |

Ga ada NAS-grade (Red Plus/IronWolf) di bawah 2jt — yang murah semua desktop/CCTV-grade dengan garansi pendek (1 tahun). Trade-off: kalau HDD cuma buat backup sesekali (ga nyala nonstop nemenin database), Seagate 2TB 1 Tahun cukup aman & hemat. Kalau rencananya jadi storage aktif yang nyala 24/7 dampingi InfluxDB, lebih aman ke WD Red Plus (NAS-grade, garansi 3 tahun).

## Sharing antar VM: Postgre + Samba

**Akses Postgre dari VM Windows** — ga butuh NAS/file share sama sekali. Postgre server jalan di VM Ubuntu (misal di Docker), VM Windows connect langsung lewat network (IP VM Ubuntu + port 5432, pakai pgAdmin/psycopg2/ODBC). Cukup pastiin kedua VM satu jaringan (default bridge Proxmox) dan firewall Postgre allow koneksi dari IP VM Windows.

**Sharing file umum (bukan database)** — pakai Samba di VM Ubuntu, diakses dari VM Windows:

1. Kedua VM (Ubuntu & Windows) dipasang di **bridge network yang sama** (default `vmbr0`) — otomatis satu jaringan lokal virtual, saling ping pakai IP masing-masing
2. Install Samba di VM Ubuntu, share folder tertentu (misal `/mnt/shared`, tempat model files/data export)
3. Dari VM Windows: File Explorer → "Map network drive" → `\\<IP-VM-Ubuntu>\<nama-share>`
4. File di VM Ubuntu keliatan & bisa diakses/ditulis dari Windows kayak drive lokal

Catatan:
- Traffic lewat virtual bridge internal Proxmox, ga keluar ke jaringan fisik — cepat, ga ada overhead router beneran
- IP VM Ubuntu sebaiknya **static** (bukan DHCP random) biar mapping network drive di Windows ga putus kalau IP berubah
- Postgre tetap diakses terpisah lewat port 5432 (bukan lewat Samba) — Samba cuma buat file, bukan query database
- Internal lab, jaringan Proxmox udah terisolasi — auth Samba simpel/guest biasanya cukup, ga perlu setup user kompleks

## Networking: bridge = tiap VM dapat IP sendiri

Mode bridge (`vmbr0`) itu bukan NAT/isolasi — nyambungin NIC fisik host ke "saklar virtual" yang tiap VM nempel di situ juga. Efeknya host + semua VM keliatan sebagai device terpisah di jaringan fisik (dapat IP dari router/DHCP), persis kayak 3 laptop fisik nyambung ke switch/router yang sama.

| Device | Contoh IP | Diakses buat |
|---|---|---|
| Proxmox host | 192.168.1.10 | Web UI manage (port 8006) |
| VM Ubuntu | 192.168.1.11 | SSH, Grafana, JupyterLab, Postgre, Samba |
| VM Windows | 192.168.1.12 | RDP, MATLAB |

Semua diakses langsung dari laptop lain di jaringan yang sama (atau via Tailscale dari luar), ga perlu lewat host dulu.

**Penting**: pakai static IP/DHCP reservation buat ketiganya — khususnya VM Ubuntu (tempat Samba+Postgre) biar mapping drive & koneksi database ga putus kalau IP berubah.

## VM Windows: pakai edisi biasa (10/11), bukan Windows Server

Kebutuhan cuma jalanin MATLAB — bukan file server/domain controller/service enterprise. Alasan pilih edisi biasa:
- Windows Server fiturnya (Active Directory, Hyper-V role, IIS) ga relevan buat 1 VM single-purpose
- GUI Server lebih "server-oriented", banyak setting/wizard ga perlu buat sekadar jalanin software desktop
- Lisensi Windows Server lebih rumit (CAL, per-core) — Windows 10/11 lebih simpel buat personal-use
- MATLAB & software lisensi lain biasanya ditest penuh di desktop edition

Windows Server baru relevan kalau nanti butuh role server beneran (domain controller, print server, dst) — bukan kasus sekarang.

## Cara kerja: install dulu, revisi lewat praktek

Proxmox murah buat diulang — install ulang host cuma ~15-20 menit, aman selama disk boleh diwipe. Yang **ga murah** diulang: data di dalam VM (dataset, config lama) — begitu ada data berharga (InfluxDB, model files), backup dulu sebelum eksperimen ulang.

Alur kerja: install Proxmox → bikin 1 VM dulu (Ubuntu) buat ngerasain alokasi CPU/RAM/disk/network nyata → kalau ada yang salah, tinggal resize/edit config VM (banyak yang bisa live, ga perlu install ulang host) → baru lanjut GPU passthrough & VM Windows setelah dasar paham.

## Yang perlu dicek sebelum install (BIOS/hardware)

- **Virtualization enable** — SVM Mode (AMD) harus aktif di BIOS
- **IOMMU enable** — wajib buat GPU passthrough ke VM
- **IOMMU groups bersih** — GPU & slotnya kepisah rapi dari device lain (baru bisa dicek setelah OS jalan, pakai command `lspci` + cek grouping)
- **Pembagian storage**: cuma 1x NVMe 1TB WD Black Gen5 — OS Proxmox + kedua VM disk + data numpuk di 1 disk (lihat scaffolding di atas)

## Open questions (carry-over dari pc-workstation-spec.md)

- [ ] Proxmox konfirmasi support passthrough GPU ke VM (bukan cuma ke host)
- [ ] VM Windows (MATLAB) & VM Ubuntu (training) butuh GPU bersamaan — perlu jadwal pakai gantian, atau split (vGPU)?
- [ ] Storage allocation per VM di 1 NVMe 1TB (host, VM Ubuntu, VM Windows, data) — angka scaffolding masih starting point, belum final

## Next step belajar

- [ ] Paham beda VM vs LXC lebih dalam — kapan pakai yang mana
- [ ] Paham cara kerja GPU passthrough (VFIO) di Proxmox
- [ ] Practical: bikin bootable USB installer Proxmox
- [ ] Practical: instalasi step-by-step (partisi disk, network config awal)
