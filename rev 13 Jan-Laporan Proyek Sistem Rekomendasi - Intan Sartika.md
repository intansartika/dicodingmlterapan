# Laporan Proyek Machine Learning - Intan Sartika Eris Maghfiroh
## Project Overview

Dalam era teknologi yang terus berkembang, banyak sektor bisnis mulai mengadopsi machine learning untuk meningkatkan efisiensi dan produktivitas. Salah satu penerapannya adalah sistem rekomendasi, yang memungkinkan bisnis meningkatkan penjualan produk dan jumlah pelanggan melalui rekomendasi yang relevan. Teknologi ini juga dapat diterapkan di toko buku atau perpustakaan untuk membantu pengunjung menemukan buku yang sesuai dengan minat mereka. Selain memberikan pengalaman yang lebih baik kepada pengguna, sistem rekomendasi ini juga dapat berkontribusi dalam meningkatkan tingkat literasi masyarakat, yang hingga kini masih menjadi tantangan besar.

Dataset yang digunakan dalam proyek ini adalah Book Recommendation Dataset, yang mencakup data anonim pengguna, termasuk informasi demografi mereka. Dataset ini mengandung data rating buku, baik yang diberikan secara eksplisit maupun implisit, yang dikumpulkan selama empat minggu pada tahun 2004 dari komunitas Book-Crossing. Dengan membangun sistem rekomendasi berbasis data tersebut, proyek ini bertujuan untuk memberikan solusi inovatif berupa rekomendasi buku yang relevan dan bermanfaat bagi pembaca, penulis, penerbit, dan penjual. Sistem ini diharapkan dapat mendorong pertumbuhan ekosistem industri buku secara menyeluruh.

## Business Understanding
### Problem Statement
Berdasarkan latar belakang yang diuraikan, proyek ini berfokus pada penyelesaian masalah berikut:
* Bagaimana membangun sistem rekomendasi yang dapat memberikan rekomendasi buku secara personal berdasarkan data pengguna menggunakan pendekatan content-based filtering?

### Goals
Proyek ini bertujuan untuk mengembangkan sistem rekomendasi yang dipersonalisasi, yang dapat memberikan rekomendasi buku yang relevan kepada setiap pengguna. Sistem ini akan memanfaatkan pendekatan content-based filtering, dengan menganalisis preferensi pengguna berdasarkan buku yang mereka beri rating atau baca, serta mencocokkan karakteristik buku lainnya untuk menghasilkan rekomendasi yang sesuai.

### Solution Statements
Proyek ini akan membangun sistem rekomendasi buku dengan alur sebagai berikut:

1. **Data Understanding**: Tahap awal untuk memahami data yang terdiri dari tiga file terpisah: buku (books), peringkat (ratings), dan pengguna (users). Tahapan ini mencakup:
   - **Data Loading**: Membaca dataset untuk mengetahui isi dan informasi di dalamnya.
   - **Univariate Exploratory Data Analysis**: Menganalisis dan mengeksplorasi setiap variabel dalam data.
   - **Data Preprocessing**: Menggabungkan file-file tersebut menjadi satu kesatuan data yang siap digunakan dalam pemodelan.

2. **Data Preparation**: Pada tahap ini, data dipersiapkan dengan mengatasi missing value dan menyamakan nomor ISBN buku. Karena setiap nomor ISBN mewakili satu kategori buku, pengecekan dilakukan untuk memastikan tiap buku hanya memiliki satu ISBN.

3. **Modeling**: Proses pengembangan model dilakukan dengan dua pendekatan:
   - **Content-Based Filtering**: Sistem merekomendasikan buku berdasarkan kesamaan dengan buku yang pernah disukai pengguna sebelumnya, dengan fitur utama seperti nama penulis. Teknik TF-IDF (Term Frequency-Inverse Document Frequency) digunakan untuk merepresentasikan fitur buku, dan cosine similarity digunakan untuk menghitung kesamaan antar buku.

4. **Evaluation**: Mengukur kinerja model dan menilai keberhasilannya menggunakan beberapa metrik. Untuk content-based filtering, metrik evaluasi yang digunakan adalah **Precision**, **Recall**, dan **F1 Score**

