# Legal Notes - Living Lab SPJ P2M UNS 2026

Tanggal: 2026-08-26  
Peran kerja: Tim legal/administrasi proyek living lab  
Sumber utama: Panduan Penyusunan Pertanggungjawaban Anggaran Penelitian dan Pengabdian kepada Masyarakat Tahun 2026, LPPM UNS  
URL sumber: https://lppm.uns.ac.id/wp-content/uploads/2026/06/Digisign-PANDUAN-PERTANGGUNGJAWABAN-ANGGARAN-P2M-TAHUN-2026-Ditandatangani-2026-06-10.pdf

## Aksi Yang Sudah Dilakukan

1. Mengunduh dokumen resmi panduan SPJ P2M UNS 2026 dari laman LPPM UNS.
2. Menyimpan salinan PDF di root workspace sebagai `panduan-spj-p2m-uns-2026.pdf`.
3. Mengekstrak teks PDF ke `panduan-spj-p2m-uns-2026.txt` untuk memudahkan pencarian aturan.
4. Membaca bagian-bagian berisiko untuk living lab: pertanggungjawaban realisasi anggaran, larangan penggunaan dana, honorarium, belanja bahan, pengadaan barang/jasa, pajak, bea meterai, BMU, BAST, dan SPTJM.
5. Meninjau `docs/pc-workstation-spec.md` dan `docs/pc-workstation-proposal.md` sebagai tim legal berdasarkan Panduan SPJ P2M UNS 2026.
6. Menyisipkan catatan legal/SPJ bertanda tangan Codex ke kedua dokumen PC workstation tersebut.
7. Mencatat red flag atas opsi pembelian "satu part satu invoice" sebagai potensi pemecahan paket untuk menghindari ambang pengadaan.
8. Mencatat red flag atas usulan memberi judul BHP/bahan habis pakai untuk belanja komponen PC workstation.

## Catatan Review PC Workstation

PC workstation harus diperlakukan sebagai aset tetap/peralatan yang berpotensi menjadi aset. Jika sumber dana adalah penelitian APBN, pembelian ini berisiko tidak diperbolehkan berdasarkan larangan pembelian alat/peralatan yang berpotensi menjadi aset. Jika sumber dana adalah Non-APBN atau sumber lain yang memperbolehkan belanja aset tetap, pembelian dapat dilanjutkan dengan pencatatan sebagai BMU dan kelengkapan BAST.

Karena estimasi nilai berada di atas Rp10.000.000, pengadaan wajib mengikuti mekanisme pengadaan barang/jasa di atas ambang tersebut. Untuk nilai sampai Rp50.000.000, dokumen harus mengetahui Pejabat Pengadaan Bidang I dan menggunakan penyedia PKP. Jika realisasi melewati Rp50.000.000, proses harus mengikuti mekanisme pengadaan yang lebih lengkap melalui Pejabat Pengadaan/UKPBJ.

Dokumen pembayaran di atas Rp5.000.000 wajib bermeterai Rp10.000. Pembelian tidak boleh dipecah hanya untuk menghindari ambang pengadaan.

### Red Flag: Satu Part Satu Invoice

Muncul pembahasan pembelian satu part satu invoice demi mencari celah aturan. Posisi legal Codex: **tidak direkomendasikan dan harus ditolak** jika substansi pembeliannya adalah satu paket PC workstation.

CPU, motherboard, RAM, GPU, storage, PSU, casing, dan komponen pendukung lain memiliki satu tujuan penggunaan yang sama, yaitu membentuk satu aset workstation. Karena itu, nilai pengadaan harus dinilai berdasarkan total kebutuhan paket, bukan masing-masing invoice komponen. Pemisahan invoice hanya dapat dipertimbangkan jika ada alasan objektif yang sah, misalnya perbedaan waktu kebutuhan, sumber dana berbeda yang sah, vendor eksklusif/garansi tertentu, atau penggantian part terpisah setelah aset berjalan. Alasan tersebut harus ditulis dan didukung bukti, bukan untuk menghindari ambang Rp10.000.000 atau Rp50.000.000.

### Red Flag: Judul BHP Untuk Komponen Workstation

Usulan memberi judul BHP/bahan habis pakai untuk pembelian komponen PC workstation tidak menyelesaikan masalah legal. Dalam SPJ, klasifikasi mengikuti substansi barang, bukan judul dokumen. CPU, motherboard, RAM, GPU, SSD/NVMe, PSU, casing, cooler, monitor, dan periferal utama memiliki umur manfaat dan membentuk aset, sehingga lebih tepat diperlakukan sebagai aset/peralatan atau komponen aset TIK.

