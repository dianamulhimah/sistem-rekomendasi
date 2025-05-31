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
* Solution 1: Content-Based Filtering (CBF) Menganalisis metadata buku seperti Book Title, Author, Publisher, dan Year of Publication untuk merekomendasikan buku serupa dengan yang telah disukai atau dibaca oleh pengguna. Teknik yang digunakan yaitu TF-IDF vectorization dan Cosine similarity.
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
![distribusi-usia-pengguna](https://raw.githubusercontent.com/dianamulhimah/sistem-rekomendasi/main/assets/distribusi-usia-pengguna.png)
Banyak pengguna berusia antara 20 hingga 40 tahun.

* Distribusi Tahun Publikasi Buku
![distribusi-tahun-publikasi-buku](https://raw.githubusercontent.com/dianamulhimah/sistem-rekomendasi/main/assets/distribusi-tahun-publikasi-buku.png)
Banyak buku yang diterbitkan antara 1990 hingga awal 2000-an.

* Distribusi Rating Pengguna
![distribusi-rating-pengguna](https://raw.githubusercontent.com/dianamulhimah/sistem-rekomendasi/main/assets/distribusi-rating-pengguna.png)
Rating **0** mendominasi, artinya pengguna mungkin hanya memberi interaksi tanpa memberikan penilaian. Rating lainnya cenderung antara **6–10**, mengindikasikan lebih banyak feedback positif.








## Data Preparation
Pada bagian ini Anda menerapkan dan menyebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan harus berurutan.

Rubrik/Kriteria Tambahan (Opsional):

Menjelaskan proses data preparation yang dilakukan
Menjelaskan alasan mengapa diperlukan tahapan data preparation tersebut.




## Modeling
Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

Rubrik/Kriteria Tambahan (Opsional):

Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.
Menjelaskan kelebihan dan kekurangan dari solusi/pendekatan yang dipilih.



## Evaluation
Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

Rubrik/Kriteria Tambahan (Opsional):

Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.
---Ini adalah bagian akhir laporan---




## Referensi
[^1]:	R. Akbar, D. Richasdy, and R. Dharayani, “Sistem Rekomendasi Buku Dengan Collaborative Filtering Menggunakan Metode Singular Value Decomposition ( SVD ),” e-Proceedings of Engineering, vol. 10, no. 5, 2023.
[^2]:	M. Alkaff, H. Khatimi, and A. Eriadi, “Sistem Rekomendasi Buku pada Perpustakaan Daerah Provinsi Kalimantan Selatan Menggunakan Metode Content-Based Filtering,” MATRIK : Jurnal Manajemen, Teknik Informatika dan Rekayasa Komputer, vol. 20, no. 1, 2020, doi: 10.30812/matrik.v20i1.617.
[^3]:	R. Ardiansyah, M. Ari Bianto, and B. D. Saputra, “Sistem Rekomendasi Buku Perpustakaan Sekolah menggunakan Metode Content-Based Filtering,” Jurnal CoSciTech (Computer Science and Information Technology), vol. 4, no. 2, 2023, doi: 10.37859/coscitech.v4i2.5131.
