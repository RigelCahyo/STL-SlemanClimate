# STL-SlemanClimate
Trend and seasonal pattern analysis of temperature and humidity in Sleman Regency 2024 using STL (Seasonal Trend Decomposition using LOESS)

> Analisis deret waktu (time series) untuk mengidentifikasi tren dan pola musiman suhu serta kelembapan udara harian di Kabupaten Sleman tahun 2024 menggunakan metode STL (Seasonal Trend Decomposition using LOESS). Studi kasus: Stasiun Klimatologi Yogyakarta.

## Deskripsi Proyek

Perubahan iklim dan variabilitas cuaca menjadi isu yang semakin relevan di Kabupaten Sleman, Daerah Istimewa Yogyakarta. Karakteristik geografis wilayah ini kompleks, mulai dari dataran rendah hingga kawasan pegunungan di sekitar Gunung Merapi, sehingga menciptakan variasi mikroklimat yang dinamis. Suhu dan kelembapan udara merupakan dua parameter utama dalam studi iklim yang memengaruhi sektor pertanian, kesehatan, serta kehidupan sosial ekonomi masyarakat.

Proyek ini merupakan Laporan Kerja Praktik yang menganalisis tren dan pola musiman suhu serta kelembapan udara di Kabupaten Sleman sepanjang tahun 2024, menggunakan data harian dari Stasiun Klimatologi Yogyakarta (BMKG). Metode utama yang digunakan adalah STL (Seasonal Trend Decomposition using LOESS), yang memisahkan data deret waktu menjadi tiga komponen utama: tren jangka panjang, pola musiman, dan residual (fluktuasi acak). Evaluasi dilakukan melalui visualisasi, uji Ljung Box terhadap residual, serta perhitungan kekuatan komponen tren dan musiman.

Proyek ini merupakan Laporan Kerja Praktik, Program Studi Sains Data, Fakultas Sains & Teknologi, Universitas Teknologi Yogyakarta (2025).

Rumusan masalah:

1. Bagaimana tren perubahan suhu dan kelembapan udara di Kabupaten Sleman sepanjang tahun 2024?
2. Sejauh mana metode STL dapat mengidentifikasi dan memisahkan komponen tren, musiman, dan residual dari data iklim tersebut?
3. Apa informasi yang dapat diperoleh dari hasil dekomposisi data suhu dan kelembapan terhadap perencanaan dan pemantauan kondisi iklim lokal?

## Dataset

- Sumber: Stasiun Klimatologi Yogyakarta, Badan Meteorologi, Klimatologi, dan Geofisika (BMKG), diperoleh melalui pengajuan resmi dan situs dataonline.bmkg.go.id.
- Jumlah data: 366 entri harian sepanjang tahun 2024.
- Variabel yang digunakan: TAVG (temperatur rata rata harian, °C) dan RH_AVG (kelembapan udara rata rata harian, %).
- Kolom lain pada berkas asli (TN, TX, RR, SS, FF_X, DDD_X, FF_AVG, DDD_CAR) tidak digunakan dalam analisis ini dan dihapus pada tahap preprocessing.
- Rentang nilai: suhu rata rata berkisar antara 25°C hingga 30°C, kelembapan udara berkisar antara 65% hingga hampir 100%.
- Terdapat 1 nilai kosong pada atribut suhu dan kelembapan, ditangani menggunakan interpolasi linier.

## Metodologi

1. Studi literatur mengenai metode time series decomposition dan penelitian iklim terdahulu.
2. Pengumpulan data harian dari Stasiun Klimatologi Yogyakarta.
3. Preprocessing data: konversi format tanggal menjadi datetime index, konversi tipe data menjadi float, penanganan nilai kosong dengan interpolasi linier.
4. Exploratory Data Analysis (EDA): visualisasi tren awal, plot Autocorrelation Function (ACF), dan boxplot untuk deteksi outlier.
5. Pemodelan menggunakan metode STL (Seasonal Trend Decomposition using LOESS) dengan parameter period 30 (pola bulanan) dan robust bernilai True.
6. Evaluasi model melalui visualisasi komponen, plot ACF dan PACF residual, serta uji Ljung Box.
7. Interpretasi hasil dekomposisi (tren, musiman, residual) untuk masing masing variabel.

## Model dan Evaluasi