Klasifikasi BHP hanya dapat digunakan untuk barang yang benar-benar habis pakai dalam kegiatan dan tidak menjadi aset utama, misalnya thermal paste, kabel kecil tertentu, label, consumable instalasi, atau material pendukung yang wajar. Jika komponen utama workstation dimasukkan sebagai BHP untuk menghindari larangan aset atau ambang pengadaan, risikonya adalah salah klasifikasi belanja dan menjadi temuan audit.

### Risk Assessment: Satu Part Satu Invoice Tokopedia

Skenario: setiap komponen workstation dibeli terpisah melalui Tokopedia, dengan satu part dalam satu invoice.

Posisi legal: skenario ini tetap berisiko tinggi jika semua part tersebut sejak awal direncanakan untuk membentuk satu PC workstation. Marketplace invoice yang terpisah tidak otomatis membuat transaksi menjadi kebutuhan yang terpisah secara substansi. Auditor atau verifikator SPJ dapat melihat pola pembelian dari judul kegiatan, waktu transaksi, daftar barang, tujuan penggunaan, dokumentasi barang, dan hasil akhirnya.

Risiko utama:

- **Pemecahan paket pengadaan:** jika CPU, motherboard, RAM, GPU, SSD, PSU, casing, dan komponen lain dibeli berdekatan untuk satu unit workstation, nilai pengadaan dapat dianggap sebagai nilai total paket.
- **Salah klasifikasi BHP:** part utama komputer bukan bahan habis pakai karena memiliki umur manfaat dan membentuk aset.
- **Menghindari ambang pengadaan:** invoice terpisah dapat dinilai sebagai upaya menghindari prosedur pengadaan di atas Rp10.000.000 atau Rp50.000.000.
- **Masalah BMU:** setelah dirakit menjadi workstation, aset tetapnya tetap perlu dicatat sebagai BMU jika dikuasai UNS.
- **Bukti marketplace tidak cukup untuk menghapus kewajiban pajak/dokumen:** bukti transaksi marketplace dapat dilampirkan, tetapi tetap perlu memperhatikan faktur pajak jika penyedia PKP, kuitansi/dokumen pembayaran, bukti pembayaran, bukti pengiriman, foto/video barang, serta meterai untuk dokumen pembayaran di atas Rp5.000.000.

Skenario terpisah hanya lebih defensible jika ada alasan objektif yang sah dan terdokumentasi, misalnya pembelian terjadi pada waktu berbeda karena kebutuhan penggantian part aset yang sudah ada, part berasal dari sumber dana berbeda yang masing-masing sah, vendor tertentu dibutuhkan untuk garansi/kompatibilitas, atau barang tidak dirakit menjadi satu aset yang sama. Alasan tersebut harus mencerminkan kondisi riil, bukan dibuat untuk melewati aturan.

#### Contoh Skenario: Pembelian CPU

Objek: 1 unit prosesor/CPU AMD Ryzen 7 untuk kebutuhan komputasi living lab.

**Skenario A - CPU sebagai bagian paket workstation baru**

Jika CPU dibeli bersama atau berdekatan dengan motherboard, RAM, GPU, storage, PSU, casing, dan komponen lain untuk membentuk satu PC workstation baru, maka CPU harus diperlakukan sebagai bagian dari paket pengadaan workstation. Nilai pengadaan yang relevan adalah nilai total paket workstation, bukan nilai CPU saja.

Konsekuensi:

- Tidak aman jika CPU dibuat invoice terpisah hanya agar nilainya terlihat di bawah ambang pengadaan.
- Tetap perlu mengikuti mekanisme pengadaan berdasarkan total nilai workstation.
- Setelah dirakit, workstation harus ditentukan status asetnya, terutama pencatatan BMU jika dikuasai UNS.
- Narasi SPJ sebaiknya jujur: "komponen pengadaan workstation living lab" atau "komponen aset TIK workstation", bukan BHP.

**Skenario B - CPU sebagai penggantian/upgrade aset yang sudah ada**

Pembelian CPU terpisah lebih defensible jika benar-benar untuk mengganti atau meng-upgrade PC/workstation/lab server yang sudah ada dan tercatat/teridentifikasi sebelumnya.

Syarat defensible:

