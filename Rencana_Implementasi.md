# Rencana Implementasi
## Sistem Pakar Identifikasi Jenis Kucing Berdasarkan Ciri Fisik

Sistem pakar ini dirancang untuk mengidentifikasi 3 ras kucing (Anggora, Persia, dan Kampung) berdasarkan ciri fisiknya. Pengguna akan mengunggah gambar kucing, kemudian sistem akan mengekstrak ciri-ciri tersebut dan mencocokkannya dengan basis pengetahuan menggunakan metode **Forward Chaining** dengan pembobotan untuk menentukan jenis rasnya.

---

## Tahapan Implementasi

### Tahap 1: Akuisisi Pengetahuan & Pengumpulan Data (Minggu 1-2)
1. **Studi Literatur**: Mengumpulkan data ciri fisik khas dari ras Anggora, Persia, dan Kampung (misal: bentuk wajah, panjang bulu, bentuk telinga, hidung, warna mata, dll).
2. **Penentuan Basis Aturan (Rule Base)**: Mendefinisikan aturan IF-THEN yang merepresentasikan ciri fisik setiap ras kucing.
3. **Penentuan Bobot Ciri**: Menentukan nilai bobot untuk setiap ciri fisik terhadap masing-masing ras berdasarkan tingkat kepentingannya.
4. **Pengumpulan Dataset Gambar**: Mengumpulkan sampel gambar dari ketiga ras kucing untuk digunakan sebagai data uji.

### Tahap 2: Perancangan Sistem (Minggu 3)
1. **Perancangan Basis Pengetahuan**: Mendefinisikan tabel ciri fisik, nilai bobot, dan aturan inferensi untuk setiap ras.
2. **Perancangan Arsitektur Sistem**: Mendefinisikan alur kerja sistem mulai dari input gambar hingga output hasil identifikasi.
3. **Perancangan Database**: Merancang struktur tabel untuk menyimpan ciri fisik, aturan, bobot, dan riwayat identifikasi.

### Tahap 3: Pengembangan Modul Ekstraksi Ciri (Minggu 4-5)
Karena input berupa gambar, sistem memerlukan modul untuk mengenali ciri fisik dari gambar.
1. **Pemrosesan Citra (Image Processing)**: Menerapkan algoritma untuk mengekstrak ciri fisik (bentuk wajah, panjang bulu, bentuk telinga, dll) dari gambar yang diunggah.
2. **Konversi Fitur**: Mengubah hasil ekstraksi citra menjadi parameter yang dapat diolah oleh mesin inferensi Forward Chaining.
> *Alternatif: Sistem juga bisa menampilkan kuesioner validasi kepada pengguna untuk mengonfirmasi ciri fisik yang kurang jelas dari gambar.*

### Tahap 4: Pembangunan Mesin Inferensi Forward Chaining (Minggu 6-7)
1. **Implementasi Forward Chaining**: Membangun mesin inferensi yang memulai dari fakta (ciri fisik terdeteksi) dan menelusuri aturan (rules) untuk mencapai konklusi (ras kucing).
2. **Perhitungan Bobot**: Mengakumulasi nilai bobot dari setiap ciri yang cocok untuk menghasilkan skor kecocokan per ras.
3. **Penentuan Hasil**: Membuat logika yang memutuskan ras kucing berdasarkan total skor bobot tertinggi dan mengubahnya menjadi persentase keyakinan.

### Tahap 5: Pengembangan Aplikasi & Antarmuka (Minggu 8-9)
1. **Desain UI/UX**: Membuat antarmuka pengguna yang intuitif, mencakup:
   - Halaman beranda (penjelasan sistem)
   - Halaman upload gambar kucing
   - Halaman hasil identifikasi (nama ras + persentase keyakinan)
   - Halaman riwayat identifikasi
2. **Integrasi Backend & Database**: Mengintegrasikan mesin inferensi, modul ekstraksi citra, dan basis data.

### Tahap 5: Pengujian (Minggu 10)
1. **Pengujian Fungsionalitas Dasar**: Memastikan alur utama sistem mulai dari upload gambar hingga menampilkan hasil klasifikasi berjalan dengan baik (tanpa error).
2. **Validasi Hasil**: Menguji sistem menggunakan beberapa sampel gambar untuk memastikan metode *Forward Chaining* memberikan hasil klasifikasi yang sesuai.

---

## Tabel Basis Aturan (Rule Base) — Ciri Fisik Kucing

Berikut adalah contoh tabel bobot ciri fisik yang akan menjadi basis pengetahuan sistem:

| No | Ciri Fisik          | Anggora | Persia | Kampung |
|----|---------------------|---------|--------|---------|
| 1  | Bulu panjang & halus | 0.8     | 0.9    | 0.1     |
| 2  | Bulu pendek         | 0.2     | 0.1    | 0.9     |
| 3  | Wajah bulat pipih   | 0.3     | 0.9    | 0.2     |
| 4  | Wajah oval/panjang  | 0.8     | 0.1    | 0.5     |
| 5  | Hidung pesek        | 0.1     | 0.9    | 0.1     |
| 6  | Telinga runcing     | 0.7     | 0.3    | 0.9     |
| 7  | Telinga kecil & bulat | 0.3   | 0.8    | 0.2     |
| 8  | Mata besar bulat    | 0.6     | 0.9    | 0.4     |
| 9  | Tubuh ramping       | 0.8     | 0.3    | 0.6     |
| 10 | Ekor panjang berbulu | 0.9    | 0.7    | 0.5     |

> Bobot bernilai 0.0 – 1.0 (semakin mendekati 1.0 = semakin kuat korelasinya dengan ras tersebut)
