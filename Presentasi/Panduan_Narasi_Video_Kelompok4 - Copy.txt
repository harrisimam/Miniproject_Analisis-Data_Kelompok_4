# Panduan Video Presentasi Kelompok 4

Durasi target: 10 menit. Slide 16 berisi referensi pendukung dan tidak membutuhkan narasi tambahan. Waktu tiap slide adalah target latihan, bukan perpindahan slide otomatis.

| Anggota | NIM | Slide | Waktu |
|---|---|---|---|
| Bagus Hardiansyah | 267150100111001 | 1–5 | 00:00–03:20 |
| Mitra Istiar Wardhana | 267150100111023 | 6–10 | 03:20–06:40 |
| Harris Imam Fathoni | 267150100111026 | 11–15 | 06:40–10:00 |

Pembagian ini merupakan rencana presentasi, bukan klaim pembagian pekerjaan eksperimen. Naskah adalah draf berbantuan AI: tinjau dan sesuaikan interpretasi dengan pemahaman kelompok sebelum merekam.

## Persiapan rekaman

- Gunakan rasio 16:9 dan tampilkan slide penuh.
- Buka Notes pada PowerPoint untuk membaca narasi setiap slide saat latihan.
- Rekam bagian tiap anggota sesuai slide yang ditentukan. Gunakan kalimat transisi pada slide 5 dan 10.
- Jika memakai kamera wajah, posisikan agar tidak menutupi tabel dan grafik.
- Lakukan latihan dengan stopwatch. Beri jeda singkat saat menunjukkan angka, sesuaikan tempo agar setiap bagian mendekati 3 menit 20 detik.
- Slide 16 dapat ditampilkan sebagai referensi setelah penutup. Durasi diam tambahan akan menambah total video jika tidak dipotong dari waktu penutup.
- Berkas PPTX belum berisi rekaman suara/video.

## Narasi per slide

### Slide 1 — Readmisi Dini Diabetes

PEMBICARA: Bagus Hardiansyah
NIM: 267150100111001
WAKTU: 00:00–00:35 (35 detik)

NARASI DRAF:
Selamat datang. Kami Kelompok 4 dari Program Doktor Ilmu Komputer Universitas Brawijaya. Presentasi ini membahas MP2 tentang pembersihan data terstruktur dan MP4 tentang exploratory data analysis. Saya Bagus Hardiansyah akan menjelaskan data dan rancangan eksperimen. Mitra Istiar Wardhana membahas keputusan prapemrosesan serta evaluasi model. Harris Imam Fathoni membahas pola EDA dan kesimpulan. Masing-masing mendapat waktu tiga menit dua puluh detik. Fokus kami adalah menunjukkan keputusan berbasis data dan membaca hasil model secara jujur.

SUMBER:
Mini Project 2 (MP2) – Pembersihan Data Terstruktur.pdf, unggahan pengguna. Angka berasal dari eksperimen mp2_colab_.ipynb. Mini Project 4 (MP4) – Exploratory Data Analysis.pdf, unggahan pengguna. Angka berasal dari eksperimen mp4_eda_colab_.ipynb.

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.

### Slide 2 — Dataset dan target eksperimen

PEMBICARA: Bagus Hardiansyah
NIM: 267150100111001
WAKTU: 00:35–01:15 (40 detik)

NARASI DRAF:
Dataset yang kami gunakan adalah Diabetes 130-US Hospitals dari UCI, dengan lisensi CC BY 4.0. Berkas aktual memuat 101.766 kunjungan, 50 kolom, dan 71.518 pasien unik. Satu baris adalah kunjungan, sehingga seorang pasien dapat memiliki lebih dari satu baris. Kami menetapkan target positif sebagai readmisi kurang dari 30 hari. Jumlahnya 11.357, atau sekitar 11,16 persen. Ketidakseimbangan ini membuat accuracy saja tidak cukup. Model memakai informasi selama perawatan, sehingga prediksi diposisikan saat pasien keluar rumah sakit.

