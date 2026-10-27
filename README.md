Bagus Hardiansyah 267150100111001
Mitra Istiar Wardhana 267150100111023
Harris Imam Fathoni 267150100111026

Peran dari masing-masing anggota : Bagus Hardiansyah akan menjelaskan data dan rancangan eksperimen. Mitra Istiar Wardhana membahas keputusan prapemrosesan serta evaluasi model. Harris Imam Fathoni membahas pola EDA dan kesimpulan.


PANDUAN NARATIF MENJALANKAN MP2 DAN MP4 DI GOOGLE COLAB
Dataset: Diabetes 130-US Hospitals for Years 1999–2008

Dasar panduan: isi notebook mp2_terstruktur_colab_fix.ipynb dan mp4_eda_colab_fix.ipynb yang dilampirkan. Panduan ini menjelaskan kode yang tersedia; bukan laporan hasil eksekusi baru. Nomor sel di bawah dihitung dari atas, termasuk sel teks/Markdown. Angka eksekusi dalam tanda [ ] di Colab dapat berbeda. Gunakan judul bagian sebagai acuan utama.

A. PERSIAPAN DAN URUTAN PENGERJAAN

Mulailah dengan MP2, kemudian lanjutkan ke MP4. MP2 melakukan audit, pembersihan, integrasi tabel referensi, pembagian data, serta eksperimen model. MP4 menggunakan data train bersih dari MP2 untuk membuat statistik, grafik, dan ringkasan EDA. Kedua notebook harus menunjuk folder hasil yang sama agar file penghubung dapat ditemukan.

Pastikan akun Google yang dipakai di Colab mempunyai dua file berikut di My Drive:
/content/drive/MyDrive/S3 UB/Materi Kuliah/Semester 1/Topik Analisis Data/ETS/Dataset/diabetic_data.csv
/content/drive/MyDrive/S3 UB/Materi Kuliah/Semester 1/Topik Analisis Data/ETS/Dataset/IDS_mapping.csv

Nama folder, spasi, huruf besar-kecil, dan nama file harus sama dengan yang ditulis dalam kode. DATA_DIR adalah lokasi dataset asli. ROOT adalah lokasi proyek dan hasil, dengan nilai bawaan:
/content/drive/MyDrive/MP2_MP4_Diabetes

Keduanya berbeda fungsi. Tidak perlu memindahkan dataset ke ROOT. Kode membaca alamat dataset secara langsung tanpa mencari seluruh isi Drive. Cara ini mengurangi waktu pencarian, tetapi pembacaan file dan pelatihan model tetap memerlukan waktu.

Buka notebook MP2 di Google Colab. Apabila file unduhan berakhiran .ipynb.json seperti lampiran yang dibaca untuk panduan ini, simpan salinannya dengan ekstensi .ipynb sebelum mengunggah; isi berkas tersebut memang berformat notebook JSON. Gunakan runtime Python dengan CPU; kode ini tidak memerlukan GPU.

Apabila sebelumnya memakai notebook lama yang mengubah versi NumPy atau SciPy, simpan pekerjaan terlebih dahulu, lalu gunakan pilihan “Disconnect and delete runtime” atau “Putuskan dan hapus runtime” pada menu Runtime. Sambungkan kembali. Tindakan ini menghapus sesi dan file sementara /content, tetapi tidak menghapus berkas di Google Drive. Restart sesi saja belum tentu mengembalikan paket yang sudah diganti. Jangan menjalankan kembali sel pip install dari notebook versi lama.

Untuk pelaksanaan pertama, jalankan sel kode satu per satu dari atas agar keluaran mudah diperiksa. Setelah konfigurasi dipastikan benar, Run all dapat digunakan. Tunggu satu sel selesai sebelum melanjutkan. Sel Markdown berisi penjelasan dan tidak melakukan perhitungan.

B. MENJALANKAN MP2: PEMBERSIHAN DATA TERSTRUKTUR

1. Memeriksa lingkungan Python — sel 3

Jalankan “Periksa lingkungan Colab”. Sel ini memeriksa impor pustaka, operasi NumPy/SciPy, model kecil, baca-tulis Parquet, dan pembuatan gambar. PyArrow hanya dipasang apabila belum tersedia, tanpa mengganti dependensi lain. Keberhasilan ditandai tulisan “PEMERIKSAAN LINGKUNGAN LULUS”, diikuti versi Python dan pustaka. Jika muncul traceback, perbaiki lingkungan sebelum meneruskan. Sel 4 hanya berupa komentar dan tidak menghasilkan keluaran.