- Ada identitas aset/PC lama: nama perangkat, lokasi, penanggung jawab, nomor BMU jika ada, atau minimal dokumentasi aset.
- Ada alasan teknis: CPU lama rusak, bottleneck komputasi, tidak kompatibel dengan beban kerja baru, atau kebutuhan upgrade yang spesifik.
- Ada bukti kondisi sebelum dan sesudah: foto perangkat lama, spesifikasi lama, foto instalasi CPU baru, dan hasil uji fungsi.
- Tidak ada pola pembelian komponen lain dalam waktu berdekatan yang menunjukkan perakitan workstation baru secara terselubung.

Narasi yang lebih aman:

> Pembelian CPU untuk upgrade/penggantian komponen komputasi pada perangkat existing living lab guna mendukung pemrosesan data sensor, server IoT, dan pelatihan model berbasis CPU.

Dokumen minimal:

- Invoice/nota marketplace.
- Bukti pembayaran.
- Bukti pengiriman/resi.
- Foto barang saat diterima.
- Foto pemasangan atau dokumentasi instalasi.
- Spesifikasi teknis CPU.
- Identitas perangkat existing yang di-upgrade.
- Berita acara internal pemasangan/penggantian komponen.
- Faktur pajak jika penyedia PKP atau bukti transaksi marketplace yang memadai jika faktur pajak tidak tersedia.
- Meterai Rp10.000 jika dokumen pembayaran/kuitansi bernilai lebih dari Rp5.000.000.

**Skenario C - CPU sebagai spare part cadangan**

Pembelian CPU sebagai spare part cadangan berisiko sedang sampai tinggi, karena CPU bukan barang habis pakai dan bernilai aset. Skenario ini hanya masuk akal jika ada justifikasi operasional yang kuat, misalnya sistem living lab wajib berjalan terus dan memiliki standar ketersediaan tertentu.

Syarat defensible:

- Ada daftar perangkat yang kompatibel dengan CPU tersebut.
- Ada kebijakan/justifikasi kebutuhan spare part.
- Ada tempat penyimpanan dan penanggung jawab.
- Ada pencatatan barang masuk dan status penggunaannya.

**Kesimpulan untuk CPU**

Pembelian CPU satu invoice Tokopedia tidak otomatis bermasalah jika memang untuk upgrade/penggantian perangkat existing dan bukti teknisnya kuat. Namun jika CPU adalah tahap pertama dari rencana merakit workstation baru, maka secara legal harus diperlakukan sebagai bagian paket workstation dan tidak boleh digunakan untuk menghindari ambang pengadaan.

## Prinsip Kepatuhan Utama

SPJ living lab harus disusun berdasarkan realisasi anggaran sesuai dana yang diterima. Jika ada dana yang tidak terserap, dana tersebut wajib dikembalikan melalui Seksi Umum dan Keuangan LPPM.

Semua transaksi kegiatan P2M wajib mengikuti ketentuan perpajakan yang berlaku. Pajak yang dapat timbul meliputi PPh Pasal 21, PPh Pasal 22, PPh Pasal 23, PPh Final Pasal 4 ayat (2), PPN, dan Bea Meterai, tergantung karakter transaksi, status penyedia atau penerima penghasilan, dan mekanisme pembayaran.

Kuitansi atau dokumen pembayaran dengan nilai nominal lebih dari Rp5.000.000 wajib dikenai Bea Meterai Rp10.000.

SPTJM wajib dibuat dan ditandatangani oleh penerima dana di atas meterai Rp10.000. Dokumen ini menyatakan bahwa seluruh pengeluaran telah dihitung benar dan penerima dana bersedia mengembalikan selisih atau dana tidak terserap kepada universitas atau negara.

## Larangan Dan Batasan Penting

Untuk dana penelitian bersumber APBN, dana tidak diperbolehkan digunakan untuk:

- Pembelian tanah atau lahan.
- Pembelian kendaraan operasional.
- Pembangunan lab baru, gedung, atau kantor.
- Pembelian alat seperti mesin, peralatan laboratorium, atau peralatan lain yang berpotensi menjadi aset.
- Pembelian atau pengadaan alat komunikasi, termasuk pulsa atau paket internet.
- Jaminan dan pinjaman kepada pihak lain.
- Hibah atau bantuan uang tunai kepada pihak lain atau masyarakat.
- Penggunaan lain yang tidak relevan dengan pencapaian target luaran.

Untuk kegiatan pengabdian bersumber APBN, komponen biaya teknologi dan inovasi minimal 50% dari total dana usulan. Komponen ini mencakup teknologi dan inovasi yang diserahkan kepada mitra, termasuk instalasi. Mitra tidak berhak memperoleh upah atau jasa dari komponen biaya ini.

