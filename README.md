# Tutorial 9 Advance Programming
Muhammad Albyarto Ghazali (2306241695)


### Subscriber Reflection

> a. What is amqp?

AMQP (Advanced Message Queuing Protocol) adalah sebuah protokol komunikasi yang digunakan untuk pertukaran pesan antar sistem secara async. Protokol ini umum digunakan dalam arsitektur yang melibatkan message broker seperti RabbitMQ, di mana satu komponen sistem (publisher) mengirimkan pesan ke sebuah antrean (queue), dan komponen lain (subscriber) akan memproses message tersebut. Dengan AMQP, komunikasi antar service menjadi lebih terpisah dan tidak saling dependent secara langsung, sehingga sistem menjadi lebih fleksibel.

> b. What does it mean? guest:guest@localhost:5672 , what is the first guest, and what is the second guest, and what is localhost:5672 is for?

Pada bagian `amqp://guest:guest@localhost:5672`, ini adalah format URL koneksi yang digunakan untuk menghubungkan aplikasi ke server AMQP seperti RabbitMQ. Kata `guest` yang pertama adalah username, dan `guest` yang kedua adalah password yang digunakan untuk otentikasi ke server RabbitMQ. Secara default, RabbitMQ memang menyediakan akun `guest` dengan password `guest` untuk keperluan pengujian atau pengembangan. Selanjutnya, `localhost` menunjukkan bahwa server RabbitMQ berjalan di komputer lokal (komputer yang sama dengan aplikasi), dan `5672` adalah nomor port standar yang digunakan oleh protokol AMQP untuk menerima koneksi. Jadi, keseluruhan URL tersebut berarti bahwa aplikasi akan mencoba terhubung ke server RabbitMQ lokal menggunakan akun default melalui port standar AMQP.

---
### Publisher Reflection

> a. How much data your publisher program will send to the message broker in one
run?

Dalam satu kali eksekusi, program publisher akan mengirimkan sebanyak lima buah pesan ke message broker. Setiap pesan berupa objek `UserCreatedEventMessage` yang memiliki dua atribut, yaitu `user_id` dan `user_name`, keduanya berupa string. Data ini diserialisasi menggunakan format Borsh, yang menghasilkan representasi biner untuk dikirimkan melalui protokol AMQP. Karena setiap pesan hanya memuat string yang relatif pendek, total data yang dikirimkan dalam satu kali jalan program sangat kecil, kemungkinan hanya beberapa ratus byte secara keseluruhan. Namun demikian, jumlah pastinya tergantung pada panjang masing-masing string dan overhead dari format Borsh itu sendiri. Secara fungsional, program ini hanya mengirim lima pesan secara berturut-turut ke antrean RabbitMQ.

> b. The url of: “amqp://guest:guest@localhost:5672” is the same as in the subscriber
program, what does it mean?

URL “amqp\://guest\:guest\@localhost:5672” yang digunakan baik pada program publisher maupun subscriber menunjukkan bahwa kedua program tersebut menggunakan message broker yang sama, yaitu RabbitMQ yang berjalan secara lokal di komputer pengguna. Dalam URL tersebut, kata "guest" pertama merupakan username, dan "guest" kedua adalah password yang digunakan untuk autentikasi ke server RabbitMQ. Sementara itu, "localhost" menunjukkan bahwa koneksi dilakukan ke server yang berada di komputer lokal, dan angka "5672" adalah port default yang digunakan untuk komunikasi AMQP. Dengan demikian, kesamaan URL ini menunjukkan bahwa publisher dan subscriber saling terhubung melalui RabbitMQ lokal yang sama, sehingga pesan yang dikirim oleh publisher dapat diterima oleh subscriber sesuai dengan event yang didaftarkan.

---

### Screenshot of Running RabbitMQ
![image](https://github.com/user-attachments/assets/a9aa833e-0b0b-4166-89c6-3fe93bd54b64)

---

### Sending and processing event

![image](https://github.com/user-attachments/assets/72d4c720-8b74-4d0d-83b5-e2d8e9a96383)
![image](https://github.com/user-attachments/assets/6a6f0eea-5040-4d72-967d-bde68120c09b)

Pada saat menjalankan perintah cargo run pada program publisher, lima event (2x cargo run maka = 10 event) dikirimkan ke message broker (RabbitMQ). Setiap event berisi informasi terkait dengan pembuatan pengguna baru (misalnya user_id dan user_name). Event-event ini kemudian diterima dan diproses oleh subscriber yang sudah terhubung dengan queue yang sama di RabbitMQ. Proses ini memungkinkan komunikasi antar program dengan menggunakan sistem berbasis message queue, yang memastikan bahwa pesan diproses secara terpisah dan async. Di console subscriber, kita bisa melihat bagaimana pesan-pesan ini diterima dan diproses sesuai dengan handler yang sudah didefinisikan.

---

### Monitoring chart based on publisher

![image](https://github.com/user-attachments/assets/f50bb6a7-50c9-4fda-9ba6-25ad49f61dab)

Ketika publisher mengirimkan event, terlihat ada spike (2 spike karena 2x run) yang terjadi pada grafik yang menunjukkan aktivitas di channel. Spike ini terjadi karena message broker harus memproses sejumlah besar pesan dalam waktu singkat, yang menghasilkan peningkatan dalam jumlah koneksi dan aktivitas pada broker. Dengan menjalankan publisher secara berulang, kita dapat melihat bagaimana RabbitMQ mengelola beban pesan dan bagaimana sistem menangani lonjakan traffic secara dinamis. Grafik ini memberikan gambaran tentang bagaimana message broker merespons beban yang ditimbulkan oleh publisher yang mengirimkan banyak event.

---



