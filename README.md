# Book-Recommendation

Original file is located at
    https://colab.research.google.com/drive/1KRTgroTtBwX_Vpt8e49886pSHsCI1MxQ

# Project Domain

Books have always been a gateway to new knowledge and self-discovery, and now, thanks to technology, finding the right books has become easier than ever. Book recommendation systems are key to helping readers discover books that match their personal interests, making it easier to navigate the vast world of literature. These systems use smart algorithms to understand what readers like and suggest books they are most likely to enjoy.

According to a 2021 report by Grand View Research, the market for book recommendation systems has been growing rapidly due to the rising demand for personalized reading experiences. Platforms like Goodreads, Amazon, and Scribd use different methods to recommend books, such as collaborative filtering (based on what other users liked) and content-based filtering (focused on book genres or themes). This personalization not only makes readers happy but also helps boost book sales.

In Indonesia, where promoting literacy is a top priority, these recommendation systems can play a big role in encouraging people to read more. According to a report by the Ministry of Education and Culture in 2023, programs like the Gerakan Literasi Nasional (National Literacy Movement) aim to improve reading habits across the country. However, one of the biggest challenges is that many people in rural areas still struggle with limited access to books. Digital platforms with recommendation systems could help solve this by offering personalized book suggestions tailored to local needs and preferences.

Studies have shown that recommendation systems really do help people read more. A 2022 study in the Journal of Educational Technology found that when book suggestions are personalized, readers, especially younger ones, are more likely to read frequently. By analyzing factors like a reader's favorite genres, past books, and user reviews, these systems can guide them toward books they’ll love.

Popular platforms like Goodreads use a method called collaborative filtering, where they look at what other readers with similar tastes have liked and suggest books based on that. For example, if someone enjoys Atomic Habits by James Clear, the system might recommend books like Deep Work by Cal Newport or Grit by Angela Duckworth. Amazon’s system takes it a step further by combining user data with content-based insights, making recommendations even more accurate and relevant.

Although these systems have made discovering books easier, they still face some challenges. One issue is bias in the algorithms, which can sometimes lead to repetitive recommendations. Another problem is that niche books, especially from underrepresented authors, don’t always get the attention they deserve. To improve this, it’s important to diversify the data these systems use and create more inclusive models. Additionally, local platforms, especially in countries like Indonesia, could do a better job by offering recommendations that include books in regional languages or from local authors.

In the end, book recommendation systems are changing the way we discover and enjoy books, offering personalized experiences that encourage us to read more. With ongoing improvements in artificial intelligence and machine learning, these systems will only get better, helping readers connect with books in deeper, more meaningful ways.

# Business Understanding

Problem Statements:

1. Which models can predict the best
2. How is ten recommendations

Goals:

1. To find out which model can predict the best
2. To find out the price rice in the future


Solutions

Dalam proyek ini, Saya menggunakan algoritme Singular Value Decomposition (SVD) untuk menganalisis dan membuat prediksi pada data rekomendasi. SVD adalah salah satu metode populer dalam sistem rekomendasi, terutama karena kemampuannya menangani data sparse (jarang) dengan efisien dan menghasilkan prediksi yang akurat. Berikut adalah tahapan pelaksanaan proyek ini.

Untuk mencapai hasil terbaik, saya melatih tiga jenis model. Pertama, model SVD serderhana. Kedua, model SDV dengan menghapus rating yang benilai nol. Serta melakukan teknik normalisasi pada rating yang ada. 

# Data Understanding

* User-ID : Id pengguna
* Age : umur pengguna
* ISBN : kode unik buku
* Title : Judul Buku
* Author : Nama penulis buku
* Year : Tahun terbit 
* Publisher  : Nama penerbit. 

Terdapat 3 file dalam dataset:
* Book, dengan 271379 baris
* User, dengan 278859 baris
* Rating, dengan 1149780 baris

Berikut beberapa baris yang kosong:
* Masing-masing 2 baris pada kolom author dan publisher
* 110232 baris pada kolom Age


# EDA

## Rating Diistribution



## Age Distribution



## Top 10 Most Rated Books



## Top 10 User Giving Most Ratings



## Book Publication Trend



## Correlation Between Rating And User Age



## Top 10 Publishers Pusblish The Books


# Data Cleaning
Berikut bebrapa teknik pembersihan data:

1. Menghapus Duplikat
2. Menangani Missing Values

* Untuk ratings, kita akan menghapus baris yang memiliki nilai rating kosong
* Untuk usia pengguna, kita bisa mengisi nilai yang hilang dengan rata-rata usia
* Untuk Tahun Publikasi Buku, kita akan mengisi nilai yang hilang dengan 0 atau median tahun


3. Memastikan Format yang Konsisten

* Mengonversi rating ke dalam format integer
* Mengonversi kolom ISBN dan User-ID menjadi tipe data string untuk konsistensi

4. Memastikan Rentang Rating yang Valid (Jika rating berada di luar 0-10, hapus)


