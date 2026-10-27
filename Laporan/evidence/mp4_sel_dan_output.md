## Sel 1 (markdown; execution_count=None)
# MP4 — Exploratory Data Analysis: Readmisi Dini Diabetes
Jalankan **MP2 terlebih dahulu** pada ROOT yang sama. Notebook ini memeriksa checksum data agar indeks split tidak digunakan pada file berbeda. Analisis memakai train dan cleaner fit train saja. Jangan memakai hasil EDA ini untuk menyesuaikan model setelah melihat test; bila mengubah model, siapkan evaluasi independen baru.

Pertanyaan: **Q1** proporsi menurut usia; **Q2** hubungan riwayat rawat inap; **Q3** perbedaan distribusi lama rawat. Grafik dan tabel dihasilkan otomatis. Jawaban/interpretasi akhir wajib mahasiswa tulis sendiri.


## Sel 2 (markdown; execution_count=None)
## Sebelum menjalankan
Gunakan hasil MP2 yang sudah berhasil, pada folder `MyDrive/MP2_MP4_Diabetes/mp2`. Tidak perlu menjalankan ulang MP2. Pilih **Run all**. Sesi sehat yang telah menjalankan MP2 boleh digunakan langsung.

Notebook ini memakai pemeriksaan lingkungan yang sama dengan MP2 terbaru, tanpa memaksa penggantian NumPy/SciPy. Jika sesi pernah menjalankan sel instalasi versi lama dan muncul konflik impor, simpan pekerjaan lalu pilih **Runtime → Disconnect and delete runtime**, sambungkan kembali, dan jalankan notebook ini. File Google Drive tetap tersimpan.

Dataset dibaca langsung dari folder ETS/Dataset yang telah ditentukan. Analisis menggunakan `eda_train_clean.parquet` hasil MP2 (cleaner fit train saja); tidak memuat ulang model joblib. Grafik, tabel, dan poster disimpan di `MyDrive/MP2_MP4_Diabetes/mp4`.


## Sel 3 (code; execution_count=1)
Pemeriksaan lingkungan lulus tanpa pemaksaan versi pustaka. Output versi sesuai lingkungan Colab yang digunakan.
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
Lokasi dataset dan folder hasil terdeteksi. Tidak ada pemindaian rekursif seluruh Drive.
Mounted at /content/drive
Dataset: /content/drive/MyDrive/S3 UB/Materi Kuliah/Semester 1/Topik Analisis Data/ETS/Dataset 
Hasil: /content/drive/MyDrive/MP2_MP4_Diabetes


## Sel 5 (code; execution_count=3)
Modul Python berhasil dimuat; pemaknaan EDA dikonfirmasi dari kode fungsi make_eda.
Modul diabetes_pipeline dan diabetes_eda berhasil dimuat.


## Sel 6 (markdown; execution_count=None)
## 1. Muat hasil MP2 dan verifikasi provenance

## Sel 7 (code; execution_count=4)
Verifikasi hasil MP2 lulus. Data asli 101.766 x 50; train bersih 60.878 x 45; 42.910 pasien. Kode memeriksa checksum dua berkas, validitas indeks, jumlah baris, serta kesesuaian numerik.
Verifikasi hasil MP2: LULUS
Dataset asli: (101766, 50) | Train: (60878, 45) | Pasien: 42910


## Sel 8 (markdown; execution_count=None)
## 2. Statistik, visualisasi, pertanyaan analitis, asumsi dan poster
Pairplot dibatasi 5 fitur dan sampel 1.500 untuk keterbacaan. Shapiro memakai maksimum 5.000 pasien (satu kunjungan per pasien) dan Q-Q plot. Data hitungan diskrit tidak diharapkan normal; normalitas bukan syarat Decision Tree. VIF dihitung dengan intercept pada fitur numerik; bukan seluruh dummy kategorik.

## Sel 9 (code; execution_count=5)
Statistik deskriptif, modus, dan skewness tersedia. Rawat jalan/darurat didominasi nol. NaN pada modus A1Cresult/max_glu_serum berasal dari pembacaan ulang literal None di CSV; bukan bukti nilai klinis hilang baru.
           Unnamed: 0    count       mean        std  min   25%   50%   75%  \
