# Laporan Proyek Machine Learning - Diana Mulhimah

## Project Overview

Seiring dengan berkembangnya teknologi informasi dan meningkatnya kebutuhan akan akses informasi yang cepat dan relevan, sistem rekomendasi memainkan peran penting dalam membantu pengguna menavigasi volume data yang sangat besar. Dalam konteks perpustakaan maupun aplikasi digital berbasis literasi, pengguna sering mengalami kesulitan dalam menemukan buku yang sesuai dengan preferensi mereka. Hal ini disebabkan oleh berbagai faktor, seperti keterbatasan waktu, minimnya informasi, serta banyaknya pilihan yang tersedia.
Untuk mengatasi permasalahan tersebut, sistem rekomendasi buku hadir sebagai solusi yang cerdas dengan tujuan menyederhanakan proses pencarian informasi dan meningkatkan pengalaman pengguna. Sistem ini bekerja dengan menganalisis perilaku pengguna, preferensi individu, serta karakteristik konten buku untuk memberikan rekomendasi yang relevan dan personal.
Salah satu pendekatan utama yang digunakan dalam sistem rekomendasi adalah Collaborative Filtering (CF), yang memanfaatkan interaksi dan penilaian dari banyak pengguna untuk menyarankan item yang disukai oleh pengguna dengan pola preferensi serupa. Metode ini terbagi menjadi dua, yaitu user-based dan item-based. CF terbukti efektif terutama saat tersedia data interaksi pengguna yang cukup banyak dan konsisten. Sebagai contoh, [^1] menerapkan algoritma Singular Value Decomposition (SVD) dalam CF, yang berhasil mereduksi sparsity dan meningkatkan kualitas prediksi rekomendasi buku.
Di sisi lain, pendekatan Content-Based Filtering (CBF) juga banyak digunakan, terutama dalam konteks perpustakaan. CBF bekerja dengan menganalisis atribut atau fitur dari item seperti judul, pengarang, dan genre untuk merekomendasikan item serupa. Penelitian oleh [^2] dan [^3] menunjukkan bahwa pendekatan ini dapat menghasilkan rekomendasi yang akurat berdasarkan deskripsi konten meskipun tanpa melibatkan data dari pengguna lain.
Proyek ini bertujuan untuk mengembangkan sistem rekomendasi buku dengan mengombinasikan pendekatan Collaborative Filtering dan Content-Based Filtering, berdasarkan efektivitas yang telah dibuktikan oleh beberapa studi sebelumnya. Dengan adanya sistem rekomendasi ini, diharapkan pengguna dapat memperoleh saran buku yang lebih personal dan relevan dengan preferensi mereka, serta mendukung digitalisasi layanan literasi yang efisien dan adaptif terhadap kebutuhan pengguna.

## Business Understanding
Pada tahap ini, dilakukan proses klarifikasi terhadap permasalahan yang dihadapi pengguna dalam menemukan buku yang relevan dengan preferensi mereka. Banyak pengguna kesulitan menemukan buku serupa setelah menyukai suatu buku karena keterbatasan informasi atau banyaknya pilihan yang tersedia. Selain itu, pengguna juga sering melewatkan buku-buku potensial yang belum pernah mereka baca karena tidak muncul dalam pencarian atau rekomendasi.

### Problem Statements
*	Bagaimana sistem dapat merekomendasikan buku yang mirip dengan buku yang disukai oleh pengguna berdasarkan kontennya?
* Bagaimana sistem dapat memberikan rekomendasi buku yang belum pernah dibaca oleh pengguna tetapi mungkin disukai, berdasarkan perilaku pengguna lain?

### Goals
* Menghasilkan rekomendasi buku berdasarkan fitur konten buku seperti judul dan penulis, sehingga pengguna mendapatkan saran buku yang mirip dengan preferensinya sebelumnya menggunakan pendekatan Content-Based filtering.
* Memberikan rekomendasi buku yang belum dibaca oleh pengguna namun disukai oleh pengguna lain dengan pola rating serupa, menggunakan pendekatan Collaborative Filtering.

