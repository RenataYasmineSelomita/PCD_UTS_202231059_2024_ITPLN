# Penjelaskan Tahapan Cara Menyelesaikan Proyek Secara Rinci
## a. Mengimpor library yang diperlukan
   - `import cv2`: OpenCV (Open Source Computer Vision Library) adalah library open-source yang ditulis dalam bahasa pemrograman C++ dan C. Library ini berfokus pada pengolahan citra, visi komputer, dan pembelajaran mesin. OpenCV menyediakan banyak fungsi dan algoritma yang dapat digunakan untuk berbagai tugas pengolahan citra, seperti membaca, menulis, memanipulasi, dan menampilkan citra.

   - `import numpy as np`: NumPy (Numerical Python) adalah library Python yang menyediakan objek array multidimensi dan berbagai fungsi untuk pengolahan array. Dalam konteks pengolahan citra, NumPy digunakan untuk merepresentasikan dan memanipulasi data citra yang berbentuk array multidimensi.

   - `import matplotlib.pyplot as plt`: Matplotlib adalah library plotting Python yang digunakan untuk memvisualisasikan data dalam bentuk grafik, plot, dan gambar. Dalam proyek ini, Matplotlib digunakan untuk menampilkan citra dan histogram citra.

## b. Membaca citra
   - `img = cv2.imread('Citra_Renata.png')`: Fungsi `cv2.imread()` dari OpenCV digunakan untuk membaca file citra dengan nama "Citra_Renata.png". Fungsi ini mengembalikan nilai array NumPy yang merepresentasikan citra yang dibaca. Array ini memiliki tiga dimensi: tinggi, lebar, dan kanal warna (BGR).

## c. Mengonversi citra dari format BGR ke RGB
   - `img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)`: OpenCV membaca citra dalam format warna BGR (Blue-Green-Red) secara default. Namun, matplotlib menggunakan format warna RGB (Red-Green-Blue). Oleh karena itu, diperlukan konversi format warna dari BGR ke RGB agar citra dapat ditampilkan dengan benar menggunakan matplotlib. Fungsi `cv2.cvtColor()` digunakan untuk melakukan konversi format warna ini.

## d. Meningkatkan kontras citra
   - `alpha = 1.9`: Nilai `alpha` digunakan sebagai faktor pengali untuk meningkatkan kontras citra. Semakin besar nilai `alpha`, semakin tinggi kontras yang dihasilkan.

   - Terdapat perulangan bersarang yang mengalikan setiap piksel citra dengan nilai `alpha`. Perulangan ini dilakukan untuk setiap baris dan kolom pada citra:
     ```python
     for x in range(baris):
         for y in range(kolom):
             gmx = img[x,y] * alpha
             citra_kontras[x,y] = gmx
     ```
     Pada setiap iterasi perulangan, nilai intensitas piksel pada posisi `(x, y)` dikalikan dengan `alpha`, dan hasilnya disimpan dalam array `citra_kontras` pada posisi yang sama.

   - `citra_kontras = citra_kontras.astype(np.uint8)`: Setelah perkalian dengan `alpha`, nilai intensitas piksel pada `citra_kontras` dapat melebihi rentang nilai 0-255 (untuk citra 8-bit). Oleh karena itu, dilakukan konversi tipe data menjadi `uint8` (unsigned 8-bit integer) untuk memastikan nilai intensitas piksel berada dalam rentang yang valid untuk ditampilkan sebagai citra.