0    time_in_hospital  60878.0   4.383899   2.978282  1.0   2.0   4.0   6.0   
1  num_lab_procedures  60878.0  43.102024  19.725136  1.0  31.0  44.0  57.0   
2      num_procedures  60878.0   1.342735   1.704734  0.0   0.0   1.0   2.0   
3     num_medications  60878.0  16.032885   8.145441  1.0  10.0  15.0  20.0   
4   number_outpatient  60878.0   0.373090   1.259535  0.0   0.0   0.0   0.0   
5    number_emergency  60878.0   0.196869   0.971644  0.0   0.0   0.0   0.0   
6    number_inpatient  60878.0   0.628043   1.241462  0.0   0.0   0.0   1.0   
7    number_diagnoses  60878.0   7.423815   1.928891  1.0   6.0   8.0   9.0   

     max  
0   14.0  
1  132.0  
2    6.0  
3   79.0  
4   42.0  
5   76.0  
6   19.0  
7   16.0  
                      column       mode  nunique
0                       race  Caucasian        5
1                     gender     Female        2
2                        age    [70-80)       10
3          admission_type_id          1        8
4   discharge_disposition_id          1       26
5        admission_source_id          7       14
6                     diag_1        428      676
7                     diag_2        276      681
8                     diag_3        250      739
9              max_glu_serum        NaN        4
10                 A1Cresult        NaN        4
11                 metformin         No        4
12               repaglinide         No        4
13               nateglinide         No        4
14            chlorpropamide         No        4
15               glimepiride         No        4
16             acetohexamide         No        2
17                 glipizide         No        4
18                 glyburide         No        4
19               tolbutamide         No        2
20              pioglitazone         No        4
21             rosiglitazone         No        4
22                  acarbose         No        4
23                  miglitol         No        3
24              troglitazone         No        2
25                tolazamide         No        2
26                   examide         No        1
27               citoglipton         No        1
28                   insulin         No        4
29       glyburide-metformin         No        4
30       glipizide-metformin         No        2
31  glimepiride-pioglitazone         No        1
32   metformin-rosiglitazone         No        1
33    metformin-pioglitazone         No        1
34                    change         No        2
35               diabetesMed        Yes        2
36                 stay_band        1-3        3
           Unnamed: 0   skewness
0    time_in_hospital   1.134415
1  num_lab_procedures  -0.235244
2      num_procedures   1.314226
3     num_medications   1.317180
4   number_outpatient   8.650394
5    number_emergency  27.849659
6    number_inpatient   3.486339
7    number_diagnoses  -0.882590

## Sel 10 (markdown; execution_count=None)
## 3. Univariat — bentuk distribusi dan kategori

## Sel 11 (code; execution_count=6)
Histogram dan bar chart menunjukkan skew kanan pada layanan dan dominasi kategori tertentu. Puncak histogram diskrit tidak cukup untuk klaim multimodal.
<IPython.core.display.Image object>
Gambar: ../figures/mp4_cell11_output1.png
<IPython.core.display.Image object>
Gambar: ../figures/mp4_cell11_output2.png

## Sel 12 (markdown; execution_count=None)
**TULIS SENDIRI:** fitur mana skew kanan? Apakah terlihat lebih dari satu puncak? Jangan menyatakan multimodal hanya dari satu bin histogram; coba jumlah bin lain bila perlu. Jelaskan dampak pada scaling, outlier dan interpretasi.

## Sel 13 (markdown; execution_count=None)
## 4. Jawab tiga pertanyaan dengan angka dan visual

## Sel 14 (code; execution_count=7)
Tiga tabel dan gambar menjawab Q1-Q3. Usia tidak monoton; proporsi meningkat menurut rawat inap sebelumnya; mean lama rawat berbeda tetapi median sama.
              n  positive      rate
age                                
[0-10)       93         3  0.032258
[10-20)     413        23  0.055690
[20-30)     981       137  0.139653
[30-40)    2275       258  0.113407
[40-50)    5870       630  0.107325
[50-60)   10295       975  0.094706
[60-70)   13349      1471  0.110196
[70-80)   15594      1839  0.117930
[80-90)   10317      1238  0.119996
[90-100)   1691       188  0.111177
                           n  positive      rate
prior_admission_group                           
0                      40533      3416  0.084277
1                      11682      1498  0.128231
2                       4511       787  0.174462
3+                      4152      1061  0.255539
            n      mean  median       std  q25  q75
