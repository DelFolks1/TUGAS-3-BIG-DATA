Nama : Raja Permata Boy <br>
NRP : 05111740000070 <br>
Kelas : BIG Data <br>

<h1> Business Understanding </h1> <br>
Kemungkinan proses yang bisa dilakukan dengan dataset ini adalah : <br>
- Mengelompokkan film berdasarkan genre <br>
- Melakukan rekomendasi film berdasarkan rating dari tahun 1995 sampe 2015 <br>
- Mengelompokkan film berdasarkan rating yang diberi pengguna <br>

<h1> Data Understanding </h1> <br>

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
