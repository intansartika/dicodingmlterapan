# Analisis Faktor Penyebab Tingginya Attrition Rate di PT Jaya Jaya Maju
### 1. Business Understanding
* Permasalahan:
Tingginya attrition rate (>10%) di perusahaan Jaya Jaya Maju. HR ingin memahami faktor-faktor yang mempengaruhi agar dapat menurunkan angka ini.

* Tujuan:
Membuat visualisasi dalam bentuk business dashboard untuk mengidentifikasi faktor-faktor yang mempengaruhi tingginya attrition rate dan memonitor karyawan.

### 2. Data Understanding
Kita akan melakukan eksplorasi awal data:
* Ukuran data, deskripsi tiap atribut dan tipe data

Diketahui bahwa terdapat 35 kolom dan 1470 baris dengan detail sebagai berikut

| Nama Atribut               | Deskripsi                                      | Tipe Data   |
|---------------------------|----------------------------------------------------------------------|-------------|
| EmployeeId                | ID Karyawan                                                         | integer       |
| Attrition                 | Apakah karyawan keluar? (0 = tidak, 1 = ya)                         | float     |
| Age                       | Usia karyawan                                                       | integer       |
| BusinessTravel            | Frekuensi perjalanan dinas                                          | object      |
| DailyRate                 | Gaji harian                                                         | integer       |
| Department                | Departemen tempat karyawan bekerja                                  | object      |
| DistanceFromHome          | Jarak rumah ke kantor (dalam km)                                    | integer       |
| Education                 | Tingkat pendidikan (1 = Di bawah kuliah, ..., 5 = Doktor)           | integer       |
| EducationField            | Bidang pendidikan                                                   | object      |
| EmployeeCount             | Jumlah karyawan (konstan)                                           | integer       |
| EnvironmentSatisfaction   | Kepuasan terhadap lingkungan kerja (1 = Rendah, ..., 4 = Sangat tinggi)| integer    |
| Gender                    | Jenis kelamin                                                       | object      |
| HourlyRate                | Gaji per jam                                                        | integer       |
| JobInvolvement            | Keterlibatan kerja (1 = Rendah, ..., 4 = Sangat tinggi)             | integer       |
| JobLevel                  | Tingkat jabatan (1 sampai 5)                                        | integer       |
| JobRole                   | Jabatan pekerjaan                                                   | object      |
| JobSatisfaction           | Kepuasan kerja (1 = Rendah, ..., 4 = Sangat tinggi)                 | integer       |
| MaritalStatus             | Status pernikahan                                                   | object      |
| MonthlyIncome             | Gaji bulanan                                                        | integer       |
| MonthlyRate               | Tarif bulanan                                                       | integer       |
| NumCompaniesWorked        | Jumlah perusahaan tempat pernah bekerja                             | integer       |
| Over18                    | Apakah usia di atas 18 tahun?                                       | object      |
| OverTime                  | Apakah lembur?                                                      | object      |
| PercentSalaryHike         | Persentase kenaikan gaji tahun lalu                                 | integer       |
| PerformanceRating         | Penilaian kinerja (1 = Rendah, ..., 4 = Luar biasa)                 | integer       |
| RelationshipSatisfaction  | Kepuasan hubungan kerja (1 = Rendah, ..., 4 = Sangat tinggi)        | integer       |
| StandardHours             | Jam kerja standar                                                   | integer       |
| StockOptionLevel          | Level opsi saham                                                    | integer       |
| TotalWorkingYears         | Total tahun pengalaman kerja                                        | integer       |
| TrainingTimesLastYear     | Jumlah pelatihan tahun lalu                                         | integer       |
| WorkLifeBalance           | Keseimbangan kerja-hidup (1 = Rendah, ..., 4 = Sangat baik)         | integer       |
| YearsAtCompany            | Lama bekerja di perusahaan                                          | integer       |
| YearsInCurrentRole        | Lama menjabat posisi saat ini                                       | integer       |
| YearsSinceLastPromotion   | Lama sejak promosi terakhir                                         | integer       |
| YearsWithCurrManager      | Lama bekerja dengan manajer saat ini                                | integer       |

* Distribusi attrition
Terdapat 879 data berlabel "No" (bertahan di perusahaan) dan 179 berlabel "Yes" (keluar dari perusahaan)
<img width="448" alt="image" src="https://github.com/user-attachments/assets/89311039-4446-494e-adfe-9912c0ef6349" />

* Korelasi potensial dengan variabel lain

Berdasarkan atribut data tadi, selanjutnya dikategorikan ke dalam variabel yang memiliki potensi berpengaruh kepada tingkat attrition

| Kategori            | Variabel                                                                 | Alasan                                                                                   |
|---------------------|--------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
| Demografi           | Age, Gender, MaritalStatus                                               | Usia dan status menikah dapat memengaruhi stabilitas kerja                              |
| Pekerjaan & Performa| JobLevel, JobRole, JobInvolvement, JobSatisfaction, PerformanceRating, YearsInCurrentRole, YearsWithCurrManager | Kepuasan dan keterlibatan kerja adalah faktor penting keluar/tidaknya karyawan          |
| Kompensasi          | MonthlyIncome, HourlyRate, PercentSalaryHike, StockOptionLevel           | Kompensasi yang rendah bisa jadi penyebab utama attrition                               |
| Lingkungan kerja    | EnvironmentSatisfaction, WorkLifeBalance, OverTime, DistanceFromHome     | Keseimbangan kerja, jarak, dan overtime punya efek signifikan                           |
| Pengalaman kerja    | NumCompaniesWorked, TotalWorkingYears, YearsSinceLastPromotion, YearsAtCompany | Riwayat kerja bisa menunjukkan kecenderungan untuk pindah kerja                        |