target                                             
0       54116  4.335409     4.0  2.972289  2.0  6.0
1        6762  4.771961     4.0  2.997913  3.0  6.0
<IPython.core.display.Image object>
Gambar: ../figures/mp4_cell14_output4.png

## Sel 15 (markdown; execution_count=None)
**TULIS SENDIRI:**
- Q1: sebutkan kelompok, jumlah observasi, dan proporsi. Kelompok kecil lebih tidak stabil.
- Q2: jelaskan pola proporsi antar kelompok riwayat rawat inap; tidak menyatakan sebab-akibat.
- Q3: bandingkan median, IQR dan mean; apakah selisih cukup berarti secara praktis?

Unit tabel adalah kunjungan; kunjungan pasien yang sama berkorelasi. Tidak ada uji p-value independensi palsu untuk menjawab pertanyaan ini. Inferensi lanjutan membutuhkan bootstrap cluster pasien atau model yang memperhitungkan kelompok.


## Sel 16 (markdown; execution_count=None)
## 5. Bivariat dan multivariat

## Sel 17 (code; execution_count=8)
Pearson/Spearman dan pairplot lima fitur tersimpan. Korelasi numerik tidak mendekati satu; kedua kelas tumpang tindih pada sampel 1.500 kunjungan. Pembacaan angka korelasi dibulatkan mengikuti anotasi grafik.
<IPython.core.display.Image object>
Gambar: ../figures/mp4_cell17_output1.png
<IPython.core.display.Image object>
Gambar: ../figures/mp4_cell17_output2.png

## Sel 18 (markdown; execution_count=None)
**TULIS SENDIRI:** bandingkan Pearson dan Spearman. Pilih sesuai bentuk hubungan; ketidaknormalan saja bukan alasan otomatis membuang Pearson. Korelasi tinggi tidak otomatis berarti harus membuang fitur.

## Sel 19 (markdown; execution_count=None)
## 6. Dampak pembersihan terhadap distribusi/korelasi

## Sel 20 (code; execution_count=9)
Race mayoritas meningkat 2,2520 poin persentase akibat modus. Selisih semua statistik numerik dan matriks Pearson nol; sesuai keputusan mempertahankan numerik asli.
              race  before_pct  after_pct
0        Caucasian   74.749499  77.001544
1  AfricanAmerican   18.762114  18.762114
2          Missing    2.252045   0.000000
3         Hispanic    2.033575   2.033575
4            Other    1.550642   1.550642
5            Asian    0.652124   0.652124
           Unnamed: 0  count  mean  std  min  25%  50%  75%  max
0    time_in_hospital    0.0   0.0  0.0  0.0  0.0  0.0  0.0  0.0
1  num_lab_procedures    0.0   0.0  0.0  0.0  0.0  0.0  0.0  0.0
2      num_procedures    0.0   0.0  0.0  0.0  0.0  0.0  0.0  0.0
3     num_medications    0.0   0.0  0.0  0.0  0.0  0.0  0.0  0.0
4   number_outpatient    0.0   0.0  0.0  0.0  0.0  0.0  0.0  0.0
5    number_emergency    0.0   0.0  0.0  0.0  0.0  0.0  0.0  0.0
6    number_inpatient    0.0   0.0  0.0  0.0  0.0  0.0  0.0  0.0
7    number_diagnoses    0.0   0.0  0.0  0.0  0.0  0.0  0.0  0.0
           Unnamed: 0  time_in_hospital  num_lab_procedures  num_procedures  \
0    time_in_hospital               0.0                 0.0             0.0   
1  num_lab_procedures               0.0                 0.0             0.0   
2      num_procedures               0.0                 0.0             0.0   
3     num_medications               0.0                 0.0             0.0   
4   number_outpatient               0.0                 0.0             0.0   
5    number_emergency               0.0                 0.0             0.0   
6    number_inpatient               0.0                 0.0             0.0   
7    number_diagnoses               0.0                 0.0             0.0   

   num_medications  number_outpatient  number_emergency  number_inpatient  \
0              0.0                0.0               0.0               0.0   
1              0.0                0.0               0.0               0.0   
2              0.0                0.0               0.0               0.0   
3              0.0                0.0               0.0               0.0   
4              0.0                0.0               0.0               0.0   
5              0.0                0.0               0.0               0.0   
6              0.0                0.0               0.0               0.0   
7              0.0                0.0               0.0               0.0   

   number_diagnoses  