2. Menghubungkan Google Drive dan menentukan lokasi — sel 5

Jalankan sel lokasi dataset. Ketika diminta, pilih akun yang menyimpan dataset dan izinkan Colab mengakses Drive. Kode memeriksa keberadaan folder serta kedua CSV, kemudian membuat subfolder src, mp2, mp4, data, dan templates di ROOT. Periksa alamat setelah tulisan “Dataset:” dan “Hasil:”. Jika alamat dataset Bapak berbeda, ubah DATA_DIR pada kedua notebook secara konsisten. Jika menggunakan ROOT khusus, pastikan MP2 dan MP4 memakai nilai yang sama; variabel lingkungan DIABETES_PROJECT_ROOT, bila diatur, mengesampingkan lokasi bawaan.

3. Memuat modul Python — sel 6

Jalankan “Muat modul Python (jalankan, tidak perlu diedit)”. Notebook sudah membawa kode modul di dalamnya. Sel ini memeriksa sintaks, menulis diabetes_pipeline.py dan diabetes_eda.py ke ROOT/src, membersihkan modul lama dari memori, lalu mengimpor versi yang disertakan. Bapak tidak perlu membuat atau mengunggah kedua modul secara manual. Tanda berhasil adalah “Modul diabetes_pipeline dan diabetes_eda berhasil dimuat.” Hindari mengedit string panjang MODULES karena perubahan tanda kutip dapat menimbulkan SyntaxError.

4. Membaca data dan mencatat asal-usulnya — sel 8, bagian 1

Sebelum menjalankan, lengkapi nilai download_date yang masih berbunyi “ISI tanggal unduh oleh kelompok” dengan tanggal unduh sebenarnya jika diketahui. Jangan menebak tanggal. Jalankan sel untuk membaca diabetic_data.csv, menampilkan jumlah label readmitted, ukuran data, dan pasien unik. Target positif adalah readmitted bernilai <30; label lainnya menjadi kelas negatif. Literal “None” pada kategori pemeriksaan dipertahankan sebagai kategori, bukan otomatis dianggap nilai kosong.

Kode membuat source_manifest.json yang mencatat sumber, versi pustaka, seed, waktu audit, dan checksum SHA-256 kedua CSV. Manifest ini diperlukan oleh MP4 untuk memastikan dataset tidak berubah.

5. Melakukan audit kualitas — sel 10, bagian 2

Jalankan sel audit dan baca tabel nilai hilang, pemeriksaan rentang, serta enam dimensi kualitas. Hasil disimpan antara lain sebagai audit_columns.csv, range_checks.csv, dan quality_dimensions.csv. Angka nol untuk masalah tertentu, termasuk duplikat, tetap merupakan hasil yang sah. Pemeriksaan rentang hanya menilai kewajaran dan tidak membuktikan akurasi klinis suatu catatan.

6. Mengintegrasikan tabel referensi — sel 12, bagian 3

Jalankan integrasi dengan IDS_mapping.csv. Parser memisahkan bagian pemetaan di file tersebut dan menghubungkan kode ke deskripsinya. Periksa integration_audit.csv. Pemeriksaan assert memastikan jumlah baris sesudah integrasi sama dengan sebelumnya. Dua CSV ini merupakan data observasi dan kamus referensi dari satu paket, sehingga jangan menyebutnya dua sumber independen tanpa menyesuaikan dengan ketentuan dosen.

7. Membagi data berdasarkan pasien — sel 14, bagian 4

Jalankan pembagian train, validation, dan test. Pembagian mendekati 60%, 20%, dan 20% pasien, sehingga persentase baris kunjungan tidak harus persis sama. Seed yang digunakan adalah 42. Periksa jumlah baris, jumlah pasien, serta positive_rate pada tabel. Split disimpan dalam split_indices.npz dan split_summary.csv. Jangan mengubah seed berulang kali untuk mencari skor lebih tinggi. Identitas pasien digunakan untuk pembagian, bukan sebagai fitur prediksi.

8. Menganalisis pola nilai hilang — sel 16, bagian 5

