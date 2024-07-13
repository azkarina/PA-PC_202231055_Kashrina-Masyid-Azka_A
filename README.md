# PA-PC_202231055_Kashrina-Masyid-Azka_A

Deteksi Daun

## Apa itu Segmentasi Citra
Segmentasi citra adalah proses membagi citra digital menjadi beberapa bagian yang lebih bermakna dan lebih mudah dianalisis. Segmentasi digunakan untuk mengisolasi objek atau area tertentu dalam citra. Dalam konteks kode yang diberikan, segmentasi dilakukan berdasarkan warna.

1. Segmentasi berdasarkan warna
Ini adalah teknik di mana bagian-bagian citra yang memiliki warna tertentu dipisahkan dari bagian lainnya. Pada kode tersebut, rentang warna untuk mangga dan daun didefinisikan menggunakan nilai minimum dan maksimum pada ruang warna HSV. Fungsi cv2.inRange digunakan untuk membuat masker biner yang menandai piksel dalam rentang warna yang ditentukan.

2. Thresholding
Thresholding adalah teknik untuk mengubah citra grayscale  atau citra berwarana menjadi citra biner. Dalam kasus ini, cv2.inRange digunakan untuk membuat citra biner (masker) di mana piksel yang berada dalam rentang warna yang ditentukan diatur ke nilai 255 (putih), dan piksel lainnya diatur ke nilai 0 (hitam).

3. Penerapan Masker
Masker yang dihasilkan digunakan untuk membuat citra yang disegmentasi. Dalam hal ini, citra asli disalin dan hanya piksel yang sesuai dengan masker yang dipertahankan, sementara piksel lainnya diatur ke hitam.

### Apa itu HSV?
HSV adalah representasi warna yang lebih mendekati persepsi manusia dibandingkan dengan ruang warna RGB. Dalam HSV, fitur warna dapat diekstraksi dengan lebih mudah karena nilai Hue (H) mewakili jenis warna, Saturation (S) mewakili intensitas atau kejenuhan warna, dan Value (V) mewakili kecerahan.
* **Hue(H) - Warna**
    - Hue mewakili jenis warna dan dinyatakan sebagai sudut dalam derajat pada roda warna (color wheel).
    - Hue memberikan informasi tentang panjang gelombang dominan warna
    - Sudut 0° dan 360° adalah merah, 120° adalah hijau, dan 240° adalah biru.

* **Saturation (S) - Kejenuhan:**
    - Saturation menunjukkan seberapa murni atau intens warna tersebut.
    - Nilai saturasi berkisar dari 0 hingga 1.
    - Saturation 0 berarti warna tersebut adalah abu-abu (tidak ada kejenuhan), sedangkan saturasi 1 berarti warna tersebut sangat jenuh (murni).

* **Value (V) - Kecerahan:**

    - Value menunjukkan seberapa terang atau gelap warna tersebut.
    - Nilai value berkisar dari 0 hingga 1.
    - Value 0 berarti warna tersebut sepenuhnya hitam (tidak ada kecerahan), sedangkan value 1 berarti warna tersebut sepenuhnya terang.

Keuntungan dari penggunaan HSV :
* Lebih Mendekati Persepsi Manusia:
Ruang warna HSV lebih mendekati cara mata manusia dan otak kita memandang warna. Ini membuatnya lebih intuitif untuk tugas-tugas seperti penyesuaian warna dan pengenalan warna.

* Kegunaan dalam Pengolahan Citra:
HSV sering digunakan dalam pengolahan citra untuk tugas-tugas seperti deteksi objek, segmentasi warna, dan penyesuaian warna karena komponen Hue memberikan informasi warna yang lebih stabil di bawah variasi pencahayaan.


## Tahapan Program
### 1. Import library

library yang digunakan adalah cv2, numpy, dan matplotlib.

    import cv2
    import numpy as np
    import matplotlib.pyplot as plt
    
>* **cv2** digunakan untuk memproses gambar
>* **numpy** digunakan untuk melakukan operasi matematika, mengubah bentuk array, dan menghitung statistik
>* **matplotlib** digunakan untuk memvisualisasi data 

### 2. Import gambar
Dengan menggunakan fungsi imread yang ada di library cv2 kita bisa memanggil gambar dari device.

### 3. Konversi BGR ke HSV
```python
  hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
```
Kita akan mengubah gambar yang sudah diimport barusan menjadi format HSV dengan menggunakan fungsi cvtColor. cv2 menyimpan citra dengan format BGR. Itulah alasan mengapa kita mengubah gambarnya dari BGR ke HSV

### 4. Mendefinisikan Rentang Warna untuk Segmentasi
```python

  lower_orange = np.array([10, 100, 100], dtype="uint8")
  upper_orange = np.array([25, 255, 255], dtype="uint8")
  lower_leaves = np.array([35, 43, 46], dtype="uint8")
  upper_leaves = np.array([77, 255, 255], dtype="uint8")
```
Karena kita akan mendeteksi 2 buah objek, yaitu jeruk dan daun. Kedua objek ini memiliki 2 warna yang berbeda. Karena itu kita perlu memberikan nilai batas atas dan bawah warna untuk buah jeruk dan juga daun. 

