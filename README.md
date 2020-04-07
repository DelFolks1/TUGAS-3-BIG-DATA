Nama : Raja Permata Boy <br>
NRP : 05111740000070 <br>
Kelas : BIG Data <br>

<h1> Business Understanding </h1>
Kemungkinan proses yang bisa dilakukan dengan dataset ini adalah : <br>
- Mengelompokkan film berdasarkan genre <br>
- Melakukan rekomendasi film berdasarkan rating dari tahun 1995 sampe 2015 <br>
- Mengelompokkan film berdasarkan rating yang diberi pengguna <br>

<h1> Data Understanding </h1> 
- Dataset yang digunakan adalah MovieLens 20M Dataset <br>

- Dataset ini menggambarkan penilaian dan kegiatan pemberian tag  MovieLens, layanan rekomendasi film. Dataset ini berisi 20000263 peringkat dan 465564 aplikasi tag di 27278 film. Data ini dibuat oleh 138493 pengguna antara 09 Januari 1995 dan 31 Maret 2015. Dataset ini dibuat pada 17 Oktober 2016.<br>

- Pengguna dipilih secara acak untuk dimasukkan. Semua pengguna terpilih memiliki nilai setidaknya 20 film.<br>

- Data tersebut terkandung dalam enam file.<br>
  - tag.csv yang berisi tag yang diterapkan ke film oleh pengguna:<br>
    - userId
    - movieId
    - tag
    - timestamp
  
  - rating.csv yang berisi rating yang diberikan oleh user:
    - userId
    - movieId
    - rating
    - timestamp

  - movie.csv yang berisi informasi ttg film:
    - movieId
    - title
    - genres

  - link.csv yang berisi pengidentifikasi yang dapat digunakan untuk menautkan ke sumber lain:
    - movieId
    - imdbId
    - tmbdId
    
  - genome_scores.csv yang berisi data relevansi tag-film:
    - movieId
    - tagId
    - relevance

  - genome_tags.csv yang berisi deskripsi tag:
    - tagId
    - tag
    
<h1> Workflow KNIME
<img src="/dokum/overall.jpg"><br>
<h1> Data Preparation </h1>
- Pertama-tama membuat spark context local menggunakan Create Local Big Data Environment<br>
<img src="/dokum/createlocalspark.jpg"><br>
- Membuat user profile dengan ID 9999 dan memberi rating pada 20 film acak<br>
<img src="/dokum/builduser.jpg"><br>
- Setting pada node file reader sesuai dengan dimana direktori dataset movies.csv berada<br>
<img src="/dokum/settingfilereader.jpg"><br>
- Add field sendiri terdiri dari beberapa node. Buka saja componentnya. Add field berguna untuk menambahkan kolom ID 9999<br>
<img src="/dokum/addfieldcomp.jpg"><br>
- Menggunakan node row splitter untuk mengambil 20 film saja. Setting dengan cara dibawah ini<br>
<img src="/dokum/confrowsplitter1.jpg"><br>
- Selanjutnya, di dalam node Ask User for Rating juga terdiri dari beberapa node. Buka saja componentnya <br>
<img src="/dokum/askuserrating.jpg"><br>
- Node Ask User for Movie Ratings ini meminta user ID 9999 untuk merating 20 movie yang terpilih<br>
- Buka Interactive View dari Ask User for Movie Ratings dan saya memberi angka 5 yang berarti sangat menyukai film tersebut dan mencari rekomendasi filmnya<br>
<img src="/dokum/yourrating.jpg"><br>
- Didalam node no rating terdapat beberapa komponen. Node ini berisi film yang belum dirating oleh user.<br>
<img src="/dokum/noratingcomp.jpg"><br>
- Kemudian pisahkan data dengan Table To Spark. Film yang dirating akan dilanjutkan dengan proses testing sedangkan film yang tidak dirating ke proses deployment. <br>
<img src="/dokum/split.jpg"><br>
- Setelah selesai dengan rating dari user, sambungkan pada Big Data Environment yang telah dibuat sebelumnya<br>
<img src="/dokum/map.jpg"><br>
- Tambahkan node CSV to Spark untuk membaca dataset ratings.csv (isi setting sesuai dengan direktori dimana dataset berada) untuk proses testing<br>
<img src="/dokum/csvtospark.jpg"><br>
- Konfigurasi node Spark Partitioning yang berfungsi untuk mempartisi data rating.csv menjadi 80% untuk proses Training dan 20% sisanya untuk proses Testing<br>
<img src="/dokum/partition.jpg"><br>

