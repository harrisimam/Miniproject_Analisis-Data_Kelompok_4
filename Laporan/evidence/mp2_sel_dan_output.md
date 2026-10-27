## Sel 1 (markdown; execution_count=None)
# MP2 — Pembersihan Data Terstruktur: Diabetes 130-US Hospitals
**Bahasa:** Python / Google Colab CPU. **Seed:** 42. **Target:** readmitted `<30` vs lainnya.

Notebook ini menjalankan audit, integrasi kamus kode, 3 strategi missing values, outlier, reduksi, transformasi, dan eksperimen model sederhana. Data dibaca langsung dari Drive Bapak. Hasil tersimpan dalam `MyDrive/MP2_MP4_Diabetes/mp2`.

**Batas tugas:** `diabetic_data.csv` + `IDS_mapping.csv` adalah data observasi + referensi dari satu paket. Integrasi ini belum otomatis memenuhi interpretasi dosen tentang *dua sumber independen*. Tidak ada konflik satuan nyata yang direkayasa. Catat/konfirmasikan interpretasi ini sebelum pengumpulan.

**Integritas akademik:** kode dibantu AI. Tabel/grafik dihitung dari data nyata; analisis dan interpretasi akhir wajib ditulis mahasiswa. Isi sel bertanda **TULIS SENDIRI**, nama anggota dan peran; susun laporan MP2 maksimum 6 halaman. Tidak ada klaim skor harus meningkat.

**Cakupan:** semua kunjungan dengan label valid; prediksi pada saat keluar rawat inap. Label `NO` bukan jaminan tidak pernah readmisi di rumah sakit lain. Data historis dan kunjungan berulang membatasi generalisasi. Belum dilakukan eksklusi disposisi meninggal/hospice; bahas implikasinya dan lakukan analisis sensitivitas jika diperlukan, sebelum membuka ulang test.


## Sel 2 (markdown; execution_count=None)
## 0. Mulai dengan runtime Colab bersih
**Jika pernah menjalankan versi lama:** simpan pekerjaan lain, lalu pilih **Runtime → Disconnect and delete runtime** (Putuskan dan hapus runtime). Ini membuang sesi dan file sementara `/content`, bukan berkas Google Drive. Sambungkan kembali, lalu **Run all** notebook ini. Jangan jalankan sel instalasi dari versi lama.

Versi ini tidak memaksa upgrade/downgrade NumPy, pandas, SciPy, scikit-learn, Matplotlib, atau Seaborn. Pustaka bawaan diperiksa dengan operasi kecil sebelum data dibaca. Hanya PyArrow dipasang jika belum tersedia, dengan `--no-deps` agar tidak mengganti pustaka lain.

Dataset langsung dari `/content/drive/MyDrive/S3 UB/Materi Kuliah/Semester 1/Topik Analisis Data/ETS/Dataset`. Lokasi hasil tetap `/content/drive/MyDrive/MP2_MP4_Diabetes`. Versi pustaka aktual dicatat dalam manifest; hasil antarversi dapat sedikit berbeda.


## Sel 3 (code; execution_count=1)
Pemeriksaan lingkungan lulus. Versi aktual Python 3.13.16 dan scikit-learn 1.6.1 dicatat; tidak ada alasan memasang ulang pustaka yang sudah bekerja.
PEMERIKSAAN LINGKUNGAN LULUS
Python: 3.13.16
numpy 2.1.3
pandas 2.2.3
scipy 1.16.3
scikit-learn 1.6.1
matplotlib 3.10.0
seaborn 0.13.2
pyarrow 23.0.1
joblib 1.6.0


## Sel 4 (code; execution_count=2)
Sel komentar tanpa keluaran; koneksi Drive dilakukan di sel berikutnya. Tidak adanya output di sini bukan kegagalan.

## Sel 5 (code; execution_count=3)
Drive berhasil terpasang. Dataset dibaca dari folder ETS/Dataset; hasil disimpan di MP2_MP4_Diabetes.
Mounted at /content/drive
Dataset: /content/drive/MyDrive/S3 UB/Materi Kuliah/Semester 1/Topik Analisis Data/ETS/Dataset 
Hasil: /content/drive/MyDrive/MP2_MP4_Diabetes


