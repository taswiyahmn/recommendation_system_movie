# Project Overview
Saat ini, mayoritas orang di sekitar kita memilih mengisi waktu luang atau mencari hiburan dengan bermain gim atau menonton film. Aktivitas menonton tidak lagi terbatas di bioskop; kemunculan layanan Over-The-Top (OTT) seperti Netflix, Disney+, dan HBO telah memudahkan akses tontonan kapan saja melalui platform berbayar. Bahkan, layanan OTT tersebut mulai menggantikan peran televisi konvensional, khususnya di kalangan masyarakat perkotaan (Slamet, 2023).

Durasi menonton yang panjang kini menjadi hal umum. Banyak pengguna menghabiskan waktu berjam-jam untuk menonton setiap harinya. Hal ini dipengaruhi oleh adanya algoritma personalisasi serta interaksi pengguna dengan konten yang semakin mendorong ketergantungan terhadap sistem rekomendasi (Wibowo, 2023).

Namun, meskipun akses konten sangat luas, pengguna kerap mengalami kebingungan saat memilih tontonan berikutnya. Proses memilih film satu per satu memakan waktu dan dapat menurunkan semangat menonton. Di sinilah pentingnya peran sistem rekomendasi. Sistem ini membantu pengguna menemukan konten sesuai dengan preferensi mereka tanpa perlu mencarinya secara manual.

Penelitian menunjukkan bahwa sistem rekomendasi yang mampu menyesuaikan dengan minat dan kebiasaan pengguna dapat meningkatkan loyalitas terhadap layanan OTT (Veriska & Pradekso, 2023). Selain itu, rekomendasi yang akurat dan relevan terbukti meningkatkan tingkat kepuasan pengguna (Rhemananda, 2023). Tak heran jika hampir semua platform OTT maupun media seperti YouTube telah menerapkan sistem rekomendasi berbasis aktivitas pengguna. Sistem ini tidak hanya memudahkan, tetapi juga memperpanjang durasi konsumsi konten dan mempererat hubungan antara pengguna dengan platform yang mereka gunakan (Veriska & Pradekso, 2023).

[1] Slamet, “Perubahan Pola Konsumsi Hiburan Digital di Era OTT,” Jurnal Komunikasi Digital, vol. 7, no. 2, pp. 134–142, 2023. <br>
[2] Wibowo, “Algoritma Rekomendasi dan Perilaku Menonton di Platform OTT,” Jurnal Teknologi Informasi dan Komunikasi, vol. 11, no. 1, pp. 55–63, 2023. <br>
[3] Veriska and R. Pradekso, “Pengaruh Personalisasi Sistem Rekomendasi terhadap Loyalitas Pengguna OTT,” Jurnal Riset Media Digital, vol. 5, no. 3, pp. 101–109, 2023. <br>
[4] Rhemananda, “Analisis Kepuasan Pengguna terhadap Sistem Rekomendasi pada Layanan Streaming,” Jurnal Sistem Informasi, vol. 9, no. 2, pp. 77–85, 2023. <br>

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
   
3. **Content-Based Filtering** diterapkan dengan pendekatan *cosine similarity* antar film berdasarkan atribut genre. Rekomendasi diberikan dengan mencari film-film yang paling mirip secara konten dengan film-film yang sudah disukai pengguna. Pendekatan ini efektif dalam merekomendasikan film yang memiliki karakteristik genre serupa tanpa perlu membangun profil preferensi pengguna secara eksplisit.


