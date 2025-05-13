# Tutorial 9 Advance Programming
Muhammad Albyarto Ghazali (2306241695)

> a. What is amqp?

AMQP (Advanced Message Queuing Protocol) adalah sebuah protokol komunikasi yang digunakan untuk pertukaran pesan antar sistem secara async. Protokol ini umum digunakan dalam arsitektur yang melibatkan message broker seperti RabbitMQ, di mana satu komponen sistem (publisher) mengirimkan pesan ke sebuah antrean (queue), dan komponen lain (subscriber) akan memproses message tersebut. Dengan AMQP, komunikasi antar service menjadi lebih terpisah dan tidak saling dependent secara langsung, sehingga sistem menjadi lebih fleksibel.

> b. What does it mean? guest:guest@localhost:5672 , what is the first guest, and what is the second guest, and what is localhost:5672 is for?

Pada bagian `amqp://guest:guest@localhost:5672`, ini adalah format URL koneksi yang digunakan untuk menghubungkan aplikasi ke server AMQP seperti RabbitMQ. Kata `guest` yang pertama adalah username, dan `guest` yang kedua adalah password yang digunakan untuk otentikasi ke server RabbitMQ. Secara default, RabbitMQ memang menyediakan akun `guest` dengan password `guest` untuk keperluan pengujian atau pengembangan. Selanjutnya, `localhost` menunjukkan bahwa server RabbitMQ berjalan di komputer lokal (komputer yang sama dengan aplikasi), dan `5672` adalah nomor port standar yang digunakan oleh protokol AMQP untuk menerima koneksi. Jadi, keseluruhan URL tersebut berarti bahwa aplikasi akan mencoba terhubung ke server RabbitMQ lokal menggunakan akun default melalui port standar AMQP.