## e. Menampilkan citra dan histogram
   - `fig, axs = plt.subplots(2,2,figsize=(15,5))`: Fungsi `subplots()` dari matplotlib digunakan untuk membuat subplot dengan 2 baris dan 2 kolom. `figsize=(15,5)` menentukan ukuran gambar yang akan ditampilkan.

   - `axs[0,0].imshow(img)`: Fungsi `imshow()` dari matplotlib digunakan untuk menampilkan citra `img` pada subplot pertama (baris 0, kolom 0).

   - `axs[0,1].hist(img.ravel(), 256, [0,256])`: Fungsi `hist()` dari matplotlib digunakan untuk menampilkan histogram citra `img`. Parameter `img.ravel()` mengubah array citra multidimensi menjadi array 1D. Parameter `256` menentukan jumlah bin histogram, dan `[0,256]` menentukan rentang nilai intensitas piksel yang akan digunakan dalam histogram (0-255 untuk citra 8-bit).

   - `axs[1,0].imshow(citra_kontras)`: Menampilkan citra kontras pada subplot ketiga (baris 1, kolom 0).

   - `axs[1,1].hist(citra_kontras.ravel(), 256, [0,256])`: Menampilkan histogram citra kontras pada subplot keempat (baris 1, kolom 1).

   - `fig.suptitle("Deteksi Warna Citra Kontras")`: Menambahkan judul utama pada gambar yang ditampilkan.

   - `plt.show()`: Menampilkan gambar yang telah dibuat menggunakan matplotlib.

## f. Memisahkan kanal warna dan menampilkan citra serta histogram
   - Untuk setiap kanal warna (merah, hijau, dan biru), langkah-langkah berikut dilakukan:
     1. `channel_red = img[:,:,0]`: Memisahkan kanal warna merah dari citra asli menggunakan slicing pada array NumPy. Indeks `[:,:,0]` mengambil semua baris dan kolom pada kanal pertama (indeks 0 untuk merah).

     2. `citra_merah = cv2.merge([channel_red, channel_red, channel_red])`: Menggabungkan kembali kanal warna merah menjadi citra grayscale menggunakan fungsi `cv2.merge()`. Karena citra grayscale hanya memiliki satu kanal, maka kanal warna merah digandakan tiga kali.

     3. Menampilkan citra asli, citra grayscale untuk kanal warna, dan histogram masing-masing menggunakan matplotlib seperti pada langkah sebelumnya.

## g. Menggabungkan citra hasil
   - Setelah memisahkan dan menampilkan setiap kanal warna secara terpisah, kode menggabungkan citra kontras, citra merah, citra hijau, dan citra biru ke dalam satu tampilan untuk mempermudah analisis.

## h. Melakukan operasi membagi warna sesuai denga tingkat warnanya
   - Melakukan perulangan untuk setiap piksel pada citra dengan koordinat (x, y).
   - Menginisialisasi variabel max1 dan max2 dengan nilai 0.
   - Melakukan perulangan untuk setiap kanal warna (merah, hijau, biru) pada piksel (x, y).

      - Jika max1 bernilai 0, maka nilai piksel pada kanal warna tersebut (img[x, y, z]) akan diassign ke max1, dan indeks kanal warna tersebut (z) akan diassign ke imax1.
      - Jika nilai piksel pada kanal warna tersebut (img[x, y, z]) lebih besar dari max1, maka nilai max1 akan diassign ke max2, nilai imax1 akan diassign ke imax2, kemudian nilai piksel pada kanal warna tersebut (img[x, y, z]) akan diassign ke max1, dan indeks kanal warna tersebut (z) akan diassign ke imax1.
      - Jika nilai piksel pada kanal warna tersebut (img[x, y, z]) lebih kecil dari max1 tetapi lebih besar dari max2, maka nilai piksel pada kanal warna tersebut (img[x, y, z]) akan diassign ke max2, dan indeks kanal warna tersebut (z) akan diassign ke imax2.
   - Setelah perulangan untuk setiap kanal warna selesai, dilakukan pengecekan apakah selisih antara max1 (nilai maksimum pertama) dan max2 (nilai maksimum kedua) pada piksel (x, y) lebih besar dari 50.
      - Jika selisih lebih besar dari 50, maka dilakukan perulangan untuk setiap kanal warna pada piksel (x, y).

      - Jika indeks kanal warna (z) tidak sama dengan imax1 (indeks kanal warna dengan nilai maksimum pertama), maka nilai piksel pada kanal warna tersebut (img[x, y, z]) akan diset menjadi 0 (hitam).

