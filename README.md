# K-Means Clustering pada Data Penindakan KPK 2019–2025

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dimasdarmakara/kmeans-penindakan-kpk/blob/main/kmeans_penindakan_kpk.ipynb)

Kode dan data pendamping artikel:

> **Penerapan Algoritma K-Means Clustering dengan Scikit-Learn pada Data Penindakan KPK untuk Pemetaan Wilayah dan Sektor Rawan Korupsi**

Repositori ini berisi seluruh langkah analisis: data perkara per tahun, pembentukan atribut, normalisasi Min-Max, penentuan jumlah klaster (DBI, SSE/Elbow, Silhouette), pengelompokan K-Means, dan visualisasi hasil.

## Isi repositori

| File / folder | Isi |
|---|---|
| `kmeans_penindakan_kpk.ipynb` | Notebook Python berisi seluruh analisis |
| `data/penindakan_kpk_2019_2025.csv` | Jumlah perkara tahap penyidikan per tahun (2019–2025) menurut wilayah, instansi, dan jenis perkara |
| `hasil/` | Gambar hasil analisis |
| `requirements.txt` | Daftar pustaka Python yang dipakai |

## Sumber data

Komisi Pemberantasan Korupsi (KPK), *Statistik Penindakan* (per Maret 2026, TW I), tabel TPK (tindak pidana korupsi) berdasarkan wilayah, instansi, dan jenis perkara. Diakses 7 Oktober 2026 dari https://www.kpk.go.id/id/publikasi-data/statistik/penindakan-2

Sel kosong pada tabel KPK ditulis 0. Tahun 2026 tidak dipakai karena baru mencakup triwulan I. Kategori Malaysia dan Singapura dikeluarkan karena tidak memiliki perkara pada 2019–2025.

## Cara menjalankan

**Google Colab (tanpa instalasi):** klik tombol **Open in Colab** di atas, lalu jalankan *Runtime → Run all*. Data sudah tertanam di dalam notebook, jadi tidak perlu mengunggah file.

**Komputer sendiri:**

```bash
pip install -r requirements.txt
jupyter notebook kmeans_penindakan_kpk.ipynb
```

## Metode singkat

1. Setiap unit diringkas menjadi empat atribut dari deret 2019–2025: **Total**, **Maks**, **SD** (simpangan baku populasi), dan **Tren** (kemiringan regresi linear terhadap tahun).
2. Atribut dinormalisasi dengan `MinMaxScaler` ke rentang 0–1.
3. `KMeans` dijalankan dengan jarak Euclidean, inisialisasi k-means++, `n_init = 50`, dan `random_state = 42`.
4. Jumlah klaster diuji pada k = 2 sampai ⌊n/2⌋ (maksimum 4) dan dipilih berdasarkan DBI terendah (`davies_bouldin_score`), dengan SSE dan `silhouette_score` sebagai pembanding.

## Ringkasan hasil

| Data | n | k terpilih | DBI | Anggota klaster |
|---|---|---|---|---|
| Wilayah | 9 | 4 | 0,110 | Pemerintah Pusat · Jawa · Sumatera · enam wilayah lain |
| Instansi | 6 | 3 | 0,388 | Pemkab/Pemkot · Kementerian/Lembaga + BUMN/BUMD · Pemprov + DPR RI + Komisi |
| Jenis perkara | 7 | 3 | 0,144 | Gratifikasi/Penyuapan · Pengadaan Barang/Jasa · lima jenis lainnya |

![Hasil klaster](hasil/03_hasil_klaster.png)

Hasil ini menggambarkan **pola penindakan KPK**, bukan tingkat korupsi sebenarnya di suatu daerah, karena data hanya mencakup perkara yang ditangani KPK.

## Lingkungan

Diuji dengan Python 3.13, numpy 2.5, pandas 3.0, scikit-learn 1.9, dan matplotlib 3.11.
