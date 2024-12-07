# PostgreSQL Row Lock

## FOR UPDATE
https://github.com/user-attachments/assets/3f8b9a26-535c-4bb6-9caf-25738e5ee972

Catatan:
- `FOR UPDATE` akan melakukan lock pada row sesuai `WHERE` condition, dan semua transaction akan menunggu hingga transaction yang berhasil lock pertama menyelesaikan tugasnya.
- `FOR UPDATE` jika dilakukan secara concurrent, sesuai contoh `Transaction A` akan mendapatkan data terlebih dahulu dan ditampilkan, namun `Transaction B` akan menunggu sampai lock di transaction A selesai. sehingga transaction B hanya menunggu sampai blok transaction di transaction A selesai (commit/rollback). Hal ini juga dapat mengakibatkan lock timeout error.
- Studi kasus yang cocok menggunakan `FOR UPDATE` seperti transaksi perbankan transfer dana yang sifatnya harus berjalan secara berurutan.
- Pro & Cons:
    - Performa paling rendah dalam handling concurrency, karena masih perlu berjalan secara berurutan dan harus menunggu secara bergantian ketika row sedang di lock.
    - Semua transaction akan menunggu row yang telah di lock selesai, dan mungkin bisa terjadi deadlock.

## FOR UPDATE NOWAIT
https://github.com/user-attachments/assets/ca602cfc-21d3-4973-93c4-cf15960f97fd

Catatan:
- `FOR UPDATE NOWAIT` akan melakukan lock pada row, dan semua transaction yang mengakses row yang telah di lock akan mengalami error.
- `FOR UPDATE NOWAIT` jika dilakukan secara concurrent, sesuai contoh `Transaction A` akan mendapatkan data, kemudian `Transaction B` langsung mengalami error tanpa harus menunggu `Transaction A` selesai.
- Pro & Cons:
    - Performa menengah dalam handling concurrency, karena masih memerlukan error handling.
    - Tidak ada kemungkinan deadlock, karena transaction lain akan langsung mendapat error ketika mengakses row yang telah di lock.

## FOR UPDATE SKIP LOCKED
https://github.com/user-attachments/assets/f952a9b9-070f-4f36-96a5-5099b980eff1

Catatan:
- `FOR UPDATE SKIP LOCKED` akan melakukan lock pada row, dan semua transaction yang mengakses row yang telah di lock akan melakukan skip data, dan dilewati.
- `FOR UPDATE SKIP LOCKED` jika dilakukan secara concurrent, sesuai contoh `Transaction A` akan mendapatkan data, kemudian `Transaction B` akan tetap sukses tetapi tidak mengembalikan data (no rows) karena di skip/dilewati.
- Studi kasus yang cocok untuk task scheduler, task queue.
- Pro & Cons:
    - Performa paling baik dalam handle concurrency dengan syarat toleransi data yang dilewati.
    - Tidak ada kemungkinan deadlock, karena data yang telah di lock langsung di skip.