### Solution statements
* Solution 1: Content-Based Filtering (CBF) Menganalisis metadata buku untuk merekomendasikan buku serupa dengan yang telah disukai atau dibaca oleh pengguna. Teknik yang digunakan yaitu TF-IDF vectorization dan Cosine similarity.
* Solution 2: Collaborative Filtering (CF) – Menganalisis interaksi pengguna terhadap buku (dalam bentuk data rating), dan menemukan pola kesamaan antar pengguna untuk merekomendasikan buku yang belum pernah dinilai oleh pengguna tersebut. Teknik yang digunakan evaluasi dengan metrik RMSE.

## Data Understanding
Paragraf awal bagian ini menjelaskan informasi mengenai jumlah data, kondisi data, dan informasi mengenai data yang digunakan. Sertakan juga sumber atau tautan untuk mengunduh dataset. Contoh: UCI Machine Learning Repository.

Selanjutnya, uraikanlah seluruh variabel atau fitur pada data. Sebagai contoh:

Variabel-variabel pada Restaurant UCI dataset adalah sebagai berikut:

accepts : merupakan jenis pembayaran yang diterima pada restoran tertentu.
cuisine : merupakan jenis masakan yang disajikan pada restoran.
dst
Rubrik/Kriteria Tambahan (Opsional):