![image](https://github.com/user-attachments/assets/977a6422-ccc2-486a-b042-69c22d3c209e)

---

# Pemahaman Data (Data Understanding)
## Deskripsi Dataset
### Dataset 1: `u.data`

#### Sumber Data
Dataset ini merupakan bagian dari MovieLens 100k dataset dan tersedia di [MovieLens 100k Dataset](https://grouplens.org/datasets/movielens/100k/).

#### Jumlah Baris dan Kolom
- Jumlah baris: 100.000
- Jumlah kolom: 4

#### Kondisi Data
- Tidak terdapat missing value
- Perlu dilakukan pengecekan duplikasi berdasarkan kombinasi `user_id`, `item_id`, dan `timestamp`
- Tidak ada outlier karena rating dibatasi pada skala 1–5

#### Penjelasan Kolom:
| Kolom       | Deskripsi                                         |
|-------------|---------------------------------------------------|
| user_id     | ID unik pengguna (1–943)                          |
| item_id     | ID unik film (1–1682)                             |
| rating      | Rating film dari pengguna (skala 1–5)             |
| timestamp   | Waktu pemberian rating dalam format UNIX time     |

---

### Dataset 2: `u.item`

#### Sumber Data
Merupakan bagian dari folder `ml-100k/` dalam MovieLens 100k dataset.

#### Jumlah Baris dan Kolom
- Jumlah baris: 1.682
- Jumlah kolom: 24

#### Kondisi Data
- Tidak terdapat missing value
- Tidak terdapat data duplikat (setiap `movie_id` unik)
- Kolom genre dalam bentuk biner (0 atau 1), tidak terdapat outlier

#### Penjelasan Kolom:
| No  | Kolom                | Deskripsi                                                  |
|-----|----------------------|------------------------------------------------------------|
| 1   | movie_id             | ID unik film                                               |
| 2   | movie_title          | Judul film                                                 |
| 3   | release_date         | Tanggal rilis film                                         |
| 4   | video_release_date   | Tanggal rilis versi video                                  |
| 5   | IMDb_URL             | Link ke halaman film di IMDb                               |
| 6–24| 19 kolom genre biner | Menandakan genre film (1 = ya, 0 = tidak)                  |

#### Daftar Genre (Kolom ke-6 hingga ke-24):
| Kolom     | Genre         |
|-----------|---------------|
| 6         | unknown       |
| 7         | Action        |
| 8         | Adventure     |
| 9         | Animation     |
| 10        | Children's    |
| 11        | Comedy        |
| 12        | Crime         |
| 13        | Documentary   |
| 14        | Drama         |
| 15        | Fantasy       |
| 16        | Film-Noir     |
| 17        | Horror        |
| 18        | Musical       |
| 19        | Mystery       |
| 20        | Romance       |
| 21        | Sci-Fi        |
| 22        | Thriller      |
| 23        | War           |
| 24        | Western       |
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
- Mengambil tahun rilis dari judul film dan menyimpannya ke kolom `release_date`, kemudian menghapus tahun tersebut dari judul agar hanya tersisa nama film saja.
- Kolom `video_release_date` dihapus karena tidak digunakan dalam pemodelan.
- Ditemukan nilai `null` pada kolom `release_date` dan `IMDb_url` di file `u.item`. Nilai-nilai tersebut diisi dengan string default seperti `'Unknown'` atau `'NA'`.  

## Untuk Collaborative Filtering  
- Digunakan **Label Encoding** pada kolom `user_id` dan `movie_id` untuk menghindari sparsity saat membangun model SVD.  
- Mapping ID asli disimpan dalam dictionary agar data dapat dikembalikan ke format aslinya bila diperlukan.  
- Semua data disalin ke objek baru untuk mencegah perubahan data mentah.

## Untuk Content-Based Filtering  
- Dilakukan **penggabungan (merge)** antara data pengguna dan data genre film.  
- Film-film yang telah diberikan rating tinggi (≥ 4) oleh user target dipilih sebagai basis film referensi.  
- Genre film diformat menjadi vektor biner untuk setiap film.  
- Kemiripan antar film dihitung menggunakan **cosine similarity** berdasarkan vektor genre.  
- Rekomendasi diperoleh dari film-film yang memiliki skor similarity tertinggi terhadap film-film yang sudah disukai pengguna tersebut.

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
- Data film dengan atribut genre dikonversi menjadi representasi vektor biner.  
- Film yang telah diberi rating tinggi oleh pengguna menjadi film acuan (query).  
- Cosine similarity digunakan untuk mengukur kedekatan antar film berdasarkan vektor genre tersebut.  
- Rekomendasi film diperoleh dengan mencari film yang paling mirip dengan film acuan, bukan berdasarkan profil eksplisit pengguna.

### Interpretasi  
![image](https://github.com/user-attachments/assets/ede9aecf-d117-4ae5-98c9-f48cc0fb7814)

- Sistem merekomendasikan film-film yang secara konten (genre) paling mirip dengan film-film yang sudah disukai pengguna.  
- Daftar rekomendasi dihasilkan dari perhitungan kemiripan antar film, sehingga film yang direkomendasikan memiliki karakteristik genre yang sangat serupa dengan film referensi.

### Kelebihan  
- Efektif untuk pengguna baru yang belum memiliki banyak interaksi karena tidak bergantung pada data interaksi pengguna lain.  
- Memberikan rekomendasi berdasarkan kesamaan konten film secara langsung.

### Kekurangan  
- Rekomendasi cenderung kurang bervariasi karena terbatas pada film yang memiliki genre serupa.  
- Tidak mempertimbangkan faktor lain seperti popularitas atau rating pengguna lain.

---

# Evaluasi Model

## Evaluasi Collaborative Filtering (dengan MAE dan RMSE)  
Evaluasi dilakukan dengan 5-fold cross-validation dan berbagai learning rate. Hasil terbaik diperoleh pada **learning rate = 0.05** dengan rata-rata error terendah:

### Hasil Evaluasi:
- **Rata-rata Training**:
  - MAE: 0.5999
  - RMSE: 0.7658
- **Rata-rata Testing**:
  - MAE: 0.8573
  - RMSE: 1.1053

Selisih antara train dan test cukup kecil, menunjukkan model tidak overfitting dan memiliki kemampuan generalisasi yang baik.

## Evaluasi Content-Based Filtering

Pendekatan Content-Based Filtering pada sistem ini menggunakan kemiripan fitur antar film untuk memberikan rekomendasi, tanpa memprediksi nilai rating secara numerik. Evaluasi dilakukan menggunakan metrik **Precision@10**, yang mengukur proporsi film relevan di antara 10 rekomendasi teratas.

Berdasarkan contoh rekomendasi film mirip *Toy Story*, Precision@10 diestimasi sekitar **0.6**, yang menunjukkan bahwa sekitar 6 dari 10 film rekomendasi memang relevan dan sesuai genre atau tema film target. Ini jauh lebih tinggi dibandingkan dengan rata-rata Precision@10 item-based Collaborative Filtering yang hanya sebesar **0.1386** dari 942 user.

Hasil ini mengindikasikan bahwa metode Content-Based Filtering mampu memberikan rekomendasi yang lebih fokus dan relevan secara tematik, khususnya pada film dengan genre spesifik. Namun, evaluasi ini masih berdasarkan studi kasus terbatas dan perlu validasi lebih lanjut menggunakan data preferensi pengguna yang lebih luas.


---

# Kesimpulan

Dua pendekatan sistem rekomendasi telah dibandingkan secara menyeluruh:

- **Collaborative Filtering (SVD)** unggul dalam menangkap preferensi pengguna secara implisit melalui pola interaksi, namun kurang cocok untuk pengguna atau item baru.
- **Content-Based Filtering** menggunakan cosine similarity antar film berhasil merekomendasikan film yang mirip secara genre dengan film-film yang sudah disukai oleh pengguna, tanpa membangun profil preferensi pengguna secara eksplisit.  
   - Metode ini sangat berguna untuk pengguna baru atau situasi cold start, meskipun rekomendasi yang dihasilkan cenderung terbatas pada variasi genre yang sama.

Rekomendasi akhir adalah untuk menggabungkan kedua pendekatan ke dalam sistem hybrid guna memaksimalkan keunggulan masing-masing metode.

## Evaluasi Relevansi Hasil dengan Tujuan Bisnis

Tujuan bisnis utama dari pengembangan sistem rekomendasi ini adalah:

- Memberikan evaluasi menyeluruh atas dua pendekatan sistem rekomendasi, sehingga pengguna atau pengembang dapat memahami kelebihan dan kekurangan masing-masing metode.  
- Menggambarkan proses pengembangan sistem rekomendasi secara lengkap, mulai dari data preprocessing, pemodelan, evaluasi, hingga interpretasi hasil.

### Apakah Hasil Sudah Relevan dan Mencapai Tujuan?

**1. Evaluasi Collaborative Filtering**  
Model collaborative filtering menunjukkan performa yang cukup baik dengan nilai MAE dan RMSE pada data test yang masih dekat dengan data train. Ini menunjukkan kemampuan generalisasi yang baik tanpa overfitting. Dengan demikian, model ini dapat diandalkan untuk memprediksi rating pengguna, memenuhi tujuan bisnis memberikan pemahaman mengenai kekuatan metode collaborative filtering.

**2. Evaluasi Content-Based Filtering**  
Content-based filtering diuji dengan Precision@10, menghasilkan nilai sekitar 0.6 pada contoh kasus Toy Story, yang berarti 6 dari 10 rekomendasi benar-benar relevan dengan film target. Angka ini jauh lebih tinggi dibandingkan rata-rata Precision@10 collaborative filtering sebesar 0.1386. Hasil ini memperlihatkan bahwa content-based filtering sangat efektif dalam memberikan rekomendasi tematik yang relevan, sesuai dengan tujuan bisnis dalam memahami kekuatan metode content-based.

**Kesimpulan**  
Hasil evaluasi secara keseluruhan sudah sangat relevan dan berhasil memenuhi tujuan bisnis, yaitu memberikan gambaran yang jelas mengenai keunggulan dan keterbatasan masing-masing metode rekomendasi. Collaborative filtering unggul dalam prediksi rating dan generalisasi, sementara content-based filtering unggul dalam relevansi tematik rekomendasi.Selain itu, proses pengembangan sistem mulai dari preprocessing, pemodelan, hingga evaluasi dan interpretasi juga sudah digambarkan secara komprehensif, memenuhi tujuan bisnis kedua. Namun, untuk memastikan hasil ini dapat diimplementasikan secara efektif dalam skala produksi, perlu dilakukan validasi lebih lanjut dengan data pengguna yang lebih besar dan beragam.
Secara keseluruhan, sistem rekomendasi yang dikembangkan sudah berada di jalur yang tepat untuk memenuhi tujuan bisnis yang ditetapkan.

