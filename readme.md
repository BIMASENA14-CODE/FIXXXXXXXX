# Analisis Sentimen Publik terhadap Cuaca dan Kualitas Udara Jakarta 2023

## Deskripsi Proyek

Proyek ini menganalisis respons publik terhadap kondisi cuaca dan kualitas udara di Jakarta tahun 2023 melalui komentar YouTube. Data dari berbagai sumber diintegrasikan untuk memahami hubungan antara kondisi lingkungan dan reaksi masyarakat.

## Tujuan

- Mengumpulkan data komentar YouTube terkait cuaca dan kualitas udara Jakarta
- Mengintegrasikan data cuaca, kualitas udara, dan sentimen publik
- Menganalisis korelasi antara kondisi lingkungan dan volume respons publik
- Mengidentifikasi pola musiman dan anomali dalam data

## Sumber Data

1. **Komentar YouTube**: Scraping dari video dengan kata kunci terkait polusi, cuaca, dan banjir Jakarta
2. **Data Cuaca**: [Dataset Cuaca Jakarta 2019-2024](https://www.kelasai.id/dataset/dataset-cuaca-jakarta-2019-2024)
3. **Data Kualitas Udara**: [Dataset Kualitas Udara 2010-2025](https://www.kaggle.com/datasets/senadu34/air-quality-index-in-jakarta-2010-2021)

## Teknologi yang Digunakan

- **Python 3.x**
- **Libraries**:
  - `pandas` - manipulasi dan analisis data
  - `requests` - API calls ke YouTube
  - `matplotlib` & `seaborn` - visualisasi data
  - `numpy` - operasi numerik
  - `re` - regular expressions untuk text cleaning

## Struktur Project

```
project/
│
├── data/
│   ├── dataset_prediksi_cuaca.xlsx
│   ├── ispu_dki4.csv
│   └── komentar_jakarta_2023_debug.csv
│
├── data_clean/
│   ├── data_sentimen_bersih.csv
│   ├── data_cuaca_jakarta_2023_bersih.csv
│   ├── kualitas_udara_jakarta_2023_bersih.csv
│   └── integrasi_cuaca_udara_sentimen_2023.csv
│
└── main code.ipynb
```

## Cara Menggunakan

### 1. Persiapan

```bash
# Install dependencies
pip install pandas requests matplotlib seaborn numpy openpyxl
```

### 2. Konfigurasi API Key

Ganti `API_KEY` di cell pertama dengan YouTube Data API v3 key Anda:

```python
API_KEY = 'YOUR_YOUTUBE_API_KEY_HERE'
```

### 3. Jalankan Notebook

Jalankan cell secara berurutan:

1. **Cell 1**: Scraping komentar YouTube
2. **Cell 2**: Cleaning data sentimen
3. **Cell 3**: Cleaning data cuaca
4. **Cell 4**: Cleaning data kualitas udara
5. **Cell 5**: Integrasi semua data
6. **Cell 6-9**: Analisis dan visualisasi

## Detail Proses

### 1. Scraping Komentar YouTube

**Kata Kunci Pencarian**:
- polusi udara jakarta, kualitas udara jakarta, cuaca jakarta
- banjir jakarta, debu jakarta, asap jakarta, kabut jakarta
- hujan jakarta, banjir jkt, polusi jkt

**Filter Komentar**:
- Tahun: 2023
- Keyword: banjir, cuaca, polusi, debu, asap, kabut, dll.

**Fitur**:
- Retry mechanism dengan backoff
- Pagination handling
- Error handling untuk API quota

### 2. Data Cleaning

#### Data Sentimen
- Menghapus karakter khusus
- Normalisasi spasi
- Deduplication berdasarkan tanggal dan komentar
- Grouping per tanggal

#### Data Cuaca
- Konversi tipe data numerik
- Interpolasi nilai kosong
- Mapping kategori cuaca ke nilai numerik:
  - Cerah: 0
  - Cerah Berawan: 2
  - Hujan Ringan: 1
  - Hujan Sedang: -1
  - Hujan Lebat: -2

#### Data Kualitas Udara
- Normalisasi format angka (koma ke titik)
- Interpolasi linear untuk missing values
- Mapping kategori ISPU:
  - Baik: 3
  - Sedang: 2
  - Tidak Sehat: 1
  - Sangat Tidak Sehat/Tidak Ada Data: 0

### 3. Integrasi Data

Penggabungan berdasarkan kolom `tanggal` menggunakan `left join`:
- Data cuaca (master)
- Data kualitas udara
- Data sentimen

Hasil: Dataset terintegrasi dengan informasi lengkap per hari.

### 4. Analisis & Visualisasi

#### Analisis yang Dilakukan:

1. **Statistik Deskriptif**
   - Mean, median, std deviation
   - Min/max values
   - Distribution plots

2. **Trend Analysis**
   - Tren harian (suhu, PM2.5, volume komentar)
   - Normalisasi untuk perbandingan
   - Identifikasi pola temporal

3. **Outlier Detection**
   - Metode IQR (Interquartile Range)
   - Deteksi anomali pada:
     - Suhu maksimal
     - Curah hujan
     - PM2.5
     - Volume komentar

4. **Correlation Analysis**
   - Heatmap korelasi antar variabel
   - Identifikasi hubungan signifikan

5. **Seasonal Analysis**
   - Trend bulanan
   - Perbandingan musim hujan vs kemarau
   - Boxplot distribusi per bulan

## Variabel dalam Dataset Final

### Cuaca
- Suhu Maks/Min (°C)
- Kelembaban (%)
- Kecepatan & Arah Angin
- Tekanan Udara (hPa)
- Tutupan Awan (%)
- Curah Hujan (mm)
- Cuaca Hari Ini/Besok (kategori)

### Kualitas Udara
- PM2.5, PM10
- SO2, CO, O3, NO2
- Kategori ISPU

### Sentimen
- Komentar per tanggal (sentimen_1, sentimen_2, ...)
- Total sentimen (jumlah komentar)

## Insights yang Dapat Diperoleh

1. **Korelasi Lingkungan-Sentimen**: Apakah kondisi cuaca/polusi buruk meningkatkan volume komentar?
2. **Pola Musiman**: Bulan mana yang memiliki respons publik tertinggi?
3. **Event Detection**: Identifikasi hari-hari dengan lonjakan komentar abnormal
4. **Environmental Triggers**: Variabel lingkungan mana yang paling mempengaruhi respons publik?

## Catatan Penting

1. **API Quota**: YouTube API memiliki limit harian, gunakan dengan bijak
2. **Rate Limiting**: Script sudah include retry mechanism
3. **Data Privacy**: Komentar publik diambil sesuai terms of service YouTube
4. **Data Quality**: Interpolasi digunakan untuk missing values, pertimbangkan implikasinya

## Contoh Output

Dataset final berisi ~365 baris (satu tahun) dengan kolom:
- 1 kolom tanggal
- 15 kolom cuaca/meteorologi
- 7 kolom kualitas udara
- N kolom sentimen (dinamis)
- 1 kolom total_sentimen

## Kontribusi

Proyek ini terbuka untuk improvement:
- Tambahkan sumber data lain (Twitter, berita online)
- Implementasi sentiment analysis (positif/negatif/netral)
- Machine learning untuk prediksi
- Dashboard interaktif

## Lisensi
Licensed under the [MIT](./LICENSE).

Gunakan sesuai kebutuhan penelitian/pembelajaran. Pastikan mematuhi terms of service platform sumber data.
