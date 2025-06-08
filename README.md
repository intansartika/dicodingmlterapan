# Analisis Faktor Penyebab Tingginya Attrition Rate di PT Jaya Jaya Maju
## 1. Business Understanding
* Permasalahan:
Tingginya attrition rate (>10%) di perusahaan Jaya Jaya Maju. HR ingin memahami faktor-faktor yang mempengaruhi agar dapat menurunkan angka ini.

* Tujuan:
Membuat visualisasi dalam bentuk business dashboard untuk mengidentifikasi faktor-faktor yang mempengaruhi tingginya attrition rate dan memonitor karyawan.

* Cakupan Proyek:
Proyek ini bertujuan mengidentifikasi faktor-faktor yang mempengaruhi tingginya tingkat turnover karyawan (attrition rate) di perusahaan Jaya Jaya Maju. Secara khusus, cakupan proyek mencakup hal-hal berikut:

  * Analisis eksploratif terhadap data karyawan yang tersedia untuk memahami faktor-faktor seperti usia, tingkat kepuasan kerja, jabatan, durasi kerja, dan gaji terhadap attrition rate.

  * Persiapan data (Data Preparation) yang mencakup pembersihan, penanganan nilai hilang, serta transformasi data sesuai kebutuhan analisis.

  * Pembuatan dashboard interaktif menggunakan tools seperti Metabase, Tableau Public, atau Looker Studio, yang bertujuan membantu departemen HR dalam memonitor berbagai faktor kunci yang mempengaruhi attrition rate.

  * Visualisasi data minimal dalam bentuk grafik, diagram batang, pie chart, heatmap atau visualisasi interaktif lainnya yang mudah dimengerti oleh pengguna (stakeholders).