Melakukan beberapa tahapan yang diperlukan untuk memahami data, contohnya teknik visualisasi data beserta insight atau exploratory data analysis.


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
![Distribusi Usia Pengguna](https://github.com/dianamulhimah/sistem-rekomendasi/blob/main/assets/distribusi-usia-pengguna.png?raw=true)
Banyak pengguna berusia antara 20 hingga 50 tahun.

* Distribusi Tahun Publikasi Buku
![distribusi-tahun-publikasi-buku](https://github.com/dianamulhimah/sistem-rekomendasi/blob/main/assets/distribusi-tahun-publikasi-buku.png?raw=true)
Banyak buku yang diterbitkan antara 1990 hingga awal 2000-an.

* Distribusi Rating Pengguna
![distribusi-rating-pengguna](https://github.com/dianamulhimah/sistem-rekomendasi/blob/main/assets/distribusi-usia-pengguna.png?raw=true)
<br/>Rating **0** mendominasi, artinya pengguna mungkin hanya memberi interaksi tanpa memberikan penilaian. Rating lainnya cenderung antara **6–10**, mengindikasikan lebih banyak feedback positif.

## Data Preparation
Pada tahap ini, dilakukan serangkaian proses untuk menyiapkan data agar dapat digunakan dalam pembuatan sistem rekomendasi. Tahapan data preparation ini mencakup pembersihan data (menghapus duplikat), konversi data menjadi format yang lebih fleksibel, dan pemilahan data menjadi subset yang relevan.
* Menghapus Kolom Tidak Relevan dari Dataset `books`
```python
books = books.drop(columns=['Image-URL-S', 'Image-URL-M', 'Image-URL-L'])
```
Alasan: Kolom `Image-URL-S`, `Image-URL-M`, dan `Image-URL-L` berisi tautan gambar sampul buku yang tidak dibutuhkan dalam analisis. Penghapusan kolom ini bertujuan untuk mengurangi dimensi dataset dan fokus pada fitur yang relevan seperti judul buku, penulis, tahun terbit, dan penerbit.

* Sampling dan Penyaringan Data
<br/>Alasan: Sampling 10.000 Pengguna Unik, dilakukan pengambilan sampel acak 10.000 pengguna untuk mengurangi ukuran data dan mempercepat proses analisis tanpa kehilangan keberagaman pengguna. Menyaring Data Rating Berdasarkan Pengguna Terpilih, data rating difilter agar hanya memuat penilaian dari 10.000 pengguna yang sudah dipilih, sehingga data lebih fokus dan relevan. Sampling 10.000 Buku dari Data Rating dipilih secara acak 10.000 buku berdasarkan rating dari pengguna terpilih untuk menjaga variasi buku sekaligus mengurangi beban pemrosesan. Pembatasan Data Rating Maksimal 10.000 Baris jumlah data rating dibatasi maksimal 10.000 baris agar proses komputasi lebih cepat dan efisien.Penyelarasan Dataset Buku dan Pengguna, dataset buku dan pengguna disesuaikan agar hanya berisi data yang terkait dengan rating terpilih demi menjaga konsistensi dan relevansi data.

* Pemeriksaan Missing Values pada `ratings_sample`
```python
ratings_sample.isnull().sum()
```
Hasil: Semua nilai dalam `ratings_sample` terdeteksi **tidak ada yang kosong (null)**.

* Penggabungan Data Rating dengan Informasi Buku
```python
all_books = pd.merge(
    ratings_sample,
    books[['ISBN', 'Book-Title', 'Book-Author']],
    on='ISBN',
    how='left'
)
```
Alasan: Menggabungkan informasi judul dan penulis buku untuk memudahkan analisis, visualisasi, serta pembangunan sistem rekomendasi berbasis konten (Content-Based Filtering).

* Menghapus Missing Value
```python
all_books_clean = all_books.dropna()
````
<br/>Alasan: Pada tahap ini, data `all_books` dibersihkan dengan menghapus semua baris yang mengandung nilai kosong (missing value). Proses ini dilakukan menggunakan fungsi `dropna()` pada pandas. Setelah pembersihan, dataset `all_books_clean` hanya berisi data lengkap tanpa missing value, yaitu sebanyak 8960 baris. Hal ini memastikan data siap untuk analisis selanjutnya tanpa masalah data yang hilang.

* Menyalin Dataset ke Variabel Baru
```
preparation = all_books_clean
```
Dataset `all_books_clean` disalin ke variabel `preparation` untuk menjaga agar data asli tetap utuh dan tidak terpengaruh oleh proses preprocessing selanjutnya.
<br/>Alasan: Praktik ini umum dilakukan agar apabila terjadi kesalahan selama manipulasi data, kita masih memiliki salinan data awal sebagai backup.

* Menyortir Dataset berdasarkan ISBN
```python
preparation.sort_values('ISBN')
```
Dataset diurutkan berdasarkan kolom `'ISBN'`, meskipun hasilnya tidak disimpan kembali ke variabel `preparation` karena tidak menggunakan parameter `inplace=True` atau penugasan ulang.
<br/>Alasan: Menyortir data bisa membantu dalam proses identifikasi data duplikat atau eksplorasi manual. Namun pada implementasi ini hasilnya tidak berpengaruh langsung ke data utama.

* Menghapus Duplikasi Berdasarkan ISBN
```python
preparation = preparation.drop_duplicates('ISBN')
```
Penjelasan Dataset difilter agar tidak mengandung ISBN yang sama (duplikat). ISBN merupakan identifier unik untuk setiap buku, sehingga setiap entri harus unik.
<br/>Alasan: Menghindari redundansi data yang bisa mempengaruhi akurasi analisis dan rekomendasi buku di tahap selanjutnya.

* Mengonversi Series Menjadi List
```python
book_id = preparation['ISBN'].tolist()
book_author = preparation['Book-Author'].tolist()
book_title = preparation['Book-Title'].tolist()
```
Penjelasan: Tiga kolom penting yaitu `ISBN`, `Book-Author`, dan `Book-Title` diubah dari bentuk pandas Series ke list Python.
<br/>Alasan: List seringkali lebih mudah digunakan untuk proses pemetaan, perulangan eksplisit, dan pembuatan struktur data baru seperti dictionary atau DataFrame baru.

* Validasi Ukuran Dataset
```python
print(len(book_id))
print(len(book_author))
print(len(book_title))
```
Mencetak panjang masing-masing list untuk memastikan bahwa seluruh data memiliki jumlah baris yang sama (6702).
<br/>Alasan: Validasi ini penting untuk memastikan bahwa tidak terjadi kehilangan data atau ketidaksesuaian saat proses konversi dari Series ke List dan pembuatan DataFrame baru.

* Membuat DataFrame Baru dengan Kolom Tertentu
```python
book_new = pd.DataFrame({
    'id': book_id,
    'book_author': book_author,
    'book_title': book_title
})
```
Dibuat DataFrame baru `book_new` yang hanya memuat kolom `id` (dari ISBN), `book_author`, dan `book_title`.
<br/>Alasan: Struktur ini lebih ringan dan fokus untuk digunakan pada sistem rekomendasi, serta memudahkan pencocokan dan filtering data.

## Modeling
Sistem rekomendasi ini dibuat untuk membantu pengguna menemukan buku-buku yang sesuai dengan minat dan preferensi mereka berdasarkan data yang tersedia, seperti judul buku, penulis, dan interaksi pengguna terhadap buku tersebut.
1. **Content-Based Filtering (TF-IDF + Cosine Similarity)**
<br/>Content-Based Filtering merekomendasikan item berdasarkan kemiripan fitur antar item yang sebelumnya disukai oleh pengguna.
**Proses Modeling**
* Menggunakan TF-IDF Vectorizer untuk mengekstrak fitur teks dari kolom `book_title`.
* Membentuk matriks fitur TF-IDF untuk tiap judul buku.
* Menghitung cosine similarity antar buku berdasarkan fitur tersebut.
* Memberikan rekomendasi buku yang mirip dengan buku atau penulis yang dipilih pengguna.
* <br/>**Top-N Recommendation**
<br/>Misalnya untuk pengguna yang menyukai penulis *Thomas Robbins*, rekomendasi buku teratas yang mirip diberikan sebagai berikut:

| book_author     | book_title       |
|-----------------|------------------|
| Elmore Leonard  | Tishomingo Blues |
| Elmore Leonard  | Bandits          |
| Elmore Leonard  | Out of Sight     |
| Elmore Leonard  | Gold Coast       |
| Elmore Leonard  | Cuba Libre       |

2. **Collaborative Filtering RecommenderNet (Neural Network Embedding)**
<br/>Collaborative Filtering didasarkan pada interaksi pengguna terhadap item. Sistem merekomendasikan buku yang disukai oleh pengguna lain yang memiliki preferensi serupa.
**Proses Modeling**
* Membuat model embedding untuk user dan buku dengan ukuran embedding 50.
* model  menggunakan pendekatan Matrix Factorization (melalui embedding) yang merupakan inti dari RecommenderNet.
* Model dilatih menggunakan optimizer Adam, loss BinaryCrossentropy, dan metrik RootMeanSquaredError (RMSE).
* Setelah training selesai, model memprediksi skor kecocokan buku yang belum dikunjungi user.
* <br/>**Top-N Recommendation**
<br/>Recommendations for User: 98484
<br/>Books with High Ratings from User

| Penulis           | Judul Buku                                                     |
|-------------------|----------------------------------------------------------------|
| Bill Bryson       | The Mother Tongue                                              |
| Martin Cruz Smith | Red Square                                                    |
| Edward Gorey      | The Haunted Looking Glass: Ghost Stories (New York Review Books Classics) |
| Robert McCloskey  | Homer Price                                                   |
| Aldous Huxley     | Brave New World                                               |

**Top 10 Book Recommendations**

| No | Penulis                  | Judul Buku                                            |
|----|--------------------------|------------------------------------------------------|
| 1  | James Patterson          | The Beach House                                      |
| 2  | Stephen King             | The Tommyknockers                                    |
| 3  | Matt Groening            | Work Is Hell                                        |
| 4  | Bradley Trevor Greive    | The Blue Day Book                                   |
| 5  | Alan Paton               | Cry, the Beloved Country (Oprah's Book Club)       |
| 6  | Alex Kava                | The Soul Catcher: A Maggie O'Dell Novel             |
| 7  | Mary Roach               | Stiff: The Curious Lives of Human Cadavers          |
| 8  | Olivia Goldsmith         | Flavor of the Month                                 |
| 9  | Anna Davis               | Cheet (Plume Books)                                 |
| 10 | Charlotte Bronte         | Jane Eyre (Bantam Classics)                          |

**Kelebihan dan Kekurangan Pendekatan**
| Pendekatan                  | Kelebihan                                              | Kekurangan                                          |
| --------------------------- | ------------------------------------------------------ | --------------------------------------------------- |
| **Content-Based Filtering** | - Mudah diimplementasikan                              | - Sulit menangani cold-start user tanpa preferensi  |
|                             | - Tidak membutuhkan data interaksi user secara luas    | - Rekomendasi terbatas pada fitur yang diekstrak    |
|                             | - Hasil rekomendasi mudah dijelaskan                   | - Kurang variatif jika fitur buku sangat mirip      |
| **Collaborative Filtering** | - Dapat menangkap pola preferensi user secara kompleks | - Membutuhkan data interaksi user yang cukup banyak |
|
* Kedua metode memberikan rekomendasi yang bermanfaat namun dengan keunggulan yang berbeda.
* Content-Based Filtering efektif untuk data yang minim interaksi user, cocok untuk sistem baru.
* Collaborative Filtering dengan embedding lebih powerful untuk personalisasi, namun memerlukan data interaksi yang cukup.

## Evaluation
1. **Content-Based Filtering**
Pada proyek sistem rekomendasi berbasis **Content-Based Filtering** ini, dilakukan evaluasi terhadap hasil rekomendasi menggunakan metrik **Precision\@N**, untuk mengetahui seberapa relevan hasil rekomendasi sistem terhadap preferensi pengguna. Metrik ini sangat sesuai digunakan pada sistem rekomendasi karena tidak memerlukan label eksplisit seperti pada supervised learning. Cocok untuk mengevaluasi kualitas Top-N rekomendasi. Fokus pada relevansi item yang diberikan kepada pengguna.

**Metrik yang Digunakan: Precision\@5**
**Precision\@N** digunakan untuk mengukur seberapa banyak item yang relevan dari total item yang direkomendasikan:

$$
\text{Precision@N} = \frac{|\text{Item relevan dan direkomendasikan}|}{N}
$$

* **N = 5**, yaitu jumlah buku yang direkomendasikan untuk user.
* **Item relevan** adalah buku yang memiliki kemiripan konten tinggi dengan buku yang pernah disukai oleh pengguna.

**Metode yang Digunakan**
* Menggunakan `TfidfVectorizer` untuk mengubah judul buku menjadi vektor numerik berdasarkan frekuensi kata.
* Kemudian dihitung cosine similarity antar buku.
* Sistem merekomendasikan 5 buku teratas yang paling mirip dengan buku yang pernah disukai oleh user

*  Evaluasi
Berdasarkan histori sistem merekomendasikan **5 buku**. Setelah diverifikasi secara manual terhadap buku yang disukai user sebelumnya, ditemukan bahwa **4 dari 5 buku** yang direkomendasikan memiliki kemiripan tinggi.

* Hasil Evaluasi
* **Jumlah rekomendasi:** 5 buku
* **Jumlah yang relevan:** 4 buku

Sehingga, perhitungan Precision\@5 adalah:

$$
\text{Precision@5} = \frac{4}{5} = 0.8 \text{ atau } 80\%
$$

Hasil evaluasi menunjukkan bahwa sistem mampu memberikan rekomendasi yang cukup relevan dengan preferensi pengguna, dengan nilai **Precision\@5 sebesar 80%**. Ini menunjukkan potensi yang baik dari pendekatan content-based filtering.

2. **Collaborative Filtering**
**Metrik Evaluasi: Root Mean Squared Error (RMSE)** adalah metrik yang umum digunakan untuk mengukur selisih antara nilai yang diprediksi oleh model dengan nilai sebenarnya. RMSE dihitung dengan rumus berikut:

$$
\text{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y_i})^2}
$$

* $y_i$ adalah nilai aktual (ground truth) data ke-$i$
* $\hat{y_i}$ adalah nilai prediksi model untuk data ke-$i$
* $n$ adalah jumlah data sampel
* Nilai RMSE yang lebih kecil menunjukkan bahwa prediksi model lebih dekat dengan nilai asli, sehingga model memiliki performa yang lebih baik.
* RMSE memiliki satuan yang sama dengan nilai target sehingga interpretasinya lebih mudah.
  
**Grafik berikut menunjukkan perkembangan nilai RMSE selama proses pelatihan model pada tiap epoch:**
![epoch](https://github.com/dianamulhimah/sistem-rekomendasi/blob/main/assets/epoch.png?raw=true)
![epoch](https://raw.githubusercontent.com/dianamulhimah/sistem-rekomendasi/blob/main/assets/epoch.png)
* Pada awal pelatihan, RMSE pada data training dan testing relatif tinggi.
* Seiring bertambahnya epoch, RMSE pada data training menurun secara signifikan, menandakan model semakin baik dalam memprediksi data pelatihan.
* Namun, RMSE pada data testing setelah awal menurun sedikit malah mulai naik perlahan, yang mengindikasikan bahwa model mulai overfitting pada data training.
* Model berhasil belajar dengan baik di data training karena penurunan RMSE yang konsisten.
* Namun, adanya kenaikan RMSE pada data testing setelah titik tertentu menunjukkan perlu adanya regularisasi atau teknik lain untuk menghindari overfitting.
* Metrik RMSE memberikan gambaran kuantitatif mengenai performa prediksi model selama proses training dan testing, sehingga dapat digunakan sebagai panduan tuning parameter model lebih lanjut.

## KESIMPULAN
* CBF berhasil merekomendasikan buku yang mirip dengan buku yang disukai pengguna berdasarkan kontennya. Dengan memanfaatkan vektorisasi TF-IDF pada judul buku dan perhitungan kesamaan kosinus, model ini menunjukkan Precision@5 sebesar 80%. Ini berarti 4 dari 5 rekomendasi teratas terbukti relevan secara konten, secara efektif memenuhi kebutuhan pengguna akan saran buku yang konsisten dengan minat mereka sebelumnya. Pendekatan ini sangat efektif dalam situasi di mana data interaksi pengguna terbatas, atau ketika pengguna mencari variasi dalam kategori yang sama.
* CF menjawab kebutuhan untuk memberikan rekomendasi buku yang belum pernah dibaca oleh pengguna tetapi mungkin disukai, berdasarkan perilaku pengguna lain. Dengan mengembangkan model RecommenderNet menggunakan neural network embedding, kami melatih sistem untuk menemukan pola kesamaan antar pengguna berdasarkan data rating. Meskipun terdapat indikasi overfitting yang perlu ditangani lebih lanjut (misalnya melalui teknik regularisasi atau penambahan data), model ini mampu mengidentifikasi dan menyarankan buku-buku baru yang memiliki kemungkinan tinggi untuk disukai oleh pengguna, yang didukung oleh preferensi kolektif dari komunitas pengguna.

## Referensi
[^1]:	R. Akbar, D. Richasdy, and R. Dharayani, “Sistem Rekomendasi Buku Dengan Collaborative Filtering Menggunakan Metode Singular Value Decomposition ( SVD ),” e-Proceedings of Engineering, vol. 10, no. 5, 2023.
[^2]:	M. Alkaff, H. Khatimi, and A. Eriadi, “Sistem Rekomendasi Buku pada Perpustakaan Daerah Provinsi Kalimantan Selatan Menggunakan Metode Content-Based Filtering,” MATRIK : Jurnal Manajemen, Teknik Informatika dan Rekayasa Komputer, vol. 20, no. 1, 2020, doi: 10.30812/matrik.v20i1.617.
[^3]:	R. Ardiansyah, M. Ari Bianto, and B. D. Saputra, “Sistem Rekomendasi Buku Perpustakaan Sekolah menggunakan Metode Content-Based Filtering,” Jurnal CoSciTech (Computer Science and Information Technology), vol. 4, no. 2, 2023, doi: 10.37859/coscitech.v4i2.5131.