## Honorarium

Untuk dana P2M Non-APBN, honorarium tidak diperbolehkan untuk ketua peneliti, anggota peneliti, tenaga pendidik, maupun tenaga kependidikan. Honorarium hanya diperbolehkan untuk pihak yang memenuhi ketentuan, seperti narasumber luar UNS, pembantu peneliti, koordinator/sekretariat peneliti, pengolah data, petugas survei, dan pembantu lapangan.

Seluruh pembayaran honorarium kepada orang pribadi dikenakan pemotongan PPh Pasal 21. Penerima honorarium wajib menyampaikan NIK/NPWP yang valid dan sesuai dengan data sistem perpajakan agar bukti potong dapat dibuat melalui Coretax atau sistem perpajakan yang berlaku.

## Pengadaan Barang Dan Jasa

Ambang pengadaan yang perlu dijaga dalam RAB dan pelaksanaan:

- Sampai dengan Rp10.000.000: dapat dilakukan oleh pelaksana P2M dengan dokumen pendukung lengkap.
- Di atas Rp10.000.000 sampai Rp50.000.000: harus mengetahui Pejabat Pengadaan Bidang I dan wajib bertransaksi dengan penyedia PKP.
- Di atas Rp50.000.000 sampai Rp500.000.000: harus melalui Pejabat Pengadaan Bidang I Universitas Sebelas Maret dengan pembelian langsung dan dokumen lengkap melalui UKPBJ.
- Di atas Rp500.000.000: harus melalui Pejabat Pembuat Komitmen Bidang I Universitas Sebelas Maret.

Dokumen umum yang perlu disiapkan untuk pengadaan meliputi nota/kuitansi, invoice, faktur pajak jika penyedia PKP, surat pernyataan non-PKP jika relevan, identitas NIK/NPWP untuk orang pribadi, bukti pembayaran, surat pesanan untuk pengadaan di atas ambang tertentu, surat kesanggupan, surat jalan, bukti pengiriman, serta foto atau video barang/jasa.

## BMU, BAST, Dan Barang Untuk Mitra

Barang hasil pembelian kegiatan P2M wajib diidentifikasi sejak awal:

- Jika digunakan dan dikuasai UNS, barang dicatat sebagai Barang Milik Universitas (BMU) dan wajib dikoordinasikan dengan pengelola BMU fakultas/sekolah/unit kerja.
- Jika diserahkan kepada mitra atau masyarakat sebagai luaran kegiatan, barang tidak dicatat sebagai BMU pada fakultas/unit kerja, tetapi wajib dilengkapi BAST kepada mitra/masyarakat, dokumentasi penyerahan, dan data penerima.

Untuk aset tetap yang menjadi BMU, dokumen minimal meliputi bukti pembelian/kuitansi/invoice, rincian barang, spesifikasi, jumlah, nilai perolehan, foto barang, bukti pengisian formulir aset tetap, dan BAST aset tetap sebagai BMU.

## Implikasi Untuk Proyek Living Lab

RAB living lab perlu disusun dengan pemisahan yang jelas antara:

- Komponen teknologi dan inovasi yang diserahkan kepada mitra.
- Operasional kegiatan seperti rapat, konsumsi, dokumentasi, dan perjalanan.
- Honorarium yang diperbolehkan.
- Pengadaan barang/jasa berdasarkan ambang nilai.
- Aset yang menjadi BMU.
- Barang atau teknologi yang diserahkan ke mitra melalui BAST.

Setiap item RAB sebaiknya sudah memiliki prediksi bukti SPJ sejak awal. Item yang tidak jelas bukti, pajak, penyedia, atau status barangnya perlu ditandai sebagai risiko legal sebelum belanja dilakukan.

## Aturan Dokumentasi Kerja Ke Depan

Mulai instruksi ini, setiap aksi substantif pada proyek akan didokumentasikan di folder `docs` dalam berkas Markdown. Untuk kerja legal dan administrasi, catatan utama berada di dokumen ini atau dokumen turunan yang relevan.

Jenis hal yang wajib dicatat:

- Sumber aturan atau referensi yang dipakai.
- Keputusan legal atau administratif.
- Asumsi yang dipakai saat menyusun RAB, kontrak, BAST, atau SPJ.
- Risiko audit dan mitigasinya.
- Perubahan dokumen, struktur, atau artefak proyek.
- Hasil review dan rekomendasi tindak lanjut.
