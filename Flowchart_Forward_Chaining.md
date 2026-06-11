# Flowchart Logika Mesin Inferensi Forward Chaining
## Sistem Pakar Identifikasi Jenis Kucing Berdasarkan Ciri Fisik

Dokumen ini menggambarkan alur logika mesin inferensi menggunakan metode **Forward Chaining** — dimulai dari **fakta awal** (ciri fisik yang terdeteksi dari gambar) kemudian menelusuri **aturan/rules** secara berurutan hingga mencapai **konklusi** (jenis ras kucing).

---

## Flowchart Utama — Alur Sistem Keseluruhan

```mermaid
flowchart TD
    A([Mulai]) --> B[Pengguna Upload Gambar Kucing]
    B --> C[Sistem Memproses & Mengekstrak Ciri Fisik dari Gambar]
    C --> D{Ciri Fisik\nBerhasil Terdeteksi?}

    D -- Tidak --> E[Tampilkan Pesan:\nGambar Tidak Jelas / Bukan Kucing]
    E --> Z([Selesai])

    D -- Ya --> F[Inisialisasi Fakta:\nDaftar Ciri Fisik Terdeteksi]
    F --> G[Muat Basis Aturan & Bobot dari Database]

    G --> H[Inisialisasi Skor:\nAnggora = 0, Persia = 0, Kampung = 0]

    H --> I[Ambil Aturan Berikutnya dari Rule Base]
    I --> J{Kondisi Aturan\nCocok dengan Fakta?}

    J -- Tidak --> K{Masih Ada\nAturan Lain?}
    J -- Ya --> L[Tambahkan Bobot Aturan\nke Skor Ras yang Sesuai]
    L --> K

    K -- Ya --> I
    K -- Tidak --> M[Hitung Persentase Keyakinan\ntiap Ras dari Total Skor]

    M --> N{Skor Tertinggi\n≥ Threshold?}
    N -- Tidak --> O[Tampilkan Hasil:\nRas Tidak Dapat Ditentukan\nPersentase terlalu rendah]
    N -- Ya --> P[Tentukan Ras dengan Skor Tertinggi]

    P --> Q[Tampilkan Hasil Identifikasi:\nNama Ras + Persentase Keyakinan]
    Q --> R[Simpan Riwayat Identifikasi ke Database]
    R --> Z([🔚 Selesai])

    O --> Z
```

---

## Flowchart Detail — Proses Forward Chaining (Pencocokan Aturan)

Menggambarkan secara rinci bagaimana setiap ciri fisik yang terdeteksi dicocokkan dengan aturan dan diakumulasikan ke masing-masing ras.

```mermaid
flowchart TD
    Start([Mulai Forward Chaining]) --> Init

    Init["Inisialisasi:\n• Fakta = Ciri Fisik Terdeteksi\n• Skor_Anggora = 0\n• Skor_Persia = 0\n• Skor_Kampung = 0"]

    Init --> R1

    subgraph FC["Mesin Inferensi Forward Chaining"]
        R1["Evaluasi Aturan:\nIF bulu_panjang = true"]
        R1 --> R1C{Cocok?}
        R1C -- Ya --> R1A["Skor_Anggora += 0.8\nSkor_Persia  += 0.9"]
        R1C -- Tidak --> R2

        R1A --> R2["Evaluasi Aturan:\nIF wajah_bulat_pipih = true"]
        R2 --> R2C{Cocok?}
        R2C -- Ya --> R2A["Skor_Persia  += 0.9\nSkor_Anggora += 0.3"]
        R2C -- Tidak --> R3

        R2A --> R3["Evaluasi Aturan:\nIF hidung_pesek = true"]
        R3 --> R3C{Cocok?}
        R3C -- Ya --> R3A["Skor_Persia  += 0.9"]
        R3C -- Tidak --> R4

        R3A --> R4["Evaluasi Aturan:\nIF telinga_runcing = true"]
        R4 --> R4C{Cocok?}
        R4C -- Ya --> R4A["Skor_Anggora  += 0.7\nSkor_Kampung += 0.9"]
        R4C -- Tidak --> R5

        R4A --> R5["Evaluasi Aturan:\nIF tubuh_ramping = true"]
        R5 --> R5C{Cocok?}
        R5C -- Ya --> R5A["Skor_Anggora += 0.8\nSkor_Kampung += 0.6"]
        R5C -- Tidak --> R6

        R5A --> R6["Evaluasi Aturan:\nIF bulu_pendek = true"]
        R6 --> R6C{Cocok?}
        R6C -- Ya --> R6A["Skor_Kampung += 0.9\nSkor_Anggora += 0.2"]
        R6C -- Tidak --> Kalkulasi

        R6A --> Kalkulasi
    end

    Kalkulasi["Hitung Total Skor:\nTotal = Skor_Anggora + Skor_Persia + Skor_Kampung\n\n% Anggora = Skor_Anggora / Total × 100\n% Persia  = Skor_Persia  / Total × 100\n% Kampung = Skor_Kampung / Total × 100"]

    Kalkulasi --> Konklusi{"Tentukan Konklusi:\nRas mana yang\nnilainya tertinggi?"}

    Konklusi -- "Anggora Tertinggi" --> HA["Konklusi: Kucing Anggora\n(disertai % keyakinan)"]
    Konklusi -- "Persia Tertinggi" --> HP["Konklusi: Kucing Persia\n(disertai % keyakinan)"]
    Konklusi -- "Kampung Tertinggi" --> HK["Konklusi: Kucing Kampung\n(disertai % keyakinan)"]

    HA --> End([Selesai])
    HP --> End
    HK --> End
```

---

## Contoh Simulasi Forward Chaining

Misalkan ciri fisik yang terdeteksi dari gambar adalah:
- ✅ Bulu panjang
- ✅ Wajah oval/panjang
- ✅ Telinga runcing
- ✅ Tubuh ramping
- ❌ Hidung pesek
- ❌ Bulu pendek

| Aturan yang Cocok | Skor Anggora | Skor Persia | Skor Kampung |
|-------------------|:---:|:---:|:---:|
| bulu_panjang = true | +0.8 | +0.9 | — |
| wajah_oval = true | +0.8 | — | +0.5 |
| telinga_runcing = true | +0.7 | — | +0.9 |
| tubuh_ramping = true | +0.8 | — | +0.6 |
| **Total Skor** | **3.1** | **0.9** | **2.0** |
| **Persentase** | **51.7%** | **15.0%** | **33.3%** |

> **Konklusi:** Kucing teridentifikasi sebagai **Anggora** dengan keyakinan **51.7%**
