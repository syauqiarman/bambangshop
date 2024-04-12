# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Dalam pola Observer, Subscriber menerima pembaruan dari subjek saat terjadi perubahan. Implementasi interface/trait digunakan untuk menggambarkan interface Subscriber. Pada kasus BambangShop yang hanya ada satu jenis subscriber, maka interface/trait tidak diperlukan, single model struct saja sudah cukup. Namun, jika membutuhkan fleksibilitas dan memerlukan behavior yang beda, disarankan untuk menggunakan trait. Dengan trait, sistem dapat diperluas dengan mudah.

2. Dengan mempertimbangkan syarat bahwa id dan url subscriber harus unik, disarankan untuk menggunakan DashMap. Dengan memanfaatkan key yang unik, operasi insert, lookup, dan deletion akan berlangsung dengan efisiensi yang tinggi. Pemilihan DashMap menjadi lebih tepat dalam konteks ini mengingat keunikan id dan url yang menjadi persyaratan. Jika memilih Vec, terutama dalam kasus di mana url dan id memiliki kemungkinan kesamaan, akan menjadi lebih rumit karena memerlukan dua Vec terpisah untuk menyimpan data yang tentunya akan meningkatkan kompleksitas manajemen data dan memperlambat proses secara keseluruhan. Karenanya, DashMap menjadi pilihan yang lebih optimal untuk kasus di mana id produk dan url subscriber harus unik.

3. Menurut saya, penggunaan Singleton pattern dalam BambangShop memastikan bahwa hanya ada satu instance dari SUBSCRIBER yang dapat diakses oleh banyak Thread. Namun, untuk memastikan keselamatan penggunaan ThreadSafe dalam konteks multithreading, lebih baik menggabungkan Singleton dengan DashMap. Hal ini tidak hanya menjamin thread safety, tetapi juga memastikan bahwa daftar subscriber tetap terpusat dan tidak tersebar di berbagai bagian DashMap.

#### Reflection Publisher-2
1. Memisahkan Service dan Repository dari Model adalah tindakan yang mendukung prinsip-prinsip SOLID, terutama Prinsip Single Responsibility (SRP). Dalam pola MVC tradisional, Model bertanggung jawab tidak hanya atas penyimpanan data tetapi juga logika bisnis, yang bertentangan dengan SRP. Oleh karena itu, solusi yang diajukan adalah memisahkan Service untuk mengatur logika bisnis dan Repository untuk mengelola akses data. Dengan menerapkan konsep Separation of Concerns dari SOLID, kita memahami bahwa pembagian tugas yang jelas antara Service dan Repository penting untuk menciptakan kode yang fleksibel dan mudah diatur.

2. Penggunaan model saja dalam penanganan data logic dan business logic akan mengakibatkan keterkaitan yang tinggi antar bagian, yang pada gilirannya akan menyulitkan perawatan dan skalabilitas aplikasi. Jadi meskipun kode masih berfungsi, kompleksitasnya menjadi tidak fleksibel karena bagian-bagian saling terkait, sehingga membuatnya sulit dipelihara dan meningkatkan kompleksitas secara keseluruhan. Dengan kata lain, tanpa memisahkan Service dan Repository dari model, kode akan sulit dipelihara dan kompleksitasnya akan meningkat.

3. Sebelumnya, saya telah menggunakan Postman dalam mata kuliah Pemrograman Berbasis Platform. Postman sangat berguna untuk menguji API dengan mengirimkan permintaan HTTP dan memeriksa apakah respons yang diterima sesuai dengan harapan. Postman mempermudah pengiriman permintaan HTTP dengan data dalam body atau parameter ke URL atau endpoint tertentu yang membantu saya memastikan apakah suatu endpoint dapat menerima data dengan benar dan apakah data responsnya dikembalikan dengan benar.

#### Reflection Publisher-3
1. Pada kasus ini, Observer Pattern dengan pendekatan Push Model untuk sistem notifikasi saat setiap proses add, delete, dan publish produk berlangsung. Ketika proses-proses tersebut terjadi, NotificationService akan secara iteratif mengirimkan notifikasi kepada semua Subscriber yang telah berlangganan pada tipe produk terkait melalui fungsi notify dengan mengiterasikan pengiriman data ke setiap Subscribernya.

2. Jika kita menggunakan metode lain yaitu pull model untuk kasus Subscriber pada tutorial ini, maka Subscriber haruslah aktif untuk meminta data kepada Publisher. Keuntungan dari hal tersebut adalah Subscriber dapat mengambil data yang relevan dengan mereka kapanpun, namun hal tersebut juga bisa menjadi kekurangan karena sistem notifikasi untuk Subscriber yang sudah berlangganan harusnya untuk sistem automasi setiap terdapat add, delete, dan publish product akan berubah menjadi Subscriber yang harus mengetahui struktur dari data yang mereka ingin agar bisa mengambil data mana yang mereka mau serta waktu pengambilannya.

Keuntungan menggunakan pull model dalam kasus Subscriber memungkinkan decoupling karena observer tidak perlu mengetahui detail data yang dibutuhkan sebelumnya. Namun, kekurangannya dapat meningkatkan kompleksitas karena observer harus mengimplementasikan logika untuk memilih data yang diinginkan dan dapat menyebabkan redundansi jika data yang sama diambil berkali-kali. Dalam konteks ini, Subscriber harus aktif dalam meminta data kepada Publisher, meskipun mereka dapat mengambil data yang relevan kapan saja tetap saja ada kekurangan yang dapat menambah kompleksitas sistem notifikasi bagi Subscriber yang sudah berlangganan.

3. Ketika subscriber dari publisher terus bertambah, antrian notifikasi akan semakin memanjang. Karena notifikasi diproses secara satu per satu berurutan tanpa penggunaan multi-threading yang dapat memperlambat kinerja aplikasi dan menyebabkan bottleneck pada sistem karena tingginya beban yang harus ditangani.