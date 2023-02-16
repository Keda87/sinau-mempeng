## Go io.* package

Untuk melakukan proses IO di Golang, bisa menggunakan interface `io.Reader` dan `io.Writer`.

Yang banyak kita ketahui untuk memulai program go bisa dengan kode berikut.
```go
package main

import "fmt"

func main() {
  fmt.Println("Hello world")
}
```
tapi sebenernya yang terjadi dibelakang layar itu proses simplifikasi dari kode berikut.
```go
package main

import (
  "fmt"
  "os"
)

func main() {
  // Penjelasan:
  // - parameter pertama Fprintln minta berupa interface io.Writer.
  // - os.Stdout itu salah satu implementasi dari interface io.Writer.
  // - parameter kedua Fprintln menerima data dengan tipe string yang akan di cetak ke stdout.
  fmt.Fprintln(os.Stdout, "Hello world")
}
```


Sudah dibaca:
- https://medium.com/dev-bits/explaining-common-i-o-patterns-in-go-cd01b1b749c4

Belum dibaca:
- https://nathanleclaire.com/blog/2014/07/19/demystifying-golangs-io-dot-reader-and-io-dot-writer-interfaces/
