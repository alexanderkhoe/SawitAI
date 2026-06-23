# SawitAI — QGIS Plugin for Palm Tree Detection

## Tutorial Plugin SawitAI: Deteksi Pohon Sawit Menggunakan QGIS: https://youtu.be/fui0e7wTpm4
**SawitAI** adalah plugin QGIS yang dikembangkan untuk membantu proses deteksi pohon kelapa sawit pada citra raster secara lebih praktis dan terintegrasi langsung di lingkungan QGIS. Plugin ini memanfaatkan model deteksi berbasis deep learning dengan format `.pt` untuk melakukan prediksi pada citra raster, kemudian menyimpan hasil deteksi dalam format shapefile (`.shp`) sehingga dapat digunakan kembali untuk analisis spasial, pemetaan, dan validasi lapangan.

Repository ini berisi plugin **SawitAI / Palm Tree Detection** yang dapat dijalankan di QGIS untuk memproses data citra raster berdasarkan area batas wilayah tertentu. Pengguna dapat memasukkan file raster (`.tif`), shapefile area of interest (`.shp`), model deteksi (`.pt`), serta menentukan lokasi output shapefile hasil deteksi. Plugin juga menyediakan pengaturan parameter seperti **tile size**, **overlap ratio**, dan **threshold** untuk menyesuaikan proses deteksi sesuai karakteristik data citra yang digunakan.

## Fitur Utama

* Integrasi langsung dengan QGIS.
* Mendukung input raster dalam format `.tif`.
* Mendukung input area batas wilayah dalam format shapefile `.shp`.
* Menggunakan model deteksi deep learning dalam format `.pt`.
* Menghasilkan output deteksi dalam format shapefile `.shp`.
* Menyediakan parameter tiling untuk memproses raster berukuran besar.
* Mendukung pengaturan confidence threshold untuk menyaring hasil deteksi.
* Dilengkapi panduan instalasi, penggunaan, pembuatan AOI, dan penanganan error umum.

## Alur Kerja Plugin

Secara umum, alur kerja SawitAI terdiri dari beberapa tahap. Pertama, pengguna memasukkan raster dan shapefile ke dalam QGIS. Kedua, pengguna memilih raster, shapefile AOI, dan model deteksi pada antarmuka plugin. Ketiga, pengguna menentukan lokasi penyimpanan output shapefile. Setelah itu, pengguna mengatur parameter seperti ukuran tile, overlap antar tile, dan threshold deteksi. Terakhir, pengguna menekan tombol **Run** untuk menjalankan proses deteksi.

Hasil akhir dari proses ini adalah shapefile yang berisi hasil deteksi objek, sehingga data dapat divisualisasikan, dianalisis, atau diekspor lebih lanjut melalui QGIS.

## Input dan Output

Plugin ini menggunakan beberapa input utama:

1. **Input Raster (.tif)**
   Citra raster yang akan diproses untuk mendeteksi pohon kelapa sawit.

2. **Input Shapefile (.shp)**
   Area batas wilayah atau Area of Interest (AOI) yang menentukan bagian raster yang ingin diproses.

3. **Input Model (.pt)**
   Model deteksi yang telah dilatih sebelumnya dan digunakan untuk melakukan prediksi pada citra raster.

4. **Output Shapefile (.shp)**
   File hasil deteksi yang disimpan dalam format shapefile.

5. **Tile Size**
   Ukuran potongan raster dalam satuan piksel. Parameter ini berguna untuk membagi raster besar menjadi bagian-bagian kecil sebelum dilakukan deteksi.

6. **Overlap Ratio**
   Nilai tumpang tindih antar tile untuk mengurangi kemungkinan objek terpotong pada batas tile.

7. **Threshold**
   Nilai ambang confidence untuk menyaring hasil deteksi. Hanya objek dengan confidence di atas threshold yang disimpan sebagai hasil deteksi.

## Instalasi Singkat

Untuk menggunakan plugin SawitAI, pengguna perlu menyalin folder plugin ke direktori plugin QGIS. Direktori ini dapat ditemukan melalui menu:

**Settings > User Profile > Open Active Profile Folder**

Setelah folder profil terbuka, masuk ke direktori:

`python > plugins`

Kemudian ekstrak folder plugin ke dalam folder tersebut. Setelah itu, restart QGIS dan aktifkan plugin melalui menu:

**Plugins > Manage and Install Plugins**

Centang plugin **Palm Tree Detection** untuk mengaktifkannya.

## Library yang Dibutuhkan

Sebelum plugin digunakan, beberapa library perlu diinstal melalui OSGeo4W Shell, yaitu:

```bash
pip install rasterio==1.3.10
pip install geopandas==0.14.0
pip install sahi==0.11.18
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

Library tersebut digunakan untuk membaca raster, mengolah data geospasial, menjalankan proses tiling dan deteksi, serta memuat model deep learning berbasis PyTorch.

## Cara Menggunakan Plugin

1. Buka QGIS.
2. Import raster dan shapefile ke dalam project QGIS.
3. Buka plugin **Palm Tree Detection / SawitAI**.
4. Pilih layer raster pada bagian **Input Raster**.
5. Pilih shapefile AOI pada bagian **Input Shapefile**.
6. Pastikan shapefile dan raster menggunakan sistem koordinat atau CRS yang sama.
7. Pilih file model deteksi dengan format `.pt`.
8. Tentukan lokasi output shapefile.
9. Sesuaikan parameter **Tile Size**, **Overlap Ratio**, dan **Threshold**.
10. Klik **Run** untuk menjalankan proses deteksi.

## Pembuatan Area of Interest (AOI)

Jika belum memiliki shapefile AOI, pengguna dapat membuatnya langsung di QGIS dengan membuat layer shapefile baru bertipe polygon. Setelah itu, pengguna dapat menggambar polygon pada area yang ingin dianalisis, menyimpan shapefile, lalu menggunakannya sebagai input area batas pada plugin.

AOI penting digunakan agar proses deteksi hanya dilakukan pada wilayah yang relevan, sehingga pemrosesan menjadi lebih efisien dan hasil deteksi lebih terfokus.

## Troubleshooting

### Error: Input shape do not overlap raster

Error ini biasanya terjadi karena shapefile dan raster memiliki sistem koordinat yang berbeda atau area shapefile tidak berada pada cakupan raster. Solusinya adalah memeriksa Coordinate Reference System (CRS) pada raster dan shapefile melalui menu **Properties > Information**. Jika CRS berbeda, buat ulang shapefile AOI dengan CRS yang sama seperti raster.

### Error: expected input 3 channels, but got 4 channels

Error ini terjadi ketika raster memiliki lebih dari 3 channel, misalnya RGB ditambah alpha channel. Model deteksi membutuhkan input 3 channel, yaitu Red, Green, dan Blue. Untuk memperbaikinya, pengguna dapat menggunakan menu:

**Processing Toolbox > GDAL > Raster Conversion > Rearrange Bands**

Kemudian pilih hanya band RGB, yaitu Red, Green, dan Blue.

## Tujuan Repository

Repository ini dibuat sebagai dokumentasi dan implementasi plugin QGIS untuk mendukung deteksi pohon kelapa sawit berbasis citra raster dan model deep learning. Dengan adanya plugin ini, pengguna dapat melakukan proses deteksi secara lebih mudah tanpa harus menjalankan kode secara manual di luar QGIS.

SawitAI diharapkan dapat membantu kegiatan pemetaan, monitoring perkebunan, analisis tutupan lahan, dan pengembangan sistem geospasial berbasis kecerdasan buatan.
