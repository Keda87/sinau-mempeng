# RabbitMQ simple consumer & producer.

- Producer: aplikasi yang ngirim pesan.
- Consumer: aplikasi yang menerima pesan.
- Producer & Consumer harus connect dengan rabbitmq yang sama dan queue yang sama.

```mermaid
graph TD;
    Producer-->hello-queue;
    Consumer-->hello-queue;
```