Jalankan analisis missingness pada train dan baca missing_patterns.csv. Hasil menguji keterkaitan pola kehilangan dengan usia atau gender. Temuan asosiasi tidak membuktikan mekanisme MAR atau MNAR. Penghapusan fitur dengan kelengkapan rendah pada pipeline penuh merupakan keputusan yang harus dijelaskan beserta kehilangan informasi yang ditimbulkannya.

9. Memeriksa outlier dan redundansi — sel 18, bagian 6

Jalankan sel untuk memperoleh deteksi IQR, Z-score, asosiasi nominal, boxplot, dan scatterplot. Baca outlier_decisions.csv, chi_square.csv, pearson.csv, dan covariance.csv. Nilai ekstrem yang masih masuk akal dipertahankan oleh kode; kandidat outlier tidak otomatis dihapus. Grafik membantu menilai konteks, bukan menjadi bukti tunggal bahwa data salah.

10. Membandingkan tiga strategi dan model — sel 20, bagian 7

Sel ini menjalankan eksperimen utama dan dapat lebih lama daripada audit. Tunggu sampai tabel validation dan test tampil. Tiga strategi adalah penghapusan baris train yang memiliki missing pada kolom terkait, imputasi modus, dan imputasi kategori menggunakan ExtraTreesClassifier. ExtraTrees digunakan sebagai imputer; model prediksi akhirnya tetap Decision Tree. Pada strategi penghapusan baris, validation/test tetap memakai populasi evaluasi yang sama dengan fallback modus dari training.

Model Decision Tree memakai max_depth=6, min_samples_leaf=100, class_weight=balanced, dan threshold 0,5. Strategi dipilih berdasarkan macro-F1 validation, dengan positive-F1 untuk memecahkan nilai seri. Setelah itu model dilatih ulang pada train+validation dan dievaluasi pada test. Simpan imputation_validation.csv, selection.json, dan test_metrics.csv sebagai bukti pemilihan serta evaluasi. Jangan memilih strategi berdasarkan skor test atau menyesuaikan model berulang kali setelah melihat test.

11. Mengekspor data bersih dan memeriksa penggunaan ulang — sel 22, bagian 8

Jalankan ekspor untuk menghasilkan diabetes_clean.parquet, pipeline.joblib, tabel reduksi fitur, dan contoh sampling 25%. Sampling tersebut hanya demonstrasi pengurangan jumlah data, bukan pengurangan data eksperimen utama. Sel ini juga secara khusus membuat eda_train_clean.parquet menggunakan cleaner yang hanya di-fit pada train, serta menyimpan eda_cleaner_train_only.joblib.

MP4 memakai eda_train_clean.parquet, bukan menggantinya dengan diabetes_clean.parquet. Perbedaan ini menjaga EDA menggunakan transformasi dari train saja. Data numerik pada tabel EDA masih menggunakan unit asli; scaling dan encoding berada di pipeline model. Tanda pemeriksaan akhir berhasil adalah “Uji serialisasi dan inferensi tanpa target: LULUS”.

12. Menyimpan keputusan dan melihat confusion matrix — sel 24, bagian 9

Jalankan sel terakhir untuk menyimpan cleaning_decisions.csv dan menampilkan confusion_baseline.png serta confusion_full_pipeline.png. Catat jumlah prediksi benar dan salah per kelas bersama metrik yang sudah tersimpan. Pada akhir sel muncul “Hasil MP2:” diikuti folder output. MP2 selesai apabila seluruh sel kode selesai tanpa traceback dan file ekspornya tersedia.

C. MENJALANKAN MP4: EXPLORATORY DATA ANALYSIS

1. Memastikan hasil MP2 tersedia

Buka notebook MP4. Tidak perlu menjalankan ulang MP2 jika hasil sebelumnya lengkap dan dataset masih sama. Tiga file yang wajib ada di ROOT/mp2 adalah source_manifest.json, split_indices.npz, dan eda_train_clean.parquet. Jangan mencampur file dari beberapa percobaan berbeda. MP4 membaca Parquet dan tidak memuat model joblib, sehingga mengurangi ketergantungan pada serialisasi model lintas versi.

2. Memeriksa lingkungan, Drive, dan modul — sel 3, 4, dan 5

Jalankan pemeriksaan lingkungan seperti pada MP2. Sambungkan Google Drive, periksa DATA_DIR dan ROOT, lalu jalankan pemuatan modul. Urutan sel MP4 berbeda dari MP2: modul berada pada sel 5, sedangkan sel 6 merupakan judul Markdown. Sesi sehat dapat digunakan; bila membuka sesi baru, tetap jalankan seluruh sel persiapan MP4 dari atas.

