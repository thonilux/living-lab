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

## Progress instalasi nyata (log)

Mobo aktual: **MSI PRO X870E-S EVO WIFI** (bukan ASROCK X870 Challenger yang dicatat di rencana awal). Detail dari manual resmi:
- PCI_E1 (dari CPU): PCIe 5.0 x16 — slot GPU utama
- PCI_E2 (dari chipset): cuma PCIe 4.0 x4 — **bukan x16 penuh**, jadi rencana lama "dual-GPU slot setara" di catatan sebelumnya tidak akurat untuk mobo ini
- M2_1 (dari CPU): PCIe 5.0 x4 — cocok dipasangi NVMe Gen5 (WD Black) buat speed penuh
- Cara masuk BIOS: tekan **Delete** saat boot (bukan F2/F11 seperti asumsi awal)

**Status BIOS setup**: SVM Mode dan IOMMU sudah **Enabled**, stabil setelah restart — prasyarat dasar virtualization untuk Proxmox VE sudah terpenuhi.

**Insiden EZ Debug LED (DRAM) saat setup**: LED DRAM (kuning) sempat nyala setelah enable SVM pertama kali dan lagi setelah enable IOMMU. Ini **normal** — bukan RAM rusak, melainkan proses memory retraining DDR5/AM5 tiap kali ada perubahan setting BIOS penting. Solusi: biarin sistem reboot sendiri tanpa diganggu (bisa makan waktu, layar hitam sebentar), jangan force restart berulang karena tiap interupsi bikin retraining mulai dari nol. Sempat dicoba Clear CMOS di tengah proses, hasil akhirnya normal setelah dikasih waktu.

**SVM (Secure Virtual Machine)**: fitur virtualization AMD (setara Intel VT-x) — instruksi CPU yang dipakai hypervisor (Proxmox) buat isolasi VM. Wajib nyala duluan sebelum VM bisa jalan sama sekali, terpisah dari IOMMU yang khusus buat GPU passthrough.

**Insiden hard lockup saat instalasi awal**: instalasi Proxmox 9.2-1 sempat gagal berkali-kali dengan error "watchdog detected hard LOCKUP on CPU X" di kernel boot log (macet setelah baris "RAS: Correctable Errors collector initialized"). Root cause: BIOS motherboard versi lama belum punya microcode AGESA yang matang buat CPU Zen 5 (Ryzen 9000). **Solusi: update BIOS motherboard ke versi terbaru via M-FLASH** (dari dalam BIOS, USB FAT32 berisi file BIOS terbaru dari situs MSI). Setelah update BIOS, pilih **PBO: Auto** (bukan Enable manual — prioritas stabilitas 24/7 daripada ekstra performa overclock), dan **SVM + IOMMU harus di-enable ulang** karena update BIOS reset semua setting ke default.

**Instalasi berhasil**: Proxmox VE 9.2-1 terinstall, IP static `10.10.10.95/24` (gateway `10.10.10.1`, jaringan rumah 10.10.10.0/24), akses Web UI di `https://10.10.10.95:8006`, login `root` + password saat instalasi.

**Verifikasi GPU & IOMMU dari Shell Proxmox** (Node > Shell di Web UI):
```
lspci | grep -i nvidia
# 01:00.0 VGA compatible controller: NVIDIA Corporation GB206 [GeForce RTX 5060 Ti]
# 01:00.1 Audio device: NVIDIA Corporation GB206 High Definition Audio Controller

dmesg | grep -e DMAR -e IOMMU
# AMD-Vi: IOMMU performance counters supported — IOMMU aktif di level kernel, bukan cuma BIOS
```

GPU RTX 5060 Ti kedetek sempurna, IOMMU aktif. Fondasi host siap buat langkah berikutnya: bikin VM Ubuntu pertama, baru nanti setup GPU passthrough.

