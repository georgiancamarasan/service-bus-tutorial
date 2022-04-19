# Send Queue Message

1. Install package:

~~~
Azure.Messaging.ServiceBus
~~~

1. Code example of sending a message:

~~~ CSharp
await using var client = new ServiceBusClient(connectionString);

ServiceBusSender sender = client.CreateSender(queueName);

ServiceBusMessage message = new ServiceBusMessage("Hello, Service Bus!");

await sender.SendMessageAsync(message);
~~~
