# Ringkasan Proyek

*(Bagian ini sengaja dikosongkan untuk diisi kemudian sesuai dengan konteks atau kebutuhan spesifik proyek, seperti deskripsi umum, latar belakang proyek, atau ruang lingkup kerja.)*

---

# Pemahaman Bisnis (Business Understanding)

## Permasalahan (Problem Statements)

1. **Bagaimana sistem rekomendasi dapat menyarankan film yang relevan berdasarkan aktivitas pengguna lain dan preferensi pengguna itu sendiri?**  
   Tujuan dari sistem ini adalah memberikan rekomendasi film yang sesuai dengan kesukaan pengguna. Hal ini dapat dicapai dengan menganalisis pola perilaku pengguna lain dan mengombinasikannya dengan input eksplisit dari pengguna yang bersangkutan.

2. **Bagaimana pendekatan sistem dapat diterapkan untuk menghasilkan rekomendasi yang akurat?**  
   Pendekatan ini mencakup dua metode utama: Collaborative Filtering dan Content-Based Filtering. Kedua pendekatan akan dibandingkan dari segi proses, hasil prediksi, dan akurasi guna menentukan metode yang paling optimal dalam konteks data yang digunakan.

## Tujuan (Goals)

- Memberikan evaluasi yang menyeluruh terhadap dua pendekatan sistem rekomendasi, sehingga pengguna atau pengembang dapat memahami kekuatan dan kelemahan masing-masing metode.
- Menggambarkan proses pengembangan sistem rekomendasi dari awal hingga akhir, termasuk data preprocessing, pemodelan, evaluasi, dan interpretasi hasil.

---

# Pendekatan Solusi (Solution Approach)

1. **Analisis Data Eksploratif (EDA)** dilakukan sebagai langkah awal untuk memahami karakteristik dan distribusi data. Ini mencakup pembersihan data, visualisasi, serta identifikasi pola-pola awal yang signifikan.
   
2. **Collaborative Filtering** diterapkan menggunakan model *Singular Value Decomposition (SVD)* yang dikembangkan secara manual dengan TensorFlow, guna memprediksi rating berdasarkan interaksi historis antar pengguna dan item.
   
3. **Content-Based Filtering** diterapkan dengan pendekatan *cosine similarity* untuk mengukur kemiripan antar film berdasarkan atribut genre. Pendekatan ini sangat efektif untuk merekomendasikan film yang memiliki karakteristik serupa dengan preferensi pengguna.

---

# Pemahaman Data (Data Understanding)

Dataset yang digunakan berasal dari MovieLens 100K, berisi informasi tentang pengguna, film, dan rating. Berikut penjelasan tiap file penting:

- **u.data**  
  Berisi 100.000 baris data rating dari 943 pengguna terhadap 1.682 film. Data tersimpan dalam format tab-separated dengan struktur: `user_id | item_id | rating | timestamp`. Rating berada dalam skala 1-5.

- **u.item**  
  Menyediakan informasi lengkap mengenai film, seperti judul, tanggal rilis, dan 19 kolom genre biner. Satu film bisa termasuk dalam beberapa genre.

- **u.user**  
  Menyimpan data demografis pengguna, meliputi usia, jenis kelamin, pekerjaan, dan kode pos.

- **u.genre dan u.occupation**  
  File referensi yang memuat daftar genre dan pekerjaan.

- **u1.base sampai u5.test**  
  Merupakan data pembagian untuk validasi silang 5-fold, memungkinkan evaluasi yang lebih stabil terhadap model.

- **ua.base dan ub.base**  
  Digunakan untuk validasi berdasarkan pembagian di mana masing-masing pengguna memiliki tepat 10 rating di data pengujian.

- **mku.sh dan allbut.pl**  
  Skrip shell untuk menghasilkan subset data dari `u.data`.

---

# Analisis Data Eksploratif (EDA)

## 1. Pemeriksaan Duplikasi  
Dilakukan pemeriksaan data duplikat pada dataset `users` dan `genres`. Hasilnya menunjukkan bahwa tidak ditemukan data yang berulang, baik dari sisi pengguna maupun jenis film. Ini memastikan bahwa analisis lanjutan tidak akan terganggu oleh data redundan.

## 2. Distribusi Jumlah Film per Genre  
Visualisasi dari kolom genre menunjukkan bahwa genre **Drama** menjadi yang paling banyak difavoritkan oleh pengguna, disusul oleh **Comedy**, **Action**, **Thriller**, dan **Romance**. Hal ini menandakan bahwa sistem rekomendasi harus memprioritaskan genre-genre populer dalam proses prediksi.

## 3. Distribusi Rating Pengguna  
Sebagian besar rating berada di kisaran 3 hingga 4, dengan rata-rata rating antara 2.6 hingga 4.3. Ini menunjukkan bahwa pengguna cenderung memberikan penilaian positif terhadap film yang mereka tonton, sehingga sistem rekomendasi perlu mampu membedakan mana film yang benar-benar disukai dari film yang hanya “cukup baik”.

