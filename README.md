# Sistem rekomendasi 

![tobias-winkelmann-367873](https://user-images.githubusercontent.com/88027268/204447678-71ea3773-9831-4b05-a974-d6862a35c256.jpg)
Gambar 1. Illustrasi bali

## Proyek overview

Bali merupakah pulau dengan tempat wisata yang paling populer dikunjungi wisatawan lokal maupun internasional. Salah satu cara untuk berkunjung ke bali adalah dengan memesan tiket pesawat atau hotel dibali secara online melalui aplikasi Travel. dan untuk memudahkan pengguna dalam memilih hotel mana yang paling sesuai dengan yang pernah dikunjungi sebelumnya adalah menggunakan sistem rekomendasi. sistem rekomendasi ini menggunakan data histori pengguna untuk direkomendasikan selanjutnya. 

Rekomendasi sistem ini dapat memudahkan user dalam memilih hotel dari beberapa pilihan yang mungkin relevan dengan pengguna. Dengan termudahkannya pengguna dalam memilih hotel dapat meningkatkan daya wisata ke orang banyak yang ingin mengunjungi pulau bali. [(Ni Made Ernawati, N M Sudarmini and N M R Sukmawati, 2018)](https://www.researchgate.net/publication/322945292_Impacts_of_Tourism_in_Ubud_Bali_Indonesia_a_community-based_tourism_perspective)
 
## Business Understanding

Bagian ini akan menjelaskan proses klarifikasi masalah.

### Problem Statements

- Berdasarkan data mengenai daftar hotel dibali, bagaimana membuat sistem rekomendasi yang dipersonalisasi dengan teknik *content-based filtering*?

### Goals

- Menghasilkan beberapa rekomendasi hotel yang dipersonalisasi untuk pengguna dengan teknik *content-based filtering*.

    ### Solution statements
    - Menggunakan dua teknik sistem rekomendasi yaitu *content based filtering* dan *collaborative filtering*

## Data Understanding
- Link download dataset hotel di bali: https://www.kaggle.com/datasets/faridansutariya/data-hotel-in-bali-from-traveloka-website

### Variabel-variabel pada dataset adalah sebagai berikut:
Dataset Hotel di bali:
- Hotel Name: Nama hotel yang ada di bali.
- Original price: Biaya penginapan di hotel tersebut sebelum mendapatkan diskon /malam.	
- Price after discount: Biaya penginapan di hotel tersebut setelah mendapatkan diskon /malam.
- Tax: Pernyataan apakah biaya telah ditambah dengan pajak.
- Rating: Rating yang diberikan pengguna ke hotel tersebut.
- location: lokasi dari hotel tersebut.

## Content-based filtering

Langkah pertama import *library* yang dibutuhkan untuk kasus kali ini:

*Library* yang akan diimport adalah *library* yang berhubungan untuk memanipulasi data, data visualisasi, *preprocessing*, teknik fitur ekstraksi, teknik untuk mencari hubungan antara fitur, menampilkan seluruh angka yang berada dibelakang koma dan mematikan warning yang didapat setelah menjalankan code. 

### Exploratory Data Analysis - Deskripsi Variabel:

Selanjutnya membaca dataset dan menampilkan 3 data random:
|     	|                Hotel Name 	| Original price 	| Price after discount 	|                Tax 	| Rating 	|         location 	|
|----:	|--------------------------:	|---------------:	|---------------------:	|-------------------:	|-------:	|-----------------:	|
| 359 	|     Adi Dharma Hotel Kuta 	|   Rp 1.020.000 	|           Rp 663.000 	| Inclusive of taxes 	|    8.5 	|       Kuta, Bali 	|
| 562 	| Beluran Serene Guesthouse 	|     Rp 399.999 	|           Rp 299.999 	| Inclusive of taxes 	|    8.6 	| Kerambitan, Bali 	|
| 442 	|              Agata Villas 	|   Rp 9.161.796 	|         Rp 3.226.541 	| Inclusive of taxes 	|    7.3 	|   Seminyak, Bali 	|

> Dataset ini terdiri dari 1012 data dan 6 kolom.


| # 	| Column               	| Non-Null Count 	| Dtype   	|
|---	|----------------------	|----------------	|---------	|
| 0 	| Hotel Name           	| 1012 non-null  	| object  	|
| 1 	| Original price       	| 1012 non-null  	| object  	|
| 2 	| Price after discount 	| 1012 non-null  	| object  	|
| 3 	| Tax                  	| 1012 non-null  	| object  	|
| 4 	| Rating               	| 876 non-null   	| float64 	|
| 5 	| location             	| 1012 non-null  	| object  	|


Fitur pada dataset terdiri dari 5 kolom tipe object dan 1 kolom bertipe float.

Pada dataset ini total hotel yang tersedia berjumlah 990 Hotel unik dengan total lokasi 84 lokasi unik.

![1](https://user-images.githubusercontent.com/88027268/204459950-eec18f4b-3e0f-4c96-9edc-5d1402403c2b.png)

Gambar 2. Distribusi lima lokasi dengan hotel terbanyak

Pada dataset ini Kuta dan ubud menjadi kota dengan hotel terbanyak diikuti oleh kota seminyak, legian dan sanur.

#### Data Cleasing

Karena fitur price pada dataset masih dalam bentuk tipe object maka kita dapat melakukan konversi ke tipe numeric untuk memudahkan dilakukannya analisis. Sebelum itu harus menghilangkan karakter *Rp* dan titik, jika tidak maka akan terjadi *error* pada saat melakukan konversi tipe data. 

Melihat dan menghapus data duplikat yang terdapat pada dataset

|    	|                         Hotel Name 	| Original price 	| Price after discount 	| Rating 	|         location 	| is_duplicate 	|
|---:	|-----------------------------------:	|---------------:	|---------------------:	|-------:	|-----------------:	|-------------:	|
|  0 	|           Bumi Linggah Villas Bali 	|        1845375 	|               830419 	| 8.8000 	|     SukawatiBali 	|        False 	|
|  1 	|           Bumi Linggah Villas Bali 	|        1845375 	|               830419 	| 8.8000 	|     SukawatiBali 	|         True 	|
|  2 	|                        Dee Mansion 	|         358938 	|               183058 	| 8.1000 	| WestDenpasarBali 	|        False 	|
|  3 	|                        Dee Mansion 	|         358938 	|               183058 	| 8.1000 	| WestDenpasarBali 	|         True 	|
|  4 	| Fourteen Roses Boutique Hotel Kuta 	|         810000 	|               518400 	| 8.1000 	|       LegianBali 	|        False 	|
|  5 	| Fourteen Roses Boutique Hotel Kuta 	|         810000 	|               518400 	| 8.1000 	|       LegianBali 	|         True 	|
|  6 	|            Jasmine Inn Nusa Penida 	|         226667 	|               170000 	| 8.6000 	|   NusaPenidaBali 	|        False 	|
|  7 	|            Jasmine Inn Nusa Penida 	|         226667 	|               170000 	| 8.6000 	|   NusaPenidaBali 	|         True 	|
|  8 	|                The Kryamaha Villas 	|        2903987 	|              1732499 	| 8.8000 	|     TanahLotBali 	|        False 	|
|  9 	|                The Kryamaha Villas 	|        2903987 	|              1732499 	| 8.8000 	|     TanahLotBali 	|         True 	|
| 10 	|  Ozora Tiying Tutul Hostel @Canggu 	|         120000 	|                90000 	| 8.1000 	|       CangguBali 	|        False 	|
| 11 	|  Ozora Tiying Tutul Hostel @Canggu 	|         120000 	|                90000 	| 8.1000 	|       CangguBali 	|         True 	|
| 12 	|                   Samsara Homestay 	|         237039 	|               177779 	| 9.2000 	|    KintamaniBali 	|        False 	|
| 13 	|                   Samsara Homestay 	|         237039 	|               177779 	| 9.2000 	|    KintamaniBali 	|         True 	|
| 14 	|                  Sanur Guest House 	|         339999 	|               254999 	| 7.9000 	|        SanurBali 	|        False 	|
| 15 	|                  Sanur Guest House 	|         339999 	|               254999 	| 7.9000 	|        SanurBali 	|         True 	|
| 16 	|                 Argasoka Bungalows 	|         316667 	|               237500 	| 8.8000 	| MonkeyForestBali 	|        False 	|
| 17 	|                 Argasoka Bungalows 	|         316667 	|               237500 	| 8.8000 	| MonkeyForestBali 	|         True 	|
| 18 	|      OYO 3062 Pondok Wisata Dewata 	|         188561 	|               118793 	| 7.1000 	|    BatubulanBali 	|        False 	|
| 19 	|      OYO 3062 Pondok Wisata Dewata 	|         188561 	|               118793 	| 7.1000 	|    BatubulanBali 	|         True 	|
| 20 	|        White Sandy Beach Menjangan 	|         380000 	|               285000 	| 8.5000 	|    MenjanganBali 	|        False 	|
| 21 	|        White Sandy Beach Menjangan 	|         380000 	|               285000 	| 8.5000 	|    MenjanganBali 	|         True 	|
| 22 	|                   Scuba Tribe Bali 	|         380000 	|               239400 	| 9.3000 	|     TulambenBali 	|        False 	|
| 23 	|                   Scuba Tribe Bali 	|         380000 	|               239400 	| 9.3000 	|     TulambenBali 	|         True 	|
| 24 	|               The Calna Villa Bali 	|        2254792 	|              1826382 	| 8.8000 	|         KutaBali 	|        False 	|
| 25 	|               The Calna Villa Bali 	|        2254792 	|              1826382 	| 8.8000 	|         KutaBali 	|         True 	|
| 26 	|         Sebuluh Sunset Hill Penida 	|        1403100 	|               841860 	| 9.5000 	|   NusaPenidaBali 	|        False 	|
| 27 	|         Sebuluh Sunset Hill Penida 	|        1403100 	|               841860 	| 9.5000 	|   NusaPenidaBali 	|         True 	|

Melihat statistika deskripsi:

|       	| Original price 	| Price after discount 	|   Rating 	|
|------:	|---------------:	|---------------------:	|---------:	|
| count 	|       876.0000 	|             876.0000 	| 876.0000 	|
|  mean 	|   1923463.9555 	|         1280057.5194 	|   8.4323 	|
|  std  	|   3979703.4323 	|         2986544.9362 	|   0.5145 	|
|  min  	|     63333.0000 	|           47500.0000 	|   5.6000 	|
|  25%  	|    380000.0000 	|          250000.0000 	|   8.2000 	|
|  50%  	|    838052.0000 	|          549999.0000 	|   8.5000 	|
|  75%  	|   1942893.7500 	|         1262362.5000 	|   8.8000 	|
|  max  	|  60980499.0000 	|        49394204.0000 	|   9.7000 	|

- Dapat dilihat bahwa rating hotel terendah yaitu 5.6 dan tertinggi 9.7.
- Dapat dilihat bahwa rata-rata biaya /malam menyentuh hampir 2 juta dan yang tertingginya yaitu 6 juta.
- Dapat dilihat bahwa biaya termurah setelah diberikan diskon yaitu 470k dan yang tertinggi 4.9 juta

### Exploratory Exploratory Data Analysis - Data Cleasing:

|                      	| Total 	| Percentage of Missing Values 	|
|---------------------:	|------:	|-----------------------------:	|
|        Rating        	|   136 	|                      13.4387 	|
|      Hotel Name      	|     0 	|                       0.0000 	|
|    Original price    	|     0 	|                       0.0000 	|
| Price after discount 	|     0 	|                       0.0000 	|
|          Tax         	|     0 	|                       0.0000 	|
|       location       	|     0 	|                       0.0000 	|

> Missing value pada fitur rating dapat dihapus karena hal tersebut adalah noise didalam dataset.

## Data Preparation

### Feature Selection
*Feature selection* adalah proses mengurangi jumlah fitur atau variabel input dengan memilih fitur-fitur yang dianggap paling relevan terhadap model.

- menghapus kolom *Tax* karena hanya memiliki satu nilai saja sehingga tidak berpengaruh pada score yang dihasilkan


## Modeling

Melakukan pemberian bobot pada fitur lokasi didataset menggunakan TF-IDF Vectorizer. lalu melihat beberapa hasil dari perhitungan idf, seperti berikut:

|    	| fitur            	|
|---:	|------------------	|
| 37 	| nusaceninganbali 	|
| 11 	| eastdenpasarbali 	|
| 62 	|          spabali 	|
| 31 	|       mengwibali 	|
| 22 	|    klungkungbali 	|

Selanjutnya setelah dilakukan perhitungan idf dapat dilanjutkan ke proses transformasi ke dalam bentuk matriks
> Matriks ini memiliki total 862 data dan 83 lokasi

Hasil matriks jika dalam bentuk dataframe:

|                                   	| singarajabali 	| balianbali 	| klungkungbali 	| nusaduabeachhotel 	| kerobokanbali 	| padangbaibali 	| lovinabali 	| tabanankotabali 	| nusapenidabali 	| kemenuhbali 	|
|----------------------------------:	|--------------:	|-----------:	|--------------:	|------------------:	|--------------:	|--------------:	|-----------:	|----------------:	|---------------:	|------------:	|
|                        Hotel Name 	|               	|            	|               	|                   	|               	|               	|            	|                 	|                	|             	|
|          Lloyd's Inn Bali         	|        0.0000 	|     0.0000 	|        0.0000 	|            0.0000 	|        0.0000 	|        0.0000 	|     0.0000 	|          0.0000 	|         0.0000 	|      0.0000 	|
|         Asher Bali Transit        	|        0.0000 	|     0.0000 	|        0.0000 	|            0.0000 	|        0.0000 	|        0.0000 	|     0.0000 	|          0.0000 	|         0.0000 	|      0.0000 	|
|   diAtas by Art Cafe Bumbu Bali   	|        0.0000 	|     0.0000 	|        0.0000 	|            0.0000 	|        0.0000 	|        0.0000 	|     0.0000 	|          0.0000 	|         0.0000 	|      0.0000 	|
|     Taman Surgawi Resort & Spa    	|        0.0000 	|     0.0000 	|        0.0000 	|            0.0000 	|        0.0000 	|        0.0000 	|     0.0000 	|          0.0000 	|         0.0000 	|      0.0000 	|
|             Hotel Ratu            	|        0.0000 	|     0.0000 	|        0.0000 	|            0.0000 	|        0.0000 	|        0.0000 	|     0.0000 	|          0.0000 	|         0.0000 	|      0.0000 	|
|        Ayola Bali Jimbaran        	|        0.0000 	|     0.0000 	|        0.0000 	|            0.0000 	|        0.0000 	|        0.0000 	|     0.0000 	|          0.0000 	|         0.0000 	|      0.0000 	|
|        Kira Cottages Penida       	|        0.0000 	|     0.0000 	|        0.0000 	|            0.0000 	|        0.0000 	|        0.0000 	|     0.0000 	|          0.0000 	|         *1.0000* 	|      0.0000 	|
| Green Palace Homestay Nusa Penida 	|        0.0000 	|     0.0000 	|        0.0000 	|            0.0000 	|        0.0000 	|        0.0000 	|     0.0000 	|          0.0000 	|         1.0000 	|      0.0000 	|
|         Pradhana Residence        	|        0.0000 	|     0.0000 	|        0.0000 	|            0.0000 	|        0.0000 	|        0.0000 	|     0.0000 	|          0.0000 	|         0.0000 	|      0.0000 	|
|         Darba Guest House         	|        0.0000 	|     0.0000 	|        0.0000 	|            0.0000 	|        0.0000 	|        0.0000 	|     0.0000 	|          0.0000 	|         0.0000 	|      0.0000 	|


Hasil diatas menunjukkan hotel dengan lokasi yang benar akan diberi nilai 1 dan hote yang tidak sesuai dengan lokasi akan diberi nilai 0. sehingga data Hotel *Kira Cottages Penida* berlokasi di Nusa Penida diberi nilai 1, Karena hotel tersebut memang berlokasi di Nusa Penida.

Setelah melakukan pemberian bobot pada tiap fitur maka akan dilanjut ke tahap mengidentifikasi korelasi antara hotel dan lokasinya. untuk melakukan hal tersebut dapat menghitung derajat kesamaan atau *similarity degree* antar lokasi menggunakan teknik *cosine similarity*.


|                          Hotel Name 	| OYO 1654 Maha Bharata Kuta Inn 	| Famous Hotel Kuta 	| Kejora Suites 	| Matta Lodge Bali 	| OYO 3244 Grand Chandra Hotel 	|
|------------------------------------:	|-------------------------------:	|------------------:	|--------------:	|-----------------:	|-----------------------------:	|
|                          Hotel Name 	|                                	|                   	|               	|                  	|                              	|
|          Hilton Bali Resort         	|                         0.0000 	|            0.0000 	|        0.0000 	|           0.0000 	|                       0.0000 	|
|     Samabe Bali Suites & Villas     	|                         0.0000 	|            0.0000 	|        0.0000 	|           0.0000 	|                       0.0000 	|
|  Stark Boutique Hotel and Spa Bali  	|                         0.0000 	|            0.0000 	|        0.0000 	|           0.0000 	|                       0.0000 	|
|         The Mulia - Nusa Dua        	|                         0.0000 	|            0.0000 	|        0.0000 	|           0.0000 	|                       0.0000 	|
|         Respati Beach Hotel         	|                         0.0000 	|            0.0000 	|        1.0000 	|           0.0000 	|                       0.0000 	|
|         Hotel Kumala Pantai         	|                         1.0000 	|            0.0000 	|        0.0000 	|           0.0000 	|                       0.0000 	|
|     OYO 90547 Pondok Saren Anyar    	|                         0.0000 	|            0.0000 	|        0.0000 	|           0.0000 	|                       0.0000 	|
|         Maha Rama Suite Amed        	|                         0.0000 	|            0.0000 	|        0.0000 	|           0.0000 	|                       0.0000 	|
|            Ila Villa Ubud           	|                         0.0000 	|            0.0000 	|        0.0000 	|           0.0000 	|                       0.0000 	|
| Aloft Bali Seminyak- CHSE Certified 	|                         0.0000 	|            0.0000 	|        0.0000 	|           0.0000 	|                       0.0000 	|


Hasil dari perhitungan derajat kesamaan akan menghasilkan dimensi (862, 862) merupakan ukuran matriks similarity dari dataset dengan kata lain mesin berhasil mengidentifikasi 862 Nama Hotel tingkat kesamaan (masing-masing dalam sumbu X dan y). *Hotel Kumala Pantai* dengan *OYO 1654 Maha Bharata Kuta Inn* memiliki nilai 1 yang berarti terdapat kemiripan lokasi. Untuk memastikan bahwa kedua hotel tersebut memiliki kesamaaan, sebagai berikut:


|     	|                     Hotel Name 	| Original price 	| Price after discount 	| Rating 	|   location 	|
|----:	|-------------------------------:	|---------------:	|---------------------:	|-------:	|-----------:	|
| 704 	|            Hotel Kumala Pantai 	|        1457592 	|              1000585 	| 8.7000 	| LegianBali 	|
| 273 	| OYO 1654 Maha Bharata Kuta Inn 	|         357452 	|               178726 	| 7.8000 	| LegianBali 	|


## Evaluation
Setelah semua tahap telah terselesaikan maka akan tiba saatnya membuat sebuah fungsi yang dimana pada fungsi tersebut akan mengembalikan 5 data hotel teratas berdasarkan lokasi dari data histori pengguna. Berikut adalah rekomendasi misalnya user sebelumnya mengunjungi Hotel *Matahari Bungalow*:

|   	|                   Hotel Name 	|   location 	|
|--:	|-----------------------------:	|-----------:	|
| 0 	|               Ossotel Legian 	| LegianBali 	|
| 1 	|               The Legian 777 	| LegianBali 	|
| 2 	|             Nakula Stay Kuta 	| LegianBali 	|
| 3 	| City Garden Bali Dwipa Hotel 	| LegianBali 	|
| 4 	|               Cempaka Losmen 	| LegianBali 	|

Data Hotel *Matahari Bungalow*:

|     	|        Hotel Name 	| Original price 	| Price after discount 	| Rating 	|   location 	|
|----:	|------------------:	|---------------:	|---------------------:	|-------:	|-----------:	|
| 656 	| Matahari Bungalow 	|         622618 	|               290768 	| 8.0000 	| LegianBali 	|

Berdasarkan tabel diatas kita dapat melihat bahwa seluruh rekomendasi yang diberikan oleh model adalah yang berlokasi di Legian, bali dan lokasi tersebut sesuai dengan data lokasi hotel *Matahari Bungalow* yang kita asumsikan sebagai data hotel yang dikunjungi sebelumnya oleh pengguna.

Disini akan menggunakan *metric precision* sebagai acuan karena harapannya sistem rekomendasi dapat memberikan beberapa rekomendasi yang benar-benar relevan dengan data hotel yang sebelumnya telah dikunjungi oleh pengguna

```math
Recall = \frac{Total-rekomendasi-yang-relevan}{Total-rekomendasi-yang-diberikan}
```

Penjelasan dari formula diatas:
- Total-rekomendasi-yang-relevan berarti sistem rekomendasi berhasil memberikan rekomendasi yang relevan dengan data hotel yang sebelumnya dikunjungi oleh pengguna.
- Total-rekomendasi-yang-diberikan berarti jumlah rekomendasi yang telah diberikan oleh sistem kepada pengguna tanpa memikirkan apakah rekomendasi itu relevan atau tidak. 

```math
Recall = \frac{5}{5} = 1.00
```

Dari hasil perhitungan diatas sistem rekomendasi yang telah dibuat mencapai score 1.00 atau sempurna berhasil memberikan rekomendasi yang relevan kepada pengguna.