## Sel 6 (code; execution_count=4)
Dua modul Python berhasil dimuat. Kode modul diperiksa sebagai dasar menafsirkan transformasi dan asal tabel, bukan sekadar membaca grafik.
Modul diabetes_pipeline dan diabetes_eda berhasil dimuat.


## Sel 7 (markdown; execution_count=None)
## 1. Baca data asli dan catat provenance
Literal `None` dipertahankan untuk kategori hasil pemeriksaan. Sumber: https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008 . Catat lisensi CC BY 4.0 sebagaimana dicantumkan pada UCI, tanggal unduh asli jika diketahui, dan checksum.

## Sel 8 (code; execution_count=5)
101.766 baris, 50 kolom, 71.518 pasien. Target <30 sebanyak 11.357; data tidak seimbang. Metadata tanggal unduh masih berupa isian pada kode.
  label      n
0    NO  54864
1   >30  35545
2   <30  11357
Ukuran: (101766, 50) Pasien unik: 71518


## Sel 9 (markdown; execution_count=None)
## 2. Audit kualitas dan enam dimensi
Tidak semua masalah harus ditemukan. Nol duplikat adalah hasil valid. Batas rentang memeriksa kewajaran, bukan membuktikan akurasi klinis.

## Sel 10 (code; execution_count=6)
Audit delapan kolom missing/invalid, nol duplikat, nol pelanggaran rentang logis, dan 16.773 pasien berulang. Nol invalid logis tidak membuktikan akurasi klinis.
                 column   dtype  unique  missing_n  missing_pct  \
5                weight  object      10      98569    96.858479   
11    medical_specialty  object      73      49949    49.082208   
10           payer_code  object      18      40256    39.557416   
2                  race  object       6       2273     2.233555   
20               diag_3  object     790       1423     1.398306   
19               diag_2  object     749        358     0.351787   
18               diag_1  object     717         21     0.020636   
3                gender  object       3          3     0.002948   
1           patient_nbr  object   71518          0     0.000000   
0          encounter_id  object  101766          0     0.000000   
9      time_in_hospital   int64      14          0     0.000000   
8   admission_source_id   int64      17          0     0.000000   
12   num_lab_procedures   int64     118          0     0.000000   
13       num_procedures   int64       7          0     0.000000   
14      num_medications   int64      75          0     0.000000   

    literal_None_n          quality_dimension  
5                0  kelengkapan / konsistensi  
11               0  kelengkapan / konsistensi  
10               0  kelengkapan / konsistensi  
2                0  kelengkapan / konsistensi  
20               0  kelengkapan / konsistensi  
19               0  kelengkapan / konsistensi  
18               0  kelengkapan / konsistensi  
3                0  kelengkapan / konsistensi  
1                0  kelengkapan / konsistensi  
0                0  kelengkapan / konsistensi  
9                0  kelengkapan / konsistensi  
8                0  kelengkapan / konsistensi  
12               0  kelengkapan / konsistensi  
13               0  kelengkapan / konsistensi  
14               0  kelengkapan / konsistensi  
               column  invalid_n                    rule  \
0    time_in_hospital          0   integer; 1 <= x <= 14   
1  num_lab_procedures          0  integer; 0 <= x <= inf   
2      num_procedures          0  integer; 0 <= x <= inf   
3     num_medications          0  integer; 0 <= x <= inf   
4   number_outpatient          0  integer; 0 <= x <= inf   
5    number_emergency          0  integer; 0 <= x <= inf   
6    number_inpatient          0  integer; 0 <= x <= inf   
7    number_diagnoses          0  integer; 1 <= x <= inf   

                                                note  
0  Batas logis; bukan validasi terhadap rekam med...  
1  Batas logis; bukan validasi terhadap rekam med...  
2  Batas logis; bukan validasi terhadap rekam med...  
3  Batas logis; bukan validasi terhadap rekam med...  
4  Batas logis; bukan validasi terhadap rekam med...  
5  Batas logis; bukan validasi terhadap rekam med...  
6  Batas logis; bukan validasi terhadap rekam med...  
7  Batas logis; bukan validasi terhadap rekam med...  
           dimension               evidence  \