Penjelasan dari kode:
>* **np.array** membuat array yang diambil dari library numpy
>* **nilai dalam kurung siku** adalah tiga nilai yang masing masing mewakili komponen dalam ruang warna HSV (Hue, Saturation,Value). Nilai ini dimaksudkan sebagai batas warna dari citra.
>* **dtype="uint8"**  adalah jenis data untuk array numpy yang sudah dibuat.

### 5. Membuat Mask Biner
Dengan menggunakan fungsi inRange yang ada di di cv2 kita akan mambuat mask. Fungsi inRange digunakan untuk melakukan thresholding dalam ruang warna tertentu.
```python
    mask_orange = cv2.inRange(hsv, lower_orange, upper_orange)
    mask_leaves = cv2.inRange(hsv, lower_leaves, upper_leaves)
```
Fungsi inRange memiliki 3 parameter utama:
>* **src** ini merupakan citra sumber dalam runag warna yang sesuai. Dalam hal ini adalah **hsv**
>* **loverb** adalah singkatan dari lower boundary yang dimaknai sebagai batas bawah dari rentang warna dalam bentuk array numpy. Dalam hal ini adalah **lower_leaves** dan **lower_orange**
>* **upperb** adalah singkatan dari upper boundary yang dimaknai sebagai batas atas dari rentang warna dalam bentuk array numpy. Dalam hal ini adalah **upper_orange** dan **upper_leaves**

* cv2.inRange : Bagian ini dimaksudkan untuk memeriksa setiap piksel dalam gambar hsv dan jika nilai HSV dari piksel tersebut berada dalam rentang yang ditentukan oleh **lower_orange** dan **upper_orange**, maka piksel tersebut diatur menjadi putih (255) pada **mask_orange**. Jika tidak, piksel diatur menjadi hitam (0). Begitu juga untuk yang daun.
* mask_orange menunjukkan area dimana warna jeruk terdeteksi
* mask_leaves menunjukkan area dimana warna daun terdeteksi

### 6. Menyalin citra asli

Dengan menggunakan fungsi copy kita akan membuat salinan citra asli untuk masing masing objek.

### 7. Menggunakan Mask untuk Segmentasi
```python
    segmented_orange[mask_orange == 0] = (0, 0, 0)
```
Pada citra segmented_orange, piksel yang tidak termasuk dalam rentang warna jeruk diatur menjadi hitam.
```python
    segmented_leaves[mask_leaves == 0] = (0, 0, 0)
```
Pada citra segmented_leaves, piksel yang tidak termasuk dalam rentang warna jeruk daun menjadi hitam.

### 8. Menampilkan gambar
Dengan menggunakan library matplotlib kita akan menampilkan beberapa gambar sekaligus dalam satu plot yang sama
```python
    fig, axs = plt.subplots(2, 2, figsize=(10, 10))
```
Fungsi ini akan membuat sebuah figur dengan 4 subplot (2 baris dan 2 kolom). Dimana setiap gambar ini memiliki ukuran 10 inci x 10 inci yang diatur dengan figsize.
```python
    ax = axs.ravel()
```
Mmengubah array 2D axs yang berisi 4 subplot menjadi array 1D "ax" sehingga lebih mudah diakses dengan indeks.

#### a. Menampilkan gambar asli

```python
    ax[0].imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
```
- Menampilkan gambar asli dengan menggunakan ax[0] sebagai subplot pertama.
- cv2.cvtColor(img, cv2.COLOR_BGR2RGB) digunakan untuk mengonversi gambar dari ruang warna BGR (yang digunakan oleh OpenCV) ke RGB (yang digunakan oleh matplotlib). Jika kita tidak melakukan konversi ini, maka tampilan gambarnya akan berbeda dari gambar dasar yang kita punya.
```python
  ax[0].set_title("Gambar Asli")
```
Mengatur judul subplot pertama menjadi "Gambar Asli".

#### b. Menampilkan Mask Jeruk
```python    
    ax[1].imshow(mask_orange, cmap='gray')
```
- Menampilkan masker jeruk dengan menggunakan ax[1] sebagai subplot kedua.
- cmap='gray' mengatur colormap menjadi grayscale.
```python
    ax[1].set_title("Mask Jeruk")
```
Mengatur judul subplot kedua menjadi "Mask Jeruk".

#### c. Menampilkan Segmentasi Jeruk
```python
    ax[2].imshow(cv2.cvtColor(segmented_orange, cv2.COLOR_BGR2RGB))
```
- Menampilkan hasil segmentasi jeruk dengan menggunakan ax[2] sebagai subplot ketiga.
- cv2.cvtColor(segmented_orange, cv2.COLOR_BGR2RGB) mengonversi gambar dari BGR ke RGB.
```python
    ax[2].set_title("Segmentasi Jeruk")
```
Mengatur judul subplot ketiga menjadi "Segmentasi Jeruk".

#### d. Menampilkan Segmentasi Daun
```python
ax[3].imshow(cv2.cvtColor(segmented_leaves, cv2.COLOR_BGR2RGB))
```
- Menampilkan hasil segmentasi daun dengan menggunakan ax[3] sebagai subplot keempat.
-cv2.cvtColor(segmented_leaves, cv2.COLOR_BGR2RGB) mengonversi gambar dari BGR ke RGB.
```python
ax[3].set_title("Segmentasi Daun")
```
- Mengatur judul subplot keempat menjadi "Segmentasi Daun".
#### e. Menampilkan Semua Gambar
```python
  plt.show()
```
Menampilkan semua subplot dalam figure.
    