3. Memverifikasi hasil MP2 — sel 7, bagian 1

Jalankan verifikasi provenance. Kode membandingkan checksum kedua CSV terhadap manifest MP2, memeriksa indeks train, jumlah baris, keberadaan kolom, serta kecocokan urutan/nilai numerik data bersih. Tanda berhasil adalah “Verifikasi hasil MP2: LULUS”, diikuti ukuran dataset, train, dan pasien. Lingkungan MP4 dicatat dalam mp4_environment.json. Jangan menonaktifkan pemeriksaan bila gagal karena tabel bersih dan label dapat menjadi tidak sejajar.

4. Membuat seluruh hasil EDA — sel 9, bagian 2

Jalankan make_eda melalui sel ini. Banyak tabel, gambar, dan ringkasan satu halaman dibuat sekaligus; sel-sel berikutnya terutama menampilkannya. Tunggu sampai selesai meskipun keluaran belum segera muncul. Statistik numerik, modus kategori, dan skewness akan tampil. Hasil hanya menggunakan train, dengan target dari baris train yang bersesuaian.

5. Membaca distribusi univariat — sel 11, bagian 3

Lihat univariate_numeric.png dan univariate_categorical.png. Jelaskan bentuk distribusi, kemencengan, kategori dominan, serta implikasi bagi interpretasi. Lengkapi sel Markdown “TULIS SENDIRI” dengan pengamatan berdasarkan grafik dan angka. Jangan menyatakan distribusi multimodal hanya dari satu pilihan bin histogram.

6. Menjawab tiga pertanyaan analitis — sel 14, bagian 4

Tabel Q1 membandingkan readmisi dini menurut kelompok usia. Tabel Q2 membandingkannya menurut jumlah riwayat rawat inap. Pada kedua tabel, n adalah jumlah kunjungan, positive adalah jumlah kunjungan dengan readmisi <30, dan rate adalah proporsi dalam skala 0–1. Kalikan rate dengan 100 untuk persen. Grafik sudah menampilkan persen. Sebutkan denominator agar kelompok kecil tidak ditafsirkan terlalu kuat.

Tabel Q3 membandingkan lama rawat antara target 0 dan 1. Gunakan mean, median, q25, dan q75; IQR dihitung sebagai q75 dikurangi q25. Kelas 0 berarti bukan label <30, sedangkan kelas 1 berarti <30. Simpan Q1_age_readmission.csv, Q2_prior_readmission.csv, Q3_stay_readmission.csv, dan three_questions.png. Tulis jawaban dengan angka keluaran aktual. Unit analisisnya kunjungan, bukan selalu pasien yang berbeda, dan hubungan ini tidak membuktikan sebab-akibat.

7. Membaca hubungan bivariat dan multivariat — sel 17, bagian 5

Bandingkan heatmap Pearson dan Spearman pada correlations.png. Pearson menggambarkan hubungan linear, sedangkan Spearman hubungan monoton berbasis peringkat. Pairplot menggunakan lima fitur dan maksimum 1.500 baris sampel train agar terbaca. Jangan menyamakan kepadatan titik pada sampel dengan jumlah seluruh dataset. Korelasi tinggi tidak otomatis mengharuskan penghapusan fitur.

8. Menilai dampak pembersihan — sel 20, bagian 6

Baca race_before_after.csv, numeric_descriptive_delta.csv, correlation_delta.csv, serta before_after.png. Perbandingan dilakukan pada baris train yang sama. Selisih numerik atau korelasi yang nol tetap valid karena nilai numerik dan outlier dapat dipertahankan. Perubahan distribusi race dapat muncul karena imputasi. Jelaskan dampak tersebut sesuai output, tanpa memaksakan adanya perubahan.

9. Memeriksa normalitas dan multikolinearitas — sel 23, bagian 7

Baca normality.csv, tabel VIF, dan qqplots.png. Shapiro memakai maksimum 5.000 pasien dengan satu kunjungan per pasien. Interpretasikan p-value bersama Q-Q plot karena sampel besar sensitif terhadap penyimpangan kecil. Data hitungan tidak harus normal dan normalitas bukan persyaratan Decision Tree. VIF dihitung pada fitur numerik, bukan semua dummy kategori. Nilai 5 atau 10 adalah pedoman interpretasi, bukan aturan penghapusan otomatis.