0            akurasi       range_checks.csv   
1        kelengkapan      audit_columns.csv   
2        konsistensi  integration_audit.csv   
3    ketepatan waktu      periode 1999-2008   
4        kepercayaan   source_manifest.json   
5  interpretabilitas        IDS_mapping.csv   

                                          limitation  
0  Kewajaran logis terukur; kebenaran klinis tida...  
1  Penanda ? dan kategori invalid dipisahkan dari...  
2              Validasi kode dan relasi many-to-one.  
3  Data historis; tidak mewakili praktik terkini ...  
4  Sumber UCI, checksum berkas; bukan validasi kl...  
5  Decode kode nominal; age adalah interval usia ...  
{'rows': 101766, 'columns': 50, 'patients': 71518, 'full_duplicates': 0, 'encounter_duplicates': 0, 'repeated_patients': 16773}


## Sel 11 (markdown; execution_count=None)
## 3. Integrasi tabel referensi
Parser memisahkan tiga header di IDS_mapping.csv. Join many-to-one menjaga satu baris per encounter. Kode nominal tidak mempunyai satuan fisik. Data durasi tetap hari, hitungan tetap jumlah; tidak diperlukan konversi unit. Deskripsi dan kode tidak dimasukkan bersamaan sebagai fitur karena redundan.

## Sel 12 (code; execution_count=7)
Tiga join many-to-one berhasil; 8/30/25 baris kamus; seluruh unmapped_n nol dan 101.766 baris tetap. Tabel referensi dari paket yang sama bukan sumber klinis independen.
                        key  mapping_rows  unmapped_n  rows_after  \
0         admission_type_id             8           0      101766   
1  discharge_disposition_id            30           0      101766   
2       admission_source_id            25           0      101766   

                                  unit_conflict  
0  Tidak ada: kode nominal, bukan ukuran fisik.  
1  Tidak ada: kode nominal, bukan ukuran fisik.  
2  Tidak ada: kode nominal, bukan ukuran fisik.  

## Sel 13 (markdown; execution_count=None)
## 4. Pembagian berdasarkan pasien
Sekitar 60/20/20% **pasien**, bukan harus persis proporsi baris. Seed tetap; tidak memilih ulang split berdasarkan skor. Test tidak digunakan untuk memilih imputasi/fitur. File indeks lokal tidak berisi nomor pasien.

## Sel 14 (code; execution_count=8)
Train/validation/test: 60.878/20.400/20.488 kunjungan dan 42.910/14.304/14.304 pasien. Proporsi positif masing-masing 11,1075/10,9216/11,5531 persen.
        split   rows  patients  positive_rate
0       train  60878     42910       0.111075
1  validation  20400     14304       0.109216
2        test  20488     14304       0.115531

## Sel 15 (markdown; execution_count=None)
## 5. Pola nilai hilang
Uji asosiasi missingness terhadap usia/gender memakai train. Asosiasi dapat menentang asumsi hilang sepenuhnya acak; hasil ini **tidak membuktikan MAR atau MNAR**. Uji eksploratif, tanpa klaim kausal; periksa expected count kecil dan banyaknya pengujian. Missing >30% (weight/payer/specialty) dikeluarkan dari pipeline penuh menurut rencana awal; keputusan ini adalah trade-off informasi, bukan aturan universal.

## Sel 16 (code; execution_count=9)
Asosiasi missingness dibaca eksploratif. Banyak sel harapan kecil dan kunjungan berkorelasi. Missing gender versus gender adalah hubungan sirkular, bukan bukti MAR/MNAR.
       missing_column associated_with          chi2              p  \
0                race             age      9.152804   4.232904e-01   
1                race          gender     68.462839   1.359826e-15   
2              gender             age      3.725597   9.285275e-01   
3              gender          gender  60878.000000   0.000000e+00   
4              diag_1             age      2.546663   9.796021e-01   
5              diag_1          gender      1.556274   4.592608e-01   
6              diag_2             age   4032.391414   0.000000e+00   
7              diag_2          gender      3.774460   1.514908e-01   
8              diag_3             age   5299.664308   0.000000e+00   
9              diag_3          gender      4.540125   1.033057e-01   
10             weight             age     26.624064   1.613633e-03   
11             weight          gender      5.723388   5.717182e-02   
12         payer_code             age    587.329068  1.096828e-120   
13         payer_code          gender      2.727137   2.557466e-01   
14  medical_specialty             age    170.813110   4.177639e-32   
15  medical_specialty          gender      5.807112   5.482792e-02   

    missing_pct  expected_lt5_pct  