<h1> Modelling </h1>
- Proses Training ini berisikan 80% data film dari ratings.csv dan 20 film yang disukai oleh user ID 999999<br>
<img src="/dokum/modelling.jpg"><br>
- Konfigurasi Spark Collaborative Filtering yang berfungsi untuk training data dengan ALS model <br>
<img src="/dokum/sparkfiltering.jpg"><br>

<h1> Testing </h1>
- Data yang digunakan pada proses Testing ini berisikan 20% data film dari ratings.csv<br>
<img src="/dokum/testing.jpg"><br>
- Hasil dari Spark Predictor<br>
<img src="/dokum/sparkpredictor.jpg"><br>
- Konfigurasi node Spark Missing Value untuk menghilangkan NaN<br>
<img src="/dokum/nan.jpg"><br>
- Konfigurasi Spark Numeric Scorer untuk menghitung kesalahan numerik antara ratings original dan rating prediksi<br>
<img src="/dokum/numeric.jpg"><br>
- Hasil dari Spark Numeric Scorer <br>
<img src="/dokum/scorer.jpg"><br>

<h1> Deployment </h1>
- Skema Deployment <br>
<img src="/dokum/deployment.jpg"><br>
- Proses deployment untuk membuat rekomendasi Top 20 dengan rekomendasi 10 film dari data Training<br>
- Hasil dari node Spark Predictor yang menghasilkan rating untuk film yang belum dirating kepada user <br>
<img src="/dokum/sparkpredictor2.jpg"><br>
- Konfigurasi node Spark To Table untuk menaruh dari dataframe spark ke KNIME<br>
<img src="/dokum/sparktotable.jpg"><br>
- node Top20 Recommended Movies terdiri dari beberapa node. Kita bisa membuka componentnya<br>
<img src="/dokum/top20.jpg"><br>
- Didalam node Display Recommendations juga terdiri dari beberapa node. Kita bisa membuka componentnya<br>
- Hasil dari Display Recommendation dan ditampilkan dalam portal web. Isi dari rekomendasi ini berdasarkan rating 5 yang kita berikan pada peratingan film acak pertama kali. <br>
<img src="/dokum/result.jpg"><br>
- Lalu, kita bisa menaruh hasil tersebut dengan CSV Writer . Berikut konfigurasinya<br>
<img src="/dokum/csvwriter1.jpg"><br>
- Hasil dari file <br>
<img src="/dokum/csvwriter2.jpg"><br>
<img src="/dokum/csvwriter3.jpg"><br>

<h1> Membandingkan berapa lama waktu yang diperlukan File Reader dan CSV to Spark untuk membaca sebuah file </h1>
- Disini kita menggunakan node Timer Info yang bisa melihat seluruh waktu yang diperlukan oleh semua proses dalam KNIME workflow <br>
<img src="/dokum/timerinfo.jpg"><br>

- Hasil dari timer info : <br>
<img src="/dokum/timerinfores.jpg"><br>

- Seperti yang kita lihat, untuk ratings.csv CSV To Spark memerlukan waktu yang lebih cepat dibandingkan dengan file reader <br>
- Namun, kita mencoba untuk menggunakan dataset yang lebih kecil. <br>
- Disini saya mencoba membuat project baru dan menggunakan dataset cereals.csv yang memiliki ukuran yang kecil (5KB) <br>
<img src="/dokum/test.jpg"><br>

- Hasil dari Timer info :<br>
<img src="/dokum/timerinfores2.jpg"><br>
- Seperti yang kita lihat, waktu yang diperlukan untuk membaca cereal.csv oleh CSV to Spark lebih lama dari File reader.<br>
- Kenapa bisa begini???<br>

<h2> Kesimpulan </h2>
- CSV to Spark membaca data secara paralel sehingga cocok untuk file dengan ukuran yang lebih besar. <br>
- Namun, jika digunakan untuk data yang kecil maka akan lebih lama memakana waktu karena proses pemilahan data<br>
