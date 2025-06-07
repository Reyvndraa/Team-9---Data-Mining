# SISTEM PERINGATAN DINI RESIKO DROP OUT MAHASISWA
##### Kelompok 9:
##### - Neli Agustin (G1A022048)
##### -  Rizki Ramadani Dalimunthe (G1A022054)
##### - Yuda Reyvandra Herman (G1A022072)

## Gambaran Umum Proyek
Proyek ini bertujuan untuk mengembangkan sistem prediksi berbasis data mining untuk mengidentifikasi mahasiswa yang memiliki risiko tinggi untuk drop out (DO) dari universitas. Dengan mendeteksi mahasiswa berisiko sejak dini, universitas dapat memberikan intervensi proaktif dan dukungan yang diperlukan, sehingga meningkatkan tingkat keberhasilan studi mahasiswa dan mengurangi angka DO. Sistem ini dibangun menggunakan data akademik dan non-akademik mahasiswa, seperti IPK per semester, tingkat kehadiran, riwayat pengambilan ulang mata kuliah, aktivitas di sistem pembelajaran daring, serta status pekerjaan dan beban kerja.

## Metode Data Mining: Random Forest
Model utama yang digunakan dalam proyek ini adalah Random Forest Classifier. Pilihan ini didasarkan pada beberapa keunggulan:
- Akurasi Tinggi: Random Forest dikenal memberikan hasil prediksi yang sangat akurat pada dataset yang kompleks.
- Ketahanan terhadap Overfitting: Model ini efektif dalam mengurangi risiko overfitting, memastikan generalisasi yang baik pada data baru.
- Fleksibilitas Data: Mampu menangani berbagai jenis data, baik numerik maupun kategorikal.
- Feature Importance: Menyediakan informasi tentang fitur mana yang paling berpengaruh dalam prediksi, memberikan wawasan berharga untuk intervensi.

## Dataset
Dataset yang digunakan dalam proyek ini adalah dataset dummy yang dibuat secara sintetis untuk mensimulasikan karakteristik data mahasiswa sungguhan. Dataset ini mencakup 5000 entri mahasiswa dengan berbagai fitur relevan yang diyakini memengaruhi risiko DO.

## Fitur-fitur (Kolom) dalam Dataset:
Setiap baris dalam dataset merepresentasikan satu mahasiswa, dengan kolom-kolom berikut sebagai fiturnya:
- NIM: Nomor Induk Mahasiswa. Ini adalah pengidentifikasi unik untuk setiap mahasiswa.
- IPK_Semester_1 s/d IPK_Semester_6: Indeks Prestasi Kumulatif (IPK) mahasiswa untuk setiap semester, dari semester 1 hingga semester 6. Ini mensimulasikan kinerja akademik mahasiswa dari waktu ke waktu.
- Kehadiran_Per_Mata_Kuliah: Rata-rata persentase kehadiran mahasiswa di seluruh mata kuliah. Fitur ini penting untuk mengukur tingkat komitmen dan partisipasi di kelas.
- Riwayat_Pengambilan_Ulang: Jumlah mata kuliah yang pernah diulang oleh mahasiswa. Pengulangan mata kuliah seringkali menjadi indikator kesulitan akademik.
- Aktivitas_Sistem_Pembelajaran_Daring: Skor yang mencerminkan tingkat aktivitas atau keterlibatan mahasiswa dalam sistem pembelajaran daring (misalnya, LMS). Ini bisa meliputi frekuensi login, partisipasi forum, atau penyelesaian tugas daring.
- Status_Pekerjaan: Status pekerjaan mahasiswa, yang bisa berupa "Bekerja" atau "Tidak Bekerja". Ini adalah faktor non-akademik yang dapat memengaruhi fokus studi.
- Beban_Kerja_JamPerMinggu: Jumlah jam kerja per minggu jika mahasiswa berstatus "Bekerja". Beban kerja yang tinggi dapat memengaruhi waktu dan energi untuk belajar.
- Status_Risiko_DO: Ini adalah variabel target kita, yang mengindikasikan apakah mahasiswa tersebut memiliki risiko tinggi untuk drop out ("Risiko Tinggi") atau tidak ("Aman"). Penentuan status ini didasarkan pada logika skor yang menggabungkan beberapa fitur lainnya.

Dataset ini dirancang untuk memberikan variasi yang cukup realistis agar dapat digunakan untuk pelatihan model data mining dalam memprediksi risiko DO.