**Troubleshooting GPU passthrough — error "Failed to set group container: Invalid argument"**: setelah VM Ubuntu jalan dan coba passthrough RTX 5060 Ti (IOMMU group 13, terisolasi bersih), start VM gagal dengan error VFIO container. Langkah troubleshoot yang sudah dicoba:
- Blacklist `nouveau` dan `nvidiafb` di `/etc/modprobe.d/blacklist.conf` — beres (nouveau berhasil dilepas dari GPU)
- Tambah `video=efifb:off` ke `GRUB_CMDLINE_LINUX_DEFAULT`, `update-grub`, reboot — belum menyelesaikan error
- `allow_unsafe_interrupts=1` — tidak relevan, AMD-Vi interrupt remapping sudah aktif (dikonfirmasi via dmesg)
- Machine type diubah ke q35 (dari default i440fx) — tidak menyelesaikan
- Cek raw device vs mapped device (Resource Mapping) — sama-sama gagal di titik identik
- Downgrade kernel dari `7.0.14-17-pve` ke `7.0.2-6-pve` (pin via `proxmox-boot-tool`) — tidak menyelesaikan, bug ada di seluruh kernel 7.0.x series
- `options vfio-pci disable_idle_d3=1` (fix yang dilaporkan berhasil di forum untuk kasus identik RTX 5060 Ti) — tidak menyelesaikan
- Cek BIOS: Above 4G Decoding, Resizable BAR, CSM — semua dicek/diubah, tidak menyelesaikan
- Verifikasi driver vfio-pci ter-bind benar di kedua device (VGA + Audio), IOMMU group 13 terisolasi bersih, device node `/dev/vfio/13` ada, tidak ada proses lain yang mengunci — semua normal

**Kesimpulan sementara**: error "Failed to set group container: Invalid argument" ini kemungkinan besar **bug genuine** pada kombinasi RTX 5060 Ti (arsitektur Blackwell, GPU sangat baru) dengan kernel Linux yang dipakai Proxmox VE 9.2.20 (kernel 7.0.x series) — dikonfirmasi ada laporan serupa di forum komunitas Proxmox dengan kartu GPU yang sama persis. Solusi yang dilaporkan berhasil di forum (downgrade ke kernel 6.14) **tidak tersedia** di repo Proxmox 9.x (baseline kernelnya sudah 6.14+/7.0.x, versi lebih lama cuma ada di Proxmox VE 8.x — downgrade itu berarti downgrade seluruh versi Proxmox, bukan cuma kernel).

**Opsi ke depan**:
1. Tunggu update kernel/qemu dari Proxmox yang memperbaiki bug ini (biasanya GPU generasi baru butuh beberapa siklus rilis sebelum passthrough-nya matang)
2. Tunggu update BIOS motherboard lebih baru dari MSI (AGESA versi lebih matang untuk Zen 5 + passthrough)
3. Sementara waktu, training AI/ML pakai classical ML (CPU-based, scikit-learn/XGBoost — sudah jadi default rencana project ini) sambil GPU belum bisa di-passthrough ke VM
4. Jika mendesak, opsi ekstrem: install Proxmox VE 8.x (downgrade penuh) untuk pakai kernel 6.x lama — belum direkomendasikan karena kehilangan fitur/fix versi 9.x lain

