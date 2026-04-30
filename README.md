# 🛡️ WomanGuard
### Indeks Kerentanan Perempuan Berbasis Feminisasi Kemiskinan dan Pemberdayaan Ekonomi
**Sebuah proyek berbasis data untuk membangun Indeks WomanGuard dalam mengukur kerentanan perempuan di berbagai kecamatan di Surabaya menggunakan PCA dan analisis statistik.
**

---

## 📖 Tentang Proyek

**WomanGuard** adalah sistem indeks komposit yang dirancang untuk mengukur dan memetakan tingkat kerentanan perempuan di setiap kecamatan di Kota Surabaya. Indeks ini dibangun dari dua dimensi utama:

- 🔴 **Feminisasi Kemiskinan** — sejauh mana perempuan lebih terdampak kemiskinan dibandingkan laki-laki
- 🟢 **Pemberdayaan Ekonomi Perempuan** — akses dan partisipasi perempuan dalam aktivitas ekonomi dan pendidikan

Hasil akhir berupa **peta prioritas pembangunan perempuan** di 31 kecamatan Kota Surabaya yang dapat digunakan sebagai acuan intervensi kebijakan berbasis data gender.

---

## 🗂️ Struktur Repositori

```
womanguard-index-surabaya/
├── Data/
│   ├── (X1) Persentase kepala keluarga berstatus cerai hidup per kecamatan/
│   ├── (X2) Persentase keluarga miskin/
│   ├── (X3) Persentase kepala keluarga perempuan/
│   ├── (X4) Persentase kepala keluarga dengan pendidikan tidakbelum tamat SD/
│   ├── (X5) Persentase kepala keluarga perempuan yang bekerja/
│   ├── (X6) Persentase perempuan berpendidikan SMA ke atas/
│   └── df_cleaned.csv
├── Plot/
│   ├── Woman_Guard_Index.png
│   ├── Clustering_Results.png
│   ├── Regression_Visualization.png
│   └── Correlation_Comparison.png
├── README.md
├── requirements.txt
└── WomanGuard.ipynb
```

---

## 📊 Variabel yang Digunakan

| Kode | Variabel | Dimensi | Arah terhadap Kerentanan |
|------|----------|---------|--------------------------|
| **X1** | Persentase kepala keluarga berstatus cerai hidup | Feminisasi Kemiskinan | ⬆️ Positif |
| **X2** | Persentase keluarga miskin | Feminisasi Kemiskinan | ⬆️ Positif |
| **X3** | Persentase kepala keluarga perempuan | Feminisasi Kemiskinan | ⬆️ Positif |
| **X4** | Persentase kepala keluarga dengan pendidikan tidak/belum tamat SD | Feminisasi Kemiskinan | ⬆️ Positif |
| **X5** | Persentase kepala keluarga perempuan yang bekerja | Pemberdayaan Ekonomi | ⬇️ Negatif |
| **X6** | Persentase perempuan berpendidikan SMA ke atas | Pemberdayaan Ekonomi | ⬇️ Negatif |

> **Sumber Data:** Dinas Sosial, Dinas Kependudukan dan Pencatatan Sipil, dan BPS Kota Surabaya (2024–2025)

---

## 🔄 Alur Metodologi

```
Load Data Mentah (X1–X6)
         │
         ▼
Preprocessing & Kalkulasi Persentase per Kecamatan
         │
         ▼
Merge → DataFrame Gabungan (31 Kecamatan × 6 Variabel)
         │
         ▼
Deteksi & Penanganan Outlier (IQR Capping)
         │
         ▼
Standardisasi (StandardScaler) + PCA pada X1–X4
         │
         ▼
Perhitungan Woman Guard Index (WGI)
WGI = w₁×PC1 + w₂×PC2
         │
         ▼
K-Means Clustering (k=3)
→ Rentan Tinggi / Rentan Sedang / Rentan Rendah
         │
         ▼
Regresi Linear: X5, X6 → WGI
         │
         ▼
Analisis Korelasi & Uji VIF
```

---

## ⚙️ Metode Analisis

### 1. Principal Component Analysis (PCA)
PCA digunakan untuk merangkum 4 variabel feminisasi kemiskinan (X1–X4) menjadi komponen utama yang mempertahankan varians terbesar. Hanya PC1 dan PC2 yang digunakan dalam pembentukan WGI.

### 2. Woman Guard Index (WGI)
WGI dihitung sebagai kombinasi berbobot dari PC1 dan PC2, di mana bobot proporsional terhadap *explained variance* masing-masing komponen:

$$WGI = \frac{Var_{PC1}}{Var_{PC1}+Var_{PC2}} \times PC1 + \frac{Var_{PC2}}{Var_{PC1}+Var_{PC2}} \times PC2$$

| Nilai WGI | Interpretasi |
|-----------|-------------|
| **Positif** | Kerentanan di atas rata-rata kota |
| **Negatif** | Kerentanan di bawah rata-rata kota |

