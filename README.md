# PCD-LETSGOWW
# 🍅 Klasifikasi Penyakit Septoria Leaf Spot dan Early Blight pada Citra Daun Tomat Menggunakan K-Nearest Neighbors, Support Vector Machine, dan Random Forest

## 👥 Anggota Kelompok 12

| Nama                    | NIM         |
| ----------------------- | ----------- |
| Nadin Mufida            | F1D02410128 |
| Muhammad Rizky Hisyam   | F1D02410018 |
| Ni Wayan Eka Aprilianti | F1D02410021 |

---

## 📖 Latar Belakang

Tomat merupakan salah satu komoditas hortikultura yang memiliki nilai ekonomi tinggi. Namun, tanaman tomat rentan terhadap berbagai penyakit daun yang dapat menurunkan kualitas maupun kuantitas hasil panen. Dua penyakit yang sering ditemukan adalah **Early Blight** dan **Septoria Leaf Spot**.

Identifikasi penyakit secara manual membutuhkan keahlian khusus, memerlukan waktu yang cukup lama, dan berpotensi menghasilkan kesalahan akibat subjektivitas pengamat. Oleh karena itu, diperlukan sistem otomatis yang mampu membantu proses identifikasi penyakit secara lebih cepat dan konsisten.

Pengolahan Citra Digital (PCD) dan Machine Learning dapat dimanfaatkan untuk mengenali pola tekstur pada daun tomat yang terinfeksi. Dengan melakukan preprocessing citra, ekstraksi fitur tekstur menggunakan Gray Level Co-occurrence Matrix (GLCM), serta klasifikasi menggunakan beberapa algoritma machine learning, sistem dapat mengidentifikasi jenis penyakit secara otomatis.

---

## 📋 Deskripsi Program

Program ini dibangun untuk mengklasifikasikan citra daun tomat ke dalam tiga kelas, yaitu **Early Blight**, **Septoria Leaf Spot**, dan **Healthy**, melalui empat tahapan utama:

### 1. Preprocessing

Mengolah citra mentah untuk meningkatkan kualitas citra dan mengurangi noise sehingga tekstur daun lebih jelas.

### 2. Ekstraksi Fitur GLCM

Mengekstraksi karakteristik tekstur citra menggunakan Gray Level Co-occurrence Matrix (GLCM) pada empat arah:

* 0°
* 45°
* 90°
* 135°

Fitur yang digunakan meliputi:

* Contrast
* Dissimilarity
* Homogeneity
* ASM
* Energy
* Correlation
* Entropy

### 3. Seleksi Fitur

Mengurangi fitur yang bersifat redundan berdasarkan korelasi antar fitur sehingga model dapat bekerja lebih efisien.

### 4. Klasifikasi

Melatih dan membandingkan performa tiga algoritma machine learning:

* K-Nearest Neighbors (KNN)
* Support Vector Machine (SVM)
* Random Forest (RF)

---

## 🍃 Dataset

Dataset yang digunakan merupakan citra daun tomat yang terdiri dari tiga kelas:

| Kelas              | Jumlah Citra |
| ------------------ | ------------ |
| Early Blight       | 1.000        |
| Septoria Leaf Spot | 1.000        |
| Healthy            | 1.000        |

**Total Dataset: 3.000 citra**

### Karakteristik Dataset

* Dataset seimbang (balanced dataset)
* Citra berwarna (BGR)
* Dibaca menggunakan OpenCV
* Bersumber dari folder *train* pada koleksi dataset daun tomat

---

## 🔬 Tahap Preprocessing

Kelompok melakukan tiga percobaan preprocessing untuk membandingkan pengaruhnya terhadap performa klasifikasi.

---

### Percobaan 1 — Pendekatan Dasar

Pipeline:

```text
RGB → Grayscale → Histogram Equalization → Median Filter
```

#### Grayscale

Mengubah citra menjadi satu kanal keabuan.

#### Histogram Equalization

Meningkatkan kontras dengan meratakan distribusi intensitas piksel.

#### Median Filter

Mengurangi noise tanpa merusak detail tepi objek.

---

### Percobaan 2 — Masking Berdasarkan Saturasi

Pipeline:

```text
Threshold Saturasi → Median Filter → Masking Daun → Grayscale
```

#### Threshold Saturasi

Memisahkan area daun dari background berdasarkan nilai saturasi warna.

#### Median Filter

Membersihkan noise kecil pada hasil segmentasi.

#### Masking

Menghapus background sehingga hanya area daun yang diproses.

#### Grayscale

Mengubah hasil masking menjadi citra grayscale.

---

### Percobaan 3 — Outline Morfologi dan Penghapusan Area Penyakit

#### Jalur A — Outline Bentuk Daun

```text
Threshold Saturasi
→ Median Filter
→ Gradien Morfologi
```

#### Jalur B — Area Daun Sehat

```text
Threshold Channel Hijau
→ Median Filter
→ Masking
→ Grayscale
```

#### Merge

Hasil dari kedua jalur digabungkan sehingga menghasilkan citra yang memuat:

* Outline bentuk daun
* Area daun sehat dalam grayscale

Pendekatan ini bertujuan mempertahankan bentuk daun sekaligus mengurangi pengaruh area penyakit.

---

## 📊 Hasil dan Analisis

### Akurasi Testing (%)

| Model         | Percobaan 1 | Percobaan 2 | Percobaan 3 |
| ------------- | ----------: | ----------: | ----------: |
| Random Forest |      81.16% |  **86.17%** |      79.17% |
| SVM           |  **85.66%** |      83.50% |      81.67% |
| KNN           |      85.16% |      85.50% |      79.17% |

### Temuan Utama

#### Akurasi Tertinggi

Random Forest pada Percobaan 2 memperoleh akurasi tertinggi sebesar **86.17%**. Pemisahan area daun dari background melalui masking membantu model lebih fokus pada tekstur daun.

#### Model Paling Stabil

SVM menunjukkan selisih akurasi training dan testing yang relatif kecil sehingga memiliki kemampuan generalisasi yang baik.

#### Pengaruh Preprocessing

Preprocessing yang lebih kompleks tidak selalu menghasilkan performa yang lebih baik. Pada Percobaan 3, akurasi seluruh model mengalami penurunan dibandingkan dua percobaan sebelumnya.

---

## 🏁 Kesimpulan

1. Teknik preprocessing memberikan pengaruh signifikan terhadap performa klasifikasi penyakit daun tomat.

2. Pendekatan masking berdasarkan saturasi warna pada Percobaan 2 menghasilkan performa terbaik, terutama pada Random Forest.

3. SVM menunjukkan kemampuan generalisasi yang paling stabil pada seluruh percobaan.

4. Preprocessing yang terlalu kompleks tidak selalu meningkatkan akurasi karena dapat menghilangkan informasi tekstur yang penting.

5. Kombinasi preprocessing, ekstraksi fitur GLCM, dan machine learning mampu mengklasifikasikan penyakit daun tomat dengan akurasi yang cukup tinggi.

---

## 📁 Struktur Repository

```text
PCD-LETSGOWW/
│
├── archive/tomato/train/
│   ├── Tomato__Early_Blight/
│   ├── Tomato__healthy/
│   └── Tomato__Septoria_Leaf_Spot/
│
├── p1.ipynb
├── p2.ipynb
├── 3.ipynb
│
├── hasil_ekstraksi_1.csv
├── hasil_ekstraksi_2.csv
└── hasil_ekstraksi_3.csv

```