**Update — diputuskan downgrade ke Proxmox VE 8.x**: setelah mengikuti panduan detail dari [kovasky.me RTX 5000 series passthrough guide](https://kovasky.me/blogs/rtx_5000_passthrough/) (tambahan `pcie_acs_override=downstream,multifunction`, `nofb nomodeset video=vesafb:off,efifb:off`, `kvm ignore_msrs=1`, ROM-Bar & Primary GPU di-uncheck saat Add PCI Device) — masih error identik "Failed to set group container: Invalid argument". Dicek juga `dma_entry_limit` dan AMD-Vi event log — semua normal, tidak ada IO_PAGE_FAULT/hardware fault tercatat. Kesimpulan: bug ada di level kernel 7.0.x series Proxmox, bukan config/BIOS/hardware. **Keputusan: install ulang Proxmox VE 8.4** (base kernel 6.8/6.11, beda major version dari 7.0.x yang bermasalah) — data VM saat itu masih minim jadi install ulang tidak banyak kerugian. Setelah instal ulang, ulangi: enable SVM+IOMMU di BIOS, buat VM Ubuntu baru, dan coba GPU passthrough lagi dari awal.

**Masalah baru setelah downgrade ke Proxmox 8.4**: NIC Ethernet onboard (Realtek RTL8126VB, 5G LAN) **tidak kedetect sama sekali** oleh installer maupun kernel 6.8 — installer cuma nampilin interface WiFi (`wlp15s0`) sebagai pilihan. Ini kebalikan dari masalah GPU: kernel 6.8 (lebih lama) belum punya driver buat NIC 5G Realtek generasi baru, sementara kernel 7.0.x (lebih baru, dipakai Proxmox 9.x) yang punya dukungan NIC ini malah punya bug GPU passthrough. Trade-off pahit: **kernel lama → NIC mati, kernel baru → GPU passthrough mati**.

Sempat dicoba workaround: USB tethering dari HP Android buat dapat internet sementara (guna `apt install dkms` + `pve-headers`, lalu compile driver [realtek-r8126-dkms](https://github.com/awesometic/realtek-r8126-dkms) dari GitHub via `.deb`) — tethering tidak stabil (interface sering DOWN sendiri, `dpkg -i` gagal karena dependency `dkms` belum ada dan repo tidak bisa diakses).

## Keputusan besar: ganti skema arsitektur — Ubuntu jadi OS utama, bukan Proxmox

Setelah 2 masalah besar (GPU passthrough gagal di kernel baru, NIC tidak kedetect di kernel lama) sama-sama berakar dari **kematangan dukungan kernel terhadap hardware yang sangat baru** (CPU Zen 5, GPU Blackwell, NIC 5G Realtek terbaru), diputuskan ganti pendekatan: **Ubuntu Desktop jadi OS host langsung** (bukan hypervisor Proxmox), VM Windows (buat MATLAB) dijalankan di dalam Ubuntu pakai VirtualBox/virt-manager.

**Alasan (pro vs Proxmox):**

| Aspek | Ubuntu host | Proxmox (semula) |
|---|---|---|
| GPU | Install driver NVIDIA native di host — tidak ada VFIO/IOMMU/container sama sekali, tidak ada bug passthrough | Perlu passthrough, kena bug kernel 7.0.x |
| Network | Kernel Ubuntu jauh lebih baru & update rutin, NIC 5G Realtek kemungkinan besar langsung kedetect | Kernel 6.8/7.0.x sama-sama bermasalah (NIC vs GPU) |
| Kompleksitas | Docker/InfluxDB/Grafana/JupyterLab langsung install di OS, tidak perlu mikir resource mapping/passthrough | Butuh setup VM + passthrough terpisah |
| Isolasi | **Kalah** — semua servis + training jalan di 1 OS yang sama, kalau crash bisa berdampak ke VM Windows juga | VM-VM terisolasi kernel masing-masing |
| Manajemen VM | Manual via virt-manager/VirtualBox GUI, tidak ada Web UI terpusat | Web UI terpusat, akses dari browser mana saja |
| Scalability jangka panjang | Kurang siap kalau nanti perlu banyak VM sekaligus | Lebih matang untuk multi-VM/clustering |

**Kesimpulan**: karena root cause masalah adalah kematangan software untuk hardware terbaru (bukan soal arsitektur yang salah), dan kebutuhan saat ini cuma 2 beban kerja (server IoT+training di 1 OS, MATLAB di 1 VM Windows) — Ubuntu host lebih pragmatis dipakai sekarang. Proxmox bisa dicoba lagi nanti setelah kernel/driver GPU & NIC generasi ini lebih matang (biasanya beberapa bulan setelah rilis hardware baru).

**Rencana baru**:
1. Install Ubuntu Desktop 24.04/26.04 LTS langsung ke NVMe (ganti Proxmox)
2. Install driver NVIDIA native — GPU RTX 5060 Ti dipakai langsung host untuk training
3. Install Docker, InfluxDB, MQTT, Grafana, JupyterLab langsung di Ubuntu host
4. Install VirtualBox atau virt-manager (KVM/QEMU) untuk jalankan VM Windows (MATLAB) — GPU tidak perlu di-passthrough ke VM ini karena MATLAB pakai simulasi CPU-bound (lihat catatan sebelumnya)
5. Cek NIC Realtek RTL8126 kedetect otomatis di installer Ubuntu — kemungkinan besar tidak perlu compile driver manual lagi

**VM Ubuntu — pilih Server, bukan Desktop**: sempat ada ISO Ubuntu Desktop 26.04.1 di tangan, tapi diputuskan download **Ubuntu Server 24.04 LTS** dulu — GUI Desktop (GNOME) ga perlu buat VM yang isinya cuma Docker/InfluxDB/MQTT/Grafana/JupyterLab (semua diakses via browser/API), dan makan RAM+storage lebih banyak sia-sia buat server 24/7.

**Catatan community script LXC**: sempat ada opsi pakai script `ct/ubuntu.sh` dari community-scripts/ProxmoxVE (bikin LXC container instan). Ditolak buat VM utama karena butuh GPU passthrough proper dan Docker nested — LXC kurang cocok buat itu (lihat alasan VM vs LXC di atas). Script itu tetap valid dipakai nanti kalau butuh container ringan terpisah (misal cuma servis kecil tanpa GPU).

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
