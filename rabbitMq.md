In .NET, RabbitMQ is commonly used as a **message broker** to let different parts of an application communicate asynchronously.

### 1. Producer (publisher)

A .NET app publishes messages to a queue or exchange.

Example: An e-commerce API receives an order and publishes an `"OrderCreated"` event.

```csharp
using RabbitMQ.Client;
using System.Text;

var factory = new ConnectionFactory() { HostName = "localhost" };
using var connection = factory.CreateConnection();
using var channel = connection.CreateModel();

channel.QueueDeclare(
    queue: "orders",
    durable: false,
    exclusive: false,
    autoDelete: false,
    arguments: null
);

string message = "New order placed";
var body = Encoding.UTF8.GetBytes(message);

channel.BasicPublish(
    exchange: "",
    routingKey: "orders",
    basicProperties: null,
    body: body
);

Console.WriteLine("Message sent");
```

What happens:

- Connects to RabbitMQ server
- Creates/ensures queue exists
- Sends a message into `orders`

---

### 2. Consumer (subscriber)

Another .NET service listens for messages.

Example: A background worker processes orders.

```csharp
using RabbitMQ.Client;
using RabbitMQ.Client.Events;
using System.Text;

var factory = new ConnectionFactory() { HostName = "localhost" };
using var connection = factory.CreateConnection();
using var channel = connection.CreateModel();

channel.QueueDeclare(
    queue: "orders",
    durable: false,
    exclusive: false,
    autoDelete: false,
    arguments: null
);

var consumer = new EventingBasicConsumer(channel);

consumer.Received += (model, ea) =>
{
    var body = ea.Body.ToArray();
    var message = Encoding.UTF8.GetString(body);

    Console.WriteLine($"Received: {message}");
};

channel.BasicConsume(
    queue: "orders",
    autoAck: true,
    consumer: consumer
);

Console.ReadLine();
```

This consumer:

- Listens to the `orders` queue
- Receives messages whenever a producer publishes one

---

### Common .NET use cases

RabbitMQ is often used in .NET for:

- **Microservices communication**
  - Order service → Payment service → Notification service

- **Background jobs**
  - Send email after registration
  - Resize uploaded images
  - Generate reports

- **Event-driven architecture**
  - Publish events like:
    - `UserRegistered`
    - `PaymentCompleted`
    - `InventoryUpdated`

- **Task queues**
  - Offload expensive work from web requests

Example:
Without RabbitMQ:

```text
API -> send email -> generate PDF -> notify user
```

(API waits for everything)

With RabbitMQ:

```text
API -> publish message -> return response immediately
Worker -> processes tasks in background
```

---

### Important RabbitMQ concepts in .NET

- **Connection** → connection to broker
- **Channel** → lightweight communication session
- **Queue** → stores messages
- **Exchange** → routes messages
- **Routing key** → determines destination
- **Consumer** → receives messages
- **Ack/Nack** → confirm processing success/failure

---

### In ASP.NET Core projects

People often integrate RabbitMQ with:

- MassTransit (very popular abstraction over RabbitMQ)
- EasyNetQ
- raw `RabbitMQ.Client`

Example with MassTransit is much cleaner:

```csharp
builder.Services.AddMassTransit(x =>
{
    x.UsingRabbitMq((context, cfg) =>
    {
        cfg.Host("localhost");
    });
});
```

MassTransit handles retries, consumers, serialization, etc., so you don’t have to manually wire everything.

---

```js
// import amqp from "amqplib";
import amqp, { Connection, Channel, ChannelModel } from "amqplib";

export type RabbitConnection = {
  createChannel: () => Promise<RabbitChannel>;
  on: (event: string, cb: (...args: any[]) => void) => void;
};

export type RabbitChannel = {
  assertQueue: (queue: string, options?: any) => Promise<any>;
  sendToQueue: (queue: string, content: Buffer, options?: any) => boolean;
  consume: (
    queue: string,
    handler: (msg: any) => void,
    options?: any,
  ) => Promise<any>;
  ack: (msg: any) => void;
  nack: (msg: any, allUpTo?: boolean, requeue?: boolean) => void;
};

// let connection: RabbitConnection;
// let channel: RabbitChannel;

// let connection!: Connection;
// let channel!: Channel;
let connection: ChannelModel;
let channel: Channel;

export const connectRabbitMQ = async () => {
  try {
    connection = await amqp.connect(
      process.env.RABBITMQ_URL || "amqp://localhost",
    );

    // let b = await a.createChannel();
    // channel = await a.createChannel();
    channel = await connection.createChannel();

    console.log("RabbitMQ connected");

    // connection.on("error", (err: any) => {
    connection.on("error", (err: any) => {
      console.error("RabbitMQ error:", err);
    });

    // connection.on("close", () => {
    connection.on("close", () => {
      console.error("RabbitMQ closed. Reconnecting...");
      setTimeout(connectRabbitMQ, 5000);
    });
  } catch (err) {
    console.error("RabbitMQ connection failed:", err);
    setTimeout(connectRabbitMQ, 5000);
  }
};

export const getChannel = () => {
  if (!channel) throw new Error("Channel not initialized");
  return channel;
};



import { getChannel } from "./connectRabbitMQ";

export const publishMessage = async (
  queue: string,
  message: Record<string, any>,
) => {
  const channel = getChannel();

  await channel.assertQueue(queue, { durable: true });

  try {
    const payload = Buffer.from(JSON.stringify(message));

    channel.sendToQueue(queue, payload, {
      persistent: true,
      contentType: "application/json",
    });

    console.log(`Event published: ${queue}`, message);
  } catch (error) {
    console.error("Failed to publish message:", error);
  }
};



import { getChannel } from "./connectRabbitMQ";

export const consumeMessage = async (
  queue: string,
  handler: (data: any) => Promise<void>,
) => {
  const channel = getChannel();

  await channel.assertQueue(queue, { durable: true });

  channel.prefetch(1);

  channel.consume(
    queue,
    async (msg) => {
      if (!msg) {
        console.error("Received null message");
        return;
      }

      let data: any;

      try {
        const raw = msg.content.toString();
        data = JSON.parse(raw);
      } catch (err) {
        console.error("Invalid JSON format:", err);

        channel.nack(msg, false, false);
        return;
      }

      try {
        await handler(data);

        channel.ack(msg);
      } catch (error) {
        console.error("Message processing failed:", error);

        channel.nack(msg, false, false);
      }
    },
    {
      noAck: false,
    },
  );

  console.log(`Listening to event: ${queue}`);
};

```
