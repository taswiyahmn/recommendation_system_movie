# Project Overview
Saat ini, mayoritas orang di sekitar kita memilih mengisi waktu luang atau mencari hiburan dengan bermain gim atau menonton film. Aktivitas menonton tidak lagi terbatas di bioskop; kemunculan layanan Over-The-Top (OTT) seperti Netflix, Disney+, dan HBO telah memudahkan akses tontonan kapan saja melalui platform berbayar. Bahkan, layanan OTT tersebut mulai menggantikan peran televisi konvensional, khususnya di kalangan masyarakat perkotaan (Slamet, 2023).

Durasi menonton yang panjang kini menjadi hal umum. Banyak pengguna menghabiskan waktu berjam-jam untuk menonton setiap harinya. Hal ini dipengaruhi oleh adanya algoritma personalisasi serta interaksi pengguna dengan konten yang semakin mendorong ketergantungan terhadap sistem rekomendasi (Wibowo, 2023).

Namun, meskipun akses konten sangat luas, pengguna kerap mengalami kebingungan saat memilih tontonan berikutnya. Proses memilih film satu per satu memakan waktu dan dapat menurunkan semangat menonton. Di sinilah pentingnya peran sistem rekomendasi. Sistem ini membantu pengguna menemukan konten sesuai dengan preferensi mereka tanpa perlu mencarinya secara manual.

Penelitian menunjukkan bahwa sistem rekomendasi yang mampu menyesuaikan dengan minat dan kebiasaan pengguna dapat meningkatkan loyalitas terhadap layanan OTT (Veriska & Pradekso, 2023). Selain itu, rekomendasi yang akurat dan relevan terbukti meningkatkan tingkat kepuasan pengguna (Rhemananda, 2023). Tak heran jika hampir semua platform OTT maupun media seperti YouTube telah menerapkan sistem rekomendasi berbasis aktivitas pengguna. Sistem ini tidak hanya memudahkan, tetapi juga memperpanjang durasi konsumsi konten dan mempererat hubungan antara pengguna dengan platform yang mereka gunakan (Veriska & Pradekso, 2023).

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

![image](https://github.com/user-attachments/assets/977a6422-ccc2-486a-b042-69c22d3c209e)

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
  Skrip shell untuk menghasilkan subset data dari `u.data`. <br><br>
Dalam proyek ini, hanya dua dataset utama yang digunakan, yaitu u.data yang merepresentasikan data interaksi pengguna dengan film (disebut sebagai users) dan u.item yang memuat informasi metadata film termasuk genre (disebut sebagai genres). Pemilihan dua dataset ini didasarkan pada kebutuhan inti proyek dalam membangun sistem rekomendasi yang efektif, di mana u.data digunakan untuk membangun model berbasis perilaku pengguna (collaborative filtering), sementara u.item digunakan untuk pendekatan berbasis konten (content-based filtering) berdasarkan genre film.
---

# Analisis Data Eksploratif (EDA)

## 1. Pemeriksaan Duplikasi
![image](https://github.com/user-attachments/assets/81d27ed7-64b2-40f6-91d9-e358dbfabba5)

Dilakukan pemeriksaan data duplikat pada dataset `users` dan `genres`. Hasilnya menunjukkan bahwa tidak ditemukan data yang berulang, baik dari sisi pengguna maupun jenis film. Ini memastikan bahwa analisis lanjutan tidak akan terganggu oleh data redundan.

## 2. Distribusi Jumlah Film per Genre  
![image](https://github.com/user-attachments/assets/f98a6e84-6424-4bab-9ac3-5666f70a0e47)

Visualisasi dari kolom genre menunjukkan bahwa genre **Drama** menjadi yang paling banyak difavoritkan oleh pengguna, disusul oleh **Comedy**, **Action**, **Thriller**, dan **Romance**. Hal ini menandakan bahwa sistem rekomendasi harus memprioritaskan genre-genre populer dalam proses prediksi.

## 3. Distribusi Rating Pengguna  
![image](https://github.com/user-attachments/assets/8d40d431-5376-41c2-bbf1-c85fcd199d52)

Sebagian besar rating berada di kisaran 3 hingga 4, dengan rata-rata rating antara 2.6 hingga 4.3. Ini menunjukkan bahwa pengguna cenderung memberikan penilaian positif terhadap film yang mereka tonton, sehingga sistem rekomendasi perlu mampu membedakan mana film yang benar-benar disukai dari film yang hanya “cukup baik”.

## 4. Film dengan Rating Tertinggi  
![image](https://github.com/user-attachments/assets/8932819f-88ce-4d39-8fd9-118ecf66b31b)

Film seperti *The Usual Suspects* menduduki posisi teratas dalam hal rating rata-rata dan jumlah ulasan. Ini menunjukkan kualitas film yang tinggi dan tingkat kesukaan pengguna yang signifikan terhadap film tersebut.

## 5. Produksi Film Berdasarkan Tahun  
![image](https://github.com/user-attachments/assets/baf55503-40fa-4fdc-835e-3c96578e9ef1)

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
Secara matematis, pendekatan SVD pada sistem rekomendasi dirumuskan sebagai berikut:

$$
\hat{r}_{ui} = \mu + b_u + b_i + q_i^T \cdot p_u
$$

Dimana:

- $\hat{r}_{ui}$ : Prediksi rating dari pengguna $u$ terhadap item $i$
- $\mu$ : Rata-rata rating global
- $b_u$ : Bias dari pengguna $u$
- $b_i$ : Bias dari item $i$
- $p_u$ : Vektor fitur laten pengguna $u$
- $q_i$ : Vektor fitur laten item $i$
- $q_i^T \cdot p_u$ : Dot product antara vektor item dan pengguna

Proses pelatihan model bertujuan untuk meminimalkan selisih antara rating sebenarnya $r_{ui}$ dan prediksi $\hat{r}_{ui}$ melalui fungsi loss berikut:

$$
\min \sum_{(u, i) \in K} \left( r_{ui} - \hat{r}_{ui} \right)^2 + \lambda \left( \|p_u\|^2 + \|q_i\|^2 + b_u^2 + b_i^2 \right)
$$

Dimana:

- $K$ : Kumpulan pasangan pengguna-item yang memiliki rating
- $\lambda$ : Faktor regulasi untuk menghindari overfitting
- Rating diprediksi berdasarkan hasil dekomposisi matriks antara pengguna dan film.  
- Optimizer Adam digunakan untuk mempercepat konvergensi.  
- Hasil prediksi rating disimpan dalam salinan dataset pengguna.

### Interpretasi  
![image](https://github.com/user-attachments/assets/27ac412b-1c63-46ba-8334-b975c4d8ed85)

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
![image](https://github.com/user-attachments/assets/6ea57bed-1007-43a6-97ca-6c5d83f32980)

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