0               0.0  
1               0.0  
2               0.0  
3               0.0  
4               0.0  
5               0.0  
6               0.0  
7               0.0  
<IPython.core.display.Image object>
Gambar: ../figures/mp4_cell20_output4.png

## Sel 21 (markdown; execution_count=None)
**TULIS SENDIRI:** pada data ini nilai numerik dapat tidak berubah karena outlier dipertahankan. Selisih nol tetap hasil yang valid. Scaling hanya pada matriks model; tabel EDA tetap unit asli. Bandingkan perubahan distribusi race akibat imputasi.

## Sel 22 (markdown; execution_count=None)
## 7. Normalitas dan multikolinearitas

## Sel 23 (code; execution_count=10)
Shapiro pada 5.000 pasien per fitur menolak bentuk normal eksak; Q-Q mendukung penyimpangan ekor. VIF 1,0276-1,5294 hanya berlaku pada delapan fitur numerik.
              feature     n         W             p   skewness
0    time_in_hospital  5000  0.884373  1.100781e-51   1.134415
1  num_lab_procedures  5000  0.983644  1.285070e-23  -0.235244
2      num_procedures  5000  0.787435  9.575811e-63   1.314226
3     num_medications  5000  0.908652  1.063609e-47   1.317180
4   number_outpatient  5000  0.276926  2.500831e-88   8.650394
5    number_emergency  5000  0.187320  5.287398e-91  27.849659
6    number_inpatient  5000  0.329802  1.303061e-86   3.486339
7    number_diagnoses  5000  0.832295  2.620625e-58  -0.882590
              feature       VIF
0    time_in_hospital  1.363158
1  num_lab_procedures  1.147583
2      num_procedures  1.192908
3     num_medications  1.529443
4   number_outpatient  1.027624
5    number_emergency  1.063149
6    number_inpatient  1.086529
7    number_diagnoses  1.109744
<IPython.core.display.Image object>
Gambar: ../figures/mp4_cell23_output3.png

## Sel 24 (markdown; execution_count=None)
**TULIS SENDIRI:** interpretasikan statistik dan Q-Q plot bersama. Sampel besar membuat uji sensitif. VIF >5/10 hanya pedoman; pertimbangkan tujuan dan jenis model. Pemeriksaan ini bukan alasan mengubah model setelah test dibuka.

## Sel 25 (markdown; execution_count=None)
## 8. Ringkasan satu halaman

## Sel 26 (code; execution_count=11)
Poster PNG tersedia dalam notebook; teks output menyebut lokasi PDF. Isi poster masih memuat instruksi untuk menulis interpretasi. Video presentasi tidak tersedia dalam lampiran.
<IPython.core.display.Image object>
Gambar: ../figures/mp4_cell26_output1.png
PDF poster: /content/drive/MyDrive/MP2_MP4_Diabetes/mp4/ringkasan_EDA_1_halaman.pdf


## Sel 27 (markdown; execution_count=None)
## 9. TULIS SENDIRI — maksimal 5 insight dan rekomendasi
Isi setiap baris dengan bukti angka/berkas, interpretasi, dan tindakan:

| No | Bukti angka/grafik | Interpretasi sendiri | Rekomendasi untuk pemodelan berikutnya |
|---|---|---|---|
| 1 | ... | ... | ... |
| 2 | ... | ... | ... |
| 3 | ... | ... | ... |
| 4 | ... | ... | ... |
| 5 | ... | ... | ... |

**Presentasi kelompok 10 menit:** 0-1 tujuan/data; 1-3 audit dan preprocessing; 3-5 eksperimen MP2; 5-8 tiga jawaban EDA; 8-9 batasan; 9-10 kesimpulan dan kontribusi anggota. Rekam video sendiri dan cantumkan tautannya pada README.

**Checklist:** notebook Run all tanpa error, poster 1 halaman, tiga pertanyaan terjawab, label/unit grafik jelas, tidak ada klaim kausal tanpa desain kausal, pernyataan AI, lisensi, peran anggota. Jika diwajibkan laporan MP4, nama `Kelompok<no>_MP4.pdf`; panduan terutama mensyaratkan notebook, ringkasan 1 halaman dan presentasi.

