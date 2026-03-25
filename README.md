# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: 

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create SubscriberRequest model struct.`
    -   [ ] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Notification repository.`
    -   [ ] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Commit: `Implement receive_notification function in Notification service.`
    -   [ ] Commit: `Implement receive function in Notification controller.`
    -   [ ] Commit: `Implement list_messages function in Notification service.`
    -   [ ] Commit: `Implement list function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

1. Menurut saya, RwLock diperlukan karena data notifikasi bisa diakses oleh banyak bagian program secara bersamaan, terutama saat ada beberapa request masuk. Karena Vec biasa tidak thread-safe, kita perlu mekanisme sinkronisasi agar tidak terjadi race condition saat data dibaca atau ditulis.
Alasan memakai RwLock dan bukan Mutex adalah karena RwLock lebih cocok untuk kasus yang memiliki banyak operasi baca dan lebih sedikit operasi tulis. Dengan RwLock, beberapa thread bisa membaca data secara bersamaan selama tidak ada thread yang sedang menulis. Kalau memakai Mutex, baik operasi baca maupun tulis harus menunggu lock yang sama, jadi akses akan lebih terbatas dan kurang efisien.

Jaadi menurut saya RwLock dipilih karena lebih fleksibel dan lebih cocok untuk kasus notifikasi yang kemungkinan lebih sering dibaca daripada diubah.

2. 
Menurut saya, alasannya karena Rust sangat ketat soal safety, terutama untuk mencegah bug yang berhubungan dengan shared mutable state. Jika isi variabel static bisa diubah sembarangan seperti di Java, maka akan lebih mudah terjadi race condition atau perilaku yang tidak aman saat program berjalan secara concurrent.

Rust ingin memastikan bahwa setiap mutasi terhadap data global benar-benar aman dan terkontrol. Karena itu, kalau kita ingin punya data global yang bisa diubah, kita perlu membungkusnya dengan mekanisme yang aman seperti RwLock, Mutex, atau struktur lain yang thread-safe. lazy_static membantu membuat inisialisasi variabel global yang kompleks, tetapi akses mutablenya tetap harus mengikuti aturan Rust. Rust tidak melarang tanpa alasan, tetapi justru memaksa programmer untuk menulis kode yang lebih aman dan lebih jelas dalam mengelola data global.




#### Reflection Subscriber-2

1. ssaya sempat melihat beberapa bagian lain seperti src/lib.rs Dari situ saya belajar bagaimana konfigurasi global seperti APP_CONFIG dan REQUEST_CLIENT diinisialisasi dan digunakan di seluruh aplikasi. Saya juga jadi lebih paham bagaimana environment variable dibaca dan dipakai untuk menentukan URL instance dan publisher.

Selain itu, saya melihat bagaimana struktur project diatur supaya modular (model, repository, service, controller), sehingga tiap bagian punya tanggung jawab masing-masing. Ini membantu saya memahami alur data dari request sampai response.

2. Menurut saya, Observer pattern sangat mempermudah penambahan subscriber karena publisher tidak perlu tahu detail tiap subscriber. Selama subscriber sudah terdaftar subscribe, publisher hanya perlu mengirim notifikasi ke semua subscriber yang ada.
Ketika kita menambah instance Receiver misalnya port 8001, 8002, 80033, sistem tetap bekerja tanpa perlu perubahan besar pada kode publisher. Ini menunjukkan bahwa Observer pattern scalable untuk penambahan subscriber.
Namun, jika menambah lebih dari satu Main app (publisher), sistem menjadi lebih kompleks. Karena subscriber harus tahu ke publisher mana mereka subscribe, dan bisa terjadi inkonsistensi data antar publisher. Jadi, menambah subscriber itu mudah, tapi menambah publisher tidak semudah itu dan butuh desain tambahan.

3.Saya mencoba menggunakan Postman untuk menguji endpoint seperti subscribe, unsubscribe, dan receive notification. Menurut saya, Postman sangat membantu karena kita bisa langsung melihat response dari API dan memastikan alur sistem berjalan dengan benar.
Fitur yang saya gunakan antara lain sepertCollection untuk mengelompokkan endpoint, Body JSON untuk mengirim data dan History untuk melihat request sebelumnya
Menurut saya Postman sangat berguna baik untuk tugas ini maupun untuk group project, karena mempermudah testing tanpa perlu frontend dan membantu debugging API dengan cepat.