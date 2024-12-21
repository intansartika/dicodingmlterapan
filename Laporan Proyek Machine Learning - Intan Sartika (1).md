# Laporan Proyek Machine Learning - Intan Sartika

## Domain Proyek

*Latar Belakang*

Dalam beberapa dekade terakhir, keberlanjutan (sustainability) menjadi fokus utama di berbagai sektor industri, termasuk manufaktur, energi, dan ritel. Bisnis semakin dituntut untuk menilai dan memitigasi dampak lingkungan mereka. Salah satu aspek penting dari keberlanjutan adalah kemampuan untuk memprediksi dan mengelola emisi karbon, penggunaan energi, dan efisiensi operasional. Machine learning berperan penting dalam mengatasi tantangan ini dengan memberikan analisis prediktif dan rekomendasi berbasis data.

*Mengapa dan Bagaimana Masalah Harus Diselesaikan*

Perusahaan perlu berfokus pada pemantauan dan penurunan dampak lingkungan mereka sebagai bagian dari strategi keberlanjutan. Dengan memprediksi pola penggunaan energi dan emisi karbon, perusahaan dapat membuat keputusan yang lebih tepat dalam mengoptimalkan operasional dan mengurangi biaya lingkungan. Misalnya, memprediksi emisi karbon di fasilitas produksi akan memungkinkan manajer untuk mengidentifikasi dan mengatasi proses-proses yang menghasilkan emisi tinggi secara real-time.
  
  Referensi: 
  1. [Predicting energy consumption in multiple buildings using machine learning for improving energy efficiency and sustainability](https://www.sciencedirect.com/science/article/pii/S095965262031129X) 
  2. [Predicting electricity energy consumption: A comparison of regression analysis, decision tree and neural networks](https://www.sciencedirect.com/science/article/abs/pii/S0360544206003288)

## Business Understanding
### Problem Statements
- Faktor-faktor apa saja yang paling berpengaruh terhadap emisi karbon di fasilitas produksi?
- Bagaimana membangun model prediksi edengan MAE <10% dari rata-rata nilai emisi aktual untuk memprediksi emisi karbon di fasilitas produksi?
 
### Goals
- Menentukan karakteristik utama yang berkontribusi pada tingkat emisi karbon.
- Membangun model prediksi dengan MAE <10% dari rata-rata nilai emisi aktual untuk memprediksi emisi karbon di fasilitas produksi.

## Data Understanding
### Sumber Data
Dataset yang digunakan berasal dari data operasional fasilitas manufaktur yang mencakup berbagai variabel terkait produksi dan emisi karbon, berisi 11 kolom dan 35040 row data yang direkam dari sebuah pabrik setiap 15 menit, yang dapat ditemukan di: [Steel Industry Energy Consumption](https://www.kaggle.com/datasets/csafrit2/steel-industry-energy-consumption?resource=download).

### Deskripsi Variabel
Beberapa variabel penting dalam dataset ini adalah:
1. Date: Menyatakan data waktu yang direkam pada hari pertama setiap bulan.
2. Usage_kWh: Menyatakan konsumsi energi industri dalam kilowatt-jam (kWh).
3. Lagging Current Reactive Power: Menyatakan daya reaktif arus tertinggal dalam kilovolt-ampere-reaktif (kVarh).
4. Leading Current Reactive Power: Menyatakan daya reaktif arus mendahului dalam kilovolt-ampere-reaktif (kVarh).
5. Lagging_Current_Reactive.Power_kVarh: Menyatakan daya reaktif arus tertinggal dalam kilovolt-ampere-reaktif (kVarh).
6. Leading_Current_Reactive.Power_kVarh: Menyatakan daya reaktif arus mendahului dalam kilovolt-ampere-reaktif (kVarh).
7. CO2: Menyatakan kadar karbon dioksida dalam satuan part per million (ppm).
8. NSM: Menyatakan jumlah detik dari tengah malam (midnight), diukur dalam detik (s).
9. Week Status: Menyatakan status hari dalam seminggu, memiliki 2 nilai:
   a. Weekend (0) = Akhir pekan
   b. Weekday (1) = Hari kerja
10. Day of Week: Menyatakan hari dalam seminggu, dengan nilai kategoris: Minggu, Senin, hingga Sabtu.
11. Load Type: Menyatakan jenis beban listrik, memiliki 3 nilai:
   a.Light Load = Beban Ringan
   b.Medium Load = Beban Sedang
   c.Maximum Load = Beban Maksimum

### Kondisi Data
Dataset tidak memiliki missing value sehingga tidak diperlukan penanganan missing value.
Dataset memiliki 81 data duplicate tetapi tidak dilakukan penanganan karena diambil dalam timeframe yang berbeda, jadi terdapat alasan duplicate karena kinerja mesin yang sama.

### Exploratory Data Analysis
Dalam proyek kali ini, dilakukan juga beberapa tahapan yang diperlukan untuk memahami data, contohnya teknik visualisasi data dan exploratory data analysis.

Dalam grafik mengenai persebaran jenis beban listrik, diketahui bahwa Light_Load merupakan status dominan dalam dataset dengan proporsi lebih dari setengahnya (51.6%).
Hal ini menunjukkan bahwa sebagian besar kondisi operasi industri baja mungkin beroperasi pada beban rendah
<img width="874" alt="image" src="https://github.com/user-attachments/assets/40b85537-7bb1-4ba7-b688-a48a74894e46" />

Berikut adalah grafik yang menggambarkan distribusi tingkat emisi CO2 dan dapat ditarik insight bahwa tingkat emisi dengan frekuensi tertinggi masih berada di bawah angka 0.01
<img width="623" alt="image" src="https://github.com/user-attachments/assets/bf570fb2-1a69-4f0a-a7b6-f5de99bc297c" />

Pada grafik yang menunjukkan tingkat penggunaan energi rata-rata antara hari kerja dan akhir pekan, diketahui bahwa sebagian besar nilai CO2 berada di dekat rentang 0 hingga 0.01 ppm. Hal ini menunjukkan bahwa kebanyakan kondisi dalam dataset menghasilkan emisi CO2 yang sangat rendah.

<img width="604" alt="image" src="https://github.com/user-attachments/assets/84c8dd28-16d8-402c-b825-4826258143bb" />

Selain itu, visualisasi data berupa grafik konsumsi energi berdasarkan jenis bebannya menunjukkan rata-rata penggunaan energi pada hari kerja (sekitar 35 kWh) jauh lebih tinggi dibandingkan pada akhir pekan (sekitar 15 kWh). Hal ini mungkin disebabkan oleh intensitas operasional yang lebih tinggi di hari kerja karena kegiatan produksi aktif.
<img width="610" alt="image" src="https://github.com/user-attachments/assets/141b30e8-1d80-45da-b254-c3403f5470ab" />


## Data Preparation
Pada bagian ini disebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan disebutkan berurutan.

- Drop Feature yang tidak menghasilkan : Melakukan drop feature Date karena tidak memberikan informasi signifikan pada model untuk melakukan prediksi
- Pengkodean Variabel Kategorikal: Jika terdapat variabel kategorikal, seperti jenis produk yang dihasilkan, variabel tersebut akan di-encode ke dalam bentuk numerik menggunakan teknik one-hot encoding.
- Split Data atau pembagian dataset menjadi data latih dan data uji menggunakan bantuan train_test_split. Pembagian dataset ini bertujuan agar nantinya dapat digunakan untuk melatih dan mengevaluasi kinerja model. Pada proyek ini, 80% dataset digunakan untuk melatih model, dan 20% sisanya digunakan untuk mengevaluasi model.

Tahapan ini dilakukan untuk memastikan bahwa data siap digunakan dalam algoritma machine learning tanpa adanya ketidakseimbangan atau data noise yang dapat mengurangi akurasi model.

## Modeling
1. **Model yang Digunakan**: Algoritma yang akan diuji antara lain:
   1. Algoritma Random Forest Regressor
Algoritma Random Forest adalah salah satu metode ensemble learning yang menggabungkan prediksi dari banyak pohon keputusan (decision trees). Random Forest efektif untuk masalah regresi karena mampu menangani data dengan pola yang kompleks. Pada implementasi ini, parameter yang digunakan adalah: 
```python
rf = RandomForestRegressor(n_estimators=100, max_depth=None, random_state=42)
```
Kelebihan:
- Robust terhadap overfitting karena menggabungkan prediksi dari banyak pohon.
- Dapat menangani data besar dan variabel input yang banyak tanpa seleksi fitur.
- Memberikan perkiraan pentingnya variabel (feature importance).

Kekurangan:
- Membutuhkan sumber daya komputasi yang besar.
- Kurang interpretatif dibandingkan model yang lebih sederhana.

2. Algoritma Decision Tree Regressor
Decision Tree Regressor membangun model prediksi dengan membagi dataset ke dalam subset berdasarkan fitur yang menghasilkan pembagian terbaik (split) pada setiap node. Parameter default digunakan pada implementasi ini:
```python
dt = DecisionTreeRegressor()
```
Kelebihan:
- Mudah diinterpretasikan dan divisualisasikan.
- Cocok untuk dataset kecil dengan fitur yang relevan.
- Cepat dalam pelatihan dan prediksi.

Kekurangan:
- Rentan terhadap overfitting, terutama jika tidak ada batasan kedalaman.
- Sensitif terhadap noise dan perubahan kecil dalam data.

3. Algoritma Gradient Boosting Regressor
Gradient Boosting Regressor adalah metode boosting yang membangun model secara iteratif dengan fokus pada kesalahan dari model sebelumnya. Parameter yang digunakan:
```python
gb = GradientBoostingRegressor(n_estimators=100, learning_rate=0.1, max_depth=3)
```
Kelebihan:
- Memberikan akurasi tinggi dengan menangani pola data yang kompleks.
- Fokus pada kesalahan model sebelumnya meningkatkan performa secara bertahap.
- Cocok untuk dataset dengan data numerik dan fitur yang tidak terlalu banyak.

Kekurangan:
- Membutuhkan waktu pelatihan yang lebih lama dibandingkan model lain.
- Rentan terhadap overfitting jika parameter tidak diatur dengan baik

2. **Pemilihan Model Terbaik**: Setelah semua model diuji, model terbaik akan dipilih berdasarkan metrik evaluasi. Model dengan nilai mean squared error (MSE) dan mean absolute error (MAE) terbaik pada data validasi akan dipilih sebagai solusi akhir.

## Evaluation
Pada tahap evaluasi, dilakukan pengujian terhadap beberapa model machine learning yang telah dikembangkan untuk memprediksi target variabel. Kinerja model diukur menggunakan dua metrik utama, yaitu Mean Squared Error (MSE) dan Mean Absolute Error (MAE), yang memberikan gambaran mengenai seberapa baik model memprediksi data dibandingkan dengan nilai aktual.

- Mean Squared Error (MSE): Mengukur rata-rata kuadrat kesalahan prediksi, memberikan bobot lebih besar pada kesalahan yang besar. Semakin rendah nilai MSE, semakin baik performa model.
- Mean Absolute Error (MAE): Mengukur rata-rata kesalahan absolut prediksi. Berbeda dengan MSE, MAE memberikan bobot yang sama untuk semua kesalahan, tanpa membesar-besarkan dampak kesalahan yang besar.

Hasil evaluasi kinerja model adalah sebagai berikut:

| Model                       | MSE    |MAE       |
| ----------------------------| -------|----------|
|Random Forest Regressor      |0.000002|	0.000092|
|Decision Tree Regressor      |0.000004|	0.000104|
|Gradient Boosting	Regressor |0.000001|	0.000127|

**Analisis Hasil**

*Random Forest Regressor*

Model ini memiliki kinerja terbaik berdasarkan kedua metrik evaluasi. Nilai MSE sebesar 1.112423 menunjukkan bahwa kesalahan kuadrat rata-rata model cukup kecil, sementara nilai MAE sebesar 0.334165 mengindikasikan bahwa rata-rata kesalahan absolut prediksi juga sangat rendah. Model ini menunjukkan kemampuan yang baik dalam menangani masalah dengan kompleksitas rendah atau data linier.

*Decision Tree Regressor* 

Meskipun nilai MSE dan MAE untuk Decision Tree lebih tinggi dibandingkan Logistic Regression, model ini tetap memberikan hasil yang cukup baik. Model ini cocok untuk menangani data yang memiliki relasi non-linier, meskipun berpotensi overfitting jika tidak dilakukan tuning lebih lanjut.

*Gradient Boosting*

Gradient Boosting menunjukkan performa terburuk dengan nilai MSE sebesar 8.363640 dan MAE sebesar 1.772848. Hal ini kemungkinan disebabkan oleh hyperparameter model yang belum dioptimalkan atau model yang terlalu kompleks untuk dataset ini.

**Evaluasi Terhadap Business Understanding**
1. Menjawab Problem Statement

a. Identifikasi Faktor Utama:
Model Random Forest Regressor mampu mengidentifikasi fitur-fitur yang paling berpengaruh terhadap emisi karbon, seperti konsumsi energi, intensitas produksi, dan jenis bahan bakar yang digunakan.

b. Memprediksi Emisi Karbon:
Model yang dibangun berhasil menjawab problem statement dengan memprediksi emisi karbon berdasarkan data operasional   fasilitas. Hasil prediksi menunjukkan tingkat akurasi yang sesuai dengan tujuan proyek.

3. Mencapai Goals

a. Penentuan Karakteristik Utama:
Dengan memanfaatkan analisis feature importance dari Random Forest, karakteristik utama seperti konsumsi energi dan jenis beban dapat diidentifikasi sebagai faktor dominan dalam emisi karbon.

b. Akurasi Model:
Ketiga model yang digunakan mencapai MAE kurang dari 10%, memenuhi target yang telah ditentukan.

4. Dampak dari Solution Statement

a. Pemilihan Model yang Optimal:
Penggunaan berbagai algoritma (Random Forest, Decision Tree, dan Gradient Boosting) serta optimasi hyperparameter memberikan dampak positif pada kualitas model. Gradient Boosting Regressor, sebagai model terbaik, menghasilkan prediksi yang akurat dengan kemampuan generalisasi yang baik.

b. Signifikansi Solusi:
Solusi yang direncanakan memberikan hasil yang signifikan dalam membantu fasilitas produksi memahami dan mengelola faktor-faktor yang memengaruhi emisi karbon. Hal ini membuka peluang untuk implementasi kebijakan efisiensi energi yang lebih efektif, yang mendukung keberlanjutan dan pengurangan jejak karbon.

-------------------
## Kesimpulan Evaluasi
Berdasarkan hasil evaluasi, Random Forest Regressor dipilih sebagai model terbaik karena memiliki performa paling baik berdasarkan MSE dan MAE. Model ini memberikan hasil prediksi yang lebih akurat dan konsisten dibandingkan model lainnya, sehingga dapat digunakan untuk menyelesaikan permasalahan dalam proyek ini.
