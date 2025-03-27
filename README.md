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

1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

    Meskipun single Model struct sudah cukup untuk memenuhi kebutuhan BambangShop saat ini, penggunaan interface tetap lebih direkomendasikan untuk skalabilitas di masa depan. Mendefinisikan Subscriber sebagai interface memungkinkan pengembangan alternatif implementasi Subscriber tanpa perlu mengubah kode yang sudah ada. Meskipun BambangShop saat ini belum memerlukan kompleksitas tersebut, mengadopsi interface sejak awal pengembangan akan memudahkan skalabilitas dan adaptasi jika di kemudian hari dibutuhkan berbagai tipe Subscriber.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

    Pengunaan DashMap dalam BambangShop lebih disarankan untuk menyimpan id pada Product dan url pada Subscriber daripada Vec karena efisiensi akses dan pencarian data. Meskipun Vec dapat digunakan untuk menyimpan list, kompleksitas pencarian menggunakan Vec adalah O(N), sedangkan DashMap memiliki komleksitas pencarian O(1). Apabila aplikasi sering membutuhkan pencarian cepat, memastikan tidak ada duplikasi, atau melakukan pembaruan data berdasarkan id atau url, DashMap akan memberikan performa yang lebih baik. Namun, jika kebutuhan hanya sekadar penyimpangan sederhana tanpa kebutuhan pencarian yang tinggi, penggunaan Vec masih dapat dipertimbangkan. Dalam konteks BambangShop yang membutuhkan efisiensi akses dan manajemen data, penggunaan DashMap menjadi pilihan yang tepat.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

    Dalam pengembangan BambangShop menggunakan Rust, Singleton dan DashMap memiliki peran penting untuk menjamin keamanan dan integritas data. DashMap secara khusus menyediakan mekanisme thread-safe yang memungkinkan akses dan manipulasi variabel Subscribers secara aman oleh beberapa thread secara bersamaan, dengan mekanisme locking internal yang efisien. Meskipun Singleton Pattern dapat menjamin hanya satu instance yang ada, hal ini tidak selalu menawarkan keamanan thread secara otomatis. Oleh karena itu, penggunaan DashMap tetap lebih baik karena tidak hanya memastikan keunikan instance, tetapi juga menjamin operasi yang aman dan efisien dalam kasus concurrent.

#### Reflection Publisher-2

1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Pemisahan "Service" dan "Repository" dari Model merupakan penerapan penting dari prinsip Separation of Concerns dan Single Responsibility Principle. Meskipun Model dalam MVC mencakup penyimpanan data dan logika bisnis, memisahkan "Repository" yang berfokus pada penyimpanan data dan "Service" yang berfokus pada logika bisnis. Dengan pendekatan ini, kode lebih mudah dipelihara, diuji, dan dimodifikasi, karena perubahan pada logika bisnis tidak akan mempengaruhi mekanisme penyimpanan data dan sebaliknya. Dengan demikian, pemisahan "Service" dan "Repository" meningkatkan skalabilitas, readability, dan flexibility.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

Apabila hanya menggunakan Model tanpa pemisahan "Service" dan "Repository", interaksi antarmodel (Program, Subscriber, dan Notification) akan menyebabkan Model menjadi bloated (terlalu padat dengan berbagai tanggung jawab). Setiap Model akan menambahkan method tambahan secara terpaksa, akan menurunkan modularitas dan maintainability. Dengan interaksi antarmodel secara langsung, kompleksitas kode akan bertambah ketika ingin melakukan modifikasi pada salah satu bagian. 

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Setelah melakukan eksplorasi lebih lanjut mengenai Postman, saya mengetahui bahwa Postman dapat digunakan untuk melakukan pengujian API secara komprehensif melalui berbagai HTTP method seperti GET dan POST. Fitur utama yang ditawarkan adalah kemampuannya untuk mengirimkan request dengan custom body, menambahkan token, dan memeriksa respons dari server. Selain fungsionalitas pengujian, Postman juga menawarkan kemampuan pembuatan dokumentasi API yang terstruktur. Saya yakin Postman akan menjadi alat yang sangat membantu dalam Tugas Kelompok dan proyek lainnya di masa mendatang.

#### Reflection Publisher-3

1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

    Dalam kasus ini, kita menggunakan Observer Pattern dengan variasi Push model, dimana Publisher secara aktif melakukan push data dan notifikasi kepada Subscriber. Hal ini terlihat jelas dari implementasi method `NotificationService::notify` yang memanggil method `update()` untuk setiap Subscriber yang telah berlangganan pada tipe produk tertentu, dengan Publisher menginisasi pengiriman data tanpa menunggu permintaan dari Subscriber. Penggunaan Push model memungkinkan pengiriman informasi secara real-time sehingga proses notifikasi menjadi lebih efisien dan responsif.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

    - Kelebihan
        
        Kelebihan utama menggunakan Pull model adalah efisiensi sumber daya, dimana Subscriber hanya mengambil informasi saat diperlukan sehingga dapat mengurangi beban pada Publisher.

    - Kekurangan
        
        Kekurangan utama Pull model adalah keterlambatan informasi (latency) karena notifikasi akan tertunda jika Subscriber tidak melakukan request secara aktif. Hal ini bertentangan dengan tujuan utama notifikasi, yaitu memberikan informasi secara real-time, sehingga berpotensi menurunkan efektivitas sistem.
   
    Meskipun Pull model menawarkan fleksibilitas dalam pengambilan data, pendekatan Push model lebih sesuai dengan kebutuhan BambangShop.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.

    Apabila tidak menggunakan multi-threading untuk notifikasi, program akan mengalami beberapa kendala signifikan. Proses pengiriman notifikasi akan berjalan secara sinkronus dalam satu thread, yang berarti setiap notifikasi harus menunggu notifikasi sebelumnya selesai terlebih dahulu. Hal ini menyebabkan delay yang substansial, terutama ketika terdapat banyak notifikasi yang perlu dikirim secara bersamaan. Hal ini mengakibatkan kinerja aplikasi menurun secara drastis, waktu eksekusi menjadi lebih lambat, dan kemampuan untuk mengirimkan notifikasi ke beberapa Subscriber secara bersamaan menjadi terbatas. Dengan demikian, tidak menggunakan multi-threading akan berdampak negatif pada performa dan efisiensi proses notifikasi dalam aplikasi.