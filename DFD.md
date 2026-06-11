# Data Flow Diagram (DFD)
## Sistem Pakar Identifikasi Jenis Kucing Berdasarkan Ciri Fisik

Dokumen ini berisi perancangan aliran data sistem pakar menggunakan **MermaidJS** dalam format DFD (Data Flow Diagram) Level 0 hingga Level 2.

---

## DFD Level 0 — Context Diagram

Menggambarkan sistem secara keseluruhan sebagai satu proses tunggal beserta entitas-entitas luar yang berinteraksi dengannya.

```mermaid
graph TD
    User(["Pengguna"])
    Admin(["Pakar / Admin"])
    Sistem(("Sistem Pakar\nIdentifikasi Kucing"))

    User -- "Upload Gambar Kucing" --> Sistem
    Sistem -- "Hasil Identifikasi Ras & Persentase Keyakinan" --> User

    Admin -- "Input Data Ciri Fisik, Aturan & Bobot" --> Sistem
    Sistem -- "Laporan Riwayat Identifikasi" --> Admin
```

---

## DFD Level 1 — Diagram Aliran Proses Utama

Memecah sistem menjadi 4 proses utama beserta penyimpanan data (Data Store) yang terlibat.

```mermaid
graph TD
    %% === Entitas Luar ===
    User(["Pengguna"])
    Admin(["Pakar / Admin"])

    %% === Proses Utama ===
    P1(("1.0\nKelola Basis\nPengetahuan"))
    P2(("2.0\nProses Gambar\n& Ekstraksi Ciri"))
    P3(("3.0\nInferensi\nForward Chaining"))
    P4(("4.0\nKelola Hasil\nIdentifikasi"))

    %% === Data Store ===
    D1[("D1\nData Ciri Fisik")]
    D2[("D2\nData Aturan & Bobot")]
    D3[("D3\nRiwayat Identifikasi (DB)")]
    D4[("D4\nLocalStorage (Browser)")]

    %% === Aliran Data: Admin → Kelola Pengetahuan ===
    Admin -- "Input/Update Ciri & Bobot" --> P1
    P1 -- "Simpan Data Ciri" --> D1
    P1 -- "Simpan Aturan & Bobot" --> D2

    %% === Aliran Data: Pengguna → Ekstraksi ===
    User -- "Upload Gambar Kucing" --> P2
    P2 -- "Parameter Ciri Fisik Terdeteksi" --> P3

    %% === Aliran Data: Inferensi ===
    D1 -- "Ambil Data Ciri Fisik" --> P3
    D2 -- "Ambil Nilai Bobot & Aturan" --> P3
    P3 -- "Hasil Skor Klasifikasi Ras" --> P4

    %% === Aliran Data: Hasil ===
    P4 -- "Simpan Riwayat (Jika Login)" --> D3
    P4 -- "Simpan Riwayat (Jika Guest)" --> D4
    P4 -- "Tampilkan Nama Ras & Persentase" --> User
    D3 -- "Rekap Data Identifikasi" --> Admin
```

---

## DFD Level 2 — Detail Proses 3.0: Inferensi Forward Chaining

Memecah Proses 3.0 menjadi langkah-langkah detail yang terjadi di dalam mesin inferensi Forward Chaining.

```mermaid
graph TD
    %% Input dari proses sebelumnya
    InputCiri["Parameter Ciri Fisik Terdeteksi\n(dari Proses 2.0)"]
    InputBobot[("D2: Data Aturan & Bobot")]

    %% Sub-proses Forward Chaining
    P3a(("3.1\nInisialisasi\nFakta Awal"))
    P3b(("3.2\nPencocokan Fakta\ndengan Aturan / Rules"))
    P3c(("3.3\nAkumulasi\nBobot per Ras"))
    P3d(("3.4\nPenentuan Ras\nSkor Tertinggi"))
    P3e(("3.5\nKonversi ke\nPersentase Keyakinan"))

    %% Output ke proses berikutnya
    OutputHasil["Hasil Klasifikasi Ras\n(ke Proses 4.0)"]

    %% Aliran
    InputCiri --> P3a
    InputBobot --> P3b
    P3a -- "Daftar Fakta (Ciri Terdeteksi)" --> P3b
    P3b -- "Fakta Cocok + Nilai Bobot" --> P3c
    P3c -- "Total Skor: Anggora / Persia / Kampung" --> P3d
    P3d -- "Ras dengan Skor Tertinggi" --> P3e
    P3e --> OutputHasil
```

---

## Keterangan Simbol DFD

| Simbol | Keterangan |
|--------|-----------|
| `([...])` | **Entitas Luar** — Pengguna atau Admin yang berinteraksi dengan sistem |
| `((...))`  | **Proses** — Aktivitas/fungsi yang mengolah data |
| `[(...)]`  | **Data Store** — Tempat penyimpanan data (database/tabel/local storage) |
| `-->`      | **Aliran Data** — Arah perpindahan data antar elemen |

---

## Ringkasan Proses

| No | Proses | Keterangan |
|----|--------|-----------|
| 1.0 | Kelola Basis Pengetahuan | Admin mengelola ciri fisik, aturan, dan nilai bobot |
| 2.0 | Proses Gambar & Ekstraksi Ciri | Sistem menganalisis gambar dan mengekstrak ciri fisik kucing |
| 3.0 | Inferensi Forward Chaining | Mencocokkan ciri dengan aturan dan menghitung skor kecocokan per ras |
| 4.0 | Kelola Hasil Identifikasi | Menyajikan hasil ras dan persentase, serta menyimpan riwayat ke DB `D3` (jika login) atau LocalStorage `D4` (jika guest) |