SUMBER:
Mini Project 2 (MP2) – Pembersihan Data Terstruktur.pdf, unggahan pengguna. Angka berasal dari eksperimen mp2_colab_.ipynb. Hlm. 1. UCI DOI https://doi.org/10.24432/C5230J

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.

### Slide 3 — Pemisahan data berdasarkan pasien

PEMBICARA: Bagus Hardiansyah
NIM: 267150100111001
WAKTU: 01:15–01:55 (40 detik)

NARASI DRAF:
Kami membagi data berdasarkan identitas pasien, bukan mengacak kunjungan secara bebas. Tujuannya agar kunjungan pasien yang sama tidak muncul pada kelompok pelatihan dan pengujian. Train berisi 60.878 kunjungan, validation 20.400, dan test 20.488. Strategi imputasi dipilih memakai validation. Setelah itu model akhir dilatih ulang pada train ditambah validation dan diuji pada test. Untuk EDA, kami memakai cleaner terpisah yang hanya mempelajari train. Dengan demikian, analisis distribusi MP4 tidak memakai cleaner yang sudah belajar dari validation.

SUMBER:
Mini Project 2 (MP2) – Pembersihan Data Terstruktur.pdf, unggahan pengguna. Angka berasal dari eksperimen mp2_colab_.ipynb. Hlm. 1 dan 4. Mini Project 4 (MP4) – Exploratory Data Analysis.pdf, unggahan pengguna. Angka berasal dari eksperimen mp4_eda_colab_.ipynb. Hlm. 1.

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.

### Slide 4 — Audit menemukan kehilangan yang tinggi

PEMBICARA: Bagus Hardiansyah
NIM: 267150100111001
WAKTU: 01:55–02:40 (45 detik)

NARASI DRAF:
Masalah kualitas terbesar adalah data hilang pada weight sebesar 96,86 persen, medical specialty 49,08 persen, dan payer code 39,56 persen. Ketiga fitur itu dikeluarkan dari pipeline penuh sebagai kompromi biaya dan informasi. Race dan diagnosis mempunyai kehilangan lebih kecil, sedangkan gender memiliki tiga kategori invalid. Audit tidak menemukan duplikat baris maupun duplikat encounter ID. Namun ada 16.773 pasien dengan kunjungan berulang. Kami mempertahankannya karena kunjungan berulang bukan duplikat catatan. Nol pelanggaran rentang logis juga tidak membuktikan bahwa semua data benar secara klinis.

SUMBER:
Mini Project 2 (MP2) – Pembersihan Data Terstruktur.pdf, unggahan pengguna. Angka berasal dari eksperimen mp2_colab_.ipynb. Hlm. 2.

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.

### Slide 5 — Integrasi kamus menjaga jumlah kunjungan

PEMBICARA: Bagus Hardiansyah
NIM: 267150100111001
WAKTU: 02:40–03:20 (40 detik)

NARASI DRAF:
Kami menggabungkan data utama dengan IDS mapping melalui tiga kode nominal, yaitu jenis penerimaan, disposisi keluar, dan asal penerimaan. Seluruh kode pada observasi berhasil dipetakan, dan setiap join mempertahankan 101.766 kunjungan. Kamus membantu interpretasi, tetapi kedua berkas berasal dari paket yang sama. Karena itu, kami tidak menyebutnya dua sumber klinis independen. Audit juga mencakup akurasi logis, kelengkapan, konsistensi, ketepatan waktu, kepercayaan, dan interpretabilitas. Selanjutnya, Mitra akan menjelaskan strategi pembersihan dan dampaknya pada model.

SUMBER:
Mini Project 2 (MP2) – Pembersihan Data Terstruktur.pdf, unggahan pengguna. Angka berasal dari eksperimen mp2_colab_.ipynb. Hlm. 2.

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.

### Slide 6 — Modus terpilih setelah hasil validation seri

PEMBICARA: Mitra Istiar Wardhana
NIM: 267150100111023
WAKTU: 03:20–04:00 (40 detik)