* Persiapan
  * Sumber Data: Data yang digunakan dalam proyek ini diperoleh dari repositori publik di GitHub dengan detail sebagai berikut:
    1. Link Data: [Dataset Employee Attrition](https://github.com/dicodingacademy/dicoding_dataset/tree/main/employee)
    2. Isi Data: Informasi mengenai atribut-atribut karyawan seperti usia, jenis kelamin, pendidikan, tingkat kepuasan kerja, gaji, jabatan, durasi kerja, dan status kerja (attrition).

  * Setup Environment: Untuk melaksanakan proyek ini, diperlukan setup environment dengan spesifikasi sebagai berikut:

    1. Bahasa Pemrograman dan Tools:
       Python (minimal versi 3.7)

       Google Colab Notebook (untuk analisis dan eksplorasi data)

       Looker Studio (untuk pembuatan dashboard interaktif)
    3. Library Utama Python:
       numpy: 2.0.2
     
       pandas: 2.2.2
     
       matplotlib: 3.10.0
     
       seaborn: 0.13.2
     
       google-colab: 1.0.0
     
       sklearn version: 1.6.1
     
       statsmodels: 0.14.4

## 2. Data Understanding
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


## 3. Data Preparation
Langkah ini meliputi:
* Membersihkan missing value (misalnya pada kolom Attrition)

Ditemukan bahwa terdapat 412 baris data attrition yang tidak ada

<img width="157" alt="image" src="https://github.com/user-attachments/assets/4da90ecf-458d-467f-88c3-965c14d2e083" />

Berikut ini tampilan data setelah dibersihkan sehingga tidak ditemukan lagi missing value

<img width="246" alt="image" src="https://github.com/user-attachments/assets/4b4120fb-f366-4dd0-a5b9-efcb50e0928b" />

* Encoding untuk variabel kategorikal

Setelahnya, dilakukan pelabelan/encoding untuk variabel kategorikal sehingga menjadi seperti berikut ini

<img width="707" alt="image" src="https://github.com/user-attachments/assets/62738a3b-bba6-457d-822d-64dd32b3471c" />

## 4. Uji Pengaruh Variabel

Untuk menentukan variabel mana yang memiliki pengaruh terhadap attrition, dilakukan identifikasi mengenai mutual information, uji pengaruh parsial, dan uji pengaruh simultan.

<img width="608" alt="image" src="https://github.com/user-attachments/assets/48f76566-d431-4d9a-8eb7-b1342bf8e96f" />

Setelah dilakukan uji pengaruh, diketahui bahwa variabel yang memiliki pengaruh signifikan secara statistik adalah Age, Department, DistanceFromHome, EmployeeCount, EnvironmentSatisfaction, JobInvolvement, JobSatisfaction, MaritalStatus, NumCompaniesWorked, OverTime, RelationshipSatisfaction, StandardHours, StockOptionLevel, WorkLifeBalance, YearsInCurrentRole, YearsSinceLastPromotion, dan YearsWithCurrManager

<img width="345" alt="image" src="https://github.com/user-attachments/assets/4c21b063-d99c-42bf-94fc-e3dc4b6d1c43" />

## 5. Modeling
Untuk memprediksi faktor apa saja yang mempengaruhi tingkat attrition, dirancang persamaan regresi menggunakan Random Forest dengan memasukkan variabel pada tahap sebelumnya 

## 6. Evaluation
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


## 7. Business Dashboard
Dashboard dapat diakses melalui Looker Studio [berikut](https://lookerstudio.google.com/u/0/reporting/88e80d8d-1bde-48e1-9f9e-91fbff64b121/page/cXqKF)

## 8. Kesimpulan
### A. Distribusi Demografi:
  * Mayoritas karyawan berada dalam kelompok usia 31-35 tahun (23.3%) diikuti kelompok 36-40 tahun (18.8%).
  * Karyawan laki-laki mendominasi dengan 59.8% dibandingkan perempuan (40.2%).
  * Sebagian besar karyawan berstatus menikah (673 karyawan), disusul yang lajang (470) dan bercerai (327).

### B. Attrition Berdasarkan Usia dan Gender:
  * Attrition cenderung lebih tinggi pada kelompok usia 26-30 tahun dan di bawah 25 tahun, menunjukkan potensi ketidakpuasan atau ketidakstabilan di kalangan karyawan muda.
  * Attrition terlihat lebih tinggi pada karyawan laki-laki dibandingkan perempuan.

### C. Faktor Kinerja dan Kepuasan:
  * Karyawan dengan rating kinerja tinggi (rating 3 dan 4) masih menunjukkan angka attrition yang signifikan, mengindikasikan bahwa performa tinggi tidak menjamin retensi.
  * Tingkat kepuasan lingkungan kerja dan keseimbangan kehidupan kerja (Work-Life Balance) memiliki hubungan yang jelas dengan tingkat attrition; semakin rendah tingkat kepuasan, semakin tinggi attrition.
  * Overtime (lembur) secara konsisten menunjukkan hubungan yang kuat dengan tingginya tingkat attrition, terutama pada karyawan yang sering melakukan overtime.
  * Kepuasan hubungan antar-karyawan dan kepuasan kerja secara keseluruhan juga berkorelasi kuat dengan tingkat attrition.

### D. Promosi dan Manajer Saat Ini:
  * Karyawan yang jarang mendapatkan promosi atau memiliki waktu lama dengan manajer yang sama cenderung memiliki tingkat attrition yang lebih tinggi, yang mungkin menunjukkan kebutuhan akan tantangan atau perubahan dalam lingkungan kerja.

## 9. Rekomendasi untuk Perusahaan:
  * *Program Retensi Karyawan Muda*: Perkenalkan program pengembangan karir yang jelas dan kesempatan pelatihan untuk karyawan muda guna meningkatkan retensi.

  * *Optimalisasi Lingkungan Kerja*: Tingkatkan kualitas lingkungan kerja, termasuk pengurangan waktu overtime dan peningkatan Work-Life Balance.

  * *Strategi Penghargaan dan Promosi*: Evaluasi sistem promosi agar lebih transparan dan adil untuk menjaga motivasi dan mempertahankan karyawan yang berkinerja baik.

  * *Penguatan Kepuasan Hubungan Kerja*: Tingkatkan hubungan antar-karyawan dan tim melalui kegiatan team-building, pelatihan interpersonal skill, dan komunikasi internal yang efektif.

Dengan mengimplementasikan rekomendasi ini, perusahaan dapat menurunkan tingkat attrition secara efektif dan menciptakan lingkungan kerja yang lebih produktif dan harmonis.