## Data Understanding
| Jenis | Keterangan |
| ------ | ------ |
| Title | _Book Recommendation Dataset_ |
| Source | [Kaggle](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset/data) |
| Maintainer | [arashnic](https://www.kaggle.com/arashnic) |
| License | Other (specified in description) |
| Visibility | Publik |
| Tags | _Online Communicataties, Literature, Art, Recommender System, Culture and Humanities_ |
| Usability | 10.00 |

Data yang dianalisis dalam proyek ini adalah **Book Recommendation Dataset** yang diperoleh dari platform Kaggle. Dataset ini dikumpulkan oleh **Cai-Nicolas Ziegler** selama satu bulan pada tahun 2004 melalui komunitas **Book-Crossing**. Dataset ini mencakup 278.858 pengguna anonim, tetapi disertai dengan informasi demografis, yang memberikan total 1.149.780 peringkat, baik secara eksplisit maupun implisit, terhadap 271.360 buku.

**Book Recommendation Dataset** terdiri dari tiga file terpisah dalam format CSV, yaitu:
1. **Books**: Berisi 271.360 baris dan 8 kolom, yang mencakup informasi tentang buku seperti nomor ISBN, judul buku, nama pengarang, tahun publikasi, penerbit, serta URL gambar buku dalam tiga ukuran (small, medium, large).
2. **Ratings**: Terdiri dari 1.149.780 baris dan 3 kolom, yang menyimpan data mengenai user ID, nomor ISBN, dan rating yang diberikan oleh pengguna.
3. **Users**: Terdiri dari 278.858 baris dan 3 kolom, berisi user ID, lokasi domisili, serta usia dari pengguna.

Ini adalah struktur folder dataset yang telah diunduh dan siap untuk diolah dalam proyek sistem rekomendasi.

* book-dataset <- Nama folder utama.
   - books.csv <- Berisi detail informasi mengenai buku.
   - ratings.csv <- Berisi data rating buku yang dibagikan oleh pengguna.
    - users.csv <- Berisi informasi pengguna atau pembaca.
  
### Data Loading
Pada bagian ini, dataset akan diambil langsung dari folder yang berisi **Book Recommendation Dataset** yang telah diunduh. Seperti yang dijelaskan sebelumnya, terdapat tiga file dataset di dalam folder tersebut, yaitu **Books**, **Ratings**, dan **Users**, yang akan digunakan dalam proses pengembangan model. Lima data teratas dari file **Books**, **Ratings**, dan **Users** dapat dilihat pada **Gambar 1 - 3**.

![image](https://github.com/user-attachments/assets/c7e0afca-71f6-4cc0-b853-0c3200b33fd5)
Gambar 1. Tampilan Isi dari Dataset Books


![image](https://github.com/user-attachments/assets/b12b408a-9e6b-4bdf-a87f-1d32086bede0)

Gambar 2. Tampilan Isi dari Dataset Ratings

![image](https://github.com/user-attachments/assets/f0c2162e-37ad-4b06-82c7-ab3f12e5ccc8)

Gambar 3. Tampilan Isi dari Dataset Users

Berdasarkan tampilan dataset pada **Gambar 1 - 3**, diperoleh informasi sebagai berikut:

1. **Variabel pada dataset Books**:
   - Terdapat 271.360 jenis buku dengan 8 kolom:
     - **ISBN**: Nomor identitas unik buku.
     - **Book-Title**: Judul buku.
     - **Book-Author**: Nama penulis buku.
     - **Year-Of-Publication**: Tahun publikasi buku.
     - **Publisher**: Nama penerbit buku.
     - **Image-URL-S**: URL gambar ukuran kecil.
     - **Image-URL-M**: URL gambar ukuran sedang.
     - **Image-URL-L**: URL gambar ukuran besar.

2. **Variabel pada dataset Ratings**:
   - Terdapat 340.556 penilaian terhadap buku dengan 3 kolom:
     - **User-ID**: Kode unik untuk pengguna anonim yang memberikan penilaian.
     - **ISBN**: Nomor identitas buku.
     - **Book-Rating**: Penilaian yang diberikan untuk buku.

3. **Variabel pada dataset Users**:
   - Terdapat 278.858 pengguna anonim dengan 3 kolom:
     - **User-ID**: Kode unik pengguna anonim.
     - **Location**: Lokasi tempat tinggal pengguna.
     - **Age**: Usia pengguna.

Setelah memperoleh informasi dari dataset tersebut, langkah berikutnya adalah melakukan eksplorasi lebih lanjut pada dataset.

### Exploratory Data Analysis
Pada tahap ini, akan dilakukan analisis dan eksplorasi terhadap setiap variabel untuk memahami distribusi serta karakteristik masing-masing variabel. Pemahaman ini penting untuk menentukan pendekatan atau algoritma yang paling sesuai untuk diterapkan pada data.

#### Variabel Books
Dalam **Gambar 4** berikut, ditampilkan 10 nama penulis teratas berdasarkan jumlah buku yang ditulis. Didapatkan informasi bahwa  **Agatha Christie** menjadi penulis dengan publikasi terbanyak dengan lebih dari 600 buku.

![image](https://github.com/user-attachments/assets/e8ab9278-131e-4055-9fbb-43702008b60a)

Gambar 4. Distribusi Data mengenai 10 nama penulis dengan berdasarkan jumlah buku diterbitkan terbanyak

Sedangkan menurut nama penerbit, ditemukan bahwa penerbit dengan jumlah publikasi buku terbanyak adalah **Harlequin** dengan jumlah lebih dari 7.000 buku seperti yang ditampilkan pada **Gambar 5**.

![image](https://github.com/user-attachments/assets/a71d895a-08ee-4c2d-872f-ea2f5a0570e5)
Gambar 5. Distribusi Data mengenai 10 nama penerbit terbanyak berdasarkan jumlah buku

#### Variabel Ratings

Selanjutnya, dilakukan eksplorasi pada variabel **ratings**, yaitu penilaian yang diberikan pembaca atau pengguna terhadap buku. Didapatkan informasi bahwa dengan menggunakan rating berskala 1-10, rating terbanyak diberikan pada nilai antara 0 dan 1 sesuai **Gambar 6**.

![image](https://github.com/user-attachments/assets/6a3fd1de-cae9-4ed9-8289-d955fa4ad6c8)
Gambar 6. Distribusi Data rating buku dari skala 1-10

#### Variabel Users
Eksplorasi berikutnya dilakukan pada variabel users, yang berisi informasi mengenai pengguna anonim beserta data demografi mereka. Data pengguna ini berguna jika ingin membangun sistem rekomendasi yang mempertimbangkan faktor demografi atau kondisi sosial pengguna. Namun, untuk studi kasus kali ini, data users tidak akan digunakan dalam pengembangan model. Model yang akan dikembangkan hanya menggunakan data dari variabel books dan ratings. Dari **Gambar 7**, diketahui bahwa ternyata pengguna yang paling banyak memberikan rating berdomisili di London, Inggris, disusul oleh pengguna di Toronto, Kanada dan Sydney, Australia. Beberapa negara lain seperti AS dan Spanyol juga masuk dalam 10 jajaran teratas

![image](https://github.com/user-attachments/assets/e001a489-3dcb-450a-ae07-8e808a370fc0)
Gambar 7. Distribusi Data 10 lokasi terbanyak dari pengguna

## Eksplorasi Missing Value, Duplicate, dan Outlier pada Dataset

### 1. **Missing Value (Null)**
Pada **dataset buku**, terdapat **missing value** pada kolom **Book Author** dan **Image-URL**. Missing value ini biasanya terjadi ketika informasi mengenai penulis atau gambar sampul buku tidak tersedia atau tidak lengkap. Untuk mengatasi masalah ini:
- Kolom **Book Author** dapat diimputasi dengan label **"Unknown"** atau dengan penulis yang paling sering berkolaborasi dengan penerbit yang sama jika memungkinkan.
- Untuk kolom **Image-URL**, dapat diisi dengan gambar sampul default atau placeholder yang mewakili buku yang tidak memiliki URL gambar.

Pada **data pengguna**, terdapat **missing value** pada kolom **age**. Hal ini dapat diatasi dengan imputasi menggunakan rata-rata atau nilai median usia pengguna, atau baris dengan missing value bisa dihapus jika jumlahnya relatif kecil dan tidak mempengaruhi dataset secara signifikan.

Pada **dataset rating**, tidak terdapat missing value, yang menunjukkan bahwa semua data rating yang diberikan oleh pengguna sudah lengkap.

### 2. **Duplicate Data**
Pada **dataset buku**, tidak ditemukan data duplikat, yang berarti setiap entri buku dalam dataset adalah unik, dan tidak ada buku yang tercatat lebih dari sekali. Oleh karena itu, tidak diperlukan tindakan lebih lanjut untuk menangani data duplikat dalam dataset buku.

Pada **dataset rating**, juga tidak ditemukan duplikat, yang menunjukkan bahwa setiap rating yang diberikan oleh pengguna untuk buku tertentu adalah unik dan tidak ada entri rating yang terulang.

### 3. **Outlier**
Pada **data pengguna**, terdapat **outlier pada usia (age)**, yang dapat mempengaruhi kualitas model dan hasil rekomendasi. Outlier ini bisa disebabkan oleh kesalahan input atau pengguna yang memiliki usia yang tidak realistis (misalnya usia yang terlalu muda atau tua untuk kelompok pengguna yang relevan). Untuk mengatasi masalah ini, outlier pada usia bisa dideteksi menggunakan teknik **Z-score** atau **Interquartile Range (IQR)**. Data dengan nilai usia yang sangat jauh dari rata-rata bisa dianggap sebagai outlier dan dihapus atau dibatasi pada rentang usia yang wajar (misalnya antara 18 hingga 100 tahun).

## Data Preparation

### Data Preparation untuk Model Pengembangan dengan Content-Based Filtering
Seperti yang telah dijelaskan dalam tahap **data understanding**, folder **Book Recommendation Dataset** terdiri dari tiga file terpisah: **books**, **ratings**, dan **users**. Pada tahap ini, dilakukan proses penggabungan file **books** dan **ratings** menjadi satu kesatuan dataset untuk keperluan pengembangan model.

Setelah penggabungan, dataset memiliki 7 variabel dan 1.149.780 baris data. Tampilan dari dataset yang sudah digabungkan dapat dilihat pada **Gambar 8**, dan inilah dataset yang akan digunakan untuk membangun sistem rekomendasi.

![image](https://github.com/user-attachments/assets/67a9f29b-3a79-47e4-9c88-d35ed3169b99)

Gambar 8. Tampilan dataset yang sudah digabungkan (merged) antara variabel ratings dan books

Pada tahap **Data Preparation**, berbagai teknik dilakukan untuk mempersiapkan data secara optimal untuk membangun sistem rekomendasi buku. Berikut adalah detail langkah-langkah yang dilakukan:

---

### 1. Menghapus Missing Value dan Data Tidak Relevan
Pada tahap pertama, data yang mengandung missing value dihapus menggunakan metode drop, karena jumlah missing value yang ditemukan dianggap tidak signifikan dan tidak relevan untuk diperbaiki. Data yang telah dibersihkan disimpan dalam variabel baru bernama all_books_clean. Setelah proses penghapusan missing value ini, dataset menyisakan sebanyak 1.031.132 baris data. Selain itu, dilakukan penghapusan data tidak relevan, seperti gambar buku dalam berbagai ukuran (S, M, dan L), yang tidak diperlukan untuk rekomendasi. Penghapusan ini bertujuan untuk mengurangi noise pada data sehingga hasil rekomendasi menjadi lebih akurat.
![image](https://github.com/user-attachments/assets/35a69d9d-63ed-4f69-8efe-32d62deec3d0)
**Gambar 9. Tahapan Penghapusan Missing Value**

---

### 2. Menghapus Data Duplikat
Pada tahap kedua, dilakukan proses deduplikasi pada kolom ISBN untuk memastikan bahwa setiap ISBN unik dan hanya mewakili satu judul buku. Hal ini penting karena ISBN yang sama untuk beberapa judul dapat menyebabkan bias dalam model rekomendasi. Setelah proses deduplikasi selesai, dataset menyisakan 270.147 baris data. Proses ini membantu meningkatkan kualitas data dan menghindari anomali dalam sistem rekomendasi. Proses ini penting untuk menghindari anomali saat membangun sistem rekomendasi, di mana satu buku bisa muncul berulang kali dengan ISBN yang sama namun memiliki metadata berbeda.

---

### 3. Mengonversi Data Series ke Dalam Bentuk List
Tahap ketiga adalah mengonversi beberapa kolom, seperti kategori buku, ke dalam format list. Konversi ini bertujuan untuk mempermudah manipulasi data saat proses pemodelan, terutama untuk model berbasis konten (Content-Based Filtering) yang memanfaatkan atribut seperti genre buku, kata kunci, atau kategori secara terstruktur. Langkah ini memastikan data lebih siap untuk dianalisis dan diproses lebih lanjut.

---

### 4. Membuat DataFrame Baru
Pada tahap keempat, dataset dipecah menjadi beberapa DataFrame baru berdasarkan kebutuhan analisis. Contohnya, DataFrame yang menyimpan metadata buku, seperti judul, penulis, genre, dan deskripsi, serta DataFrame yang berisi informasi interaksi pengguna-buku, seperti rating atau aktivitas pembelian buku. Pemecahan ini bertujuan untuk mempermudah pengelolaan data dan mempersiapkannya untuk digunakan pada model rekomendasi, baik berbasis kolaborasi maupun konten.

---

### 5. Menggunakan Teknik TF-IDF
Tahap kelima melibatkan penggunaan teknik TF-IDF (Term Frequency-Inverse Document Frequency) untuk menganalisis deskripsi buku. Teknik ini digunakan untuk menghitung bobot setiap kata berdasarkan frekuensi kemunculannya pada satu dokumen relatif terhadap semua dokumen. Representasi ini sangat berguna untuk model rekomendasi berbasis konten, karena memungkinkan sistem untuk memahami hubungan antar buku berdasarkan deskripsi teks. Sebagai contoh, jika sebuah buku memiliki deskripsi seperti "thriller misteri pembunuhan," sistem dapat mengenali genre ini dan memberikan rekomendasi buku lain dengan kata kunci serupa.

---

### Data Preparation untuk Model Pengembangan dengan Content-Based Filtering
### Penjelasan Data Preparation untuk Collaborative Filtering

Proses data preparation untuk sistem rekomendasi berbasis **collaborative filtering** melibatkan beberapa langkah penting yang memastikan data siap digunakan dalam model.

#### 1. Pengolahan Data Pengguna (User Encoding)
Langkah pertama adalah menyiapkan data pengguna. Melalui kode `user_ids = ratings['User-ID'].unique().tolist()`, kita mengambil daftar unik ID pengguna dari dataset `ratings`. Ini memastikan bahwa semua pengguna dalam sistem dapat diidentifikasi. Selanjutnya, dengan `user_to_user_encoded = {x: i for i, x in enumerate(user_ids)}`, ID pengguna diubah menjadi angka numerik untuk memudahkan pemrosesan oleh model. Terakhir, dictionary terbalik dibuat dengan `user_encoded_to_user = {i: x for i, x in enumerate(user_ids)}` agar kita bisa mengonversi kembali ID numerik menjadi ID pengguna asli jika diperlukan.

#### 2. Pengolahan Data Buku (Book Titles)
Selain data pengguna, data buku juga diproses untuk mendapatkan informasi tentang buku yang ada dalam dataset. Dengan kode `num_rating_df = ratings_with_name.groupby('Book-Title').count()['Book-Rating'].reset_index()`, kita mengelompokkan data berdasarkan judul buku dan menghitung jumlah rating yang diterima setiap buku. Hal ini memberikan gambaran tentang popularitas setiap buku dalam sistem rekomendasi. Setelah itu, kolom baru `num_ratings` dibuat menggunakan `num_rating_df.rename(columns={'Book-Rating':'num_ratings'}, inplace=True)` untuk membedakan data rating yang dihitung dengan data lainnya.

#### 3. Pengolahan Rating (Rating Processing)
Rating buku dalam dataset perlu diproses agar dapat digunakan oleh model. Kode `ratings_with_name['Book-Rating'] = pd.to_numeric(ratings_with_name['Book-Rating'], errors='coerce')` digunakan untuk memastikan bahwa nilai rating dalam dataset dikonversi menjadi angka yang valid, mengatasi nilai yang mungkin tidak terformat dengan benar atau kosong. Kemudian, kita mengonversi rating menjadi tipe data `float32` dengan kode `df_rating['Book-Rating'] = df_rating['Book-Rating'].values.astype(np.float32)` untuk memastikan efisiensi dalam pemrosesan dan perhitungan lebih lanjut.

#### 4. Normalisasi Rating
Agar model dapat bekerja dengan optimal, nilai rating perlu dinormalisasi ke dalam rentang yang seragam. Proses normalisasi dilakukan dengan rumus `(x - min_rating) / (max_rating - min_rating)`, yang memastikan bahwa semua rating berada dalam rentang 0 hingga 1. Ini sangat penting karena sistem rekomendasi membutuhkan skala rating yang konsisten untuk memprediksi dan memberikan rekomendasi yang akurat berdasarkan interaksi pengguna.

#### 5. Pembagian Data Latih dan Validasi
Setelah data pengguna, buku, dan rating diproses, data dibagi menjadi dua bagian utama: data latih (training) dan data validasi. Kode `train_indices = int(0.9 * df_rating.shape[0])` membagi dataset menjadi 90% data latih dan 10% data validasi. Pembagian ini penting untuk mengevaluasi kinerja model, di mana data latih digunakan untuk melatih model dan data validasi digunakan untuk mengukur akurasi model setelah pelatihan.

Secara keseluruhan, tahapan data preparation ini memastikan bahwa data pengguna, buku, dan rating sudah terstruktur dengan baik dan siap digunakan untuk membangun sistem rekomendasi berbasis **collaborative filtering** yang efektif.

## Modeling
### Model Development dengan Content Based Filtering
Dalam pengembangan model ini, dilakukan pencarian representasi fitur penting dari setiap judul buku menggunakan TF-IDF (Term Frequency - Inverse Document Frequency) Vectorizer. TF-IDF adalah alat yang digunakan untuk mengubah dokumen teks menjadi representasi vektor berdasarkan frekuensi kemunculan kata (TF) dan seberapa unik atau jarangnya kemunculan kata tersebut di seluruh koleksi dokumen (IDF). Vektor ini digunakan untuk menemukan fitur penting dari judul buku berdasarkan nama penulis menggunakan teknik Content Based Filtering.

Pada proyek ini, digunakan fungsi `tfidfvectorizer()` dari library Scikit-Learn. Langkah pertama adalah mengimpor fungsi `tfidfvectorizer()` untuk menghitung IDF pada data `book_author`. Kemudian, dilakukan pemetaan array dari indeks fitur integer ke nama fitur menggunakan fungsi `get_feature_name_out()`. Setelah itu, dilakukan proses fitting dan transformasi data ke dalam bentuk matriks berukuran (15000, 7216), di mana 15.000 adalah jumlah data dan 7216 adalah jumlah penulis buku yang berbeda. Untuk menghasilkan vektor TF-IDF dalam bentuk matriks, digunakan fungsi `todense()`.

Selanjutnya, untuk menghitung derajat kesamaan (similarity degree) antar judul buku, digunakan teknik cosine similarity. Metode ini berfungsi untuk mengukur kesamaan antara dua vektor dalam ruang berdimensi tinggi dengan menghitung sudut kosinus antara kedua vektor tersebut; semakin kecil sudutnya, semakin besar kesamaan di antara vektor-vektor tersebut. Pada proyek ini, fungsi `cosine_similarity` dari library Scikit-Learn digunakan untuk menghitung nilai kesamaan berdasarkan matriks TF-IDF yang telah dibuat. Dengan menggunakan fungsi `cosine_similarity()`, diperoleh nilai kesamaan antar judul buku dalam bentuk matriks kesamaan yang berupa array. Selanjutnya, dibuat dataframe dari hasil perhitungan cosine similarity, dengan baris dan kolom yang mewakili nama judul buku. Tampilan dataframe hasil perhitungan cosine similarity ini ditunjukkan pada Gambar 9.

![image](https://github.com/user-attachments/assets/20bf5172-1839-4034-98e5-0c19d7039e7b)


Langkah terakhir adalah menampilkan matriks TF-IDF untuk beberapa judul buku dan nama penulis buku dalam bentuk dataframe. Pada dataframe ini, kolom-kolom diisi dengan nama penulis buku, sementara baris-barisnya diisi dengan judul buku, seperti yang ditunjukkan pada **Gambar 10**.

![image](https://github.com/user-attachments/assets/09e6c333-893b-4441-93ac-76f7f2385b06)
Gambar 10. Dataframe dari matriks tf-idf

Hasil dari model ini dengan mencoba mencari 10 rekomendasi untuk buku berjudul **365 Ways to Cook Chinese** dapat dilihat pada **Gambar 11**

![image](https://github.com/user-attachments/assets/28608de8-9c0a-4625-be56-2b1a85c61b18)
Gambar 11. Hasil Rekomendasi dengan Content-Based Filtering Berdasarkan Nama Penulis

Rekomendasi ini menunjukkan bahwa sistem berhasil mengidentifikasi dan menyarankan buku-buku dari penulis yang relevan dengan kategori "Rosa Ross". Namun, perlu diperhatikan bahwa dalam data yang tersedia, tidak terdapat buku yang ditulis oleh Rosa Ross. Sebagai gantinya, terdapat beberapa buku yang ditulis oleh penulis dengan nama belakang "Ross", seperti Ross Thomas, Ann B. Ross, Jim Ross, Dave Ross, Sheila Ross, Ross W. Greene, dan Sheldon Ross.

### Model Development dengan Collaborative Filtering
Dengan menggunakan data dari 2.180 pengguna dan 17.178 buku yang diberikan rating, dikembangkan sistem rekomendasi yang memberikan saran berdasarkan pola preferensi dan perilaku pengguna lain yang serupa. Teknik ini bekerja dengan memanfaatkan data historis, seperti rating atau interaksi pengguna terhadap item tertentu, untuk menemukan kesamaan antar pengguna atau antar item. Collaborative Filtering dibagi menjadi dua pendekatan utama: User-Based Filtering, yang merekomendasikan item berdasarkan preferensi pengguna dengan profil serupa, dan Item-Based Filtering, yang merekomendasikan item yang sering dinilai atau digunakan bersama dengan item lain yang telah dipilih oleh pengguna. Metode ini tidak memerlukan informasi langsung tentang atribut item, sehingga cocok digunakan dalam berbagai domain. Namun, Collaborative Filtering memerlukan jumlah data interaksi yang besar untuk memberikan rekomendasi yang akurat, dan dapat mengalami kendala seperti cold start problem pada pengguna atau item baru yang belum memiliki data interaksi.

Model RecommenderNet adalah sebuah sistem rekomendasi berbasis collaborative filtering yang menggunakan pendekatan embedding untuk memodelkan interaksi antara pengguna dan buku. Pada dasarnya, model ini memiliki dua entitas utama, yaitu pengguna dan judul buku, yang digunakan untuk memprediksi preferensi pengguna terhadap buku. Model ini diinisialisasi dengan parameter seperti num_users untuk jumlah total pengguna, num_book_title untuk jumlah total judul buku, serta embedding_size yang menentukan ukuran vektor representasi bagi masing-masing entitas. Selain itu, dropout_rate juga disertakan untuk mengurangi risiko overfitting selama pelatihan.

Model ini menggunakan empat lapisan embedding utama: user_embedding untuk merepresentasikan pengguna, user_bias untuk bias pengguna, book_title_embedding untuk representasi buku, dan book_title_bias untuk bias buku. Vektor-vektor ini kemudian digunakan untuk menghitung interaksi antara pengguna dan buku melalui perkalian dot product (dot_user_book_title), yang mewakili kecocokan antara pengguna dan buku tertentu. Hasil dari interaksi ini kemudian dijumlahkan dengan bias pengguna dan bias buku untuk menghasilkan prediksi. Akhirnya, output dari model diproses melalui fungsi aktivasi sigmoid untuk menghasilkan nilai antara 0 dan 1, yang menunjukkan tingkat preferensi pengguna terhadap buku tersebut.

Model **RecommenderNet** digunakan untuk training data dan setelah dilakukan pengujian acak untuk salah satu pengguna didapatkan hasil rekomendasi pada **Gambar 12** berikut.
![image](https://github.com/user-attachments/assets/6db6f39e-bba5-4fc1-895c-eef290947c49)
Gambar 12. Hasil Rekomendasi Collaborative Filtering

## Evaluasi
### Content-Based Filtering
Kinerja model dievaluasi menggunakan metrik Precision, Recall, dan F1-Score. Precision mengukur relevansi rekomendasi yang dihasilkan, Recall mengevaluasi sejauh mana item relevan berhasil diidentifikasi, dan F1-Score memberikan keseimbangan antara precision dan recall. Untuk evaluasi, label ground truth dibuat berdasarkan cosine similarity (1 untuk item yang mirip, dan 0 untuk item yang tidak mirip), dengan ambang batas (threshold) tertentu untuk menentukan kemiripan. Pada implementasi kode, ambang batas diatur sebesar 0,5, namun nilai ini dapat disesuaikan tergantung pada hasil rekomendasi. Matriks ground truth dibuat menggunakan fungsi np.where() dari NumPy, dengan nilai 1 diberikan jika cosine similarity memenuhi atau melampaui ambang batas, dan 0 jika sebaliknya. Matriks ini kemudian disajikan dalam bentuk dataframe yang diindeks berdasarkan judul buku.

Setelah matriks ground truth dibuat, evaluasi model dilakukan menggunakan metrik precision, recall, dan F1-Score. Fungsi precision_recall_fscore_support dari Sklearn digunakan untuk menghitung metrik tersebut. Untuk mempercepat proses evaluasi, hanya 10.000 sampel yang digunakan, dan matriks data diratakan menjadi array satu dimensi. Nilai cosine similarity dikategorikan sebagai 1 atau 0 berdasarkan ambang batas yang ditentukan, dan hasilnya disimpan dalam array prediksi. Precision, recall, dan F1-Score dihitung sebagai klasifikasi biner dengan penanganan pembagian nol. Berikut rumus confusion matrix yang digunakan untuk mengevaluasi model

![image](https://github.com/user-attachments/assets/ff75ac33-5eb7-4f3a-92a7-3d070aff5d52)


Hasil evaluasi pada **Gambar 13** menunjukkan bahwa model bekerja dengan sangat baik, dengan Precision sebesar 1,0 (tidak ada false positive), Recall sebesar 1,0 (berhasil mengidentifikasi hampir semua item yang relevan), dan F1-Score mendekati 1,0, yang mencerminkan keseimbangan yang kuat antara precision dan recall. Hasil ini membuktikan bahwa model sangat efektif dalam memberikan rekomendasi menggunakan pendekatan content-based filtering.

![image](https://github.com/user-attachments/assets/90c7fdfa-1072-41be-a0ca-91d6fb347049)
Gambar 13. Confusion Matrix

**Interpretasi Hasil Evaluasi:**

- Nilai **Precision** sebesar 1.0 menunjukkan bahwa semua item yang direkomendasikan oleh model benar-benar relevan, tanpa ada kesalahan (false positive).
- Nilai **Recall** sebesar 1.0 menunjukkan bahwa model berhasil mengidentifikasi 100% dari semua item yang relevan, tanpa ada item yang terlewat (false negative).
- Nilai **F1-Score** sebesar 1.0 mengindikasikan keseimbangan sempurna antara precision dan recall, yang berarti model berkinerja sangat baik dalam menangani kedua kelas (positif dan negatif).

### Collaborative Filtering
Untuk mengukur kesuksesan model collaborative filtering, digunakan skor Root Mean Square Error (RMSE). Visualisasi evaluasi Root Mean Square Error (RMSE) pada **Gambar 14** menunjukkan bahwa model mencapai konvergensi setelah sekitar 50 epoch, dengan nilai MSE yang rendah. Error akhir yang dicapai adalah 0,2924, sedangkan error validasi adalah 0,3389. Hasil ini menunjukkan performa model yang baik, karena nilai RMSE yang lebih rendah menunjukkan prediksi preferensi pengguna yang lebih akurat, sehingga sistem rekomendasi menjadi lebih efektif.

![image](https://github.com/user-attachments/assets/2901170b-f224-42d7-8aea-a4c823e46ebc)

![image](https://github.com/user-attachments/assets/08105813-e25e-4f86-a0e3-dc116ead4714)
Gambar 14. Kinerja Model Berbasis Collaborative Filtering

### Dampak Sistem Rekomendasi Buku Menggunakan Content-Based Recommendation dan Collaborative Filtering terhadap Business Understanding

## 1. Menjawab Problem Statement:
- Dengan menggunakan dua pendekatan, yaitu content-based recommendation dan collaborative filtering, sistem rekomendasi buku ini mampu memberikan rekomendasi yang lebih tepat dan personal kepada pengguna. Content-based recommendation berfokus pada kesamaan atribut atau karakteristik buku yang disukai pengguna, sementara collaborative filtering mengandalkan data interaksi pengguna lain untuk menentukan buku yang relevan. Evaluasi model menunjukkan bahwa kedua pendekatan ini efektif dalam menjawab masalah rekomendasi yang dipersonalisasi, sesuai dengan kebutuhan pengguna.

## 2. Mencapai Goals yang Diharapkan:
- Sistem ini berhasil mencapai tujuan yang diharapkan, yaitu meningkatkan pengalaman pengguna dengan memberikan rekomendasi buku yang relevan dan menarik. Content-based recommendation memperkaya pengalaman pengguna dengan menyarankan buku serupa berdasarkan preferensi spesifik mereka, sementara collaborative filtering meningkatkan akurasi rekomendasi dengan memanfaatkan data kolektif dari interaksi pengguna lainnya. Hasil evaluasi menunjukkan bahwa kedua pendekatan ini memberikan kontribusi signifikan terhadap peningkatan precision, recall, dan F1-Score, yang mencerminkan kemampuan model dalam menyediakan rekomendasi yang tepat sasaran.

## 3. Dampak Solusi Statement:
- Penerapan kedua teknik rekomendasi memberikan dampak positif terhadap strategi bisnis. Content-based filtering memungkinkan sistem untuk memberikan rekomendasi tanpa membutuhkan data interaksi pengguna lain, yang cocok untuk pengguna yang baru bergabung atau memiliki sedikit data interaksi. Di sisi lain, collaborative filtering membantu memperkaya rekomendasi dengan mencocokkan pola perilaku pengguna yang serupa. Kedua teknik ini bersama-sama mendukung tujuan bisnis dalam meningkatkan kepuasan pengguna dan potensi penjualan buku

# Kesimpulan

Sistem rekomendasi buku yang menggunakan kombinasi teknik **content-based recommendation** dan **collaborative filtering** terbukti efektif dalam memberikan rekomendasi yang relevan dan personal kepada pengguna. Teknik **content-based recommendation** berhasil menyediakan saran buku yang sesuai dengan preferensi spesifik pengguna berdasarkan atribut buku, sementara **collaborative filtering** meningkatkan akurasi rekomendasi dengan memanfaatkan data interaksi antar pengguna. Evaluasi model menunjukkan bahwa kedua pendekatan ini berkontribusi signifikan terhadap peningkatan kinerja model, yang tercermin dalam nilai precision, recall, dan F1-Score yang baik. Secara keseluruhan, penerapan kedua teknik ini tidak hanya memenuhi kebutuhan teknis dalam menyediakan rekomendasi yang tepat, tetapi juga memberikan dampak positif terhadap tujuan bisnis, seperti meningkatkan kepuasan pengguna dan potensi penjualan buku. Dengan demikian, sistem rekomendasi ini diharapkan dapat meningkatkan engagement pengguna dan mendukung pertumbuhan industri buku secara keseluruhan.