NARASI DRAF:
Terima kasih, Bagus. Kami membandingkan tiga strategi menggunakan model dan validation yang sama. Penghapusan baris hanya berlaku pada train, sementara missing pada validation tetap memakai modus train. Strategi kedua mengisi kategori dengan modus. Strategi ketiga memakai ExtraTrees untuk memprediksi kategori yang hilang dari delapan fitur numerik. Modus dan imputasi model memperoleh F1 makro yang sama, yaitu 0,527771. Pengurutan stabil memilih modus ketika kedua kriteria seri. Pilihan ini juga lebih sederhana. Kesamaan metrik tidak membuktikan bahwa semua hasil imputasinya identik.

SUMBER:
Mini Project 2 (MP2) – Pembersihan Data Terstruktur.pdf, unggahan pengguna. Angka berasal dari eksperimen mp2_colab_.ipynb. Hlm. 3.

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.

### Slide 7 — Outlier tetap dipertahankan

PEMBICARA: Mitra Istiar Wardhana
NIM: 267150100111023
WAKTU: 04:00–04:35 (35 detik)

NARASI DRAF:
Deteksi memakai IQR, Z-score, boxplot, dan scatter plot. Kami mempertahankan kandidat outlier karena hitungan layanan yang tinggi dapat merupakan observasi valid. Contoh pentingnya adalah rawat jalan dan darurat sebelumnya. Kuartil pertama dan ketiganya sama-sama nol, sehingga semua nilai positif ditandai oleh batas IQR. Menghapus semuanya akan membuang riwayat penggunaan layanan. Keputusan mempertahankan ini tidak menyatakan bahwa semua nilai pasti benar. Artinya, bukti statistik yang tersedia belum cukup untuk menyebutnya salah catat.

SUMBER:
Mini Project 2 (MP2) – Pembersihan Data Terstruktur.pdf, unggahan pengguna. Angka berasal dari eksperimen mp2_colab_.ipynb. Hlm. 3.

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.

### Slide 8 — Transformasi dan reduksi fitur

PEMBICARA: Mitra Istiar Wardhana
NIM: 267150100111023
WAKTU: 04:35–05:15 (40 detik)

NARASI DRAF:
Pipeline penuh melakukan standardisasi numerik, encoding ordinal usia, one-hot untuk kategori nominal, serta pengelompokan lama rawat. VarianceThreshold menyaring fitur konstan dan SelectPercentile memilih 75 persen fitur yang tersisa. Matriks hasil model final berubah dari 944 menjadi 941 lalu 705 dimensi. Angka ini diperoleh saat pipeline final diterapkan pada train. Sampling bertingkat mengurangi 60.878 menjadi 15.219 baris hanya untuk demonstrasi numerosity, bukan eksperimen utama. Lama rawat berjenis diskrit, sehingga binning ini belum memenuhi secara literal persyaratan atribut kontinu.

SUMBER:
Mini Project 2 (MP2) – Pembersihan Data Terstruktur.pdf, unggahan pengguna. Angka berasal dari eksperimen mp2_colab_.ipynb. Hlm. 4.

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.

### Slide 9 — Recall naik, accuracy dan F1 turun

PEMBICARA: Mitra Istiar Wardhana
NIM: 267150100111023
WAKTU: 05:15–06:05 (50 detik)

NARASI DRAF:
Pada test yang sama, baseline memperoleh accuracy 0,624512 dan pipeline penuh 0,599717. F1 positif juga turun sedikit, dari 0,272942 menjadi 0,270828. Sebaliknya recall naik dari 0,610055 menjadi 0,643431. Jadi, pembersihan tidak meningkatkan semua ukuran kinerja secara bersamaan. Kedua model menggunakan Decision Tree dengan kedalaman maksimum enam, minimum seratus sampel daun, bobot kelas balanced, dan ambang nol koma lima. ROC-AUC keduanya sekitar 0,662. Karena beberapa komponen berubah sekaligus, kami belum dapat mengaitkan perubahan skor pada satu langkah tertentu. Diperlukan ablasi dengan rancangan validasi baru.

SUMBER:
Mini Project 2 (MP2) – Pembersihan Data Terstruktur.pdf, unggahan pengguna. Angka berasal dari eksperimen mp2_colab_.ipynb. Hlm. 5.

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.