## i. Mengonversi citra ke grayscale
   - `imgGray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)`: Mengonversi citra asli ke grayscale menggunakan fungsi `cv2.cvtColor()` dengan parameter `cv2.COLOR_BGR2GRAY`. Citra grayscale diperlukan untuk persiapan pencarian nilai ambang batas.

## j. Mencari nilai ambang batas
   - Kode menggunakan fungsi `cv2.threshold()` untuk mencari nilai ambang batas yang sesuai untuk menghasilkan citra biner dengan berbagai kombinasi warna.

   - Fungsi `cv2.threshold()` memiliki parameter-parameter berikut:
     - `img`: Citra input (dalam format grayscale)
     - `thresh`: Nilai ambang batas intensitas
     - `maxval`: Nilai maksimum yang akan diberikan pada piksel yang melebihi `thresh`
     - `type`: Jenis thresholding

# Teori yang Mendukung Proyek 

## 1. Pengolahan Citra Digital   
Pengolahan citra digital (digital image processing) adalah bidang yang mempelajari teknik-teknik untuk memanipulasi dan menganalisis citra digital menggunakan komputer. Citra digital merupakan representasi numerik (dalam bentuk array atau matriks) dari suatu gambar atau objek dua dimensi. Tujuan utama pengolahan citra digital adalah untuk memperbaiki kualitas citra, mengekstraksi informasi dari citra, atau menyiapkan citra untuk proses selanjutnya seperti pengenalan pola atau analisis citra. Beberapa operasi dasar dalam pengolahan citra digital antara lain konversi format warna, peningkatan kontras, pemfilteran, segmentasi, dan deteksi tepi.

## 2. Kontras Citra
Kontras citra mengacu pada perbedaan intensitas antara bagian-bagian terang dan gelap pada citra. Citra dengan kontras yang baik memiliki perbedaan intensitas yang jelas antara objek dan latar belakang, sehingga detail objek terlihat lebih jelas. Peningkatan kontras citra dapat dilakukan dengan menggunakan operasi seperti pembobotan (weighting), pemetaan histogram (histogram equalization), atau transformasi intensitas (intensity transformation). Dalam proyek ini, peningkatan kontras dilakukan dengan mengalikan setiap piksel citra dengan faktor pengali (alpha) tertentu. Semakin besar nilai alpha, semakin tinggi kontras yang dihasilkan.

## 3. Ruang Warna RGB dan BGR
Ruang warna RGB (Red-Green-Blue) dan BGR (Blue-Green-Red) adalah dua model warna yang umum digunakan dalam pengolahan citra digital. Dalam model RGB, setiap piksel citra diwakili oleh tiga nilai intensitas, yaitu intensitas merah (R), hijau (G), dan biru (B). Model BGR mirip dengan RGB, namun urutan kanal warnanya berbeda, yaitu biru (B), hijau (G), dan merah (R). OpenCV menggunakan model warna BGR secara default, sedangkan matplotlib menggunakan model RGB. Oleh karena itu, diperlukan konversi antara dua model warna tersebut untuk menampilkan citra dengan benar.

## 4. Histogram Citra   
Histogram citra adalah representasi grafis dari distribusi intensitas piksel pada citra. Histogram memberikan informasi tentang kecerahan, kontras, dan distribusi warna pada citra. Sumbu horizontal pada histogram menunjukkan nilai intensitas piksel (misalnya 0-255 untuk citra 8-bit), sedangkan sumbu vertikal menunjukkan jumlah piksel yang memiliki nilai intensitas tersebut. Analisis histogram dapat membantu dalam mengevaluasi kualitas citra, mengidentifikasi masalah seperti over-exposure atau under-exposure, dan memandu proses pengolahan citra seperti pemetaan histogram (histogram equalization) atau pemfilteran.

