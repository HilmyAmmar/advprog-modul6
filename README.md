## Commit 1 Reflection Notes
Pada method handle_connection, terdapat `BufReader` yang akan mengelilingi referensi mutable terhadap stream yang diberikan. `BufReader` akan menambahkan buffering, yang berarti kode ini akan menangani pemanggilan metode-metode train `std::io:Read`. Selanjutnya, method ini juga memiliki `http_request` untuk mengumpulkan baris dari permintaan browser yang dikirimkan ke server kita. Adanya `Vec<_>` berarti kita ingin mengumpulkan baris-baris ini dalam bentuk vektor. 
<br>
`BufReader` yang kita gunakan ini akan mengimplementasikan trait `std::io::BufRead1` untuk menyediakan `lines` method. Method ini akan mengembalikan iterator `Result<String, std::io::error` dengan membagi aliran data setiap kali menemukan adanya baris byte baru. Untuk mendapatkan setiap `String`, kita akan melakukan *map* dan *unwrap* pada setiap `Result`. `Result` akan error apabila datanya tidak valid UTF-8 atau ada masalah ketika sedang membaca *stream*. Untuk kasus sekarang, kita akan menghentikan program ketika ada error dan belum melakukan *error handling*

## Commit 2 Reflection Notes
![Commit 2 screen capture](/assets/images/commit2.png)

## Commit 3
![Commit 3 screen capture](/assets/images/commit3.png)