0      2.252045          5.000000  
1      2.252045         33.333333  
2      0.004928         50.000000  
3      0.004928         66.666667  
4      0.014784         50.000000  
5      0.014784         66.666667  
6      0.340024         15.000000  
7      0.340024         33.333333  
8      1.371596          5.000000  
9      1.371596         33.333333  
10    96.777161          5.000000  
11    96.777161         33.333333  
12    39.667860          0.000000  
13    39.667860         33.333333  
14    48.904366          0.000000  
15    48.904366         33.333333  

## Sel 17 (markdown; execution_count=None)
## 6. Outlier dan redundansi
Deteksi IQR, |Z|>3, boxplot dan scatter. Keputusan default per kolom: **pertahankan** hitungan ekstrem yang masih valid; tidak ada bukti data salah untuk menghapus/cap. PCA tidak wajib karena seleksi fitur dipakai. Pearson dan kovarian untuk numerik; chi-square + Cramer V untuk nominal. Signifikansi saja tidak berarti fitur redundan.

## Sel 18 (code; execution_count=10)
Tabel IQR/Z-score, chi-square, boxplot dan scatter tersimpan. Seluruh kandidat outlier dipertahankan. IQR nol pada rawat jalan/darurat membuat semua nilai positif ditandai. Kovarian dihitung dan ditulis ke CSV, tetapi tabelnya tidak tampil dalam notebook.
               column  iqr_lower  iqr_upper  iqr_outliers  z_abs_gt3  action  \
0    time_in_hospital       -4.0       12.0          1332        612  retain   
1  num_lab_procedures       -8.0       96.0            84         20  retain   
2      num_procedures       -3.0        5.0          2966          0  retain   
3     num_medications       -5.0       35.0          1556        826  retain   
4   number_outpatient        0.0        0.0         10182        852  retain   
5    number_emergency        0.0        0.0          6742        577  retain   
6    number_inpatient       -1.5        2.5          4152       1165  retain   
7    number_diagnoses        1.5       13.5           164        164  retain   

                                              reason  
0  Hitungan layanan/diagnosis dapat valid; ekstre...  
1  Hitungan layanan/diagnosis dapat valid; ekstre...  
2  Hitungan layanan/diagnosis dapat valid; ekstre...  
3  Hitungan layanan/diagnosis dapat valid; ekstre...  
4  Hitungan layanan/diagnosis dapat valid; ekstre...  
5  Hitungan layanan/diagnosis dapat valid; ekstre...  
6  Hitungan layanan/diagnosis dapat valid; ekstre...  
7  Hitungan layanan/diagnosis dapat valid; ekstre...  
                           a                         b          chi2  \
20                    change               diabetesMed  15596.114999   
12         admission_type_id       admission_source_id  68457.161452   
11         admission_type_id  discharge_disposition_id  12839.564824   
16  discharge_disposition_id                    change    628.840552   
8                       race       admission_source_id   2821.194909   
15  discharge_disposition_id       admission_source_id   6349.161240   
17  discharge_disposition_id               diabetesMed    459.963227   
2                     gender  discharge_disposition_id    668.011011   
6                       race         admission_type_id   1251.661306   
13         admission_type_id                    change    248.712866   
0                     gender                      race    375.393927   
18       admission_source_id                    change    168.920352   
7                       race  discharge_disposition_id    790.341832   
14         admission_type_id               diabetesMed    119.516360   
19       admission_source_id               diabetesMed     82.960482   

                p  cramers_v  expected_lt5_pct  
