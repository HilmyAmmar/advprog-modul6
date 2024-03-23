## Commit 1 Reflection Notes
Pada method handle_connection, terdapat `BufReader` yang akan mengelilingi referensi mutable terhadap stream yang diberikan. `BufReader` akan menambahkan buffering, yang berarti kode ini akan menangani pemanggilan metode-metode train `std::io:Read`. Selanjutnya, method ini juga memiliki `http_request` untuk mengumpulkan baris dari permintaan browser yang dikirimkan ke server kita. Adanya `Vec<_>` berarti kita ingin mengumpulkan baris-baris ini dalam bentuk vektor. 
<br>
<br>
`BufReader` yang kita gunakan ini akan mengimplementasikan trait `std::io::BufRead1` untuk menyediakan `lines` method. Method ini akan mengembalikan iterator `Result<String, std::io::error` dengan membagi aliran data setiap kali menemukan adanya baris byte baru. Untuk mendapatkan setiap `String`, kita akan melakukan *map* dan *unwrap* pada setiap `Result`. `Result` akan error apabila datanya tidak valid UTF-8 atau ada masalah ketika sedang membaca *stream*. Untuk kasus sekarang, kita akan menghentikan program ketika ada error dan belum melakukan *error handling*

## Commit 2 Reflection Notes
![Commit 2 screen capture](/assets/images/commit2.png)
Tujuan utama dalam memperbarui method handle_connection adalah untuk mengimplemntasi fungsionalitas dalam menampilkan sesuatu dalam web browser, bukan hanya blank page saja. Sebelum mengupdate method handle_connection, kita terlebih dahulu membuat file html sebagai konten dari halaman di browser nantinya. Method ini akan memanfaatkan modul `fs` yang menyediakan fungsionalitas untuk bisa berinteraksi dengan sistem file. Selanjutnya, kita akan menggunakan `format!` untuk menambahkan konten file sebagai body dari respon yang sukses. Respon yang sukses dapat dilihat dari status_linenya, dimana pada kode ini statusnya adalah "HTTP/1.1 200 OK". Lalu, untuk memastikan respon HTTPnya valid, kita akan menambahkan `Content-Length` yang di-set sesuai ukuran dari file html. Dengan menjalankan `cargo run` dan menjalankan 127.0.0.1:7878 dalam browser, kita dapat melihat konten html pada page browser. Pada bagian ini, saya mempelajari bagaimana caranya dalam menampilkan konten html ke page browser saat program dijalankan

## Commit 3 Reflection Notes
![Commit 3 screen capture](/assets/images/commit3.png)
Pada bagian ini, kita akan membagi respon menjadi 2, yaitu respon sukses dan gagal. Sebelumnya, kita telah membuat bagian respon yang sukses. Dengan cara yang sama ditambah pemanfaatan conditional if-else, kita membuat bagian respon yang gagal. Jadi project memiliki 2 file html, file html pertama akan ditampilkan ketika respon sukses, file html lainnya akan ditampilkan jika respon gagal. Respon akan berhasil ketika status_linenya adalah "HTTP/1.1 200 OK", respon akan gagal apabila status_linenya adalah "HTTP/1.1 404 NOT FOUND". 
<br>
<br>
Bisa dilihat bahwa program ini memanfaatkan if-else conditional dengan cara mengecek value dari `request_line`. Pengunaan if-else seperti ini dalam menghandle respon yang sukses dan respon yang gagal mempunyai beberapa repetisi/pengulangan. Kedua statement membaca file dan menulis konten file ke dalam `stream`. Untuk memperbaikinya, kita akan langsung memanfaatkan if-else conditional dalam men-assign value dari `status_line` dan `filename`. Dengan ini, *refactoring* yang dilakukan akan menghasilkan kode yang lebih bersih dan terhindar dari repetisi kode yang tidak perlu.

## Commit 4 Reflection Notes
Melalui kode `thread::sleep(Duration::from_secs(5))`, pengaksesan browser akan ditunda selama 5 detik. Lalu, jika kita mengakses 127.0.0.1/sleep dalam beberapa page browser sekaligus, bisa kita lihat bahwa lama waktu page browser untuk diakses bisa lebih dari 5 detik. Hal ini terjadi karena setiap *request* yang masuk ke endpoint tersebut akan memicu penundahaan ini. Jika ada banyak *request* yang masuk, *request* hanya akan ditangani secara bergantian atau satu per satu saja. Sebagai contoh, jika ada 5 *request* yang masuk secara bersamaan, durasi total untuk menyelesaikan semua permintaan menjadi sekitar 25 detik (5 * 5), meskipun tiap *request* hanya membutuhkan 5 detik saja. 

## Commit 5 Reflection Notes