# Model 1: Sistem Rekomendasi Collaborative Filtering (SVD)

# Preprocessing
1. Menyiapkan dataset
* Reader
Membuat objek pembaca yang menentukan rentang nilai penilaian (rating scale). Dalam hal ini, penilaian berada dalam rentang 0 hingga 10.

* Dataset.load_from_df
Mengonversi DataFrame pandas, yaitu ratings[['User-ID', 'ISBN', 'Rating']], menjadi format dataset yang sesuai dengan Surprise. Kolom yang digunakan:

User-ID: Identitas unik pengguna.
ISBN: Identitas unik buku.
Rating: Penilaian yang diberikan pengguna pada buku.
Fungsi ini menghasilkan dataset yang siap digunakan oleh algoritma Collaborative Filtering di Surprise.

2. Membagi data menjadi train dan test set dengan perbandingan 80:20


# Modelling

# Inisialisasi model SVD (Singular Value Decomposition)
Langkah ini menciptakan objek svd, yang merepresentasikan algoritme matrix factorization. SVD membagi matriks rating (user-item matrix) menjadi tiga matriks dengan dimensi yang lebih kecil. Proses ini memungkinkan model memahami pola preferensi pengguna dan karakteristik item secara tersembunyi (latent features), yang kemudian digunakan untuk merekonstruksi nilai rating yang belum diberikan.

Menurut Koren et al. (2009), SVD telah terbukti menjadi teknik efektif dalam merekomendasikan item dengan mengungkap hubungan laten yang kompleks di antara data pengguna dan item .

# Melatih model menggunakan trainset

Proses ini memungkinkan model untuk belajar dari pola historis rating dalam data. Data pelatihan biasanya berupa matriks besar yang mengandung banyak nilai kosong, di mana pengguna belum memberikan rating terhadap sebagian besar item. SVD membantu mengisi kekosongan ini dengan memperkirakan rating berdasarkan hubungan laten yang dipelajari selama pelatihan.

Sebagaimana dijelaskan dalam Ricci et al. (2011), model seperti SVD sangat cocok untuk data sparse, karena mampu mengekstraksi fitur-fitur penting tanpa memerlukan input eksplisit pada semua elemen matriks 

# Melakukan prediksi pada testset

Hasil prediksi mencakup estimasi rating yang akan diberikan pengguna pada item yang sebelumnya belum mereka nilai. Proses ini menguji performa model menggunakan data baru untuk mengevaluasi akurasi prediksi

# Menghitung akurasi menggunakan RMSE
Metrik RMSE menghitung rata-rata akar kuadrat dari selisih antara rating aktual dan rating yang diprediksi. Semakin kecil nilai RMSE, semakin baik performa model dalam memprediksi rating yang mendekati nilai sebenarnya. RMSE didefinisikan sebagai:

RMSE adalah metrik yang digunakan untuk mengevaluasi seberapa baik model prediksi. Rumusnya adalah sebagai berikut:

$$
\text{MAE} = \frac{1}{n} \sum_{i=1}^{n} \left| y_i - \hat{y}_i \right|
$$

di mana $y_i$ adalah nilai aktual dan $\hat{y}_i$ adalah nilai prediksi.


RMSE mirip MAE. Formulanya sebagai berikut:

$$
\text{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2}
$$




# Rekomendasi Berdasarkan Model SVD







# Model 2: SDV Dengan Menghapus Rating = 0

# Menghapus rating nol

* ratings['Rating'] > 0
Bagian ini memfilter dataset ratings, hanya memilih baris di mana kolom Rating memiliki nilai lebih dari nol. Ini berarti rating negatif atau nol tidak akan dimasukkan ke dalam dataset baru.

* ratings[... ]
Filter ini diterapkan pada dataset ratings, sehingga hanya baris yang memenuhi kriteria (rating > 0) yang disimpan dalam dataset baru ratings_filtered.

* ratings_filtered.shape[0]
Fungsi shape[0] menghitung jumlah baris dalam dataset ratings_filtered. Nilai ini kemudian dicetak untuk memberi informasi tentang jumlah data yang tersisa setelah penyaringan.


#  Evaluasi

Evaluasi model juga menggunakan metriks MSE dan RMSE. 

## Rekomendasi


# Model 3: SDV Dengan Normalisasi Rating

## Normalization of ratings to a 0-1 scale

ratings['Rating']
Bagian ini mengakses kolom Rating dari dataset ratings, yaitu kolom yang berisi nilai rating asli pengguna terhadap item.

ratings['Rating'].max()
Fungsi .max() mencari nilai tertinggi dari kolom Rating. Nilai ini digunakan sebagai pembagi agar semua rating berada dalam skala relatif terhadap nilai maksimum. Misalnya:

Jika rating maksimum adalah 10, maka rating 5 akan dinormalisasi menjadi 0.5.
Jika rating maksimum adalah 5, maka rating 3 akan menjadi 0.6.

## Evaluasi
Evaluase juga menggunakan RMSE

## Rekomendasi

