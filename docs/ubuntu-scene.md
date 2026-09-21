# Skema Ubuntu — pengganti Proxmox

Status: keputusan final setelah 2 hari troubleshooting GPU passthrough + NIC di Proxmox gagal total (lihat [proxmox-belajar.md](proxmox-belajar.md) untuk histori lengkap). Root cause: hardware terlalu baru (CPU Zen 5, GPU Blackwell RTX 5060 Ti, NIC Realtek RTL8126 5G) — kernel Proxmox versi manapun (7.0.x maupun 6.8.x) tidak bisa dukung semuanya sekaligus.

## Kenapa pindah dari Proxmox ke Ubuntu

| Aspek | Ubuntu host | Proxmox (semula) |
|---|---|---|
| GPU | Install driver NVIDIA native di host — tidak ada VFIO/IOMMU/container, tidak ada bug passthrough | Perlu passthrough, kena bug kernel 7.0.x ("Failed to set group container: Invalid argument") |
| Network | Kernel Ubuntu lebih baru & update rutin, NIC 5G Realtek kemungkinan besar langsung kedetect | Kernel 6.8 (NIC tidak kedetect) vs 7.0.x (GPU rusak) — dua-duanya bermasalah |
| Kompleksitas | Docker/InfluxDB/Grafana/JupyterLab langsung install di OS | Butuh setup VM + passthrough terpisah |
| Isolasi | Kalah — semua servis + training jalan di 1 OS yang sama | VM-VM terisolasi kernel masing-masing |
| Manajemen VM | Manual via virt-manager/VirtualBox GUI | Web UI terpusat, akses browser |
| Scalability jangka panjang | Kurang siap untuk banyak VM sekaligus | Lebih matang untuk multi-VM/clustering |

Kesimpulan: kebutuhan project cuma 2 beban kerja (server IoT+training di 1 OS, MATLAB di 1 VM Windows) — isolasi penuh ala Proxmox bukan kebutuhan mendesak. Ubuntu native lebih pragmatis sekarang, Proxmox bisa dicoba lagi nanti setelah kernel/driver GPU+NIC generasi ini matang.

## Struktur baru

### Host: Ubuntu Desktop 24.04/26.04 LTS
Jalan langsung di hardware, pegang GPU native — tidak ada lapisan hypervisor lagi.

**Layer 1 — Servis dasar** (native di Ubuntu, dijalankan via Docker container):
- InfluxDB (database time-series)
- MQTT broker (Mosquitto)
- Grafana (dashboard)
- JupyterLab (notebook eksplorasi data)

**Layer 2 — Training environment**:
- Miniconda, env `livinglab-training` — isolasi cukup lewat conda env, tidak perlu VM terpisah lagi
- scikit-learn/XGBoost (classical ML, CPU — tetap default sesuai keputusan lama)
- PyTorch + CUDA (opsional, GPU native langsung kepake tanpa passthrough)

**Layer 3 — VM Windows** (di dalam Ubuntu, pakai VirtualBox atau virt-manager/KVM):
- Khusus MATLAB dan software Windows lain
- CPU + RAM saja, GPU tidak di-passthrough (MATLAB simulasi CPU-bound, sudah dikonfirmasi sebelumnya)
- Diakses via RDP internal atau tampilan langsung VirtualBox

### Remote access
- Tailscale/WireGuard di Ubuntu host — akses jaringan dari mana saja
- Grafana/JupyterLab lewat browser
- MATLAB VM lewat RDP kalau remote, atau langsung monitor kalau di lokasi

### Storage
- Semua di 1 NVMe (WD Black 1TB Gen5) — OS Ubuntu, Docker volumes, training env, disk VM Windows berbagi disk yang sama

## Beda paling penting dari skema Proxmox lama

