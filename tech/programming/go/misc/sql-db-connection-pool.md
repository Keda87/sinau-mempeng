# Connection Pooling

- `sql.DB`
  - itu object pool connection yang isinya koneksi database yang terpakai dan idle.
  - ketika ada perintah database `sql.DB` akan cek dulu, apakah ada koneksi yang idle atau ga, jika ada maka dipakai jika tidak maka akan buat koneksi baru.
- `SetMaxOpenConns(...)`
  - secara default tidak ada batasnya.
  - untuk set jumlah koneksi database maksimal (termasuk yang sedang terpakai dan idle).
  - `db.SetMaxOpenConns(5)` misal kita set max open 5 dan 5 koneksi sedang terpakai semua, jika ada kebutuhkan ke database, maka perintah ke 6 perlu menunggu koneksi ada yang idle.
- `SetMaxIdleConns(...)`
  - secara default `sql.DB` koneksi idle maksimal 2 dan kita bisa ubah dengan function ini.
  - secara teori semakin besar idle connection membantu meningkatkan performa, karena tidak perlu buat koneksi db baru dari awal ketika digunakan (hemat resource).
  - setup koneksi idle harus kurang dari atau sama dengan `SetMaxOpenConns(...)`


Referensi: https://www.alexedwards.net/blog/configuring-sqldb
