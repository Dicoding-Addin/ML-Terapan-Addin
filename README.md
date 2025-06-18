# Laporan Proyek Machine Learning - Addin Hadi Rizal

## Project Overview

Seiring dengan bertambahnya jumlah konten digital seperti film dan serial, pengguna semakin sulit dalam menemukan tontonan yang sesuai dengan preferensinya. Menurut Ricci et al. (2022), tantangan utama dalam industri hiburan digital adalah menyajikan rekomendasi yang personal dan relevan guna meningkatkan kepuasan serta keterlibatan pengguna. Sistem rekomendasi hadir sebagai solusi penting untuk membantu pengguna menyaring informasi dan menemukan konten yang sesuai dengan minat mereka secara otomatis.

Salah satu pendekatan yang populer adalah _Content-Based Filtering_, di mana sistem merekomendasikan item (film) yang mirip dengan apa yang disukai oleh pengguna sebelumnya. Teknik ini banyak digunakan karena tidak bergantung pada interaksi dari pengguna lain, sehingga efektif dalam menangani masalah cold-start user (Schafer et al., 2021). Dalam proyek ini, digunakan representasi teks genre dengan metode _TF-IDF_ dan pengukuran kemiripan menggunakan _cosine similarity_ untuk membangun sistem rekomendasi yang sederhana namun cukup efektif.

Penerapan sistem rekomendasi berbasis konten ini sangat penting terutama bagi platform film dan streaming untuk mempertahankan pengguna, mengurangi tingkat kebingungan dalam memilih, dan meningkatkan durasi keterlibatan pengguna dalam platform.

**Referensi:**

- Ricci, F., Rokach, L., & Shapira, B. (2022). Recommender Systems Handbook (3rd ed.). Springer.

- Schafer, J. B., Frankowski, D., Herlocker, J., & Sen, S. (2021). Collaborative Filtering Recommender Systems. In The Adaptive Web (pp. 291–324). Springer.

---

## Business Understanding

### Problem Statements

Dalam penelitian ini, beberapa pertanyaan utama yang akan dijawab adalah:

- Berdasarkan data mengenai pengguna dan film, bagaimana membuat sistem rekomendasi yang dipersonalisasi dengan teknik content-based filtering?
- Dengan data rating yang dimiliki, bagaimana kita dapat merekomendasikan film lain yang mungkin disukai dan belum pernah ditonton oleh pengguna?

### Goals

Untuk menjawab penyataan masalah di atas, penelitian ini memiliki tujuan sebagai berikut:

- Membangun sistem rekomendasi yang dapat menghasilkan sejumlah rekomendasi film yang dipersonalisasi untuk pengguna dengan teknik content-based filtering.
- Membangun sistem rekomendasi yang dapat menghasilkan sejumlah rekomendasi film yang sesuai dengan preferensi pengguna dan belum pernah ditonton sebelumnya dengan teknik collaborative filtering.

### Solution Approach

Untuk mencapai tujuan yang telah dirumuskan, penelitian ini mengusulkan dua pendekatan utama dalam membangun sistem rekomendasi film, yaitu:

- Content-Based Filtering dengan Cosine Similarity

Pendekatan ini merekomendasikan film berdasarkan **kemiripan konten** dari film itu sendiri, terutama menggunakan fitur `genres`. Setiap film direpresentasikan dalam bentuk vektor menggunakan teknik **TF-IDF (Term Frequency-Inverse Document Frequency)**, kemudian dihitung kesamaannya menggunakan **cosine similarity**. Rekomendasi diberikan kepada pengguna berdasarkan film yang pernah mereka tonton atau sukai, dan dibandingkan dengan film lain yang memiliki genre serupa.

- Collaborative Filtering dengan Deep Learning

Pendekatan ini menggunakan data interaksi pengguna–film berupa rating dan membangun model prediksi menggunakan arsitektur **neural network sederhana**. Model ini memetakan user dan film ke dalam **embedding space**, lalu memperkirakan seberapa besar kemungkinan seorang user menyukai suatu film. Model dilatih menggunakan data historis rating dan dievaluasi dengan metrik seperti RMSE.

---

## Data Understanding

