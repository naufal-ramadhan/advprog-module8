## AdvProg - Tutorial Module 08

<h2>
Nama : Muhammad Naufal Ramadhan

NPM  : 2306241700

Kelas  : B
</h2>

1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

Unary RPC merupakan model komunikasi dimana klien mengirimkan satu permintaan dan menerima satu respons, contohnya digunakan untuk operasi CRUD sederhana seperti pengambilan data atau autentikasi. Server streaming RPC memungkinkan server mengirimkan aliran data secara continuous sebagai respons terhadap satu permintaan klien, cocok untuk kasus seperti pemantauan real-time atau pengiriman data besar secara bertahap. Sedangkan bi-directional streaming memungkinkan kedua pihak mengirim aliran data secara bersamaan tanpa harus menunggu pihak lainnya selesai, contohnya untuk aplikasi chat.

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

Beberapa hal penting yang harus diperhatikan terkait keamanan dalam implementasi gRPC dalam rust ada sebagai berikut, autentikasi harus diimplementasikan melalui TLS/SSL untuk mengenkripsi koneksi dan Menggunakan token JWT atau OAuth2 untuk verifikasi identitas pengguna. Enkripsi data sebaiknya diterapkan baik dalam transit maupun saat penyimpanan, dengan penggunaan TLS sebagai standar minimal. Otorisasi juga penting dengan menerapkan sistem kontrol akses berbasis peran (RBAC) dan validasi token pada setiap permintaan, dengan memanfaatkan interceptor untuk memeriksa izin.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

Ada beberapa tantangan yang dapat muncul ketika menghadapi bidirectional streaming, Kompleksitas konkurensi menjadi masalah utama karena perlu mengelola beberapa aliran data secara bersamaan, dengan risiko race condition dan deadlock. Kemudian penanganan error juga rumit, terutama saat koneksi terputus tiba-tiba atau timeout terjadi, yang memerlukan strategi pemulihan yang kuat. Penggunaan memori harus dioptimalkan untuk menjaga performa saat mengelola banyak koneksi jangka panjang. Pelacakan status koneksi dan sinkronisasi state antar pengguna menjadi kompleks.

4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

Keuntungan menggunakan tokio_stream::wrappers::ReceiverStream untuk streaming adalah mempunyai integrasi yang mulus dengan lingkungan asinkronus Tokio, memiliki kemampuan mengendalikan aliran data yang efisien, dan mampu memberikan abstraksi yang bersih untuk komunikasi antar task. Untuk kekurangannya ada penggunaan memori tambahan karena mekanisme channel yang digunakan, dan potensi penurunan performa saat volume pesan sangat tinggi.

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

Bisa dilakukan dengan beberapa pendekatan seperti memisahkan tanggung jawab pada struktur proyek sehingga proyek lebih terorganisir, Pemisahan layanan dari implementasinya menggunakan trait sebagai abstraksi sehingga memungkinkan implementasi alternatif atau pengujian dengan mock, Injeksi dependensi melalui trait dan generics mendukung pengujian unit dan fleksibilitas konfigurasi. Semua pendekatan ini dapat menghasilkan kode yang lebih modular, dapat diuji, dan mudah dipelihara seiring berkembangnya aplikasi.

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

Untuk menangani logika pembayaran yang lebih kompleks, ada beberapa hal yang harus ditambahkan pada MyPaymentService, seperti integrasi dengan gateway pembayaran pihak ketiga seperti Stripe atau PayPal untuk memproses transaksi nyata. Kedua, penerapan sistem manajemen transaksi yang dapat menangani berbagai status pembayaran seperti pending, completed, failed, atau refunded. Dan terakhir, pembuatan laporan (logging) dan analitik terhadap pembayaran yang dilakukan.

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

Penggunaan gRPC sebagai protokol komunikasi membawa beberapa perubahan signifikan pada arsitektur sistem terdistribusi. Pertama, gRPC memungkinkan komunikasi antar layanan lebih cepat dan efisien karena menggunakan HTTP/2 dan format biner. Kedua, definisi kontrak API menjadi lebih jelas dan terstruktur melalui Protocol Buffers, memudahkan kolaborasi antar tim pengembangan. Ketiga, dukungan dengan banyak bahasa sehingga memungkinkan layanan code dalam bahasa yang berbeda namun tetap dapat berkomunikasi dengan lancar. Di sisi lain, gRPC mungkin menimbulkan tantangan interoperabilitas dengan sistem lama atau pihak ketiga yang hanya mendukung REST, sehingga seringkali memerlukan gateway API.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

Untuk kelebihan terdapat sebagai berikut, Pertama, HTTP/2 memungkinkan pengiriman beberapa permintaan dan tanggapan melalui satu koneksi (multiplexing), mengurangi waktu tunggu dan penggunaan sumber daya. Kedua, kompresi header mengurangi jumlah data yang dikirim, membuat komunikasi lebih efisien. Ketiga, prioritisasi permintaan memungkinkan sumber daya penting diproses lebih dahulu. dan untuk kekurangan terdapat sebagai berikut, pertama, lebih sulit untuk dipantau dan di-debug tanpa alat khusus karena formatnya biner. Kedua, memerlukan dukungan TLS yang menambah kompleksitas, dan infrastruktur jaringan lama mungkin tidak mendukungnya sepenuhnya. Dibandingkan dengan WebSocket, HTTP/2 menawarkan fitur streaming dua arah yang serupa tetapi dengan overhead protokol yang lebih tinggi.

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

REST menggunakan pola komunikasi yang mengharuskan klien memulai setiap interaksi dengan permintaan baru, yang kurang efisien untuk pembaruan secara continuous dan memerlukan teknik tambahan seperti polling atau WebSocket untuk pendekatan real-time. Sebaliknya, gRPC dengan streaming dua arah memungkinkan pertukaran data berkelanjutan melalui satu koneksi tanpa perlu memulai permintaan baru setiap kali.

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

Jika menggunakan gRPC dibandingkan JSON, dengan skema yang terdefinisi jelas, gRPC memberikan keamanan tipe yang lebih baik dan kemungkinan kesalahan lebih kecil saat runtime, karena validasi dilakukan pada saat kompilasi. Perubahan API juga menjadi lebih terkontrol dengan aturan kompatibilitas yang jelas, sehingga lebih dapat diandalkan saat sistem berkembang. Format biner Protocol Buffers menghasilkan pesan yang lebih kecil, menghemat bandwidth dan meningkatkan kinerja. Di sisi lain, JSON dalam REST menawarkan fleksibilitas yang lebih besar, memungkinkan perubahan cepat tanpa harus mendefinisikan ulang skema. JSON juga lebih mudah dibaca manusia dan di-debug secara langsung tanpa alat khusus.