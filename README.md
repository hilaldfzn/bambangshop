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
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or **trait** in Rust) in this BambangShop case, or a single Model **struct** is enough?
- Dalam konteks penggunaan Observer design patterns, kebutuhan untuk mengimplementasikan interface (atau trait di Rust), bergantung pada keragaman dan jenis observer yang terlibat. Untuk kasus pada BambangShop, di mana semua **Subscriber** dianggap memiliki behaviour yang sama dan hanya diwakili oleh satu class, penggunaan trait dianggap tidak perlu. Hal ini karena tidak ada kebutuhan untuk menangani berbagai jenis **Subscriber** dengan behaviour yang berbeda. Namun, apabila di kedepannya terdapat penambahan jenis observer baru yang memerlukan behaviour khusus, maka pengimplementasian trait menjadi penting untuk memfasilitasi polimorfisme dan memastikan fleksibilitas dalam menangani berbagai jenis observer.

**id** in **Product** and **url** in **Subscriber** is intended to be unique. Explain based on your understanding, is using **Vec** (list) sufficient or using **DashMap** (map/dictionary) like we currently use is necessary for this case?
- Penggunaan **DashMap** untuk mengelola **Product** dan **Subscriber** dalam kasus di mana **id** dan **url** dijamin unik adalah pilihan yang lebih efisien dibandingkan dengan **Vec** (list). **DashMap** memungkinkan penggunaan **id** atau **url** yang unik sebagai key, yang secara signifikan membantu mempercepat proses pencarian, insertion, dan deletion. Selain itu, jika kita mengganti penggunaan **DashMap** dengan **Vec**, maka perlu membuat dua **Vec** terpisah, untuk menyimpan **url** dan **Subscriber**, yang tidak hanya memperlambat proses tetapi juga mempersulit pengelolaan data. Oleh karena itu, penggunaan **DashMap** dibandingkan **Vec** akan memberikan keuntungan dalam hal efisiensi dan kemudahan pengelolaan data yang berkaitan dengan **Product** dan **Subscriber** yang memiliki key unik.

When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of List of Subscribers (**SUBSCRIBERS**) static variable, we used the **DashMap** external library for **thread-safe HashMap**. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?
- Dalam kasus BambangShop yang akan memanfaatkan *multithreading*, pemilihan struktur data yang *thread-safe* penting untuk menghindari kondisi *race condition* saat data diakses atau dimodifikasi oleh banyak *thread* secara bersamaan. Menurut pemahaman saya, **DashMap** menjadi pilihan yang cocok untuk kebutuhan dalam kasus ini karena merupakan versi *thread-safe* dari **HashMap** yang mendukung operasi concurrent pada pasangan key-value. **DashMap** memungkinkan akses dan modifikasi data secara simultan oleh berbagai *thread* tanpa mengorbankan keamanan data. Selain itu, penerapan Singleton pattern melalui `lazy_static` berguna untuk memastikan bahwa hanya terdapat satu *instance* dari objek yang digunakan untuk menyimpan daftar **Subscriber**. Hal ini untuk memastikan bahwa semua akses dan perubahan pada daftar **Subscriber** dilakukan pada satu sumber data yang sama. Penggunaan Singleton pattern bersama dengan DashMap dapat menjadi solusi yang efisien untuk memastikan bahwa kita dapat mengelola data **Subscriber** dengan aman dalam lingkungan *multithreaded*.

#### Reflection Publisher-2

In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?
- Prinsip *Separation of Concerns* dalam desain sistem *software* menekankan pentingnya untuk memisahkan tiap bagian sistem berdasarkan fokus fungsinya, sehingga memungkinkan perubahan spesifik hanya pada bagian yang terkait. Dalam konteks ini, *Repository* bertanggung jawab atas penyimpanan dan pengambilan data, sementara *Service* mengelola logika bisnis. Pemisahan antara *Service* dan *Repository* mengikuti prinsip Single Responsibility Principle (SRP), di mana *Service* mengolah data yang diperoleh dari *Repository*, dan *Repository* berfungsi sebagai *layer* akses ke database untuk operasi seperti update dan delete. Pemisahan kedua *layer* ini tidak hanya memfasilitasi pengembangan tetapi juga meningkatkan kemudahan dalam pemeliharaan kode.

What happens if we only use the Model? Explain your imagination on how the interactions between each model **(Program, Subscriber, Notification)** affect the code complexity for each model?
- Jika kita hanya mengandalkan model untuk menangani penyimpanan data dan logika bisnis, maka dapat menyebabkan *tightly coupled*, di mana model harus saling mengetahui dan bergantung satu sama lain. Hal ini berarti bahwa perubahan pada satu model dapat memicu kebutuhan akan perubahan pada model lain sehingga membuat kode menjadi sulit untuk di*maintain*. Jika hanya menggunakan *layer* model tanpa adanya pemisahan tugas ke *layer* lain, maka akan menghasilkan program yang kurang fleksibel, di mana perubahan kecil dapat memerlukan revisi luas terhadap kode, meningkatkan kompleksitas, dan mengurangi kemudahan pemeliharaan.

Have you explored more about **Postman**? Tell us how this tool helps you to test your current work. Maybe you want to also list which features in Postman you are interested in or feel like it’s helpful to help your Group Project or any of your future software engineering projects.
- **Postman** adalah alat yang digunakan untuk pengujian API yang memungkinkan pengguna untuk melakukan berbagai jenis HTTP request seperti GET, POST, DELETE, dan lainnya dengan mudah. Fitur yang cukup menarik bagi saya adalah kemampuan untuk menyesuaikan jenis request dan berbagi *collection of requests* hanya dengan *copy-paste text*. Ini sangat membantu dalam menguji aplikasi yang kita buat untuk memastikan bahwa aplikasi memberikan response yang diharapkan terhadap request yang dibuat. Dengan **Postman**, pengguna dapat mengeksplorasi dan memverifikasi fungsi CRUD aplikasi untuk memeriksa apakah data yang di-*retrieve* akurat.

#### Reflection Publisher-3