Salah satu contoh visualisasi berikut menunjukkan bahwa umur karyawan yang keluar dari perusahaan terbanyak berada di rentang 25-34 tahun
<img width="674" alt="image" src="https://github.com/user-attachments/assets/fe693355-8c6b-474b-9b03-352c77ef6747" />

Selain itu ditemukan juga bahwa laki-laki dan juga karyawan berstatus lajang lebih cenderung banyak keluar dari perusahaan

<img width="426" alt="image" src="https://github.com/user-attachments/assets/d3774e73-4011-474b-9aa6-f29ea34cc1df" />

<img width="427" alt="image" src="https://github.com/user-attachments/assets/cec677a8-61ca-4716-a479-0a4226827894" />


### 2. Data Preparation
Langkah ini meliputi:
* Membersihkan missing value (misalnya pada kolom Attrition)

Ditemukan bahwa terdapat 412 baris data attrition yang tidak ada

<img width="157" alt="image" src="https://github.com/user-attachments/assets/4da90ecf-458d-467f-88c3-965c14d2e083" />

Berikut ini tampilan data setelah dibersihkan sehingga tidak ditemukan lagi missing value

<img width="246" alt="image" src="https://github.com/user-attachments/assets/4b4120fb-f366-4dd0-a5b9-efcb50e0928b" />

* Encoding untuk variabel kategorikal

Setelahnya, dilakukan pelabelan/encoding untuk variabel kategorikal sehingga menjadi seperti berikut ini

<img width="707" alt="image" src="https://github.com/user-attachments/assets/62738a3b-bba6-457d-822d-64dd32b3471c" />

### 3. Uji Pengaruh Variabel

Untuk menentukan variabel mana yang memiliki pengaruh terhadap attrition, dilakukan identifikasi mengenai mutual information, uji pengaruh parsial, dan uji pengaruh simultan.

<img width="608" alt="image" src="https://github.com/user-attachments/assets/48f76566-d431-4d9a-8eb7-b1342bf8e96f" />

Setelah dilakukan uji pengaruh, diketahui bahwa variabel yang memiliki pengaruh signifikan secara statistik adalah Age, Department, DistanceFromHome, EmployeeCount, EnvironmentSatisfaction, JobInvolvement, JobSatisfaction, MaritalStatus, NumCompaniesWorked, OverTime, RelationshipSatisfaction, StandardHours, StockOptionLevel, WorkLifeBalance, YearsInCurrentRole, YearsSinceLastPromotion, dan YearsWithCurrManager

<img width="345" alt="image" src="https://github.com/user-attachments/assets/4c21b063-d99c-42bf-94fc-e3dc4b6d1c43" />

### 4. Modeling
Untuk memprediksi faktor apa saja yang mempengaruhi tingkat attrition, dirancang persamaan regresi menggunakan Random Forest dengan memasukkan variabel pada tahap sebelumnya 

### 5. Evaluation
Setelah model tadi dijalankan, dilakukan evaluasi dan mendapatkan hasil sebagai berikut

<img width="277" alt="image" src="https://github.com/user-attachments/assets/0bc608d9-0524-46a6-accd-c2ca914fdcf0" />

Berdasarkan hasil evaluasi model Random Forest Classifier yang dibangun untuk memprediksi attrition karyawan, diperoleh metrik evaluasi sebagai berikut:

Accuracy (akurasi): 85,38%

Precision untuk kelas Yes (attrition): 1.00

Recall untuk kelas Yes: 0.205

F1-Score untuk kelas Yes: 0.34

Hasil evaluasi menunjukkan bahwa model memiliki akurasi keseluruhan yang cukup tinggi, yaitu sekitar 85%, dan precision sempurna (1.00) untuk memprediksi karyawan yang akan keluar. Namun demikian, nilai recall untuk kelas “Yes” (karyawan yang keluar) sangat rendah, yaitu hanya 0.205. Artinya, dari seluruh karyawan yang sebenarnya keluar, hanya sekitar 20% yang berhasil terdeteksi oleh model.

📊 Confusion Matrix:
- True Positif (TP): 8 — Karyawan keluar yang berhasil diprediksi keluar

- False Negatif (FN): 31 — Karyawan keluar yang diprediksi tetap

- True Negatif (TN): 173 — Karyawan tetap yang diprediksi tetap

- False Positif (FP): 0 — Tidak ada karyawan tetap yang salah diprediksi keluar

*Interpretasi:*
- Model ini cenderung sangat berhati-hati dan tidak memprediksi karyawan akan keluar kecuali sangat yakin. Hal ini menghasilkan precision tinggi tapi recall rendah.

- Dalam konteks HR, ini berarti banyak potensi attrition yang terlewatkan, sehingga model kurang efektif sebagai sistem peringatan dini (early warning system).

- Namun, model ini dapat dijadikan dasar awal untuk menganalisis faktor-faktor yang dominan pada prediksi “keluar” (misalnya fitur penting seperti OverTime, JobSatisfaction, DistanceFromHome, dll.)

*Rekomendasi:*
- Coba penyesuaian threshold probabilitas untuk menyeimbangkan precision dan recall.

- Gunakan oversampling seperti SMOTE untuk menangani ketidakseimbangan kelas (karena attrition = Yes hanya sebagian kecil dari data).

- Eksperimen dengan model lain (misalnya XGBoost, Logistic Regression) dan tuning hyperparameter.