## 5. Pemisahan Kanal Warna   
Citra berwarna terdiri dari tiga kanal warna (merah, hijau, dan biru) yang digabungkan. Dengan memisahkan setiap kanal warna, kita dapat menganalisis dan memproses masing-masing kanal secara terpisah. Pemisahan kanal warna dapat dilakukan dengan menggunakan slicing pada array yang merepresentasikan citra. Setelah memproses masing-masing kanal warna, kanal-kanal tersebut dapat digabungkan kembali menjadi citra berwarna menggunakan operasi penggabungan (merge).

## 6. Thresholding (Pencarian Nilai Ambang Batas)   
Thresholding adalah proses mengonversi citra grayscale menjadi citra biner (hitam dan putih) dengan menentukan nilai ambang batas tertentu. Piksel dengan intensitas di atas nilai ambang batas akan diubah menjadi putih (1), sedangkan piksel dengan intensitas di bawah nilai ambang batas akan diubah menjadi hitam (0). Thresholding sering digunakan dalam proses segmentasi citra, di mana objek atau area tertentu dipisahkan dari latar belakang berdasarkan nilai ambang batas. Pemilihan nilai ambang batas yang tepat sangat penting dalam proses thresholding untuk mendapatkan hasil yang baik.

## 7. Representasi Citra Digital   
Citra digital merupakan representasi numerik dari suatu gambar atau objek dua dimensi. Citra digital terdiri dari array atau matriks elemen-elemen gambar yang disebut piksel (picture elements). Setiap piksel memiliki nilai intensitas yang merepresentasikan kecerahan atau warna pada titik tersebut. Nilai intensitas piksel biasanya diwakili dalam rentang tertentu, misalnya 0-255 untuk citra 8-bit atau 0-65535 untuk citra 16-bit. Operasi pengolahan citra digital dilakukan dengan memanipulasi nilai-nilai intensitas piksel dalam array atau matriks yang merepresentasikan citra tersebut.

# Analisis Hasil Proyek

Saya akan menganalisis apa yang terjadi pada citra dan histogram serta memberikan penjelasan terperinci mengenai nilai ambang batas yang digunakan beserta alasannya.

## A. Analisis Deteksi Citra dan Histogram

### 1. Citra Kontras
   - Pada citra kontras, histogram menunjukkan bahwa intensitas piksel terdistribusi dengan baik di rentang nilai yang lebih tinggi, yaitu dari sekitar 100 hingga 190.
   - Ini terjadi karena setiap piksel pada citra asli dikalikan dengan faktor pengali (`alpha = 1.9`), yang menyebabkan nilai intensitas piksel menjadi lebih besar.
   - Peningkatan kontras ini membuat perbedaan antara daerah terang dan gelap pada citra menjadi lebih jelas, sehingga detail-detail pada citra menjadi lebih mudah dilihat.

### 2. Deteksi Warna Merah
   - Pada deteksi warna merah, histogram menunjukkan bahwa sebagian besar intensitas piksel terpusat pada rentang nilai yang rendah, yaitu dari 0 hingga sekitar 25.
   - Ini terjadi karena kanal warna merah pada citra asli memiliki intensitas yang relatif rendah pada sebagian besar piksel.
   - Hal ini mengindikasikan bahwa citra tidak memiliki banyak warna merah, atau warna merah yang ada memiliki intensitas yang lemah.

### 3. Deteksi Warna Hijau
   - Pada deteksi warna hijau, histogram menunjukkan bahwa intensitas piksel terdistribusi di rentang nilai yang lebih tinggi, yaitu sekitar 190 hingga 225.
   - Ini terjadi karena kanal warna hijau pada citra asli memiliki intensitas yang cukup kuat pada sebagian besar piksel.
   - Hal ini mengindikasikan bahwa citra mengandung warna hijau dengan intensitas yang cukup tinggi.

### 4. Deteksi Warna Biru
   - Pada deteksi warna biru, histogram menunjukkan bahwa intensitas piksel terdistribusi di rentang nilai yang sangat tinggi, yaitu sekitar 200 hingga 250.
   - Ini terjadi karena kanal warna biru pada citra asli memiliki intensitas yang sangat kuat pada sebagian besar piksel.
   - Hal ini mengindikasikan bahwa citra mengandung warna biru dengan intensitas yang sangat tinggi.

