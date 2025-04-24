# Module 8 Advance Programming - High Level Networking

## Nama : Ischika Afrilla
## NPM : 2306227955

### Reflection
1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

    - Unary RPC adalah bentuk paling sederhana, di mana klien mengirim satu permintaan dan server memberikan satu respons. Jenis ini cocok untuk operasi yang cepat dan sederhana, seperti mengambil detail data tertentu. 
    - Server streaming RPC memungkinkan klien mengirim satu permintaan, tetapi server dapat mengirim respons secara bertahap dalam bentuk aliran (stream). Jenis ini sangat berguna untuk mengirim data besar atau bertahap, seperti laporan atau daftar item panjang.
    - bi-directional streaming RPC memungkinkan klien dan server saling bertukar data secara terus-menerus dan bersamaan, cocok untuk aplikasi real-time seperti layanan chat atau komunikasi antara perangkat IoT.

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

    - Autentikasi: Pastikan hanya klien yang sah yang bisa mengakses layanan, misalnya dengan sertifikat TLS atau token.
    - Otorisasi: Setelah autentikasi, pastikan klien hanya bisa mengakses data atau fungsi yang diizinkan.
    - Enkripsi Data: Gunakan TLS untuk mengenkripsi data yang dikirim agar tidak bisa disadap pihak ketiga.
    
    Selain itu, pastikan juga validasi input, pembatasan akses, dan update dependensi tetap aman.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

    - Sinkronisasi: Harus mengelola aliran data masuk dan keluar secara bersamaan tanpa bentrok.
    - Manajemen koneksi: Perlu menangani klien yang terputus atau lambat agar tidak mempengaruhi sistem.
    - Concurrency: Mengelola banyak klien sekaligus butuh penggunaan async dan thread-safe yang benar.
    - Error handling: Harus bisa tangani error jaringan atau pesan yang gagal tanpa crash.
    - Resource usage: Streaming terus-menerus bisa boros memori jika tidak dikelola dengan baik.

4. What are the advantages and disadvantages of using the `tokio_stream::wrappers::ReceiverStream` for streaming responses in Rust gRPC services?

    Kelebihan:
    - Mudah digunakan: Praktis untuk mengubah `tokio::mpsc::Receiver` jadi stream.
    - Cocok untuk async: Terintegrasi baik dengan async Rust dan tokio.
    - Fleksibel: Bisa kirim data dari task lain secara dinamis ke stream.

    Kekurangan:
    - Overhead: Ada sedikit beban performa karena lewat channel.
    - Buffer terbatas: Jika tidak hati-hati, buffer bisa penuh dan menyebabkan blocking atau kehilangan data.
    - Error handling terbatas: Kurang cocok untuk logika kompleks atau error yang rumit.

    Cocok untuk streaming sederhana, tetapi kurang cocok untuk alur yang kompleks atau butuh kontrol lebih besar.

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

    - Pisahkan modul: Buat modul terpisah untuk service, handler (logika bisnis), dan protokol (protobuf).
    - Gunakan trait: Definisikan trait untuk layanan agar mudah diimplementasikan ulang atau diuji.
    - Pakai dependency injection: Gunakan parameter untuk mengirim dependensi, bukan langsung membuat di dalam fungsi.
    - Abstraksi logika bisnis: Jangan campur logika bisnis dengan kode gRPC, pisahkan dalam modul khusus.
    - Gunakan util/helper: Tempatkan kode umum di modul utilitas agar bisa digunakan ulang.

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

    Menambahkan proses autentikasi, agar hanya pengguna yang sah dapat melakukan pembayaran, serta mengubah metode `process_payment` menjadi server streaming. Dengan begitu, server dapat mengirim respons secara bertahap, sehingga lebih efisien dalam mengirim data yang kompleks dan mengurangi overhead koneksi antara klien dan server.

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

    - Performa tinggi: gRPC cepat dan efisien karena pakai HTTP/2 dan Protobuf.
    - Interoperabilitas baik: Mendukung banyak bahasa (Rust, Go, Java, Python, dll), cocok untuk sistem lintas platform.
    - Struktur lebih jelas: Kontrak antar layanan didefinisikan lewat file .proto, jadi komunikasi lebih terstandar.
    - Kompleksitas awal: Butuh setup dan tooling lebih banyak dibanding REST.
    - Cocok untuk microservices: Memudahkan komunikasi antar layanan kecil dalam sistem besar.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

    Kelebihan:
    - Multiplexing: Bisa kirim banyak permintaan dalam satu koneksi tanpa antre.
    - Lebih cepat: Efisien dalam penggunaan jaringan dan lebih responsif.
    - Header lebih ringan: Kompresi header mengurangi ukuran data.
    - Streaming bawaan: Mendukung streaming data dua arah secara alami.

    Kekurangan:
    - Lebih kompleks: Konfigurasi dan debugging lebih rumit.
    - Tidak didukung semua alat/browser: Terutama jika dibandingkan dengan HTTP/1.1.

    Dibandingkan dengan HTTP/1.1 + WebSocket:
    - HTTP/2 lebih efisien dan terstruktur.
    - WebSocket lebih fleksibel untuk komunikasi bebas format, tapi butuh lebih banyak kode custom.

9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

    Model REST bersifat request-response satu arah: klien kirim permintaan, server balas satu kali. Model ini cocok untuk komunikasi sederhana, tetapi kurang responsif untuk real-time karena tidak bisa kirim data terus-menerus.

    Sedangkan gRPC dengan bidirectional streaming memungkinkan klien dan server saling kirim data secara langsung dan berkelanjutan, tanpa harus tunggu permintaan baru sehingga lebih responsif dan efisien untuk aplikasi real-time seperti chat atau notifikasi langsung.

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

    Pendekatan berbasis schema di gRPC, menggunakan Protocol Buffers (Protobuf), lebih terstruktur dan lebih cepat karena data sudah didefinisikan dalam format yang jelas sehingga mempermudah validasi, dokumentasi, dan pengoptimalan performa.

    Sebaliknya, JSON di REST API lebih fleksibel dan dinamis, memungkinkan data dikirim tanpa perlu schema yang ketat, tapi kurang efisien dan lebih lambat karena tidak ada kompresi atau validasi otomatis seperti di Protobuf.