Dataset yang digunakan dalam proyek kali ini berjudul [Movie Lens Dataset](https://www.kaggle.com/datasets/ayushimishra2809/movielens-dataset/data). Dataset ini berisi data rating dan free tagging (label bebas yang diberikan user kepada film) dari website [movielens.org](https://movielens.org/), sebuah layanan rekomendasi film. Untuk lebih detilnya, dataset ini berisi 105339 rating dan 10329 film. Data ini dibuat oleh 668 pengguna antara 29 Maret 1996 dan 24 September 2018.

Setelah dataset diunduh dan diekstrak, akan didapatkan file sebagai berikut.
- movies.csv
- ratings.csv

Pada proyek kali ini, file yang akan digunakan meliputi movies.csv dan ratings.csv.

### **Sumber Data**

Dataset dapat diakses dan diunduh melalui tautan berikut: [[Movie Lens Dataset](https://www.kaggle.com/datasets/ayushimishra2809/movielens-dataset/data)]

### **Struktur Dataset**

1. movies.csv

Dataset `movies_df` berisi informasi tentang 9.742 film yang akan digunakan dalam sistem rekomendasi film. Dataset ini memiliki tiga fitur utama yang menjelaskan identitas dan klasifikasi dari setiap film.

| Nama Fitur | Tipe Data | Deskripsi |
|------------|-----------|-----------|
| `movieId`  | Integer   | ID unik untuk setiap film. Contoh: `1`, `2`, `3`, dst. |
| `title`    | Object    | Judul dari film. Contoh: `Toy Story (1995)` |
| `genre`    | Object    | Genre dari film. Jika film memiliki lebih dari satu genre, genre dipisahkan dengan tanda `pipe`, Contoh `Action 'pipe' Comedy` |

2. ratings.csv

Dataset `rating_df` berisi 100.836 data rating yang merepresentasikan interaksi pengguna dengan film. Dataset ini memiliki empat fitur utama yang digunakan dalam sistem rekomendasi, khususnya untuk pendekatan **Collaborative Filtering**.

| Nama Fitur  | Tipe Data | Deskripsi |
| ----------- | ------- | ----------- |
| `userId`    | Integer | ID unik untuk setiap pengguna. Contoh: `1`, `2`, `3`, dst|
| `movieId`   | Integer | ID unik untuk setiap film. Cocok dengan `movieId` pada `movies_df`.                                  |
| `rating`    | Float   | Nilai rating yang diberikan oleh pengguna terhadap film, dengan skala mulai dari `0.5` hingga `5.0`. |
| `timestamp` | Integer | Waktu ketika rating diberikan, dalam format Unix timestamp.  |

### Exploratory Data Analysis

#### Analisis Univariate

Analisis ini merupakan analisis pada tiap fitur dengan tujuan untuk mengetahui karakteristik dan struktur pada variabel.

- **Melihat genre unik dalam data**

  ![Submission_Recommendation_System ipynb - Colab](https://github.com/AddinRizal/Film-Recommender/blob/main/image/1.png?raw=true)

- **Top 10 userId pemberi rating terbanyak**

  ![Submission_Recommendation_System ipynb - Colab](https://github.com/AddinRizal/Film-Recommender/blob/main/image/2.png?raw=true)

- **Distribusi rating dalam dataset**

  ![Submission_Recommendation_System ipynb - Colab](https://github.com/AddinRizal/Film-Recommender/blob/main/image/3.png?raw=true)

Dari analisis univariate yang dilakukan didapatkan beberapa poin, sebagai berikut:

- Dataset proyek ini memiliki 10329 data film unik. Dengan 20 kategori genre unik, meliputi Action hingga Western. Selain itu, terdapat pula genre '(no genres listed)' yang menunjukkan ada beberapa film yang data genrenya kurang lengkap
- Terdapat 7 data Genre bernilai '(no genres listed)' dalam dataset. Untuk proyek ini, kita akan menggantinya menjadi satu kata untuk mengurangi term saat proses TFIDF nantinya
- Dataset pada proyek ini memiliki 668 data pengguna unik, serta 105339 data rating yang diberikan pada film
- Jumlah data film unik pada movies_df dan ratings_df berbeda. Dapat disimpulkan bahwa tidak semua film dalam movies_df sudah diberi rating oleh pengguna
- Tidak ditemukan adanya nilai kosong dan duplikat pada data, sehingga penanganan tidak diperlukan
- Melalui eksplorasi, ditemukan bahwa pengguna dengan userId 668 merupakan pengguna yang paling banyak memberikan rating pada film (2698 rating), disusur dengan userId 599(2478 rating), serta userId 474 (2108 rating)
- Rating yang diberikan pengguna dalam dataset cukup beragam dari 0.5 hingga 5.0. Rating 4.0 merupakan rating terbanyak yang diberikan pengguna pada film, sedangkan 0.5 adalah rating dengan jumlah paling sedikit yang diberikan pengguna pada film

#### Analisis Multivariate

Analisis Multivariate adalah jenis analisis statistik atau eksplorasi data yang melibatkan tiga atau lebih variabel secara bersamaan, dengan tujuan untuk Mencari pola, hubungan, atau pengaruh antar variabel.

- **Top 10 film dengan rating terbaik diurutkan berdasarkan rating dan jumlah rating**

  ![Submission_Recommendation_System ipynb - Colab](https://github.com/AddinRizal/Film-Recommender/blob/main/image/4.png?raw=true)

Dari gambar di atas, ditemukan bahwa Belle époque (1992), Come and See (Idi i smotri) (1985), Enter the Void (2009), Heidi Fleiss: Hollywood Madam (1995), Jonah Who Will Be 25 in the Year 2000 (Jonas qui aura 25 ans en l'an 2000) (1976), Lamerica (1994), dan Lesson Faust (1994) merupakan film dengan rating terbaik (5.0) jika diurutkan berdasarkan rating dan jumlah rating. Jumlah rating yang dimiliki oleh film tersebut adalah 2, yang mana sangat sedikit

- **Top 10 paling banyak diberi rating**

  ![Submission_Recommendation_System ipynb - Colab](https://github.com/AddinRizal/Film-Recommender/blob/main/image/5.png?raw=true)

Dari gambar di atas, ditemukan bahwa Pulp Fiction (1994) merupakan film yang memiliki jumlah rating terbanyak yang diberikan pengguna (325 rating), disusul oleh Forrest Gump, The (1994) (311 rating), dan Shawshank Redemption, The (1994) (308 rating)

---

## Data Preparation

Pada tahap ini, dilakukan serangkaian proses _data preparation_ untuk memastikan data yang digunakan dalam pemodelan _recommender system_ dalam kondisi bersih, konsisten, dan sesuai dengan kebutuhan model. Adapun beberapa teknik yang diterapkan, secara berurutan, adalah sebagai berikut:

### 1. Menghapus Fitur yang Tidak Diperlukan

Fitur `timestamp` dihapus dari data karena informasi waktu pemberian rating tidak diperlukan dalam proses pemodelan sistem rekomendasi berbasis konten maupun kolaboratif. Keberadaannya tidak memberikan informasi yang relevan terhadap minat pengguna atau karakteristik film, sehingga dapat dihapus untuk menyederhanakan data.

### 2. Transformasi Fitur `genres`

Nilai pada fitur `genres` mengalami dua bentuk transformasi:

- Mengganti karakter pemisah `|` menjadi spasi agar mempermudah proses tokenisasi saat dilakukan ekstraksi fitur teks.
- Mengubah string `(no genres listed)` menjadi `(no_genres_listed)` agar tetap dianggap sebagai satu token dalam proses pembentukan vektor fitur. Hal ini membantu mengurangi jumlah dimensi yang tidak perlu pada matriks hasil ekstraksi teks seperti TF-IDF.

Transformasi ini dilakukan untuk membersihkan dan menyeragamkan data teks sebelum masuk ke tahap vektorisasi.

### 3. Persiapan Data untuk Model Content-Based Filtering

#### a. Mengubah Genre Menjadi Vektor TF-IDF

Fitur `genres` kemudian diubah menjadi representasi numerik menggunakan teknik _TF-IDF (Term Frequency - Inverse Document Frequency)_. Teknik ini digunakan karena mampu memberi bobot pentingnya genre terhadap masing-masing film, serta mengurangi pengaruh genre yang terlalu sering muncul.

Hasil transformasi berupa matriks TF-IDF akan digunakan untuk mengukur tingkat kemiripan antar film berdasarkan genre-nya.

#### b. Menghitung Cosine Similarity

Setelah mendapatkan representasi TF-IDF, dilakukan perhitungan _cosine similarity_ untuk menilai tingkat kemiripan antar film. Matriks kesamaan ini menjadi dasar dalam pemberian rekomendasi pada model berbasis konten, di mana film yang paling mirip dengan film yang pernah disukai oleh pengguna akan direkomendasikan.

### 4. Persiapan Data untuk Model Collaborative Filtering

#### a. Penggabungan Data

Data `ratings` digabungkan dengan `movies` berdasarkan `movieId` untuk memperoleh informasi tambahan yang diperlukan, seperti judul dan genre film.

#### b. Encoding ID Pengguna dan Film

Agar data dapat digunakan dalam model pembelajaran mesin, ID pengguna (`userId`) dan ID film (`movieId`) diubah ke format numerik (integer) melalui proses _encoding_. Ini dilakukan karena model hanya dapat menerima input berupa angka.

#### c. Pengacakan Dataset

Pengacakan dataset dalam hal ini bertujuan agar model collaborative filtering yang kita buat nanti tidak bias urutan dan memastikan distribusi data untuk training dan validasi model menjadi lebih variatif.

#### d. Normalisasi Rating

Nilai rating yang diberikan oleh pengguna dinormalisasi ke dalam rentang 0–1. Normalisasi ini penting untuk mempercepat proses pelatihan model dan menghindari dominasi nilai besar terhadap hasil model.

#### e. Pembagian Data Pelatihan dan Validasi

Dataset kemudian dibagi menjadi dua bagian, yaitu 80% untuk pelatihan (_training_) dan 20% untuk validasi. Pembagian ini bertujuan untuk mengevaluasi kemampuan model dalam melakukan generalisasi terhadap data yang belum pernah dilihat sebelumnya.

### Catatan Tambahan

Pembersihan data seperti menghapus nilai kosong (_missing values_) atau data duplikat tidak dilakukan dalam proyek ini karena hasil pemeriksaan menunjukkan bahwa tidak terdapat nilai kosong maupun data ganda dalam dataset yang digunakan.

---

## Modeling

Setelah melakukan Data Preparation, selanjutnya adalah melakukan modelling machine learning. Modelling dalam sistem rekomendasi adalah proses membangun metode yang dapat memberikan rekomendasi berdasarkan preferensi dan kebutuhan pengguna. Dalam kasus ini, sistem rekomendasi yang dibuat menggunakan pendekatan Content-Based Filtering berbasis similarity dan Collaborative Filtering berbasis deep learning

### 1. Content-Based Filtering (Cosine Similarity)

Pendekatan pertama yang digunakan adalah **Content-Based Filtering**, yaitu metode yang memberikan rekomendasi berdasarkan kesamaan konten antar item. Dalam hal ini, yang digunakan sebagai fitur konten adalah _genres_ dari setiap film.

**Langkah-langkah:**

- Data pada kolom `genres` diubah menjadi representasi numerik menggunakan TF-IDF.
- Kemudian dihitung _cosine similarity_ antar film berdasarkan TF-IDF tersebut.
- Untuk setiap film yang pernah disukai oleh pengguna, sistem akan merekomendasikan Top-N film lain yang memiliki tingkat kemiripan tertinggi.

**Kelebihan:**

- Tidak membutuhkan data rating dari pengguna lain.
- Dapat memberikan rekomendasi kepada pengguna baru asalkan mereka pernah menyukai setidaknya satu film.

**Kekurangan:**

- Tidak bisa merekomendasikan film di luar kesamaan konten (genre).
- Tidak mampu menangkap selera pengguna secara keseluruhan, hanya fokus pada item yang mirip.

### 2. Collaborative Filtering (Deep Learning)

Pendekatan kedua adalah **Collaborative Filtering berbasis Deep Learning**. Metode ini belajar dari interaksi antara pengguna dan item (film) dalam bentuk rating, tanpa melihat konten dari film tersebut. Model ini digunakan untuk memprediksi rating yang mungkin diberikan pengguna terhadap film yang belum pernah ditonton. Dari hasil tersebut diambil **Top-10 film rekomendasi** dengan prediksi rating tertinggi.

**Langkah-langkah:**

- Melakukan encoding terhadap `userId` dan `movieId` menjadi integer.
- Normalisasi nilai rating ke skala 0–1.
- Data kemudian dibagi menjadi data pelatihan dan validasi (80:20).
- Model Deep Learning dibangun melalui Class RecommenderNet. model kemudian dibangun menggunakan embedding layer untuk data pengguna dan film, lalu digabungkan dan diproses melalui dense layers untuk memprediksi rating.
- Model kemudian di-_compile_ dengan hyperparameter tuning sebagai berikut: `loss: Binary Crossentropy`, `optimizer: adam (learning rate: 0.001)`, `metrics: Root Mean Squared Error`
- Model kemudian difit atau dilatih dengan `epoch` sebanyak `30` dan `batch size` sebesar `32`, model dilatih dengan data train dan divalidasi dengan data validation

**Kelebihan:**

- Dapat menangkap pola preferensi pengguna dari keseluruhan data.
- Rekomendasi lebih personal karena berdasarkan kebiasaan pengguna lain yang serupa.

**Kekurangan:**

- Tidak bisa memberikan rekomendasi untuk pengguna baru yang belum pernah memberi rating (_cold start_).
- Membutuhkan data interaksi yang cukup besar dan proses training yang lebih kompleks.

---

## Result

Pada bagian ini, akan ditampilkan contoh hasil rekomendasi yang didapat dari model.

### 1. Model Content-based Filtering

Rekomendasi untuk: **Jumanji (1995)**
**Genre**: Adventure, Children, Fantasy

| No  | Judul Film                                                 | Genre                      |
| --- | ---------------------------------------------------------- | -------------------------- |
| 1   | Chronicles of Narnia: The Lion, the Witch and the Wardrobe, The (2005)                         | Adventure Children Fantasy |
| 2   | Return to Oz (1985)	         | Adventure Children Fantasy |
| 3   | Indian in the Cupboard, The (1995)                              | Adventure Children Fantasy |
| 4   | Chronicles of Narnia: Prince Caspian, The (2008)                          | Adventure Children Fantasy |
| 5   | Golden Compass, The (2007)                     | Adventure Children Fantasy |
| 6   | Darby O'Gill and the Little People (1959)                                  | Adventure Children Fantasy |
| 7   | Escape to Witch Mountain (1975) | Adventure Children Fantasy |
| 8   | Magic in the Water (1995)                                        | Adventure Children Fantasy |
| 9   | NeverEnding Story, The (1984)                                | Adventure Children Fantasy |
| 10  | Water Horse: Legend of the Deep, The (2007)           | Adventure Children Fantasy |

### 2. Model Collaborative Filtering

Rekomendasi untuk User ID: **461**

**Film dengan Rating Tinggi dari User**

| No  | Judul Film                                | Genre                |
| --- | ----------------------------------------- | -------------------- |
| 1   | Christmas Story, A (1983) | Children Comedy               |
| 2   | Vampire Hunter D: Bloodlust (Banpaia hantâ D) (2000)       | Animation Fantasy Horror Sci-Fi                |
| 3   | Nine Queens (Nueve reinas) (2000)     | Crime Thriller |
| 4   | Dawn of the Dead (1978)                     | Drama Horror        |
| 5   | Kung Fu Hustle (Gong fu) (2004)          | Action Comedy            |

**Top 10 Rekomendasi Film untuk userId 461**

| No  | Judul Film                                 | Genre                        |
| --- | ------------------------------------------ | ---------------------------- |
| 1   | Notorious (1946)                      | Film-Noir Romance Thriller                    |
| 2   | Touch of Evil (1958)        | Crime Film-Noir Thriller                |
| 3   | Raise the Red Lantern (Da hong deng long gao gao gua) (1991)             | Drama                        |
| 4   | Splendor in the Grass (1961)                          | Romance               |
| 5   | Samouraï, Le (Godson, The) (1967)                       | Crime Drama Thriller                        |
| 6   | Memories of Murder (Salinui chueok) (2003) | Crime Drama Mystery Thriller |
| 7   | Persepolis (2007)                       | Drama        |
| 8   | Dr. Horrible's Sing-Along Blog (2008)              | Comedy Drama Musical Sci-Fi       |
| 9   | Cosmos (1980)                   | Documentary                        |
| 10  | Black Mirror (2011)                    | Drama Sci-Fi             |

---

## Evaluation

Pada bagian ini digunakan dua metrik evaluasi untuk menilai kinerja model sistem rekomendasi yang dibangun, yaitu **Root Mean Squared Error (RMSE)** dan **Precision@K**. Kedua metrik ini digunakan untuk mengukur performa model dari sisi regresi dan klasifikasi Top-N rekomendasi. RMSE untuk model Collaborative Filtering, sedangkan Precision@K untuk model Content-based Filtering

#### 1. Root Mean Squared Error (RMSE)

**RMSE** digunakan untuk mengevaluasi model **Collaborative Filtering** berbasis deep learning, karena model ini bertugas memprediksi nilai rating numerik yang diberikan pengguna terhadap suatu film. **RMSE** digunakan untuk mengukur seberapa baik model memprediksi rating numerik (relevan untuk model Collaborative Filtering berbasis deep learning).
Cara kerja RMSE adalah menghitung selisih kuadrat antara rating yang diprediksi dan rating sebenarnya, lalu mengambil akar dari nilai rata-rata tersebut. Nilai RMSE yang lebih kecil menunjukkan prediksi model yang lebih akurat.

**Formula:**

![rmse](https://media.geeksforgeeks.org/wp-content/uploads/20200622171741/RMSE1.jpg)

- Predicted: nilai rating yang diprediksi model
- Actual: nilai rating aktual dari pengguna
- N: jumlah total prediksi

#### 2. Precision@K

**Precision@K** digunakan untuk mengevaluasi **Content-Based Filtering** maupun hasil Top-N rekomendasi dari Collaborative Filtering, dengan melihat seberapa banyak rekomendasi yang relevan dari total rekomendasi yang diberikan. Cara kerjanya adalah menghitung proporsi item yang benar-benar relevan dari total rekomendasi yang ditampilkan kepada pengguna. Metrik ini sangat penting untuk sistem rekomendasi, karena pengguna hanya akan melihat sebagian kecil (Top-N) hasil.

#### Formula:

![Precision@k](https://cdn.prod.website-files.com/660ef16a9e0687d9cc27474a/662c4327f27ee08d3e4d4b34_65777ee1fd55288155f28d37_precision_recall_k2.png)

- K: Jumlah rekomendasi yang diberikan

### Hasil Evaluasi Model

#### Content-based Filtering

Pada bagian evaluasi content-based filtering ini, digunakan metrik precision@k. Kita mencoba melihat 10 rekomendasi film berdasarkan judul film Jumanji (1995).

Rekomendasi untuk: **Jumanji (1995)**
**Genre**: Adventure, Children, Fantasy

| No  | Judul Film                                                 | Genre                      | Relevan |
| --- | ---------------------------------------------------------- | -------------------------- | ------- |
| 1   | Chronicles of Narnia: The Lion, the Witch and the Wardrobe, The (2005)                         | Adventure Children Fantasy | Ya      |
| 2   | Return to Oz (1985)	         | Adventure Children Fantasy | Ya      |
| 3   | Indian in the Cupboard, The (1995)                              | Adventure Children Fantasy | Ya      |
| 4   | Chronicles of Narnia: Prince Caspian, The (2008)                          | Adventure Children Fantasy | Ya      |
| 5   | Golden Compass, The (2007)                     | Adventure Children Fantasy | Ya      |
| 6   | Darby O'Gill and the Little People (1959)                                  | Adventure Children Fantasy | Ya      |
| 7   | Escape to Witch Mountain (1975) | Adventure Children Fantasy | Ya      |
| 8   | Magic in the Water (1995)                                        | Adventure Children Fantasy | Ya      |
| 9   | NeverEnding Story, The (1984)                                | Adventure Children Fantasy | Ya      |
| 10  | Water Horse: Legend of the Deep, The (2007)           | Adventure Children Fantasy | Ya      |

Terlihat pada tabel di atas, bahwa Jumlah rekomendasi yang relevan adalah adalah 10. Oleh sebab itu, maka dapat disimpulkan `nilai precision@k untuk model ini adalah 10/10 atau 100%` (hasil dari jumlah rekomendasi yang relevan dibagi dengan total rekomendasi)

#### Collaborative Filtering

Pada bagian evaluasi collaborative filtering ini, digunakan metrik RMSE. Dari proses pelatihan dan validasi didapatkan grafik RMSE sebagai berikut.

![Screenshot 2025-05-31 at 16-46-06 Submission_Recommendation_System ipynb - Colab](https://github.com/user-attachments/assets/d8499d8e-cf3c-49c0-81ff-d36486f338ff)

Dari gambar grafik pelatihan tersebut RMSE dapat diketahui beberapa poin sebagai berikut:

- RMSE data train menurun secara konsisten dari sekitar 0.23 ke sekitar 0.185, menunjukkan bahwa model berhasil belajar dengan baik pada data pelatihan.
- RMSE data test awalnya juga menurun tajam hingga sekitar epoch ke-3 atau ke-4, tetapi kemudian cenderung stagnan dan sedikit fluktuatif di kisaran 0.20.
- Gap antara RMSE train dan test mulai melebar setelah epoch ke-5, yang mengindikasikan adanya kemungkinan overfitting—model terlalu menyesuaikan diri dengan data pelatihan dan kurang generalisasi ke data test.
- Diketahui nilai `RMSE akhir` untuk data `train` sebesar `0.1866`, sedangkan `nilai RMSE` untuk data `validation` sebesar `0.2023`

##### Menampilkan Rekomendasi

Rekomendasi untuk User ID: **461**

Film dengan Rating Tinggi dari User

| No  | Judul Film                                | Genre                |
| --- | ----------------------------------------- | -------------------- |
| 1   | Christmas Story, A (1983) | Children Comedy               |
| 2   | Vampire Hunter D: Bloodlust (Banpaia hantâ D) (2000)       | Animation Fantasy Horror Sci-Fi                |
| 3   | Nine Queens (Nueve reinas) (2000)     | Crime Thriller |
| 4   | Dawn of the Dead (1978)                     | Drama Horror        |
| 5   | Kung Fu Hustle (Gong fu) (2004)          | Action Comedy            |

Top 10 Rekomendasi Film untuk userId 461

| No  | Judul Film                                 | Genre                        |
| --- | ------------------------------------------ | ---------------------------- |
| 1   | Notorious (1946)                      | Film-Noir Romance Thriller                    |
| 2   | Touch of Evil (1958)        | Crime Film-Noir Thriller                |
| 3   | Raise the Red Lantern (Da hong deng long gao gao gua) (1991)             | Drama                        |
| 4   | Splendor in the Grass (1961)                          | Romance               |
| 5   | Samouraï, Le (Godson, The) (1967)                       | Crime Drama Thriller                        |
| 6   | Memories of Murder (Salinui chueok) (2003) | Crime Drama Mystery Thriller |
| 7   | Persepolis (2007)                       | Drama        |
| 8   | Dr. Horrible's Sing-Along Blog (2008)              | Comedy Drama Musical Sci-Fi       |
| 9   | Cosmos (1980)                   | Documentary                        |
| 10  | Black Mirror (2011)                    | Drama Sci-Fi             |

Dari tabel hasil rekomendasi di atas, dapat disimpulkan bahwa hasil rekomendasi cukup presisi dalam merekomendasikan film untuk userId 461. Terlihat bahwa film yang direkomendasikan setidaknya memiliki satu genre yang mirip dengan film yang diberi rating user. Rata-rata film yang direkomendasikan rilis di bawah tahun 2012.

---

**---Ini adalah bagian akhir laporan---**