### Slide 10 — Tambahan deteksi disertai false positive

PEMBICARA: Mitra Istiar Wardhana
NIM: 267150100111023
WAKTU: 06:05–06:40 (35 detik)

NARASI DRAF:
Confusion matrix menjelaskan perubahan tadi. Pipeline penuh mendeteksi 1.523 kasus positif, dibandingkan 1.444 pada baseline. Ada tambahan 79 kasus terdeteksi, tetapi false positive bertambah 587. Precision turun menjadi sekitar 17,15 persen, sehingga sebagian besar peringatan positif masih keliru. Kami menyimpulkan bahwa ada kompromi antara sensitivitas dan jumlah peringatan salah, bukan peningkatan universal. Hasil ini belum memadai untuk keputusan klinis. Berikutnya Harris akan menjelaskan pola data yang ditemukan melalui EDA train.

SUMBER:
Mini Project 2 (MP2) – Pembersihan Data Terstruktur.pdf, unggahan pengguna. Angka berasal dari eksperimen mp2_colab_.ipynb. Hlm. 5.

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.

### Slide 11 — Q1: proporsi readmisi tidak monoton terhadap usia

PEMBICARA: Harris Imam Fathoni
NIM: 267150100111026
WAKTU: 06:40–07:20 (40 detik)

NARASI DRAF:
Terima kasih, Mitra. MP4 menggunakan 60.878 kunjungan train dari 42.910 pasien, dengan cleaner yang hanya fit pada train. Pertanyaan pertama adalah hubungan interval usia dengan proporsi readmisi dini. Proporsi tertinggi teramati pada usia dua puluh sampai kurang dari tiga puluh tahun, sekitar 13,97 persen. Sementara kelompok tujuh puluh sampai kurang dari delapan puluh tahun memiliki jumlah positif terbanyak karena ukuran kelompoknya besar. Polanya tidak monoton. Kelompok anak hanya 93 kunjungan, sehingga angka proporsinya perlu ditafsirkan hati-hati. Ini hubungan deskriptif, belum mengendalikan karakteristik pasien lainnya.

SUMBER:
Mini Project 4 (MP4) – Exploratory Data Analysis.pdf, unggahan pengguna. Angka berasal dari eksperimen mp4_eda_colab_.ipynb. Hlm. 1 dan 3.

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.

### Slide 12 — Q2 dan Q3: riwayat layanan dan lama rawat

PEMBICARA: Harris Imam Fathoni
NIM: 267150100111026
WAKTU: 07:20–08:05 (45 detik)

NARASI DRAF:
Pertanyaan kedua menunjukkan kenaikan proporsi readmisi menurut rawat inap sebelumnya. Proporsinya 8,43 persen pada kelompok tanpa riwayat dan 25,55 persen pada kelompok tiga atau lebih. Selisihnya 17,13 poin persentase dan rasio proporsinya sekitar tiga kali. Ini belum merupakan efek kausal karena kompleksitas pasien dan faktor lain dapat berperan. Untuk pertanyaan ketiga, mean lama rawat kelas positif adalah 4,772 hari, lebih tinggi daripada 4,335 hari pada kelas lain. Namun median keduanya tetap empat hari. Jadi lama rawat sendiri memberi pemisahan yang terbatas.

SUMBER:
Mini Project 4 (MP4) – Exploratory Data Analysis.pdf, unggahan pengguna. Angka berasal dari eksperimen mp4_eda_colab_.ipynb. Hlm. 3.

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.

### Slide 13 — Sebaran miring, multikolinearitas numerik rendah

PEMBICARA: Harris Imam Fathoni
NIM: 267150100111026
WAKTU: 08:05–08:45 (40 detik)

