# Sistem-Rekomendasi-dengan-jenis-Content-Based-Filtering-

## Project Overview

Perusahaan *e-commerce*  bernama superstore, membutuhkan sistem rekomendasi produk untuk *platform* *e-commerce* mereka. Kondisi *platform* masih baru dan penggunannya masih sedikit, sehingga untuk mengatasi kondisi *cold start* tersebut sistem rekomendasi yang sesuai yaitu *content-based filtering*. Sistem rekomendasi yang dibuat akan merokemendasikan produk sepatu berdasarkan kategori atau subkategorinya.

Weihong (2006) melakukan penelitian tentang *E-Commerce Recommender System* (*Content-Based Filtering*) menggunakan metode *Support Vector Machine* (SVM) menghasil nilai *precision* sebesar 61.4525% [[1]](https://link.springer.com/article/10.1007/BF02829216). Sejalan dengan hal tersebut, solusi yang ditawarkan pada kasus ini adalah sistem rekomendasi (*content based filtering*) dengan menggunakan metode *Cosine Similarity* dan *Euclidean Similarity* dengan metrik yang digunakan metrik *precision*.

## Business Understanding
### Problem Statements

Bagaimana cara membuat sistem rekomendasi dengan metode terbaik untuk rekomendasi produk superstore berdasarkan kategori atau subkategorinya?

### Goals

Mengetahui cara membuat sistem rekomendasi dengan metode terbaik untuk rekomendasi produk superstore berdasarkan kategori atau subkategorinya.

### Solution Statements

Menawarkan solusi sistem rekomendasi dengan jenis *content based filtering*  yang menghasilkan rekomendasi produk superstore berdasarkan kategori atau subkategorinya. Untuk mendapatkan solusi terbaik, akan digunakan dua model (metode) yang berbeda yaitu *Cosine Similarity* dan *Euclidean Similarity*. Selain itu, untuk mengukur kinerja model akan digunakan metrik *Precision* dan lama waktu komputasi setiap modelnya.

## Data Understandings

Tabel 1. Informasi Dataset

| | Keterangan |
|---|---|
| Sumber | [Kaggle - Superstore Dataset](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final) |
| Jumlah Data | 9994 |
| *Usability* | 10.00 |
| Lisensi | *For educational purposes only* |
| *Rating* | *silver* |
| Jenis dan Ukuran Berkas | csv (563 kB) |

### Deskripsi Variabel

Berdasarkan informasi dari sumber dataset, variabel-variabel pada dataset adalah sebagai berikut:

- Row ID => ID untuk setiap baris pada dataset
- Order ID => ID untuk untuk setiap pesanan produk.
- Order Date => Tanggal pemesanan produk.
- Ship Date => Tanggal pengiriman produk.
- Ship Mode=> Mode pengiriman produk yang dipilih pemesan.
- Customer ID => ID Unik untuk mengidentifikasi setiap pelanggan.
- Customer Name => Nama pelanggan.
- Segment => Segmentasi tempat tinggal pelanggan.
- Country => Negara tempat tinggal pelanggan.
- City => Kota tempat tinggal pelanggan.
- State => Provinsi tempat tinggal pelanggan.
- Postal Code => Kode Pos setiap pelanggan.
- Region => Wilayah tempat tinggal pelanggan.
- Product ID => ID unik untuk mengidentifikasi setiap produk.
- Category => Kategori produk yang dipesan.
- Sub-Category => Sub-kategori produk yang dipesan.
- Product Name => Nama produk.
- Sales => Penjualan produk.
- Quantity => Kuantitas produk.
- Discount => Diskon yang diberikan.
- Profit => Untung atau rugi yang terjadi.

Berikut contoh lima data teratas pada dataset:

Tabel 2. Tampilan Lima Teratas pada Dataset

|index|Row ID|Order ID|Order Date|Ship Date|Ship Mode|Customer ID|Customer Name|Segment|Country|City|State|Postal Code|Region|Product ID|Category|Sub-Category|Product Name|Sales|Quantity|Discount|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|0|1|CA-2016-152156|11/8/2016|11/11/2016|Second Class|CG-12520|Claire Gute|Consumer|United States|Henderson|Kentucky|42420|South|FUR-BO-10001798|Furniture|Bookcases|Bush Somerset Collection Bookcase|261\.96|2|0\.0|
|1|2|CA-2016-152156|11/8/2016|11/11/2016|Second Class|CG-12520|Claire Gute|Consumer|United States|Henderson|Kentucky|42420|South|FUR-CH-10000454|Furniture|Chairs|Hon Deluxe Fabric Upholstered Stacking Chairs, Rounded Back|731\.94|3|0\.0|
|2|3|CA-2016-138688|6/12/2016|6/16/2016|Second Class|DV-13045|Darrin Van Huff|Corporate|United States|Los Angeles|California|90036|West|OFF-LA-10000240|Office Supplies|Labels|Self-Adhesive Address Labels for Typewriters by Universal|14\.62|2|0\.0|
|3|4|US-2015-108966|10/11/2015|10/18/2015|Standard Class|SO-20335|Sean O'Donnell|Consumer|United States|Fort Lauderdale|Florida|33311|South|FUR-TA-10000577|Furniture|Tables|Bretford CR4500 Series Slim Rectangular Table|957\.5775|5|0\.45|
|4|5|US-2015-108966|10/11/2015|10/18/2015|Standard Class|SO-20335|Sean O'Donnell|Consumer|United States|Fort Lauderdale|Florida|33311|South|OFF-ST-10000760|Office Supplies|Storage|Eldon Fold 'N Roll Cart System|22\.368|2|0\.2|

### Menentukan Fitur yang Akan Digunakan

Fitur yang akan digunakan adalah `Product Name`, `Product ID` dan `Sub-Category`. Pada kasus ini kita lebih mengutamakan penggunaan `Sub-Category` daripada `Category` karena memiliki nilai yang lebih variatif yang dapat dilihat pada Tabel 3.

Tabel 3. Jumlah Banyak *Data Point*

| Fitur | Jumlah |
|---|:---:|
| Produk Superstore | 1850 |
| Kategori | 3 |
| Sub Kategori | 17 |

Tabel 4. Tampilan Lima Teratas pada Dataset setelah Pemilihan Fitur

|index|Product ID|Product Name|Sub-Category|
|---|---|---|---|
|0|FUR-BO-10001798|Bush Somerset Collection Bookcase|Bookcases|
|1|FUR-CH-10000454|Hon Deluxe Fabric Upholstered Stacking Chairs, Rounded Back|Chairs|
|2|OFF-LA-10000240|Self-Adhesive Address Labels for Typewriters by Universal|Labels|
|3|FUR-TA-10000577|Bretford CR4500 Series Slim Rectangular Table|Tables|
|4|OFF-ST-10000760|Eldon Fold 'N Roll Cart System|Storage|

## Data Preparation
### Menangani Missing Value

Untuk mendeteksi *missing value* digunakan fungsi isnull().sum() dan untuk mendeteksi nilai NAN digunakan isna().sum().

Tabel 5. Hasil Deteksi *Missing Value*

| Fitur | Jumlah *Missing Value* |
|---|:---:|
| Product ID    |  0 |
| Product Name  |  0 |
| Sub-Category  |  0 |

Tabel 6. Hasil Deteksi Nilai NAN

| Fitur | Jumlah Nilai NAN |
|---|:---:|
| Product ID    |  0 |
| Product Name  |  0 |
| Sub-Category  |  0 |

Dari Tabel 5. dan Tabel 6. terlihat bahwa setiap fitur tidak memiliki Missing Value (NULL) maupun Nilai NAN sehingga dapat dilanjutkan ke tahapan selanjutnya yaitu menghilangkan data duplikat.

### Menghilangkan data Duplikat

Pada tahap ini, akan dilakukan penghapusan data yang duplikat berdasarkan nama produk. Tujuannya adalah agar sistem rekomendasi yang dibuat dapat menghasilkan rekomendasi yang berbeda tanpa ada nama produk yang muncul lebih dari satu kali. Berikut perbandingan banyak data sebelum dan sesudah penghapusan data duplikat.

Tabel 7. Perbandingan Banyak Data Sebelum dan Sesudah Penghapusan Data Duplikat

| Banyak Data (*Before*) | Banyak Data (*After*) |
|:---:|:---:|
| 9994 | 1850 |

## Modelling

Pada tahap ini, untuk sistem rekomendasi yang dibuat akan menggunakan model dengan metode Cosine Similarity dan Euclidean Similarity. Namun sebelum itu akan dilakukan perubahan tipe data dari kategorikal menjadi data numerik menggunakan metode `TF-IDF Vectorizer`.

### TF-IDF Vectorizer

Pada tahap ini akan dilakukan ekstraksi fitur sekaligus menyeleksi fitur mana saja yang sering muncul dan jarang muncul. Berikut daftar fitur hasil `TF-IDF Vectorizer`.

Tabel 8. Daftar Fitur Hasil `TF-IDF Vectorizer`

| Nama Fitur |
|---|
| 'accessories' |
| 'appliances' |
| 'art' |
| 'binders' |
| 'bookcases' |
| 'chairs' |
| 'copiers' |
| 'envelopes' |
| 'fasteners' |
| 'furnishings' |
| 'labels' |
| 'machines' |
| 'paper' |
| 'phones' |
| 'storage' |
| 'supplies' |
| 'tables' |

Kemudian, dilakukan *one-hot-encoding* pada setiap produk terhadap fitur-fitur yang dihasilkan oleh `TF-IDF Vectorizer`. Berikut matrik TF-IDF yang dihasilkan.

Tabel 9. Matrik TF-IDF dengan 10 Sampel pada Kolom dan 5 Sampel pada Baris

|Product Name|binders|furnishings|storage|tables|accessories|machines|supplies|paper|envelopes|labels|
|---|---|---|---|---|---|---|---|---|---|---|
|Xerox 192|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|1\.0|0\.0|0\.0|
|Deflect-o RollaMat Studded, Beveled Mat for Medium Pile Carpeting|0\.0|1\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|
|PowerGen Dual USB Car Charger|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|
|Samsung Galaxy Note 3|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|0\.0|
|Anker Ultra-Slim Mini Bluetooth 3\.0 Wireless Keyboard|0\.0|0\.0|0\.0|0\.0|1\.0|0\.0|0\.0|0\.0|0\.0|0\.0|

Pada Tabel 9. di atas, matrik TF-IDF memiliki ukuran 1850 x 1850 dimana 1850 menyatakan banyak produk superstore yang berbeda.

### Cosine Similarity

Rumus *Cosine Similarity* didefinisikan sebagai berikut [[2]](http://www.snet.tu-berlin.de/fileadmin/fg220/courses/SS11/snet-project/recommender-systems_asanov.pdf):
$$\text{S}_{\text{Cosinus}}(\vec{x},\vec{y})=\cos(\theta)=\frac{\vec{x}\cdot\vec{y}}{\|\vec{x}\|\|\vec{y}\|}$$
Dengan:
* $\theta$ merupakan sudut antara vektor $\vec{x}$ dan $\vec{y}$.
* $\|\vec{x}\|$ dan $\|\vec{y}\|$ berturut-turut adalah besar atau panjang vektor $\vec{x}$ dan $\vec{y}$. 

Catatan: 
$$\|\vec{x}\|=\sqrt{\sum_{i=1}^n {x_{i}^2}}$$
dimana $x_{i}$ merupakan elemen vektor $\vec{x}$ posisi ke $i$.

Kelebihan metode ini adalah tidak bergantung pada besarnya vektor. Contoh vektor $\vec{x}=[2, 0, 4]$ dengan vektor $\vec{y}=[1, 0, 2]$ yang memiliki arah yang sama namun berbeda besarannya (akibat berbeda nilai pada fiturnya). Jika vektor $\vec{x}$ dan $\vec{y}$ dihitung tingkat kemiripan atau relevansi dengan metode ini maka nilainya $1$ (kemiripan penuh). Namun kelebihan ini dapat menjadi kekurangan jika pada kasus tertentu, makna frekuensi kemunculan fitur menjadi penting. Sedangkan pada kasus ini, *Cosine Similarity* aman digunakan karena sudah dilakukan tahap *one-hot-encoding* pada matrik tfidf. Sehingga frekuensi tiap kategori pada produk mempunyai bobot yang sama yaitu 0 (tidak ada) atau 1 (ada).

Untuk implementasinya menggunakan fungsi `cosine_similarity()` dari *library* sklearn dengan lama waktu komputasinya sebagai berikut.
```
Execution Time Cosine Similarity (Seconds) : 0.04910159111022949
```

Dengan hasil matrik *Cosine Similarity* nya sebagai berikut.

Tabel 10. Matrik *Cosine Similarity* dengan 5 Sampel pada Kolom dan 10 Sampel pada Baris

|Product Name|Ideal Clamps|SAFCO Mobile Desk Side File, Wire Frame|Acco Perma 3000 Stacking Storage Drawers|Micro Innovations USB RF Wireless Keyboard with Mouse|Polycom CX300 Desktop Phone USB VoIP phone|
|---|---|---|---|---|---|
|Eureka Hand Vacuum, Bagless|0\.0|0\.0|0\.0|0\.0|0\.0|
|Kingston Digital DataTraveler 32GB USB 2\.0|0\.0|0\.0|0\.0|1\.0|0\.0|
|Xerox 1933|0\.0|0\.0|0\.0|0\.0|0\.0|
|DXL Angle-View Binders with Locking Rings by Samsill|0\.0|0\.0|0\.0|0\.0|0\.0|
|Brother MFC-9340CDW LED All-In-One Printer, Copier Scanner|0\.0|0\.0|0\.0|0\.0|0\.0|
|Hunt BOSTON Model 1606 High-Volume Electric Pencil Sharpener, Beige|0\.0|0\.0|0\.0|0\.0|0\.0|
|Logitech Mobile Speakerphone P710e - speaker phone|0\.0|0\.0|0\.0|0\.0|1\.0|
|Smead Alpha-Z Color-Coded Second Alphabetical Labels and Starter Set|0\.0|0\.0|0\.0|0\.0|0\.0|
|Eldon ClusterMat Chair Mat with Cordless Antistatic Protection|0\.0|0\.0|0\.0|0\.0|0\.0|
|Southworth 25% Cotton Granite Paper & Envelopes|0\.0|0\.0|0\.0|0\.0|0\.0|

### Euclidean Similarity

*Euclidean* Distances didefinisikan sebagai berikut:

$$ \text{D}_{\text{Euclidean}}(\vec{x},\vec{y})=\sqrt{\sum^{n}_{i=1}(x_{i}-y_{i})^2} $$

Dengan $x_{i}$ dan $y_{i}$ berturut-turut adalah elemen ke $i$ pada vektor $\vec{x}$ dan $\vec{y}$.

Dalam buku berjudul `Programming Collective Intelligence` karya Tobby Segaran (2007) mendefinisikan persamaan *Euclidean Similarity* sebagai berikut [[3]](https://www.oreilly.com/library/view/programming-collective-intelligence/9780596529321/):

$$ \text{S}_{\text{Euclidean}}(\vec{x},\vec{y})=\frac{1}{1+\text{D}_{\text{Euclidean}}(\vec{x}, \vec{y})} $$

Kelebihan Euclidean adalah dapat memperoleh nilai perbedaan antara dua vektor yang sama arahnya namun beda besarannya. Sedangkan kekurangan algoritma ini adalah fitur dengan frekuensi kemunculan paling banyak akan mendominasi fitur lain dalam hasil komputasi jarak euclideannya.

Untuk implementasinya menggunakan fungsi `euclidean_distances()` dari *library* sklearn dengan lama waktu komputasinya sebagai berikut.

```
Execution Time Euclidean Similarity (Seconds) : 0.07311177253723145
```

Dengan hasil matrik *Euclidean Similarity* nya sebagai berikut.

Tabel 11. Matrik *Euclidean Similarity* dengan 5 Sampel pada Kolom dan 10 Sampel pada Baris

|Product Name|Computer Printout Index Tabs|Eldon Imàge Series Desk Accessories, Clear|Kensington Orbit Wireless Mobile Trackball for PC and Mac|Barricks 18&quot; x 48&quot; Non-Folding Utility Table with Bottom Storage Shelf|Hewlett-Packard Desktjet 6988DT Refurbished Printer|
|---|---|---|---|---|---|
|Office Impressions End Table, 20-1/2"H x 24"W x 20"D|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|1\.0|0\.4142135623730951|
|Eureka Hand Vacuum, Bagless|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|
|Tensor Brushed Steel Torchiere Floor Lamp|0\.4142135623730951|1\.0|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|
|Fellowes Command Center 5-outlet power strip|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|
|Cardinal HOLDit\! Binder Insert Strips,Extra Strips|1\.0|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|
|Xerox 1933|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|
|Avery Hi-Liter Pen Style Six-Color Fluorescent Set|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|
|Avery 05222 Permanent Self-Adhesive File Folder Labels for Typewriters, on Rolls, White, 250/Roll|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|
|RCA H5401RE1 DECT 6\.0 4-Line Cordless Handset With Caller ID/Call Waiting|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|
|Xerox 1880|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|0\.4142135623730951|

### Mendapatkan Rekomendasi

Pada tahap ini akan dilakukan pengujian hasil top-10 rekomendasi produk superstore. Kemudian untuk sampel yang dipilih adalah sebagai berikut.

Tabel 12. Sampel Produk Superstore yang Digunakan 

|index|Product ID|Product Name|Sub-Category|
|---|---|---|---|
|1680|FUR-FU-10000758|DAX Natural Wood-Tone Poster Frame|Furnishings|

#### Rekomendasi dengan Cosine Similarity

Tabel 13. Top-10 Rekomendasi dengan Cosine Similarity

|index|Product Name|Product ID|Sub-Category|
|---|---|---|---|
|0|Rubbermaid ClusterMat Chairmats, Mat Size- 66" x 60", Lip 20" x 11" -90 Degree Angle|FUR-FU-10002298|Furnishings|
|1|Dana Swing-Arm Lamps|FUR-FU-10001546|Furnishings|
|2|Howard Miller 13-1/2" Diameter Rosebrook Wall Clock|FUR-FU-10003553|Furnishings|
|3|GE 4 Foot Flourescent Tube, 40 Watt|FUR-FU-10000409|Furnishings|
|4|Eldon Expressions Punched Metal & Wood Desk Accessories, Black & Cherry|FUR-FU-10003832|Furnishings|
|5|Eldon 200 Class Desk Accessories, Smoke|FUR-FU-10000771|Furnishings|
|6|Luxo Professional Combination Clamp-On Lamps|FUR-FU-10004188|Furnishings|
|7|Master Caster Door Stop, Large Neon Orange|FUR-FU-10002456|Furnishings|
|8|Deflect-o RollaMat Studded, Beveled Mat for Medium Pile Carpeting|FUR-FU-10003601|Furnishings|
|9|Document Clip Frames|FUR-FU-10002508|Furnishings|

Pada Tabel 13. di atas, terlihat bahwa untuk 10 hasil rekomendasi dengan *Cosine Similarity* mempunyai kategori yang sama dengan sampel yaitu `Furnishings` sehingga top-10 rekomendasi di atas relevan dengan sampel.

#### Rekomendasi dengan Euclidean Similarity

Tabel 14. Top-10 Rekomendasi dengan Euclidean Similarity

|index|Product Name|Product ID|Sub-Category|
|---|---|---|---|
|0|Rubbermaid ClusterMat Chairmats, Mat Size- 66" x 60", Lip 20" x 11" -90 Degree Angle|FUR-FU-10002298|Furnishings|
|1|Dana Swing-Arm Lamps|FUR-FU-10001546|Furnishings|
|2|Howard Miller 13-1/2" Diameter Rosebrook Wall Clock|FUR-FU-10003553|Furnishings|
|3|GE 4 Foot Flourescent Tube, 40 Watt|FUR-FU-10000409|Furnishings|
|4|Eldon Expressions Punched Metal & Wood Desk Accessories, Black & Cherry|FUR-FU-10003832|Furnishings|
|5|Eldon 200 Class Desk Accessories, Smoke|FUR-FU-10000771|Furnishings|
|6|Luxo Professional Combination Clamp-On Lamps|FUR-FU-10004188|Furnishings|
|7|Master Caster Door Stop, Large Neon Orange|FUR-FU-10002456|Furnishings|
|8|Deflect-o RollaMat Studded, Beveled Mat for Medium Pile Carpeting|FUR-FU-10003601|Furnishings|
|9|Document Clip Frames|FUR-FU-10002508|Furnishings|

Pada Tabel 14. di atas, terlihat bahwa untuk 10 hasil rekomendasi dengan *Euclidean Similarity* mempunyai kategori yang sama dengan sampel yaitu `Furnishings` sehingga top-10 rekomendasi di atas relevan dengan sampel.

## Evaluasi

$$\text{Recommender system precision (P)} = \frac{\text{\#of our recommendation that relevant}}{\text{\#of item we recommend}}\times 100% $$

Dari hasil rekomendasi di atas, dapat diketahui bahwa `DAX Natural Wood-Tone Poster Frame` termasuk ke dalam kategori `Furnishings`. Dari 10 produk yang direkomendasikan, berikut nilai _precision_ pada model _cosine similarity_ dan _euclidean distance_.

Tabel 15. Komparasi Metrik *Precision*

|Model | Sesuai | Tidak Sesuai |Total| Precision |
|---|---|---|---|---|
|_Cosine Similarity_|10|0|10|100%|
|_Euclidean Similarity_|10|0|10|100%|
 
Pada tabel di atas, terlihat bahwa model *Cosine Similiarity* dan *Euclidean Distance* memiliki nilai _precision_ yang sama pada top-10 rekomendasi di atas.

Selain dari nilai _precision_, lama komputasi setiap metode juga perlu dipertimbangkan. Berikut perbandingannya:

Tabel 16. Komparasi Waktu Komputasi

|index|Cosine Similarity|Euclidean Similarity|
|---|---|---|
|Time \(Seconds\)|0\.04910159111022949|0\.07311177253723145|

Berdasarkan output di atas, waktu komputasi pada metode Cosine Similarity (0.04910 detik) lebih cepat dibandingkan Euclidean Similarity (0.07311 detik).

Jadi dapat disimpulkan bahwa model terbaik untuk sistem rekomendasi produk superstore adalah model dengan metode *Cosine Similarity*.

## Daftar Referensi
[1] Weihong, H., Yi, C. An E-commerce recommender system based on content-based filtering. Wuhan Univ. J. Nat. Sci. 11, 1091–1096 (2006). https://doi.org/10.1007/BF02829216. Tersedia: [tautan](https://link.springer.com/article/10.1007/BF02829216).  
[2] Melville, Prem, and Vikas Sindhwani. "Recommender systems." Encyclopedia of machine learning 1 (2010): 829-838. Tersedia: [tautan](http://www.snet.tu-berlin.de/fileadmin/fg220/courses/SS11/snet-project/recommender-systems_asanov.pdf).
[3] Segaran, Toby. "Programming Collective Intelligence". O'Reilly Media, Inc. 2007. Tersedia: [O'Reilly Media](https://www.oreilly.com/library/view/programming-collective-intelligence/9780596529321/).