- Tidak ada "VM Ubuntu" terpisah lagi — Ubuntu **jadi host itu sendiri**
- Servis (Docker dkk) jalan langsung di host, bukan di dalam VM
- Cuma ada **1 VM** (Windows/MATLAB), bukan 2 VM seperti rencana awal
- GPU otomatis available untuk training, tidak perlu setup passthrough rumit

## Rencana eksekusi

1. Install Ubuntu Desktop 24.04/26.04 LTS langsung ke NVMe (ganti Proxmox)
2. Cek NIC Realtek RTL8126 kedetect otomatis di installer — validasi asumsi kernel lebih baru menyelesaikan masalah
3. Install driver NVIDIA native — GPU RTX 5060 Ti dipakai langsung host untuk training
4. Install Docker, InfluxDB, MQTT, Grafana, JupyterLab langsung di Ubuntu host
5. Install VirtualBox atau virt-manager (KVM/QEMU), buat VM Windows untuk MATLAB

## Progress eksekusi (log)

**Instalasi berhasil**: Ubuntu 26.04.1 LTS (codename "resolute"), kernel `7.0.0-31-generic`, hostname `dewa-iotlab`, user `iotlab`.

**NIC Realtek RTL8126 — kedetect otomatis, tanpa perlu compile driver manual**: `enp14s0` langsung UP dengan driver `r8169` bawaan kernel. Ini konfirmasi dugaan utama alasan pindah ke Ubuntu — kernel lebih baru (7.0.0) langsung dukung NIC 5G generasi baru ini, beda dari kernel 6.8 Proxmox 8.4 yang sama sekali tidak kedetect.

**GPU NVIDIA — driver native berhasil, tanpa passthrough sama sekali**:
```
sudo apt install nvidia-driver-595-open
sudo reboot
```
Sempat muncul layar **MOK Management** (biru, karena Secure Boot aktif) — pilih **Perform MOK management > Enroll MOK > Continue**, masukin password yang dibuat saat instalasi driver, **Reboot**. Setelah reboot, verifikasi:
```
nvidia-smi
```
Hasil: driver `595.91.07`, CUDA `13.2`, GPU RTX 5060 Ti terdeteksi penuh dengan 16GB VRAM. **Ini menyelesaikan masalah yang 2 hari mentok di Proxmox** (`Failed to set group container: Invalid argument`) — GPU langsung jalan di host tanpa VFIO/IOMMU/container sama sekali.

## Open questions

- [x] ~~Konfirmasi NIC Realtek RTL8126 kedetect otomatis di Ubuntu installer~~ — sukses, driver r8169 otomatis
- [x] ~~Konfirmasi driver NVIDIA terbaru support penuh RTX 5060 Ti di Ubuntu LTS~~ — sukses, nvidia-driver-595-open, CUDA 13.2
- [ ] Tentukan tool VM Windows: VirtualBox (lebih mudah, GUI ramah) vs virt-manager/KVM (lebih ringan, performa lebih baik)
- [ ] Setup Tailscale/WireGuard di Ubuntu host untuk remote access
- [x] ~~Install Docker~~ — sukses via `get.docker.com` script
- [x] ~~Setup akses remote GUI~~ — GNOME Remote Desktop (RDP) sudah aktif bawaan, port 3389, tidak perlu setup VNC/noVNC manual (sempat dicoba TigerVNC tapi crash — GNOME Shell modern/Wayland tidak cocok dijalankan lewat xstartup VNC biasa). Cek kredensial RDP harus dari terminal desktop langsung (`grdctl status --show-credentials`), gagal lewat SSH karena butuh akses `$DISPLAY` X11
- [x] ~~Install cloudflared~~ — sukses, native (bukan Docker), alasan: infrastructure-level, harus tetap hidup independen dari container lain
- [ ] Setup docker-compose: InfluxDB + Mosquitto (MQTT) + Grafana
- [ ] Setup JupyterLab
- [ ] Setup Miniconda + env `livinglab-training`
- [ ] Install VirtualBox/virt-manager untuk VM Windows (MATLAB)