10. Melihat ringkasan satu halaman — sel 26, bagian 8

Jalankan sel terakhir untuk menampilkan ringkasan_EDA_1_halaman.png. Versi PDF juga tersimpan sebagai ringkasan_EDA_1_halaman.pdf. Ringkasan ini menyediakan bukti numerik dan visual; jawaban akhir serta maksimal lima insight tetap perlu ditulis mahasiswa berdasarkan hasilnya. MP4 selesai ketika seluruh sel kode berhasil dan file ringkasan dapat dibuka.

D. MENYIMPAN DAN MENGGUNAKAN LUARAN

Hasil MP2 berada di MyDrive/MP2_MP4_Diabetes/mp2, sedangkan hasil MP4 berada di MyDrive/MP2_MP4_Diabetes/mp4. File CSV dapat dipakai untuk menyusun tabel laporan, PNG untuk gambar, dan PDF ringkasan untuk presentasi atau lampiran. Simpan juga notebook beserta output selnya agar alur eksperimen dapat ditelusuri.

Menjalankan ulang notebook dengan ROOT yang sama dapat menimpa file hasil bernama sama. Apabila ingin mempertahankan satu percobaan, cadangkan folder hasilnya sebelum menjalankan ulang. Jika menggunakan ROOT baru, lakukan pada kedua notebook dan jalankan MP2 sampai selesai sebelum MP4. Catat perubahan dataset, parameter, serta versi pustaka; jangan menganggap hasil dari konfigurasi berbeda sebagai satu percobaan.

E. PENANGANAN KENDALA YANG SESUAI DENGAN KODE

Jika muncul “Folder dataset tidak ditemukan” atau “Berkas tidak ditemukan”, periksa koneksi Drive, akun yang dipilih, alamat DATA_DIR, serta ejaan kedua nama CSV. Pastikan berkas benar-benar berada pada lokasi yang dituju, bukan hanya tersimpan di komputer lokal.

Jika muncul “Lingkungan Python belum sehat”, simpan pekerjaan, hapus runtime melalui pilihan yang dijelaskan sebelumnya, sambungkan kembali, dan jalankan notebook versi ini dari atas. Jangan mencoba serangkaian upgrade/downgrade paket tanpa memeriksa pesan kesalahan. Jika kegagalan tetap muncul, simpan traceback lengkap, versi pustaka, dan nama sel untuk diagnosis.

Jika muncul NameError seperti ROOT, dp, train, atau q1 belum didefinisikan, biasanya sel sebelumnya belum dijalankan atau runtime sudah terputus. Jalankan kembali sel persiapan dan seluruh sel yang diperlukan secara berurutan. Membuka notebook yang masih menampilkan output lama tidak berarti variabelnya sudah tersedia dalam memori sesi baru.

Jika SyntaxError muncul di sel pemuatan modul, periksa apakah notebook yang dibuka benar versi fix dan apakah string MODULES pernah diedit. Gunakan kembali salinan notebook yang utuh lalu jalankan dari atas. Jangan mengubah sembarang tanda kutip dalam blok kode panjang. Jika tetap gagal, catat baris dan traceback; keberhasilan tidak dapat dijamin hanya dengan mengulang sel.

Jika MP4 menampilkan “Hasil MP2 belum lengkap”, periksa tiga file wajib di ROOT/mp2. MP2 harus selesai hingga ekspor sel 22, dan idealnya sampai sel 24. Jika MP2 sebelumnya terhenti, selesaikan MP2 terlebih dahulu. Mengganti ROOT di MP4 saja tidak membuat hasil MP2 berpindah otomatis.

Jika checksum berbeda, dataset saat ini tidak sama persis dengan dataset yang dipakai saat MP2. Gunakan kembali kedua CSV asli dari percobaan tersebut atau jalankan ulang MP2 dengan dataset baru, kemudian MP4. Jangan mengubah nilai checksum di manifest hanya untuk melewati pemeriksaan.

Jika jumlah baris, kolom, atau urutan numerik tidak cocok, jangan menyortir atau mengedit Parquet secara manual. Gunakan hasil MP2 yang konsisten dari satu percobaan, atau buat ulang dengan menjalankan MP2 sampai selesai.

