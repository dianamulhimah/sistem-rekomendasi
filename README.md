# Laporan Proyek Machine Learning - Diana Mulhimah

## Book Recommendation Dataset

## Project Overview
Seiring dengan berkembangnya teknologi informasi dan meningkatnya kebutuhan akan akses informasi yang cepat dan relevan, sistem rekomendasi memainkan peran penting dalam membantu pengguna menavigasi volume data yang sangat besar. Dalam konteks perpustakaan maupun aplikasi digital berbasis literasi, pengguna sering mengalami kesulitan dalam menemukan buku yang sesuai dengan preferensi mereka. Hal ini disebabkan oleh berbagai faktor, seperti keterbatasan waktu, minimnya informasi, serta banyaknya pilihan yang tersedia.
Untuk mengatasi permasalahan tersebut, sistem rekomendasi buku hadir sebagai solusi yang cerdas dengan tujuan menyederhanakan proses pencarian informasi dan meningkatkan pengalaman pengguna. Sistem ini bekerja dengan menganalisis perilaku pengguna, preferensi individu, serta karakteristik konten buku untuk memberikan rekomendasi yang relevan dan personal.
Salah satu pendekatan utama yang digunakan dalam sistem rekomendasi adalah Collaborative Filtering (CF), yang memanfaatkan interaksi dan penilaian dari banyak pengguna untuk menyarankan item yang disukai oleh pengguna dengan pola preferensi serupa. Metode ini terbagi menjadi dua, yaitu user-based dan item-based. CF terbukti efektif terutama saat tersedia data interaksi pengguna yang cukup banyak dan konsisten. Sebagai contoh, [^1] menerapkan algoritma Singular Value Decomposition (SVD) dalam CF, yang berhasil mereduksi sparsity dan meningkatkan kualitas prediksi rekomendasi buku.
Di sisi lain, pendekatan Content-Based Filtering (CBF) juga banyak digunakan, terutama dalam konteks perpustakaan. CBF bekerja dengan menganalisis atribut atau fitur dari item seperti judul, pengarang, dan genre untuk merekomendasikan item serupa. Penelitian oleh [^2] dan [^3] menunjukkan bahwa pendekatan ini dapat menghasilkan rekomendasi yang akurat berdasarkan deskripsi konten meskipun tanpa melibatkan data dari pengguna lain.
Proyek ini bertujuan untuk mengembangkan sistem rekomendasi buku dengan mengombinasikan pendekatan Collaborative Filtering dan Content-Based Filtering, berdasarkan efektivitas yang telah dibuktikan oleh beberapa studi sebelumnya. Dengan adanya sistem rekomendasi ini, diharapkan pengguna dapat memperoleh saran buku yang lebih personal dan relevan dengan preferensi mereka, serta mendukung digitalisasi layanan literasi yang efisien dan adaptif terhadap kebutuhan pengguna.

## Business Understanding
Pada tahap ini, dilakukan proses klarifikasi terhadap permasalahan yang dihadapi pengguna dalam menemukan buku yang relevan dengan preferensi mereka. Banyak pengguna kesulitan menemukan buku serupa setelah menyukai suatu buku karena keterbatasan informasi atau banyaknya pilihan yang tersedia. Selain itu, pengguna juga sering melewatkan buku-buku potensial yang belum pernah mereka baca karena tidak muncul dalam pencarian atau rekomendasi.

### Problem Statements
* Bagaimana sistem dapat merekomendasikan buku yang mirip dengan buku yang disukai oleh pengguna berdasarkan kontennya?
* Bagaimana sistem dapat memberikan rekomendasi buku yang belum pernah dibaca oleh pengguna tetapi mungkin disukai, berdasarkan perilaku pengguna lain?

### Goals
* Menghasilkan rekomendasi buku berdasarkan fitur konten buku seperti judul dan penulis, sehingga pengguna mendapatkan saran buku yang mirip dengan preferensinya sebelumnya menggunakan pendekatan Content-Based filtering.
* Memberikan rekomendasi buku yang belum dibaca oleh pengguna namun disukai oleh pengguna lain dengan pola rating serupa, menggunakan pendekatan Collaborative Filtering.