20   0.000000e+00   0.506149          0.000000  
12   0.000000e+00   0.400802         52.678571  
11   0.000000e+00   0.173579         57.692308  
16  1.122754e-116   0.101634         26.923077  
8    0.000000e+00   0.096272         44.047619  
15   0.000000e+00   0.089569         70.329670  
17   1.464455e-81   0.086922         25.000000  
2   5.661218e-109   0.074071         51.282051  
6   2.664845e-240   0.064125         31.250000  
13   5.206306e-50   0.063917         12.500000  
0    1.611264e-74   0.055526         33.333333  
18   3.059489e-29   0.052676         25.000000  
7    3.582046e-97   0.050956         47.435897  
14   9.660723e-23   0.044308         18.750000  
19   3.050077e-12   0.036915         25.000000  
<IPython.core.display.Image object>
Gambar: ../figures/mp2_cell18_output3.png
<IPython.core.display.Image object>
Gambar: ../figures/mp2_cell18_output4.png

## Sel 19 (markdown; execution_count=None)
## 7. Tiga strategi dan model pembanding
**Strategi:** (1) hapus baris train yang missing pada race/gender/diagnosis; (2) modus kategori; (3) ExtraTreesClassifier untuk imputasi kategori menggunakan delapan fitur numerik, tanpa target readmisi. Median adalah fallback numerik bila diperlukan. ExtraTrees hanya imputer; model akhir tetap Decision Tree.

Semua varian dinilai pada validation identik. Untuk strategi hapus baris, missing pada validation/test diisi modus training agar populasi evaluasi sama; ini adalah strategi *drop train + fallback holdout*, bukan complete-case evaluasi.

Model imputer dibatasi 12.000 baris training berlabel kategori per kolom, 30 pohon; ini menjaga biaya komputasi. Imputasi satu nilai memiliki ketidakpastian yang tidak direpresentasikan penuh.

Model akhir: DecisionTree max_depth=6, min_samples_leaf=100, class_weight=balanced; threshold 0,5. Full pipeline: StandardScaler numerik, ordinal usia dan discretization lama rawat, one-hot nominal, VarianceThreshold lalu SelectPercentile 75%. Pemilihan strategi berdasarkan **macro-F1 validation**, tie positive-F1. Setelah itu refit train+validation, evaluasi test sekali. Baseline memakai model/split sama, imputasi minimal Unknown, tanpa scaling/reduksi. Perbandingan mengukur paket preprocessing, bukan efek satu komponen secara kausal.

## Sel 20 (code; execution_count=11)
Tiga strategi dibandingkan pada validation yang sama. Mode dan model seri pada F1; mode dipilih oleh urutan stabil. Pada test, recall naik, accuracy/F1 turun. Waktu pengukuran tunggal tidak diperlakukan sebagai benchmark berulang.
    strategy  train_rows  validation_rows   seconds  accuracy  f1_positive  \
0  drop_rows       58671            20400  5.681148  0.676814     0.261454   
1       mode       60878            20400  5.717197  0.677500     0.261865   
2      model       60878            20400  6.778248  0.677500     0.261865   

   f1_macro  precision_positive  recall_positive  balanced_accuracy   roc_auc  \
0  0.527301            0.174205         0.523788           0.609682  0.658624   
1  0.527771            0.174570         0.523788           0.610067  0.659160   
2  0.527771            0.174570         0.523788           0.610067  0.659094   

   average_precision  
0           0.197768  
1           0.198406  
2           0.198396  
           model  strategy  train_rows  test_rows  accuracy  f1_positive  \
0       baseline  constant       81278      20488  0.624512     0.272942   
1  full_pipeline      mode       81278      20488  0.599717     0.270828   

   f1_macro  precision_positive  recall_positive  balanced_accuracy   roc_auc  \
0  0.509921            0.175797         0.610055           0.618228  0.662488   
1  0.497485            0.171509         0.643431           0.618719  0.662165   

   average_precision  
0           0.209263  
1           0.209170  
Strategi terpilih dari validation: mode


## Sel 21 (markdown; execution_count=None)
## 8. Ekspor data bersih, reduksi dan pipeline
Parquet berisi nilai numerik dalam unit asli untuk EDA; matriks model mempunyai scaling dan encoding dalam pipeline. Sample 25% bertingkat hanya untuk demonstrasi numerosity, **tidak mengurangi data eksperimen utama**. Model tersimpan di joblib; muat hanya artefak tepercaya.

