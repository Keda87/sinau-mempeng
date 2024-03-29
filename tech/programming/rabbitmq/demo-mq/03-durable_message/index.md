# RabbitMQ Message Durability

Secara default message di rabbitmq itu disimpan di memory, queue di rabbitmq ini hanya sebuah buffer memory yang menyimpan message secara sementara.
Jadi meskipun sudah implement acknowledgement untuk handle di sisi consumer, tetapi kalo rabbitmq nya crash/restart maka semua message akan hilang.

Untuk mengatasi permasalahan message hilang saat crash/restart, maka perlu di set message durability.

- Queue yang telah dibuat sebelumnya, tidak bisa langsung diubah menjadi durable, sehingga hanya berlaku di queue yang baru.
- Opsi durable ini harus di define di queue, di consumer & producer:
  ```go
  q, err := ch.QueueDeclare(
    "queue-durable",  // name
    true,             // durable
    false,            // delete when unused
    false,            // exclusive
    false,            // no-wait
    nil,              // arguments
  )
  ```
- Producer harus di set persistent.
  ```go
  err = ch.PublishWithContext(ctx,
    "",     // exchange
    q.Name, // routing key
    false,  // mandatory
    false,  // immediate
    amqp.Publishing{
        DeliveryMode: amqp.Persistent,
        ContentType:  "application/json",
        Body:         msgInBytes,
    })
  ```