## 4. Film dengan Rating Tertinggi  
Film seperti *The Usual Suspects* menduduki posisi teratas dalam hal rating rata-rata dan jumlah ulasan. Ini menunjukkan kualitas film yang tinggi dan tingkat kesukaan pengguna yang signifikan terhadap film tersebut.

## 5. Produksi Film Berdasarkan Tahun  
Tahun **1995** tercatat sebagai tahun dengan jumlah produksi film terbanyak (215 film), diikuti oleh 1994, 1993, 1997, dan 1992. Hal ini menunjukkan tren peningkatan produksi film selama era 90-an yang menjadi bagian besar dari dataset.

---

# Persiapan Data (Data Preparation)

## Umum  
- Ditemukan nilai `null` pada kolom `release_date` dan `IMDb_url` di file `u.item`. Nilai-nilai tersebut diisi dengan string default seperti `'Unknown'` atau `'NA'`.  
- Kolom `video_release_date` dihapus karena tidak digunakan dalam pemodelan.

## Untuk Collaborative Filtering  
- Digunakan **Label Encoding** pada kolom `user_id` dan `movie_id` untuk menghindari sparsity saat membangun model SVD.  
- Mapping ID asli disimpan dalam dictionary agar data dapat dikembalikan ke format aslinya bila diperlukan.  
- Semua data disalin ke objek baru untuk mencegah perubahan data mentah.

## Untuk Content-Based Filtering  
- Dilakukan **penggabungan (merge)** antara data pengguna dan genre.  
- Pemilihan film difokuskan pada yang memiliki rating ≥ 4 oleh user target, dan hanya kolom genre yang diambil untuk keperluan perhitungan kemiripan.

---

# Pemodelan dan Hasil (Modelling and Result)

## Collaborative Filtering

### Proses Implementasi  
- Model SVD dibangun secara manual dengan TensorFlow.  
- Rating diprediksi berdasarkan hasil dekomposisi matriks antara pengguna dan film.  
- Optimizer Adam digunakan untuk mempercepat konvergensi.  
- Hasil prediksi rating disimpan dalam salinan dataset pengguna.

### Interpretasi  
- Sistem menyaring film yang belum ditonton oleh user target.  
- Dari prediksi rating yang dihasilkan, diambil 5 film dengan skor tertinggi sebagai rekomendasi personal.

### Kelebihan  
- Efektif untuk pengguna dengan banyak riwayat interaksi.  
- Mampu menemukan pola kompleks dari data historis.

### Kekurangan  
- Tidak bekerja optimal jika pengguna atau item baru (cold start).  
- Butuh data dalam jumlah besar untuk pelatihan.

---

## Content-Based Filtering

### Proses Implementasi  
- Menggabungkan data pengguna dan genre.  
- Diambil film yang memiliki rating tinggi dari user target.  
- Genre diformat menjadi vektor biner.  
- Cosine similarity digunakan untuk mengukur kedekatan antar film berdasarkan vektor genre.

### Interpretasi  
- Hasil perhitungan similarity menghasilkan daftar film yang mirip dengan preferensi pengguna.  
- Top 3 film dengan skor similarity tertinggi direkomendasikan.

### Kelebihan  
- Relevan untuk pengguna baru.  
- Tidak bergantung pada interaksi pengguna lain.

### Kekurangan  
- Kurang variatif karena hanya merekomendasikan film dengan genre serupa.  
- Tidak mempertimbangkan popularitas film.

---

# Evaluasi Model

## Evaluasi Collaborative Filtering (dengan MAE dan RMSE)  
Evaluasi dilakukan dengan 5-fold cross-validation dan berbagai learning rate. Hasil terbaik diperoleh pada **learning rate = 0.05** dengan rata-rata error terendah:

- **Train MAE:** 0.5872  
- **Train RMSE:** 0.7514  
- **Test MAE:** 0.8542  
- **Test RMSE:** 1.1038

Selisih antara train dan test cukup kecil, menunjukkan model tidak overfitting dan memiliki kemampuan generalisasi yang baik.

## Evaluasi Content-Based Filtering  
Karena pendekatan ini tidak memprediksi nilai numerik, maka evaluasi dilakukan dengan pendekatan kualitatif dan pengukuran cosine similarity. Kinerja model tergantung pada keberagaman dan representasi genre.

---

# Kesimpulan

Dua pendekatan sistem rekomendasi telah dibandingkan secara menyeluruh:

- **Collaborative Filtering (SVD)** unggul dalam menangkap preferensi pengguna secara implisit melalui pola interaksi, namun kurang cocok untuk pengguna atau item baru.
- **Content-Based Filtering** cocok untuk pengguna baru dan memberikan kontrol lebih besar berdasarkan preferensi eksplisit, namun bisa memberikan hasil yang terlalu sempit.

Rekomendasi akhir adalah untuk menggabungkan kedua pendekatan ke dalam sistem hybrid guna memaksimalkan keunggulan masing-masing metode.
