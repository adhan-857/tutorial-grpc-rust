# Tutorial Modul 9: High Level Networking
### Ramadhan Andika Putra (2206081976) - AdPro A <br>

***[ Reflection ]***<br>

1. **What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?**
    > Perbedaan utama antara *unary RPC, server streaming,* dan *bi-directional streaming methods* terletak pada cara data ditransmisikan antara *client* dan server. 
    >
    > Pada ***unary RPC***, *client* mengirim satu permintaan ke server dan menerima satu respons. Metode ini cocok untuk transaksi sederhana seperti pengambilan data yang tidak memerlukan komunikasi berkelanjutan.
    > 
    > ***Server streaming*** digunakan ketika server perlu mengirim sejumlah respons berkelanjutan ke *client (stream of responses)*.
    >
    > ***Bi-directional streaming*** memungkinkan kedua belah pihak, *client* dan server, untuk mengirim dan menerima serangkaian pesan dalam urutan yang fleksibel. Hal ini ideal untuk aplikasi yang memerlukan interaksi dua arah dan *real-time*.
<br>

2. **What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?**
    > Dalam mengimplementasikan layanan gRPC menggunakan Rust, contoh pertimbangan keamanan yang harus diperhatikan di antaranya adalah autentikasi, otorisasi, dan enkripsi data. Autentikasi dan otorisasi penting untuk memastikan bahwa hanya *user* yang berhak yang dapat mengakses layanan. gRPC mendukung berbagai skema autentikasi seperti token dan TLS/SSL untuk enkripsi data, yang digunakan untuk melindungi data yang sensitif dan pribadi selama transmisi.
<br>

3. **What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?**
    > Menurut saya, antangan yang mungkin muncul saat menangani *bi-directional streaming* di Rust gRPC, khususnya pada aplikasi chat, adalah *concurrency*, seperti beberapa *client* dapat mengirim dan menerima pesan secara bersamaan. Dalam keadaan tersebut, kita juga harus menjamin *message ordering* yang sesuai. Selain itu, perlu diperhatikan *error handling* agar jika terjadi masalah, seperti gangguan jaringan atau *client disconnected*, kita perlu sebisa mungkin mencegah sistem mengalami *error* atau *crashing*.
<br>

4. **What are the advantages and disadvantages of using the `tokio_stream::wrappers::ReceiverStream` for streaming responses in Rust gRPC services?**
    > **Kelebihan:**<br>
    > * `ReceiverStream` menyediakan cara yang sederhana untuk mengkonvert sebuah `Receiver` menjadi `Stream` yang dapat langsung digunakan pada gRPC *methods* yang mengembalikan *stream of response*.
    > * Terintegrasi baik dengan modul-modul Tokio yang lain
    > <br>
    > 
    > **Kekurangan:**
    > * Bersifat *one way communication*, karena `ReceiverStream` hanya membungkus `Receiver`, bukan `Sender`.
    > * `ReceiverStream` tidak menyediakan *built-in error handling*.
<br>

5. **In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?**
    > Kode gRPC Rust dapat diatur untuk memfasilitasi *reusability* kode dan modularitas dengan beberapa cara. Misalnya, definisi `Protobuf` dapat di-*centralize* dan digunakan kembali di antara berbagai komponen layanan. Penggunaan *traits* dan *generic types* juga dapat membantu dalam membangun komponen yang dapat dikonfigurasi dan digunakan kembali.
    >
    > Selain itu, pembagian kode ke dalam modul-modul kecil yang berfokus pada *function* tertentu dapat membantu dalam *maintenance* dan menambah kode dengan lebih mudah.
<br>

6. **In the `MyPaymentService` implementation, what additional steps might be necessary to handle more complex payment processing logic?**
    > Menurut saya, langkah tambahan yang dapat dilakukan untuk meng-*handle payment processing logic* yang lebih kompleks diantaranya adalah melakukan *validasi request* untuk mengecek semua informasi yang dibutuhkan terdapat pada *request* dan dalam format yang benar. Selain itu, dapat juga menambahkan *error handling* jika terjadi kesalahan pada proses yang dilakukan. Kita juga dapat menyimpan *transaction* yang dilakukan untuk kebutuhan audit jika diperlukan.
<br>

7. **What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?**
    > Adopsi gRPC sebagai protokol komunikasi berdampak signifikan pada arsitektur dan desain sistem terdistribusi, terutama dalam hal interoperabilitas dengan teknologi dan platform lain. gRPC mendukung berbagai bahasa pemrograman, sehingga cocok untuk diterapkan di lingkungan yang terdapat banyak teknologi dan platform yang berbeda.
<br>

8. **What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?**
    > Keuntungan menggunakan HTTP/2 untuk gRPC dibandingkan dengan HTTP/1.1 atau HTTP/1.1 dengan WebSocket untuk REST APIs antara lain adalah ***multiplexing***, memungkinkan beberapa permintaan dan respons dalam satu koneksi, yang mengurangi latensi. HTTP/2 juga melakukan *header compression* untuk mengurangi *overhead*. 
    >
    > Namun, kerugiannya termasuk kompleksitas implementasi dan dukungan infrastruktur yang lebih tinggi. HTTP/2 bisa menjadi lebih rumit untuk di-*debug* dibandingkan dengan HTTP/1.1 karena sifat binernya, dan tidak semua platform mendukung HTTP/2 secara *native*.
<br>

9. **How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?**
    > REST API menggunakan *synchronous request-response model*. Dimana *client* perlu mengirimkan *request* dan menunggu *response* dari server. Model ini bersifat *stateless* dan sederhana, akan tetapi kurang cocok untuk *real-time communication* karena *client* tidak bisa menerima data dari server, kecuali *client* mengirimkan *request*. Sementara itu, gRPC memiliki kemampuan *bidirectional streaming*. Hal ini berarti *client* dan server dapat mengirim dan menerima data kapan saja, tanpa tergantung sama lain. Model ini lebih cocok untuk *real-time communication* karena data dapat dikirimkan dari server ke *client* saat *client* tersedia dan *client* juga dapat mengirimkan data ke server kapan saja.
<br>

10. **What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?**
    > *Schema-based approach* dengan gRPC bersifat ***strongly typed*** dan membutuhkan *predefined schema*. Hal ini akan menghasilkan kode yang lebih *robust* dan *reliable* di mana kesalahan umum yang terjadi bisa dideteksi saat *compile time*. Selain itu, `Protocol Buffers` lebih *compact* dan cepat untuk di-*serialize* atau di-*deserialize* dari pada `JSON` sehingga dapat meningkatkan performa sistem. Akan tetapi `JSON` bersifat lebih fleksibel, dimana kita dapat menambahkan *fields* atau mengganti struktur dari data tanpa perlu meng-*update* *schema*.