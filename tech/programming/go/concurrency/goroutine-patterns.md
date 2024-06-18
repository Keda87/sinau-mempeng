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

## Fire or Error
Biasanya kalo kita punya beberapa function, dan kita hanya perlu semuanya sukses, atau berhenti jika ada salah satu yang error.
```Go
import (
	"errors"
	"fmt"
	"log"
	"time"
)

func first(isError bool) error {
	time.Sleep(time.Second)
	if isError {
		return errors.New("first: unknown error")
	}
	return nil
}

func second(isError bool) error {
	time.Sleep(time.Second)
	if isError {
		return errors.New("second: unknown error")
	}
	return nil
}

func main() {
	const numberOfTask = 2
	errChan := make(chan error, numberOfTask) // buffered channel, sebanyak function yang dipanggil di goroutine.
	defer close(errChan)

	go func(c chan<- error) {
		err := first(false)
		c <- err
	}(errChan)

	go func(c chan<- error) {
		err := second(false)
		c <- err
	}(errChan)

	totalFinishedTask := 0
	for totalFinishedTask < numberOfTask {
		if err := <-errChan; err != nil {
			log.Println(err)
			return
		}
		totalFinishedTask++
	}
}

```

## Goroutine Compose Result Or Error
Biasanya kalo punya beberapa function yang harus diolah
```Go
import (
	"errors"
	"time"
)

type profile struct {
	name string
}

type resultWrapper struct { // buat bungkus hasil masing-masing function.
	Data any
	Err  error
}

func getProfile(name string, isMockError bool) (profile, error) {
	time.Sleep(time.Second)
	if isMockError {
		return profile{}, errors.New("i/o error")
	}
	return profile{name: name}, nil
}

func getBalance(isMockError bool) (float64, error) {
	time.Sleep(time.Second)
	if isMockError {
		return 0, errors.New("i/o error")
	}
	return 10_000_000, nil
}

func main() {
	const numberOfTask = 2

	resultChan := make(chan resultWrapper, numberOfTask) // buffered channel.
	defer close(resultChan)

	go func(chn chan<- resultWrapper) {
		p, err := getProfile("john doe", false)
		chn <- resultWrapper{Data: p, Err: err}
	}(resultChan)

	go func(chn chan<- resultWrapper) {
		b, err := getBalance(false)
		chn <- resultWrapper{Data: b, Err: err}
	}(resultChan)

	var (
		name         string
		balance      float64
		finishedTask = 0
	)

	for finishedTask < numberOfTask {
		r := <-resultChan
		if r.Err != nil {
			fmt.Println("Error: ", r.Err)
			return
		}

		switch r.Data.(type) {
		case profile:
			name = r.Data.(profile).name
		case float64:
			balance = r.Data.(float64)
		}

		finishedTask++
	}
}
```
