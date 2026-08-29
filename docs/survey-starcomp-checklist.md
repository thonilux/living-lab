# Checklist Survey ke Starcomp

Tujuan: verifikasi harga & stok fisik komponen racikan PC workstation, sebelum keputusan beli final. Referensi lengkap: [pc-workstation-spec.md](pc-workstation-spec.md), [katalog/starcomp-origin.md](katalog/starcomp-origin.md).

## Bawa sebelum berangkat

- [ ] Print/screenshot daftar komponen racikan final (Skenario A atau C — lihat [pc-workstation-spec.md](pc-workstation-spec.md) bagian "Perbandingan toko")
- [ ] Catatan budget cap: **Rp40.000.000**
- [ ] Alamat cabang yang dituju — cek dulu Solo atau Origin (beda lokasi)

## Cek harga & stok fisik

| # | Komponen | Data online (acuan) | Yang perlu dicek di lokasi |
|---|---|---|---|
| 1 | CPU Ryzen 7 9700X | Rp5.379.000 (Tray, Youngs) / Rp5.675.000 (Box, Starcomp) | Versi Tray (no fan) tersedia fisik? Harga sama? |
| 2 | Mobo ASROCK X870 Challenger WIFI | Rp4.450.000 (Solo) / Rp4.505.000 (Origin) | Stok fisik ada? (sempat ada tanda "last stok" di data lain) |
| 3 | RAM DDR5 32GB kit | Rp7.350.000 (ADATA Lancer Blade, Solo) / Rp7.972.000 (Kingston Fury Beast, Youngs) | Yang fisik ready yang mana? |
| 4 | **GPU RTX 5060 Ti 16GB** | **Tidak ada di katalog/listing kedua cabang Starcomp** | Tanya langsung — mungkin bisa inden/pesan meski ga ada di listing online |
| 5 | Storage ADATA Legend 900 1TB NVMe | Rp3.012.000 (data Youngs, Starcomp cuma ada 512GB) | Cek fisik ada varian 1TB? |
| 6 | PSU 1000W (FSP VITA PM Platinum) | Rp2.250.000 (Solo) | Stok fisik ada? |

## Verifikasi teknis — PALING PENTING

- [ ] **Slot PCIe x16 kedua di mobo X870** — minta tunjukin fisik atau spek sheet resmi. Pastikan slot kedua beneran ada lane (x8/x4 elektrik), bukan cuma slot kosong tanpa jalur data. Ini syarat utama buat requirement dual-VGA (upgrade GPU kedua nanti)
- [ ] Tanya rekomendasi toko: PSU 1000W cukup buat skenario 2 GPU ke depan?

## Administratif (procurement institusi — wajib buat SPJ)

- [ ] Minta **invoice/quotation resmi** per komponen (bukan struk kasir biasa)
- [ ] Tanya apakah toko **PKP** (Pengusaha Kena Pajak) — bisa terbitkan faktur pajak
- [ ] Konfirmasi cara pembayaran, bisa termin/bertahap (CPU+RAM+Mobo dulu, GPU nyusul)?
- [ ] Cek dokumen yang bisa dikasih toko: surat pesanan, surat jalan/bukti pengiriman

Referensi lengkap syarat SPJ: [legal-living-lab-spj-p2m-2026.md](legal-living-lab-spj-p2m-2026.md) — dokumen minimal: invoice/kuitansi, faktur pajak, bukti pembayaran, surat pesanan, surat kesanggupan, surat jalan, foto barang, rincian spesifikasi, BAST BMU.

## Catatan penting

- **Jangan pecah pembelian jadi banyak invoice kecil** cuma buat menghindari ambang pengadaan — itu red flag menurut review legal. Kalau beli bertahap (CPU dulu, GPU nyusul), itu boleh, tapi alasannya harus jelas (nunggu budget/ketersediaan), bukan akal-akalan administratif.
- **Starcomp (Solo maupun Origin) tidak bisa penuhi racikan sendirian** — GPU RTX 5060 Ti dan storage NVMe murah cuma ada di Youngs Computer. Kalau survey ke Starcomp hasilnya emang ga ada GPU, itu sesuai ekspektasi (bukan berarti perlu ganti racikan), tinggal beli GPU+storage dari Youngs terpisah.
- **Harga listing online (Tokopedia) bisa beda dari harga toko fisik** — kadang lebih mahal (ongkir termasuk), kadang beda karena stok lama/baru. Anggap semua angka di atas sebagai starting point negosiasi, bukan harga final.
- Kalau nemu harga fisik lebih murah dari catatan di atas, update balik ke [pc-workstation-spec.md](pc-workstation-spec.md) biar racikan final makin akurat.
