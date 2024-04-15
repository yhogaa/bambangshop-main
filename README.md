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
1. **In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?** </br>
Pada case BambangShop, menurut saya meskipun Model struct bisa mewakili Publisher (BambangShop), tetapi kita tetap memerlukan interface atau trait untuk mewakili Subscriber agar pola Observer dapat diterapkan secara efektif. Ini penting karena setiap Subscriber mungkin memiliki perilaku atau respons yang berbeda terhadap pembaruan dari BambangShop, dan interface atau trait memungkinkan kita untuk memiliki banyak jenis Subscriber dengan perilaku yang berbeda-beda.
2. **id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?** </br>
Menggunakan Vec (list) untuk menyimpan id di Program dan url di Subscriber mungkin cukup jika kita yakin bahwa id dan url tersebut akan unik dan tidak berubah. Namun, menggunakan DashMap (map/dictionary) seperti yang saat ini digunakan dapat lebih baik karena memastikan bahwa setiap id dan url benar-benar unik dan memungkinkan pencarian dan penghapusan dengan lebih efisien. DashMap memiliki kompleksitas waktu yang lebih baik daripada Vec untuk operasi pencarian dan penghapusan, dengan O(1) untuk operasi tersebut dibandingkan dengan O(n) untuk Vec. Oleh karena itu, penggunaan DashMap bisa menjadi pilihan yang lebih baik dalam hal kinerja dan keamanan data.
3. **When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?** </br>
Penggunaan DashMap dalam kasus ini menurut saya lebih baik, karena implementasi Singleton pattern saja tidak cukup untuk memastikan thread safety. Singleton pattern dapat memastikan bahwa hanya ada satu instance dari struktur data yang dibuat, namun tidak menjamin bahwa akses ke instance tersebut aman dari race condition. Dengan demikian, meskipun Singleton pattern dapat membantu mengatasi kebutuhan akan satu instance tunggal dari SUBSCRIBERS, penggunaan DashMap masih diperlukan untuk memastikan thread-safe access pada HashMap tersebut. Dashmap juga mampu menghanddle concurrency dengan baik jadi kita tidak repot untuk menambah sinkronasi tambahan.

#### Reflection Publisher-2
1. **In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?** </br>
   Pemisahan "Service" dan "Repository" dari Model dalam Model-View-Controller (MVC) diperlukan untuk memisahkan tanggung jawab yang berbeda dalam aplikasi. "Service" bertanggung jawab untuk mengatur logika bisnis, sedangkan "Repository" bertanggung jawab untuk mengelola akses dan manipulasi data. Dengan memisahkan "Service" dan "Repository" dari Model, kita dapat mencapai prinsip Single Responsibility Principle (SRP) yang menyatakan bahwa setiap kelas harus bertanggung jawab hanya untuk satu hal tertentu. Pemisahan ini juga memungkinkan aplikasi untuk lebih mudah dikelola, diuji, dan dimodifikasi karena masing-masing komponen memiliki tanggung jawab yang terdefinisi dengan jelas.
2. **What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?** </br>
   Jika kita hanya menggunakan Model tanpa memisahkan "Service" dan "Repository", maka seluruh logika bisnis dan akses data akan ditempatkan dalam Model. Ini dapat menyebabkan peningkatan kompleksitas kode di setiap Model (Program, Subscriber, Notification). Interaksi antara model dalam skenario ini dapat meningkatkan kompleksitas karena setiap model harus berurusan dengan logika bisnis dan manipulasi data secara langsung, tanpa adanya lapisan abstraksi yang jelas. Hal ini dapat menyulitkan pemeliharaan, pengujian, dan pengembangan aplikasi karena kode menjadi sulit dipahami dan berpotensi terjadi duplikasi logika. Dengan memisahkan "Service" dan "Repository" dari Model, kita dapat mengurangi kompleksitas ini dengan menyediakan lapisan abstraksi yang memisahkan tanggung jawab dan menyederhanakan interaksi antar model.
3. **Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects. </br>**
Saya baru mencari tahu lebih banyak tentang postman ketika semester 3 yaitu pada mata kuliah PBP. Postman ini berguna untuk API Testing terhadap aplikasi saya dan menampilkan responya dalam berbagai format, seperti JSON atau XML. Fitur yang menarik dari Postman sendiri adalah menyimpan dan mengelola request yang sering digunakan dalam koleksi, sehingga memudahkan untuk pengujian berulang dan juga seperti yang sudah disebutkan bahwa akan mengembalikan response dengan format yang beragam. Saya akan menggunakan Postman pada proyek kelompok saya untuk menguji dan mengelola API yang digunakan dalam proyek kelompok saya. 
#### Reflection Publisher-3
1. **Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?** </br>
Observer pattern yang digunakan pada case tutorial ini adalah push model, ini bisa dilihat dari notify function publisher yang mengiterasi seluruh subscriber agar mendapat update terbaru.
2. **What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)**</br>
Jika kita menggunakan pull model, kelebihanya Subscriber memiliki kontrol atas kapan akan ambil data dari publisher. Sehingga subscriber akan request update ketika dibutuhkan. Namun kekurangannya pengiriman notificationnya tidak real time dan subscriber harus aktif meminta data dari publisher yang membaut penundaan dalam pembaruan data jika penerima tidak meminta data cukup sering atau jika tidak aktid selama periode waktu tertentu.
3. **Explain what will happen to the program if we decide to not use multi-threading in the notification process.**</br>
   Jika kita memutuskan untuk tidak menggunakan multi-threading dalam proses notifikasi, maka proses notifikasi akan dilakukan secara synchronous. Hal ini berarti bahwa setiap kali notifikasi harus dikirim, program akan menunggu hingga notifikasi selesai dikirim ke semua subscriber sebelum melanjutkan eksekusi berikutnya. Akibatnya, proses notifikasi dapat menjadi lambat, terutama jika jumlah subscriber yang besar atau jika terdapat keterlambatan dalam koneksi ke subscriber tertentu. Ini juga dapat menyebabkan penundaan dalam respons sistem secara keseluruhan, karena proses notifikasi memblokir eksekusi program lainnya. Dengan menggunakan multi-threading, program dapat mengirim notifikasi ke subscriber secara bersamaan, meningkatkan efisiensi dan responsivitas sistem secara keseluruhan.