Model: STL (Seasonal Trend Decomposition using LOESS) dari pustaka statsmodels, dengan period 30 dan robust bernilai True, diterapkan secara terpisah pada data suhu dan data kelembapan.

| Metrik | Suhu | Kelembapan |
|---|---|---|
| Kekuatan Tren | 0.642 | 0.425 |
| Kekuatan Musiman | 0.191 | 0.188 |
| Ljung Box p value (lag 10) | 8.86e 05 | 5.56e 05 |

Nilai p yang jauh lebih kecil dari 0.05 pada kedua variabel menunjukkan bahwa residual masih mengandung autokorelasi dan belum sepenuhnya bersifat acak (white noise). Meski demikian, kekuatan tren yang jauh lebih tinggi dibandingkan kekuatan musiman pada kedua variabel menunjukkan bahwa komponen tren merupakan penyumbang variasi paling dominan dalam data.

## Insight / Analisis

- Komponen tren suhu menunjukkan penurunan bertahap pada pertengahan tahun, kemudian kembali meningkat menjelang akhir tahun.
- Komponen musiman kelembapan menunjukkan fluktuasi yang lebih tajam dibandingkan suhu, sejalan dengan variasi kelembapan yang jauh lebih besar pada data mentah.
- Residual pada kedua variabel belum sepenuhnya acak, mengindikasikan adanya faktor eksternal nonmusiman yang belum tertangkap model, seperti cuaca ekstrem, aktivitas manusia, atau perubahan penggunaan lahan.
- Ditemukan satu nilai outlier pada kelembapan udara (97 persen, tanggal 10 Desember 2024), namun tidak berdampak signifikan terhadap hasil dekomposisi karena sifat STL yang robust terhadap nilai ekstrem.

## Teknologi yang Digunakan

- Bahasa: Python (dijalankan di Google Colab)
- Library: pandas dan numpy (pengolahan data), matplotlib (visualisasi), statsmodels (STL, plot_acf, plot_pacf, acorr_ljungbox untuk uji Ljung Box)
- Tools: Google Colab / Jupyter Notebook

## Struktur Proyek

```
STL_SlemanClimate/
│
├── README.md
├── requirements.txt
│
├── data/
│   └── data_thn_2024_full.xlsx
│
├── notebook/
│   └── KP_ANALISIS_TREN_POLA_MUSIMAN.ipynb
│
└── samples/
    └── hasil_dekomposisi/
        ├── stl_suhu.png
        └── stl_kelembapan.png
```

## Cara Menjalankan

1. Clone repository ini
```bash
git clone https://github.com/username/STL_SlemanClimate.git
cd STL_SlemanClimate
```
2. Install dependencies
```bash
pip install -r requirements.txt
```
3. Buka notebook menggunakan Jupyter Notebook, JupyterLab, atau upload ke Google Colab
```bash
jupyter notebook notebook/KP_ANALISIS_TREN_POLA_MUSIMAN.ipynb
```
4. Jalankan seluruh sel secara berurutan dari atas ke bawah.

## Batasan

- Penelitian ini hanya berfokus pada wilayah Kabupaten Sleman, Daerah Istimewa Yogyakarta.
- Data yang digunakan terbatas pada tahun 2024 (satu tahun), sehingga tren yang teramati hanya mencerminkan pola dalam periode tersebut.
- Analisis tidak mencakup faktor eksternal lain seperti curah hujan, tekanan udara, atau kecepatan angin.
- Penelitian hanya menggunakan metode STL dan tidak dibandingkan dengan metode dekomposisi lain seperti Classical Decomposition atau X 13ARIMA SEATS.
- Residual dari hasil dekomposisi belum sepenuhnya acak, menunjukkan masih ada pola atau variabel jangka pendek yang belum terjelaskan oleh model.

## Catatan Keamanan dan Etika Data

Repository ini tidak menyertakan kredensial API apa pun. Dataset yang digunakan berupa data iklim harian (suhu dan kelembapan udara) yang bersifat publik, diperoleh secara resmi dari Stasiun Klimatologi Yogyakarta di bawah BMKG, untuk keperluan penelitian akademik nonkomersial dalam rangka mata kuliah Kerja Praktik. Dataset tidak mengandung data pribadi maupun informasi sensitif individu.

## Penulis
Rigel Cahyo Gumilang Susanto, Program Studi Sains Data, Universitas Teknologi Yogyakarta. Project ini dikerjakan sebagai bagian dari mata kuliah Kerja Praktik.
