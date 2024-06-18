# Pola Goroutine Yang Biasa Digunakan (Practical Usage Goroutine)

## Fire and Forget
Ini biasanya kalo perlu panggil beberapa function, tapi kita gak perlu cek error atau result dari function.
Jadi cukup dipanggil pake goroutine dan tunggu semuanya selesai pake `sync.WaitGroup`.

```Go
package main

import (
	"fmt"
	"sync"
	"time"
)

func publish(message string) {
	time.Sleep(time.Second) // pura-pura nya function ini makan waktu 1 detik buat publish message.
	fmt.Println("publish: ", message)
}

func main() {
	var wg sync.WaitGroup
	wg.Add(3) // jumlah goroutine yang jalan.

	go func() {
		defer wg.Done()
		publish("satu")
	}()
	go func() {
		defer wg.Done()
		publish("dua")
	}()
	go func() {
		defer wg.Done()
		publish("tiga")
	}()

	wg.Wait()
}
```
