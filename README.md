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

>1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Dalam kasus ini, saya rasa kita tidak membutuhkan interface atau trait khusus, khususnya karena kita hanya memiliki satu Observer yaitu Subscriber. Kita cukup menggunakan struct model Subscriber yang ada untuk menyimpan data yang dibutuhkan. Interface atau trait khusus biasanya digunakan ketika kita menggunakan berbagai jenis Observer yang dikelompokkan dalam kelas-kelas yang berbeda.

>2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

Menurut saya, penggunaan DashMap lebih cocok untuk kasus ini karena sangat mempermudah pengelolaan data produk beserta Subscriber-nya. Hal ini dikarenakan DashMap memungkinkan kita untuk mencari data menggunakan *key* yang unik (misalnya id dalam Program atau url dalam Subscriber) sehingga pencarian data menjadi efisien. Sebaliknya jika kita menggunakan Vec, pencarian data akan dilakukan satu per satu untuk setiap data yang ada, sehingga cenderung rumit dan kurang efisien jika dibandingkan dengan DashMap.

>3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Kita bisa saja menggunakan pola Singleton untuk memastikan hanya ada satu instance dari data. Namun, pada kasus Bambangshop yang menggunakan multithreading, penggunaan DashMap menjadi lebih cocok karena ia mendukung multithreading dengan fitur thread safety secara otomatis. Apabila kita menggunakan pola Singleton, kita harus menangani sinkronisasi secara manual, sehingga penggunaan DashMap menjadi lebih efisien mengingat DashMap adalah built-in data structure yang mendukung multithreading.

#### Reflection Publisher-2

>1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Menurut pemahaman saya, memisahkan Service dan Repository dari Model akan membantu membuat kode menjadi lebih terstruktur karena peran masing-masing komponen jadi terlihat jelas. Service bertanggung jawab untuk mengatur proses jalannya aplikasi, misalnya seperti mengambil atau mengolah data. Sementara itu,Repository akan menangani akses dan penyimpanan data ke tempat pemyimpanan data. Dengan demikian, selain kode menjadi lebih mudah dipahami dan dipelihara, proses pengujian kode juga akan menjadi lebih aman karena perubahan di suatu bagian tidak akan mengganggu bagian lainnya.

>2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

Jika kita hanya menggunakan Model (tanpa pembagian seperti Service dan Repository), maka aturan proses aplikasi dan penyimpanan data akan tergabung ke dalam satu komponen. Hal ini dapat menimbulkan risiko tertentu, seperti tingkat kompleksitas kode yang bertambah sehingga mempersulit pemeliharaan kode ke depannya. Interaksi antar Model (Program, Subscriber, dan Notification) akan terjadi secara langsung, sehingga  perubahan pada suatu bagian yang bisa saja berdampak pada bagian lain yang tidak ingin diubah.

>3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Selama masa perkuliahan, saya sudah pernah beberapa kali menggunakan Postman untuk melakukan pengujian endpoint aplikasi, yang dalam kasus ini adalah untuk Bambangshop. Dengan Postman, kita dapat dengan mudah melihat respons aplikasi kita terhadap berbagai request, karena Postman membantu kita untuk mengirim request tanpa perlu membuat tampilan halaman terlebih dahulu. Adapun fitur yang menurut saya cukup membantu adalah fitur Collections yang dapat mengorganisir dan mengelola sekelompok requests; serta fitur Environment yang dapat menyimpan variabel dalam API requests, misalnya URL, token autentikasi, dan lain-lain.

#### Reflection Publisher-3

>1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

Pada modul kali ini kita menerapkan Push Model, di mana setiap perubahan data (seperti create, update, atau delete) akan memicu pemanggilan NotificationService. Publisher akan mengirimkan informasi tersebut ke Subscriber melalui HTTP POST ke URL yang telah diberikan, yang berisi data terkait perubahan atau produk tersebut.

>2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

Keuntungan menggunakan Pull Model adalah Subscriber dapat menentukan kapan mereka akan melakukan pull data dari Publisher. Ini akan meningkatkan efisiensi karena program hanya mengirimkan data ketika Subscriber memerlukannya, sehingga menghindari beban pengiriman data yang tidak diperlukan. Subsciber dapat mengambil data ketika mereka membutuhkan data tersebut.
Namun, penggunaan Pull Model juga memiliki kerugian. Misalnya adalah Subscriber harus memahami struktur data sumber agar dapat melakukan request yang tepat. Subscriber juga harus mengatur mekanisme request pembaruan secara berkala agar tidak ada data yang tertinggal.

>3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.

Jika kita tidak menggunakan multi-threading dalam proses notification, maka kita akan menghadapi masalah antrian yang panjang. Hal ini dikarenakan NotificationService akan mengirim pemberitahuan kepada setiap Subscriber satu per satu, dan tidak akan melanjutkan pemberitahuan ke Subscriber selanjutnya sebelum Subscriber yang sekarang selesai. Artinya, jumlah Subscriber yang banyak akan membuat aplikasi menjadi lebih lambat sehingga mempengaruhi waktu respons aplikasi. Ini dapat menyebabkan bottleneck dan membuat proses pengiriman notifikasi menjadi tidak efisien.