## B. Analisis Nilai Ambang Batas dan Alasannya

### 1. None: Nilai ambang batas 0-0
   - Dengan menggunakan nilai ambang batas 0-0, kode menghasilkan citra biner yang menampilkan piksel dengan intensitas warna biru pada rentang nilai 0-0 dari citra asli.
   - Piksel dengan intensitas di atas 0 akan dikonversi menjadi putih (1), sedangkan piksel dengan intensitas di bawah atau sama dengan 0 akan dikonversi menjadi hitam (0).
   - Pada citra hasil, hanya daerah dengan warna biru dengan intensitas yang sangat rendah (sekitar 0) yang akan terlihat, sedangkan daerah lainnya akan terlihat hitam.

### 2. Blue: Nilai ambang batas 15-256
   - Dengan menggunakan nilai ambang batas 15-256, kode menghasilkan citra biner yang menampilkan piksel dengan intensitas warna biru pada rentang nilai 15 hingga 256 dari citra asli.
   - Piksel dengan intensitas di atas 15 dan kurang dari atau sama dengan 256 akan dikonversi menjadi putih (1), sedangkan piksel dengan intensitas di bawah 15 atau di atas 256 akan dikonversi menjadi hitam (0).
   - Pada citra hasil, daerah dengan warna biru dengan intensitas yang cukup tinggi (rentang 15-256) akan terlihat, sedangkan daerah lainnya akan terlihat hitam.

### 3. Red-Blue: Nilai ambang batas 55-256
   - Dengan menggunakan nilai ambang batas 55-256, kode menghasilkan citra biner yang menampilkan piksel dengan intensitas warna merah dan biru pada rentang nilai 55 hingga 256 dari citra asli.
   - Piksel dengan intensitas di atas 55 dan kurang dari atau sama dengan 256 (baik pada kanal merah, biru, atau keduanya) akan dikonversi menjadi putih (1), sedangkan piksel dengan intensitas di bawah 55 atau di atas 256 akan dikonversi menjadi hitam (0).
   - Pada citra hasil, daerah dengan warna merah dan biru dengan intensitas yang cukup tinggi (rentang 55-256) akan terlihat, sedangkan daerah lainnya akan terlihat hitam.

### 4. Red-Blue-Green: Nilai ambang batas 150-256
   - Dengan menggunakan nilai ambang batas 150-256, kode menghasilkan citra biner yang menampilkan piksel dengan intensitas warna merah, biru, atau hijau pada rentang nilai 150 hingga 256 dari citra asli.
   - Piksel dengan intensitas di atas 150 dan kurang dari atau sama dengan 256 (pada salah satu atau lebih kanal warna) akan dikonversi menjadi putih (1), sedangkan piksel dengan intensitas di bawah 150 atau di atas 256 akan dikonversi menjadi hitam (0).
   - Pada citra hasil, daerah dengan warna merah, biru, atau hijau dengan intensitas yang cukup tinggi (rentang 150-256) akan terlihat, sedangkan daerah lainnya akan terlihat hitam.

Alasan pemilihan nilai ambang batas tersebut adalah untuk memisahkan dan mengidentifikasi daerah-daerah pada citra yang memiliki intensitas warna tertentu dalam rentang nilai yang diinginkan. Dengan menggunakan nilai ambang batas yang berbeda, kita dapat mengamati dan menganalisis bagaimana setiap rentang intensitas warna mempengaruhi penampilan citra biner.

Pemilihan nilai ambang batas yang tepat sangat penting dalam proses thresholding untuk mendapatkan hasil yang baik. Dalam kasus ini, nilai ambang batas dipilih berdasarkan rentang intensitas warna yang terdeteksi pada histogram citra asli, sehingga kita dapat memisahkan dan mengidentifikasi daerah-daerah dengan warna dan intensitas yang spesifik.

# Jurnal 
[Jurnal.pdf]