Jika sel terasa lama, lihat apakah eksekusi masih berjalan. Eksperimen imputasi/model pada MP2 dan pembuatan seluruh EDA pada MP4 memang lebih berat daripada penampilan gambar. Lama proses bergantung pada runtime dan akses Drive; tidak ada batas waktu pasti yang dijanjikan. Jangan menjalankan salinan notebook secara bersamaan ke folder hasil yang sama.

F. PEMERIKSAAN SEBELUM PENGUMPULAN

Pastikan MP2 dan MP4 selesai tanpa traceback, file hasil sesuai percobaan yang sama, dan notebook dengan output telah tersimpan. Lengkapi tanggal unduh bila diketahui, interpretasi grafik, jawaban Q1–Q3, serta alasan keputusan pembersihan. Bedakan hasil validation untuk pemilihan strategi dari hasil test untuk evaluasi. Jangan memakai EDA setelah melihat test untuk menyetel ulang model lalu mengklaim test yang sama sebagai evaluasi independen. Jelaskan bantuan AI sesuai ketentuan tugas dan gunakan angka aktual dari keluaran, bukan angka contoh.


Mini Project 2 (MP2) – Pembersihan Data Terstruktur
Daftar Pustaka
[1] J. Clore, K. Cios, J. DeShazo, dan B. Strack. Diabetes 130-US Hospitals for Years 1999–2008. UCI Machine Learning
Repository, 2014. Dataset, CC BY 4.0. https://doi.org/10.24432/C5230J.
[2] B. Strack, J. P. DeShazo, C. Gennings, J. L. Olmo, S. Ventura, K. J. Cios, dan J. N. Clore. “Impact of HbA1c
Measurement on Hospital Readmission Rates: Analysis of 70,000 Clinical Database Patient Records.” BioMed Research
International, 2014, artikel 781670. https://doi.org/10.1155/2014/781670.
[3] F. Pedregosa dkk. “Scikit-learn: Machine Learning in Python.” Journal of Machine Learning Research, 12, 2825–2830,
2011. https://jmlr.org/papers/v12/pedregosa11a.html.
[4] Scikit-learn developers. “GroupShuffleSplit.” Dokumentasi daring, diakses 26 Oktober 2026. https://scikit-learn.
org/stable/modules/generated/sklearn.model_selection.GroupShuffleSplit.html.
[5] SciPy community. “scipy.stats.shapiro.” Dokumentasi daring, diakses 26 Oktober 2026. https://docs.scipy.org/doc/
scipy/reference/generated/scipy.stats.shapiro.html.
[6] NIST/SEMATECH. e-Handbook of Statistical Methods, Bab 1: Exploratory Data Analysis. Diakses 26 Oktober 2026.
https://www.itl.nist.gov/div898/handbook/eda/eda.htm.


Mini Project 4 (MP4) – Exploratory Data Analysis
Daftar Pustaka
[1] J. Clore, K. Cios, J. DeShazo, dan B. Strack. Diabetes 130-US Hospitals for Years 1999–2008. UCI Machine Learning
Repository, 2014. Dataset, CC BY 4.0. https://doi.org/10.24432/C5230J.
[2] B. Strack, J. P. DeShazo, C. Gennings, J. L. Olmo, S. Ventura, K. J. Cios, dan J. N. Clore. “Impact of HbA1c
Measurement on Hospital Readmission Rates: Analysis of 70,000 Clinical Database Patient Records.” BioMed Research
International, 2014, artikel 781670. https://doi.org/10.1155/2014/781670.
[3] F. Pedregosa dkk. “Scikit-learn: Machine Learning in Python.” Journal of Machine Learning Research, 12, 2825–2830,
2011. https://jmlr.org/papers/v12/pedregosa11a.html.
[4] Scikit-learn developers. “GroupShuffleSplit.” Dokumentasi daring, diakses 26 Oktober 2026. https://scikit-learn.
org/stable/modules/generated/sklearn.model_selection.GroupShuffleSplit.html.
[5] SciPy community. “scipy.stats.shapiro.” Dokumentasi daring, diakses 26 Oktober 2026. https://docs.scipy.org/doc/
scipy/reference/generated/scipy.stats.shapiro.html.
[6] NIST/SEMATECH. e-Handbook of Statistical Methods, Bab 1: Exploratory Data Analysis. Diakses 26 Oktober 2026.
https://www.itl.nist.gov/div898/handbook/eda/eda.htm.