### 3. K-Means Clustering
Kecamatan dikelompokkan ke dalam 3 cluster berdasarkan posisi PC1 dan PC2. Label cluster ditentukan otomatis berdasarkan rata-rata WGI per cluster:
- 🔴 **Rentan Tinggi** — prioritas intervensi utama
- 🟡 **Rentan Sedang** — perlu pemantauan dan program preventif
- 🟢 **Rentan Rendah** — kondisi relatif lebih baik

### 4. Regresi Linear
Menguji pengaruh variabel pemberdayaan (X5, X6) terhadap WGI:

$$WGI = \beta_0 + \beta_1 \cdot X5 + \beta_2 \cdot X6 + \varepsilon$$

### 5. Uji Multikolinearitas (VIF)
Memastikan X5 dan X6 tidak saling berkorelasi berlebihan sebelum hasil regresi diinterpretasikan.

---

## 🛠️ Teknologi yang Digunakan

![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-2.x-150458?logo=pandas&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-1.x-F7931E?logo=scikit-learn&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-statistical%20viz-4C72B0)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

| Library | Versi | Kegunaan |
|---------|-------|----------|
| `pandas` | ≥ 2.0 | Manipulasi data tabular |
| `numpy` | ≥ 1.24 | Operasi numerik |
| `scikit-learn` | ≥ 1.3 | PCA, KMeans, LinearRegression, StandardScaler |
| `matplotlib` | ≥ 3.7 | Visualisasi grafik |
| `seaborn` | ≥ 0.12 | Visualisasi statistik (heatmap, boxplot) |
| `statsmodels` | ≥ 0.14 | Uji VIF multikolinearitas |

---

## 🚀 Cara Menjalankan

### 1. Clone repositori
```bash
git clone https://github.com/username/WomanGuard.git
cd WomanGuard
```

### 2. Install dependensi
```bash
pip install pandas numpy scikit-learn matplotlib seaborn statsmodels openpyxl
```

### 3. Siapkan data
Tempatkan seluruh file Excel sumber data ke dalam folder `Data/` sesuai struktur repositori di atas.

### 4. Jalankan notebook
```bash
jupyter notebook WomanGuard.ipynb
```

> ⚠️ **Catatan:** Sesuaikan variabel `home_path_x1` hingga `home_path_x6` di cell *Load Data* dengan path lokal Anda sebelum menjalankan notebook.

### 5. Gunakan data yang sudah dibersihkan (opsional)
Jika ingin langsung menggunakan data hasil preprocessing tanpa menjalankan ulang dari awal, file `df_cleaned.csv` sudah tersedia dan berisi variabel X1–X6 yang siap dianalisis.

```python
import pandas as pd
df = pd.read_csv("df_cleaned.csv")
df.head()
```

---

## 📁 Format Data Input

Setiap file Excel data mentah mengikuti format ekspor dari sistem administrasi kependudukan Kota Surabaya. Kolom kunci yang dibutuhkan per variabel:

| Variabel | Kolom Kunci yang Diperlukan |
|----------|----------------------------|
| X1 & X3 | `KECAMATAN/KELURAHAN`, `WILAYAH`, `CERAI_HIDUP_L`, `CERAI_HIDUP_P`, kolom jumlah per status perkawinan |
| X2 | `Kecamatan`, `Jumlah (KK)` (miskin), `Jumlah KK` (total) |
| X4 | `KECAMATAN/KELURAHAN`, `WILAYAH`, kolom jumlah per jenjang pendidikan (L & P) |
| X5 | `NAMA KECAMATAN`, `NAMA KELURAHAN`, kolom jenis pekerjaan perempuan |
| X6 | `Kecamatan\nDistrict`, kolom jumlah perempuan per jenjang pendidikan |

---

## 📈 Output & Visualisasi

| Output | Deskripsi |
|--------|-----------|
| `Woman_Guard_Index.png` | Bar chart WGI per kecamatan + scatter PC1 vs PC2 |
| `Clustering_Results.png` | Peta cluster K-Means dalam ruang PCA |
| `Regression_Visualization.png` | Plot regresi X5/X6 terhadap WGI + actual vs predicted |
| `Correlation_Comparison.png` | Heatmap korelasi X1–X6 dengan WGI |

---

## 🎯 Potensi Pengguna

- **Dinas Pemberdayaan Perempuan dan Perlindungan Anak (DP3A) Kota Surabaya** — alokasi program intervensi berbasis prioritas
- **Bappeko Surabaya** — perencanaan pembangunan berbasis data gender
- **Akademisi & Peneliti** — referensi indeks kerentanan berbasis feminisasi kemiskinan
- **LSM & Organisasi Masyarakat Sipil** — advokasi kebijakan perempuan berbasis bukti

---

## 👩‍💻 Kontributor

Proyek ini dikembangkan sebagai bagian dari riset pemetaan sosial berbasis data di Kota Surabaya.

---

## 📄 Lisensi

Repositori ini menggunakan lisensi [MIT](LICENSE). Data yang digunakan bersumber dari instansi pemerintah Kota Surabaya dan bersifat publik.

---

<div align="center">
  <i>WomanGuard — Memastikan setiap perempuan di Surabaya mendapat perlindungan dan kesempatan yang layak.</i>
</div>