## Metodologi Proyek: CRISP-DM
Proyek ini mengikuti kerangka kerja CRISP-DM (Cross-Industry Standard Process for Data Mining), yang terdiri dari fase-fase berikut:
- Pemahaman Bisnis (Business Understanding): Mendefinisikan tujuan proyek (mengurangi DO mahasiswa) dan kriteria keberhasilan.
- Pemahaman Data (Data Understanding): Eksplorasi awal data mahasiswa, mengidentifikasi kualitas data dan karakteristiknya.
- Persiapan Data (Data Preparation): Proses preprocessing data seperti penanganan duplikat, encoding variabel kategorikal, feature scaling, dan penanganan imbalanced data menggunakan SMOTE.
- Pemodelan (Modeling): Membangun dan melatih model Random Forest, termasuk hyperparameter tuning menggunakan GridSearchCV.
- Evaluasi (Evaluation): Menilai kinerja model menggunakan berbagai metrik (Confusion Matrix, Recall, Precision, F1-Score, Balanced Accuracy, ROC AUC, MCC) pada data pengujian.
- Penyebaran (Deployment): Rencana untuk mengintegrasikan model ke sistem universitas dan visualisasi hasil.

##  Preprocessing Data (Persiapan Data)
Tahap persiapan data sangat krusial untuk memastikan kualitas dan kesiapan data untuk pemodelan:
1. Penanganan Duplikat: Baris duplikat berdasarkan NIM dihapus untuk menjaga keunikan setiap entri mahasiswa.
2. Penanganan Missing Values: Meskipun dataset dummy Anda bersih, dalam skenario nyata, missing values pada fitur numerik akan diisi dengan median kolom tersebut, dan pada fitur kategorikal dengan modus.
3. Encoding Kategorikal:
- Variabel target Status_Risiko_DO diubah menjadi 0 (Aman) dan 1 (Risiko Tinggi) menggunakan LabelEncoder.
- Fitur Status_Pekerjaan juga di-encode menjadi 0 (Tidak Bekerja) dan 1 (Bekerja) menggunakan LabelEncoder.
4. Feature Scaling: Semua fitur numerik distandarisasi menggunakan StandardScaler agar memiliki rata-rata 0 dan standar deviasi 1.
5. Penanganan Imbalanced Data: SMOTE (Synthetic Minority Over-sampling Technique) diterapkan pada data pelatihan untuk menyeimbangkan jumlah sampel antara kelas 'Aman' dan 'Risiko Tinggi', memastikan model tidak bias.

## Hasil dan Evaluasi Model
Model Random Forest yang terlatih menunjukkan kinerja yang kuat:
Akurasi (Test Set): 0.99
Classification Report:

                 precision    recall  f1-score   support
          0       1.00      0.99      0.99       943
          1       0.89      0.95      0.92        57
          
    accuracy                           0.99      1000
    macro avg       0.94      0.97      0.95      1000
    weighted avg       0.99      0.99      0.99      1000

Confusion Matrix:

       [[936   7]  # True Negative: 936 (Aman diprediksi Aman)
       [  3  54]] # False Negative: 3 (Risiko diprediksi Aman); True Positive: 54 (Risiko diprediksi Risiko)

Balanced Accuracy: 0.9700
Matthews Correlation Coefficient (MCC): 0.9105
ROC AUC Score: Sekitar 0.9958 (nilai dapat sedikit bervariasi karena randomness dalam pembuatan data).

Hasil ini menunjukkan bahwa model memiliki kemampuan yang sangat baik dalam mendeteksi mahasiswa berisiko tinggi (Recall 0.95 untuk kelas 1) sambil tetap menjaga precision yang tinggi.

## Rencana Pengembangan Sistem ke Depan
Untuk menjadikan sistem ini lebih fungsional dan berdampak, rencana pengembangan meliputi:
1. Integrasi dengan Sistem Akademik: Membangun API untuk memungkinkan sistem informasi universitas mengirim data mahasiswa dan menerima prediksi risiko secara otomatis.
2. Dashboard Visual Interaktif: Membuat dashboard (menggunakan Power BI, Tableau, Dash, atau Streamlit) untuk rektorat dan penasihat. Dashboard ini akan menampilkan tren risiko DO, faktor-faktor pendorong utama (berdasarkan feature importance), dan distribusi risiko berdasarkan kriteria tertentu.
3. Peningkatan Berkelanjutan: Menerapkan feedback loop dengan mengumpulkan data hasil intervensi. Model akan dilatih ulang secara berkala dengan data terbaru untuk mempertahankan akurasi dan beradaptasi dengan perubahan tren.
4. Explanaible AI (XAI): Menerapkan teknik seperti SHAP atau LIME untuk memberikan penjelasan yang lebih detail mengapa seorang mahasiswa diprediksi berisiko, membantu penasihat memberikan bimbingan yang lebih personal.