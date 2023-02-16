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

import (
  "bytes"
  "io"
  "fmt"
)

func main() {

  var (
    w1 bytes.Buffer
    w2 bytes.Buffer
  )
  
  mw := io.MultiWriter(&w1, &w2)
  
  fmt.Fprintln(mw, "Hello world")
  
  fmt.Println(w1.String())
  fmt.Println(w2.String())
  
}
```
Penjelasan:
- `w1` dan `w2` adalah writer dari `bytes.Buffer`
- `mw` ini yang akan melakukan tulis ke beberapa writer (`w1` dan `w2`) bersamaan.

## Referensi
Sudah dibaca:
- https://medium.com/dev-bits/explaining-common-i-o-patterns-in-go-cd01b1b749c4

Belum dibaca:
- https://nathanleclaire.com/blog/2014/07/19/demystifying-golangs-io-dot-reader-and-io-dot-writer-interfaces/