## Sel 22 (code; execution_count=12)
Dimensi 944 ke 941 ke 705; numerosity 60.878 ke 15.219. Pipeline final sudah fit train+validation. Cleaner EDA terpisah fit train saja. Uji serialisasi/inferensi tanpa target lulus.
            stage   rows  columns
0         encoded  60878      944
1     nonconstant  60878      941
2  selected_75pct  60878      705
   before_n  after_n  before_columns  after_columns  before_positive_rate  \
0     60878    15219              48             48              0.111075   

   after_positive_rate  
0             0.111045  
Uji serialisasi dan inferensi tanpa target: LULUS


## Sel 23 (markdown; execution_count=None)
## 9. Tabel keputusan pembersihan dan metrik visual

## Sel 24 (code; execution_count=13)
Delapan baris awal tabel keputusan ditampilkan dan dua confusion matrix tersimpan. Jumlah positif yang benar naik 79 dengan tambahan 587 false positive. Baris keputusan lain diperiksa dari kode, bukan diklaim telah tampil seluruhnya.
                                 masalah              tindakan  \
0             weight: kelengkapan rendah  drop fitur pada full   
1         payer_code: kelengkapan rendah  drop fitur pada full   
2  medical_specialty: kelengkapan rendah  drop fitur pada full   
3                  race: missing/invalid                  mode   
4                gender: missing/invalid                  mode   
5                diag_1: missing/invalid                  mode   
6                diag_2: missing/invalid                  mode   
7                diag_3: missing/invalid                  mode   

                                              alasan  baris_terdampak  \
0  Biaya imputasi dan ketidakpastian; pilihan awa...            98569   
1  Biaya imputasi dan ketidakpastian; pilihan awa...            40256   
2  Biaya imputasi dan ketidakpastian; pilihan awa...            49949   
3    Dipilih memakai macro-F1 validation; bukan test             2273   
4    Dipilih memakai macro-F1 validation; bukan test                3   
5    Dipilih memakai macro-F1 validation; bukan test               21   
6    Dipilih memakai macro-F1 validation; bukan test              358   
7    Dipilih memakai macro-F1 validation; bukan test             1423   

   kolom_terdampak  
0                1  
1                1  
2                1  
3                1  
4                1  
5                1  
6                1  
7                1  
<IPython.core.display.Image object>
Gambar: ../figures/mp2_cell24_output2.png
<IPython.core.display.Image object>
Gambar: ../figures/mp2_cell24_output3.png
Hasil MP2: /content/drive/MyDrive/MP2_MP4_Diabetes/mp2


## Sel 25 (markdown; execution_count=None)
## 10. TULIS SENDIRI — diskusi dan laporan maksimal 6 halaman
1. Mengapa strategi terpilih masuk akal? Apakah selisih validation cukup berarti? Satu split tidak membuktikan superioritas umum.
2. Mengapa tidak menghapus semua outlier? Apa risiko menghapus weight/specialty/payer?
3. Apakah full pipeline meningkatkan atau menurunkan accuracy/F1? Tulis angka dari test_metrics.csv; jangan ubah pipeline lagi berdasarkan test.
4. Apa konsekuensi drop-train dan imputasi pada evaluasi? Bagaimana distribusi race berubah?
5. Bahas data historis, bias kelompok, readmisi yang tidak tercatat, dan pasien berulang.
6. Nyatakan status persetujuan integrasi observasi + kamus referensi. Bila dosen mensyaratkan dua sumber independen, bagian ini belum selesai.

**Struktur laporan 6 halaman:** (1) data/tujuan, (2) audit/integrasi, (3) missing/outlier, (4) transformasi/reduksi, (5) eksperimen dan tabel keputusan, (6) interpretasi/batasan/referensi/pernyataan AI. Nama: `Kelompok<no>_MP2.pdf`.

**Pernyataan penggunaan AI — sesuaikan dengan praktik sebenarnya:** Kode awal dan struktur notebook dibantu ChatGPT. Kelompok memeriksa kode, menjalankan eksperimen, memverifikasi keluaran, dan menulis analisis serta interpretasi akhir sendiri.

