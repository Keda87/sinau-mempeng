## Go io.* package

Untuk melakukan proses IO di Golang, bisa menggunakan interface `io.Reader` dan `io.Writer`.

### 1. Tulis ke standard output

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
  fmt.Fprintln(os.Stdout, "Hello world")
}
```
Penjelasan:
- parameter pertama `fmt.Fprintln` minta berupa interface `io.Writer`.
- `os.Stdout` itu salah satu implementasi dari interface `io.Writer`.
- parameter kedua `fmt.Fprintln` menerima data dengan tipe string yang akan di cetak ke stdout.

### 2. Tulis ke custom writer
```go
package main

import (
  "bytes"
  "fmt"
)

func main() {
  var b bytes.Buffer
  fmt.Fprintln(&b, "Hello world") // set string "Hello world" masuk ke dalam buffer b
  
  fmt.Println(b.String()) // cetak "Hello world" ke console/stdout.
}
```
Penjelasan:
- empty buffer `b` dari `bytes.Buffer` itu implement dari interface `io.Writer`

### 3. Tulis ke multiple writer
```go
package main

func main() {
  
}
```

## Referensi
Sudah dibaca:
- https://medium.com/dev-bits/explaining-common-i-o-patterns-in-go-cd01b1b749c4

Belum dibaca:
- https://nathanleclaire.com/blog/2014/07/19/demystifying-golangs-io-dot-reader-and-io-dot-writer-interfaces/