NARASI DRAF:
Riwayat layanan memiliki distribusi yang sangat miring ke kanan. Skewness kunjungan darurat mencapai 27,85, sedangkan rawat jalan 8,65. Shapiro-Wilk pada lima ribu pasien per fitur menghasilkan nilai p di bawah nol koma nol lima untuk seluruh fitur. Hasil ini wajar dibaca bersama sifat data diskrit dan penumpukan nol, bukan alasan memaksakan normalitas. Decision Tree tidak mensyaratkan normalitas fitur. VIF pada delapan numerik berada antara 1,03 dan 1,53. Korelasi lama rawat dengan obat sekitar nol koma empat enam, dan pairplot menunjukkan kelas saling tumpang tindih.

SUMBER:
Mini Project 4 (MP4) – Exploratory Data Analysis.pdf, unggahan pengguna. Angka berasal dari eksperimen mp4_eda_colab_.ipynb. Hlm. 2, 4, dan 5.

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.

### Slide 14 — Imputasi mengubah komposisi kategori

PEMBICARA: Harris Imam Fathoni
NIM: 267150100111026
WAKTU: 08:45–09:25 (40 detik)

NARASI DRAF:
Pembersihan kategori dan numerik memiliki dampak berbeda. Imputasi modus menaikkan proporsi Caucasian dari 74,75 menjadi 77 persen. Kenaikan 2,252 poin persentase sama dengan proporsi race yang hilang. Jadi kelengkapan membaik, tetapi komposisi kategori berubah. Sebaliknya, statistik numerik dan korelasi sebelum serta sesudah tetap sama karena nilainya dipertahankan. Pada tampilan modus laboratorium, NaN berasal dari pembacaan ulang teks None di CSV, bukan perubahan hasil model. Untuk studi selanjutnya, indikator missing dan evaluasi antarkelompok patut dipertimbangkan dalam validasi baru.

SUMBER:
Mini Project 4 (MP4) – Exploratory Data Analysis.pdf, unggahan pengguna. Angka berasal dari eksperimen mp4_eda_colab_.ipynb. Hlm. 2 dan 4.

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.

### Slide 15 — Kesimpulan dan batas eksperimen

PEMBICARA: Harris Imam Fathoni
NIM: 267150100111026
WAKTU: 09:25–10:00 (35 detik)

NARASI DRAF:
Kesimpulannya, pipeline menghasilkan alur yang dapat ditelusuri, tetapi tidak meningkatkan semua metrik. EDA menunjukkan riwayat rawat inap sebagai kandidat informasi penting, usia yang tidak monoton, dan tumpang tindih lama rawat. Data historis dan kunjungan berulang membatasi generalisasi. Integrasi dua sumber independen serta syarat atribut kontinu masih perlu dituntaskan. Kami tidak memakai temuan EDA untuk menyesuaikan model setelah melihat test. Bantuan AI digunakan untuk kode dan draf materi, sedangkan interpretasi akhir harus ditinjau kelompok. Terima kasih.

SUMBER:
Mini Project 2 (MP2) – Pembersihan Data Terstruktur.pdf, unggahan pengguna. Angka berasal dari eksperimen mp2_colab_.ipynb. Hlm. 6. Mini Project 4 (MP4) – Exploratory Data Analysis.pdf, unggahan pengguna. Angka berasal dari eksperimen mp4_eda_colab_.ipynb. Hlm. 6.

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.

### Slide 16 — Referensi dan sumber hasil

PEMBICARA: Harris Imam Fathoni
NIM: 267150100111026
WAKTU: Referensi pendukung

NARASI DRAF:
Slide pendukung setelah penutup, tidak menambah durasi narasi sepuluh menit. Referensi lengkap juga tersedia di laporan MP2 dan MP4. Materi presentasi ini merupakan draf berbantuan AI yang perlu ditinjau anggota sebelum direkam.

SUMBER:
Mini Project 2 (MP2) – Pembersihan Data Terstruktur.pdf, unggahan pengguna. Angka berasal dari eksperimen mp2_colab_.ipynb. Mini Project 4 (MP4) – Exploratory Data Analysis.pdf, unggahan pengguna. Angka berasal dari eksperimen mp4_eda_colab_.ipynb.

Catatan: sesuaikan narasi dengan pemahaman kelompok. Pembagian ini adalah rencana presentasi, bukan rekonstruksi kontribusi eksperimen.