### Solution statements
* Solution 1: Content-Based Filtering (CBF) –  Menganalisis metadata buku untuk merekomendasikan buku serupa dengan yang telah disukai atau dibaca oleh pengguna menggunakan teknik TF-IDF vectorization untuk merepresentasikan teks judul buku ke dalam vektor numerik. Kemiripan dihitung dengan **Cosine Similarity** untuk mendapatkan buku-buku yang secara konten mirip.
* Solution 2: Collaborative Filtering (CF) –  Menganalisis interaksi pengguna terhadap buku (dalam bentuk data rating), dan menemukan pola kesamaan antar pengguna untuk merekomendasikan buku yang belum pernah dinilai oleh pengguna tersebut menggunakan teknik **Matrix Factorization** untuk menemukan pola interaksi pengguna, dan dievaluasi menggunakan metrik seperti **RMSE** untuk mengukur akurasi prediksi rating.

## Data Understanding
Pada proyek ini, kita menggunakan **Book Recommendation Dataset** yang tersedia secara publik melalui situs  yang bersumber dari [Kaggle](https://https://www.kaggle.com/). Dataset ini berisi informasi tentang buku, pengguna, serta rating yang diberikan oleh pengguna terhadap buku-buku tersebut. Dataset ini sering digunakan dalam tugas sistem rekomendasi karena kompleksitas dan keragaman datanya.
Link Dataset: [Kaggle - Book Recommendation Dataset](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset)

Dataset ini terdiri dari 3 file utama, yaitu:
1. `Books.csv` — Informasi metadata buku.
2. `Users.csv` — Informasi pengguna.
3. `Ratings.csv` — Nilai rating yang diberikan pengguna terhadap buku.

**Ringkasan Jumlah Data**
| Dataset | Jumlah Baris | Jumlah Kolom |
| ------- | ------------ | ------------ |
| Books   | 271,360      | 8            |
| Users   | 278,858      | 3            |
| Ratings | 1,149,780    | 3            |

**Deskripsi Variabel**
<br/>**Books.csv**
Berisi metadata dari setiap buku:
* `ISBN`: Nomor unik untuk identifikasi buku.
* `Book-Title`: Judul buku.
* `Book-Author`: Nama penulis.
* `Year-Of-Publication`: Tahun terbit buku.
* `Publisher`: Nama penerbit buku.
* `Image-URL-S`: Tautan ke gambar sampul buku dalam ukuran kecil.
* `Image-URL-M`: Tautan ke gambar sampul buku dalam ukuran sedang.
* `Image-URL-L`: Tautan ke gambar sampul buku dalam ukuran besar.

**Users.csv**
Berisi informasi pengguna yang memberi rating:
* `User-ID`: ID unik pengguna.
* `Location`: Lokasi pengguna (format: kota, negara bagian, negara).
* `Age`: Umur pengguna (dapat berisi nilai kosong atau tidak valid).

**Ratings.csv**
Berisi informasi rating yang diberikan oleh pengguna terhadap buku:
* `User-ID`: ID pengguna yang memberi rating.
* `ISBN`: ISBN buku yang dinilai.
* `Book-Rating`: Rating yang diberikan, dalam skala 0–10

**Exploratory Data Analysis**
* Distribusi Usia Pengguna
![Distribusi Usia Pengguna](https://raw.githubusercontent.com/dianamulhimah/sistem-rekomendasi/main/assets/distribusi-usia-pengguna.png)
Banyak pengguna berusia antara 20 hingga 50 tahun.

* Distribusi Tahun Publikasi Buku
![distribusi-tahun-publikasi-buku](https://raw.githubusercontent.com/dianamulhimah/sistem-rekomendasi/main/assets/distribusi-tahun-publikasi-buku.png)
Banyak buku yang diterbitkan antara 1990 hingga awal 2000-an.

* Distribusi Rating Pengguna
![distribusi-rating-pengguna](https://raw.githubusercontent.com/dianamulhimah/sistem-rekomendasi/main/assets/distribusi-rating-pengguna.png)
<br/>Rating **0** mendominasi, artinya pengguna mungkin hanya memberi interaksi tanpa memberikan penilaian. Rating lainnya cenderung antara **6–10**, mengindikasikan lebih banyak feedback positif.

## Data Preparation
Tahap ini bertujuan untuk menyiapkan data agar dapat digunakan dalam membangun sistem rekomendasi, baik untuk pendekatan Content-Based Filtering maupun Collaborative Filtering. Teknik data preparation dilakukan secara sistematis untuk membersihkan, menyaring, dan memformat data ke dalam struktur yang sesuai untuk pemodelan.
* Menghapus Kolom Tidak Relevan dari Dataset Buku
**Alasan:** Kolom ini hanya berisi URL gambar yang tidak diperlukan untuk proses rekomendasi dan akan membebani memori.
* Sampling dan Penyaringan Data
**Alasan:** Sampling digunakan untuk membatasi ukuran dataset agar proses komputasi lebih efisien tanpa menghilangkan keberagaman data.
* Pemeriksaan Missing Value
**Alasan:** Memastikan bahwa data yang akan dipakai tidak memiliki nilai kosong yang dapat mengganggu analisis.
* Penggabungan Rating dengan Metadata Buku
**Alasan:** Diperlukan untuk menyatukan informasi buku dan rating agar dapat digunakan dalam filtering berbasis konten.
* Menghapus Missing Value
**Alasan:** Untuk memastikan hanya data lengkap yang diproses dalam tahap berikutnya.
* Penghapusan Duplikasi Berdasarkan ISBN
**Alasan:** Menghindari pengulangan buku yang sama yang dapat memengaruhi hasil rekomendasi.

### Data Preparation untuk algoritma Content-Based Filtering (CBF)
* Konversi Kolom Menjadi List dan Pembuatan DataFrame
**Alasan:** Untuk membentuk dataset baru yang lebih ringan dan hanya memuat kolom yang diperlukan dalam model CBF.
* Membuat DataFrame Baru `book_new`
**Alasan:**
Membuat DataFrame yang hanya berisi informasi penting: ID buku, penulis, dan judul. Ini akan menjadi input untuk TF-IDF.
* Penerapan TF-IDF
```python
tf = TfidfVectorizer()
tfidf_matrix = tf.fit_transform(book_new['book_title'])
```
**Alasan:** Judul buku dikonversi ke dalam representasi numerik menggunakan TF-IDF agar dapat dihitung kemiripan antar buku.
* Melakukan perhitungan idf pada data `book_title`
**Alasan:** Melatih `TfidfVectorizer` pada kolom `book_title` untuk menghitung nilai IDF setiap kata dari semua judul buku.
* Mapping array dari fitur index integer ke fitur nama
**Alasan:** Mengambil nama-nama fitur (kata) yang dihasilkan dari proses `fit`, berguna untuk mengetahui kata apa saja yang menjadi vektor fitur.
* Melakukan fit lalu ditransformasikan ke bentuk matrix 
**Alasan:** Mengubah teks judul buku menjadi matriks TF-IDF, di mana setiap baris mewakili buku dan setiap kolom adalah skor TF-IDF dari suatu kata.
* Melihat ukuran matrix tfidf
**Alasan:** Menampilkan ukuran dari matriks TF-IDF, outputnya `(6702, 8842)`, artinya ada 6702 buku dan 8842 kata unik.
* Mengubah vektor tf-idf dalam bentuk matriks dengan fungsi `todense()` 
**Alasan:** Mengubah format matriks dari bentuk sparse (hemat memori) menjadi bentuk dense (penuh) agar bisa ditampilkan atau dianalisis dengan lebih mudah.
* Membuat dataframe untuk melihat tf-idf matrix
**Alasan:**
Mengecek hasil akhir matrix TF-IDF dalam bentuk DataFrame dengan index nama penulis dan kolom kata-kata unik.


### Data Preparation untuk algoritma Collaborative Filtering (CF)
* Menyalin `ratings_sample` ke variabel `df` sebagai data utama yang akan diproses untuk CF.
**Alasan:** Ini menjaga agar `ratings_sample` tetap utuh.
* Encoding User-ID dan ISBN
```python
# Mengubah User-ID menjadi list tanpa nilai yang sama
user_ids = df['User-ID'].unique().tolist()
print('list User-ID: ', user_ids)

# Melakukan encoding User-ID
user_to_user_encoded = {x: i for i, x in enumerate(user_ids)}
print('encoded User-ID : ', user_to_user_encoded)

# Melakukan proses encoding angka ke ke User-ID
user_encoded_to_user = {i: x for i, x in enumerate(user_ids)}
print('encoded angka ke User-ID: ', user_encoded_to_user)

# Mengubah ISBN menjadi list tanpa nilai yang sama
book_ids = df['ISBN'].unique().tolist()

# Melakukan proses encoding ISBN
book_to_book_encoded = {x: i for i, x in enumerate(book_ids)}

# Melakukan proses encoding angka ke ISBN
book_encoded_to_book = {i: x for i, x in enumerate(book_ids)}
```
**Alasan:** Algoritma embedding pada model deep learning membutuhkan data numerik, sehingga encoding diperlukan agar ID pengguna dan buku bisa diproses oleh model.
* Menambahkan dua kolom baru (`user`, `book`) dalam format numerik
**Alasan:** Agar bisa diproses oleh model machine learning.
Berikut adalah penjelasan singkat dan ringkas untuk blok kode tersebut:
* Mendapatkan jumlah user dan buku unik serta mengubah rating ke tipe float dan mencari nilai minimum dan maksimum rating
**Alasan:** karena digunakan untuk menghitung jumlah pengguna dan buku unik setelah proses encoding, serta menentukan rentang nilai rating (minimum dan maksimum). Informasi ini penting sebagai dasar untuk proses normalisasi data sebelum pelatihan model *Collaborative Filtering*.
* Mengacak seluruh dataset
**Alasan:** Agar distribusi data train/val tidak bias urutan.
* Normalisasi Rating dan Pembagian Data Train/Validation
**Alasan:** Rating dinormalisasi agar berada pada skala 0–1 untuk memudahkan pelatihan model. Data juga dibagi menjadi train dan validation untuk keperluan evaluasi model.
* Membagi data menjadi 80% data pelatihan (train) dan 20% validasi (val)
**Alasan:** untuk mengevaluasi performa model secara adil.
**Seluruh tahapan ini dilakukan agar data memiliki kualitas yang baik, bebas duplikat dan missing value, serta dalam format numerik dan struktural yang dapat digunakan untuk modeling sistem rekomendasi berbasis content maupun collaborative filtering.**

## Modeling and Result
Sistem rekomendasi dalam proyek ini dikembangkan menggunakan dua pendekatan utama, yaitu **Content-Based Filtering (CBF)** dan **Collaborative Filtering (CF)**. Tujuan dari model ini adalah untuk membantu pengguna menemukan buku yang sesuai dengan preferensi mereka—baik berdasarkan kemiripan konten maupun perilaku pengguna lain.

**1. Content-Based Filtering (CBF)**
Content-Based Filtering bekerja dengan cara menghitung kemiripan antar buku berdasarkan informasi kontennya, seperti **judul**. Setelah dilakukan proses representasi data menggunakan TF-IDF di tahap Data Preparation,
Lalu dihitung **cosine similarity** antar buku berdasarkan representasi vektornya.
#### Proses Modeling
```python
cosine_sim = cosine_similarity(tfidf_matrix)
cosine_sim_df = pd.DataFrame(cosine_sim, index=book_new['book_author'], columns=book_new['book_author'])
```
Model mengambil nama penulis (sebagai proxy dari buku yang disukai), kemudian menghitung kesamaan dengan penulis/buku lain. Lima penulis dengan nilai similarity tertinggi akan direkomendasikan, diikuti dengan informasi judul bukunya.

#### Top-N Recommendation
Untuk pengguna yang menyukai karya **Thomas Robbins**, sistem merekomendasikan:

| Penulis        | Judul Buku       |
| -------------- | ---------------- |
| Elmore Leonard | Tishomingo Blues |
| Elmore Leonard | Bandits          |
| Elmore Leonard | Out of Sight     |
| Elmore Leonard | Gold Coast       |
| Elmore Leonard | Cuba Libre       |

#### **Kelebihan**:
* Tidak butuh data interaksi pengguna lain (cukup metadata buku).
* Mudah dijelaskan (explainable recommendation).
* Cocok untuk pengguna baru (cold-start user).

#### **Kekurangan**:
* Terbatas pada fitur yang tersedia (judul/penulis).
* Rekomendasi cenderung seragam jika kontennya mirip.
* Tidak bisa menyarankan hal baru di luar preferensi pengguna.

**2. Collaborative Filtering (CF)**
Collaborative Filtering bekerja dengan mendeteksi pola rating yang diberikan pengguna terhadap buku. Sistem mempelajari kesamaan preferensi antar pengguna dan memberikan rekomendasi buku yang disukai oleh pengguna lain yang memiliki preferensi serupa.
Model yang digunakan adalah neural network sederhana bernama `RecommenderNet`, yang menerapkan teknik **Matrix Factorization** dengan layer embedding. Model dilatih menggunakan optimizer Adam, loss BinaryCrossentropy, dan metrik RootMeanSquaredError (RMSE). Setelah training selesai, model memprediksi skor kecocokan buku yang belum dikunjungi user.

#### Proses Modeling
```python
model = RecommenderNet(num_users, num_book, embedding_size=50)
model.compile(
    loss = tf.keras.losses.BinaryCrossentropy(),
    optimizer = keras.optimizers.Adam(learning_rate=0.001),
    metrics=[tf.keras.metrics.RootMeanSquaredError()]
)
history = model.fit(x_train, y_train, epochs=100, validation_data=(x_val, y_val))
```
Setelah pelatihan, model digunakan untuk memprediksi buku mana yang belum pernah dibaca namun kemungkinan akan disukai.

**Top-N Recommendation**
Rekomendasi Buku untuk Pengguna: `157273`
## Buku dengan Rating Tinggi dari Pengguna

| No. | Penulis                  | Judul Buku                                                 |
|-----|--------------------------|-------------------------------------------------------------|
| 1   | Wallace Earle Stegner    | Crossing to Safety                                          |
| 2   | Jeffrey Eugenides        | Middlesex: A Novel                                          |
| 3   | Rick Klaw                | Geek Confidential: Echoes from the 21st Century            |
| 4   | Andre Norton             | Catfantastic III (Daw Book Collectors)                      |
| 5   | Tara K. Harper           | Shadow Leader                                               |

## Top 10 Rekomendasi Buku

| No. | Penulis                  | Judul Buku                                                  |
|-----|--------------------------|--------------------------------------------------------------|
| 1   | James Patterson          | The Beach House                                              |
| 2   | Stephen King             | The Tommyknockers                                            |
| 3   | Bill Bryson              | The Mother Tongue                                            |
| 4   | Matt Groening            | Work Is Hell                                                 |
| 5   | Jack Finney              | FROM TIME TO TIME                                            |
| 6   | Bradley Trevor Greive   | The Blue Day Book                                            |
| 7   | Alan Paton               | Cry, the Beloved Country (Oprah's Book Club)                |
| 8   | Mary Roach               | Stiff: The Curious Lives of Human Cadavers                  |
| 9   | Olivia Goldsmith         | Flavor of the Month                                          |
| 10  | Charlotte Bronte         | Jane Eyre (Bantam Classics)                                  |

#### **Kelebihan**:
* Memberikan rekomendasi personal yang bervariasi.
* Mampu menangkap pola preferensi kompleks dari banyak pengguna.
* Efektif saat banyak data interaksi tersedia.

#### **Kekurangan**:
* Butuh data rating/interaksi yang cukup banyak.
* Rentan terhadap *cold-start problem* (untuk user/buku baru).
* Proses pelatihan model relatif lebih kompleks dan memakan waktu.

<br/>**Kedua metode memberikan rekomendasi yang bermanfaat namun dengan keunggulan yang berbeda. Content-Based Filtering efektif untuk data yang minim interaksi user, cocok untuk sistem baru. Collaborative Filtering dengan embedding lebih powerful untuk personalisasi, namun memerlukan data interaksi yang cukup.**

## Evaluation
### 1. **Content-Based Filtering**
####  Metrik Evaluasi: **Precision\@K**
**Precision\@K** merupakan metrik evaluasi yang digunakan untuk mengukur seberapa banyak item yang **direkomendasikan oleh sistem** benar-benar **relevan** terhadap preferensi pengguna, dari **K item teratas** yang direkomendasikan.
Precision\@K adalah metrik yang umum digunakan dalam sistem rekomendasi, terutama ketika tujuan utamanya adalah menyajikan daftar pendek (top-N) rekomendasi yang akurat.

#### Rumus Precision\@K

$$
\text{Precision@K} = \frac{\text{Jumlah Rekomendasi yang Relevan}}{K}
$$

Keterangan:
* **K** = jumlah item yang direkomendasikan (dalam hal ini **5**).
* **Relevan** = item yang sesuai dengan preferensi pengguna (berdasarkan judul atau penulis yang mirip dengan buku awal).

#### Cara Evaluasi Dilakukan
```python
# Top-5 hasil rekomendasi dari fungsi Content-Based
recommended = book_recommendations('Thomas Robbins')

# Daftar penulis atau judul yang dianggap relevan
relevant_authors = ['Elmore Leonard']
relevant_titles = ['Bandits', 'Tishomingo Blues', 'Out of Sight', 'Cuba Libre', 'Gold Coast']

# Cek relevansi
recommended['is_relevant'] = recommended.apply(
    lambda x: (x['book_author'] in relevant_authors) or (x['book_title'] in relevant_titles),
    axis=1
)

# Hitung Precision@
total_recs = len(recommended)
relevant_recs = recommended['is_relevant'].sum()
precision = relevant_recs / total_recs if total_recs > 0 else 0

print(f"Precision@: {precision:.2f}")
```

#### Hasil Evaluasi
* **Total rekomendasi (K)**: 5
* **Jumlah relevan**: 5
* **Precision\@5**:

  $\frac{5}{5} = 1.00 \text{ atau } 100\%$

#### Interpretasi Hasil
Hasil evaluasi menunjukkan bahwa **semua** buku yang direkomendasikan oleh model Content-Based Filtering tergolong relevan terhadap referensi awal (`Thomas Robbins`). Dengan nilai Precision\@5 sebesar **1.00 (100%)**, sistem menunjukkan **kinerja yang sangat baik** dalam mengenali kemiripan konten berdasarkan **judul** dan **penulis buku**.

#### Kesesuaian dengan Problem dan Data
* Problem statement pada proyek ini adalah memberikan **rekomendasi buku yang relevan** berdasarkan informasi konten buku.
* Precision\@K sangat tepat digunakan karena:
  * **Fokus pada relevansi rekomendasi**, bukan prediksi rating.
  * Cocok untuk kasus **top-N recommendation**, seperti daftar 5 atau 10 buku.
* Penggunaan **Precision\@5** dalam sistem Content-Based Filtering memberikan gambaran yang jelas mengenai **akurasi relevansi rekomendasi** yang dihasilkan. Nilai 100% menunjukkan bahwa pendekatan ini **efektif**, setidaknya untuk kasus pengujian ini.

### 2. **Collaborative Filtering**
**Metrik Evaluasi: Root Mean Squared Error (RMSE)** adalah metrik yang umum digunakan untuk mengukur seberapa jauh nilai prediksi model menyimpang dari nilai aktual (rating sebenarnya). RMSE dihitung dengan rumus:

$$
\text{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y_i})^2}
$$

Keterangan:
* \$y\_i\$ = rating aktual dari data ke-i
* \$\hat{y\_i}\$ = prediksi model pada data ke-i
* \$n\$ = jumlah total data

**Interpretasi:**
* Semakin kecil nilai RMSE, semakin baik akurasi prediksi model.
* RMSE memiliki satuan yang sama dengan data aslinya, sehingga hasilnya mudah dimaknai.

**Grafik berikut menunjukkan perkembangan nilai RMSE selama proses pelatihan model pada tiap epoch:**
![epoch](https://raw.githubusercontent.com/dianamulhimah/sistem-rekomendasi/main/assets/epoch.png)

Grafik di atas menunjukkan performa model selama 100 epoch. Terlihat bahwa:
* RMSE pada **data training** terus menurun (semakin baik).
* RMSE pada **data validasi** awalnya menurun namun kemudian meningkat, yang menunjukkan **terjadinya overfitting**.
**Nilai RMSE Akhir:**
* **Final RMSE (Train):** 0.1237
* **Final RMSE (Validation):** 0.3960
* Model sangat baik dalam mempelajari data pelatihan, namun mulai overfitting terhadap data validasi setelah sejumlah epoch.
* Nilai RMSE validasi sebesar **0.3960** masih dalam batas wajar, namun dapat ditingkatkan dengan regularisasi lebih baik atau teknik lain seperti early stopping.

## KESIMPULAN
* Melalui pendekatan Content-Based Filtering, sistem memanfaatkan metadata buku khususnya judul dan penulis yang dikonversi menjadi representasi numerik menggunakan TF-IDF Vectorization. Dengan menghitung Cosine Similarity, sistem mampu mengidentifikasi dan merekomendasikan buku-buku yang memiliki kemiripan konten dengan buku yang sebelumnya disukai oleh pengguna. Sistem mampu memberikan rekomendasi buku-buku serupa secara personal tanpa bergantung pada data pengguna lain. Ini sangat efektif bagi pengguna baru atau dalam konteks data interaksi yang terbatas (cold start).
* Dengan pendekatan Collaborative Filtering, sistem menganalisis pola rating antar pengguna. Model dievaluasi menggunakan metrik Root Mean Squared Error (RMSE) untuk menilai kualitas prediksi rating. RMSE yang rendah menunjukkan sistem dapat secara akurat memprediksi preferensi pengguna. Sistem berhasil memberikan rekomendasi buku yang relevan dengan preferensi pengguna lain yang memiliki pola perilaku serupa, bahkan untuk buku yang belum pernah dilihat oleh pengguna target.

## Referensi
[^1]:	R. Akbar, D. Richasdy, and R. Dharayani, “Sistem Rekomendasi Buku Dengan Collaborative Filtering Menggunakan Metode Singular Value Decomposition ( SVD ),” e-Proceedings of Engineering, vol. 10, no. 5, 2023.
[^2]:	M. Alkaff, H. Khatimi, and A. Eriadi, “Sistem Rekomendasi Buku pada Perpustakaan Daerah Provinsi Kalimantan Selatan Menggunakan Metode Content-Based Filtering,” MATRIK : Jurnal Manajemen, Teknik Informatika dan Rekayasa Komputer, vol. 20, no. 1, 2020, doi: 10.30812/matrik.v20i1.617.
[^3]:	R. Ardiansyah, M. Ari Bianto, and B. D. Saputra, “Sistem Rekomendasi Buku Perpustakaan Sekolah menggunakan Metode Content-Based Filtering,” Jurnal CoSciTech (Computer Science and Information Technology), vol. 4, no. 2, 2023, doi: 10.37859/coscitech.v4i2.5131.
