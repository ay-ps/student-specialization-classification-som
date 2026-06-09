# Model Klasifikasi Peminatan Mahasiswa Berbasis Self-Organizing Map

Repository ini berisi source code implementasi model klasifikasi peminatan mahasiswa berbasis **Self-Organizing Map (SOM)**. Model ini digunakan untuk memetakan pola data akademik mahasiswa ke dalam kategori peminatan studi.

## Deskripsi Singkat

Project ini dikembangkan sebagai bagian dari penelitian mengenai penerapan metode **Self-Organizing Map** untuk klasifikasi peminatan mahasiswa. Source code dalam repository ini mencakup proses preprocessing data, pembentukan fitur rumpun peminatan, pelatihan model SOM, pelabelan neuron menggunakan majority voting, prediksi data uji, dan evaluasi model.

Repository ini memuat dua alur implementasi model, yaitu model baseline SOM dan model SOM dengan penerapan SMOTE pada data latih.

## Isi Repository

* `SOM_Baseline.ipynb`
  Berisi source code implementasi model baseline Self-Organizing Map tanpa penerapan SMOTE.

* `SOM_SMOTE.ipynb`
  Berisi source code implementasi model Self-Organizing Map dengan penerapan SMOTE pada data latih.

* `.gitignore`
  Berisi daftar file atau folder yang tidak disertakan dalam repository.

## Metode

Repository ini memuat dua alur implementasi model, yaitu model baseline SOM dan model SOM dengan SMOTE.

### 1. Model Baseline Self-Organizing Map

Tahapan pada model baseline meliputi:

1. pemanggilan dataset,
2. pemilihan fitur akademik,
3. normalisasi data menggunakan Min-Max Scaling,
4. pembentukan fitur rumpun peminatan,
5. transformasi fitur,
6. pembagian data latih dan data uji,
7. pelatihan model Self-Organizing Map,
8. pelabelan neuron menggunakan majority voting,
9. prediksi data uji,
10. evaluasi model baseline.

### 2. Model Self-Organizing Map dengan SMOTE

Tahapan pada model SOM dengan SMOTE meliputi:

1. pemanggilan dataset,
2. pemilihan fitur akademik,
3. normalisasi data menggunakan Min-Max Scaling,
4. pembentukan fitur rumpun peminatan,
5. transformasi fitur,
6. pembagian data latih dan data uji,
7. penerapan SMOTE pada data latih,
8. pelatihan model Self-Organizing Map menggunakan data latih hasil SMOTE,
9. pelabelan neuron menggunakan majority voting,
10. prediksi data uji asli,
11. evaluasi model SOM dengan SMOTE.

## Catatan Dataset

Dataset tidak disertakan dalam repository ini. Repository hanya memuat source code implementasi model.

## Teknologi dan Library

Project ini menggunakan bahasa pemrograman Python dengan beberapa library utama, yaitu:

* `pandas`, digunakan untuk membaca dan mengolah dataset.
* `numpy`, digunakan untuk operasi numerik dan transformasi fitur.
* `scikit-learn`, digunakan untuk normalisasi data, pembagian data latih dan data uji, serta evaluasi model seperti accuracy, classification report, dan confusion matrix.
* `MiniSom`, digunakan untuk membangun dan melatih model Self-Organizing Map.
* `imbalanced-learn`, digunakan untuk menerapkan SMOTE pada data latih.

## Jenis Ciptaan

Source code ini merupakan bagian dari ciptaan program komputer dengan judul:

**Model Klasifikasi Peminatan Mahasiswa Berbasis Self-Organizing Map**
