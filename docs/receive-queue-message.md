# Receive Queue Message

1. Install package:

~~~
Azure.Messaging.ServiceBus
~~~

1. Code example of sending a message:

~~~ CSharp
await using var client = new ServiceBusClient(connectionString);

ServiceBusReceiver receiver = client.CreateReceiver(queueName);
// ServiceBusReceiver receiver = client.CreateReceiver(topicName, subscriptionName);

ServiceBusReceivedMessage receivedMessage = await receiver.ReceiveMessageAsync();

string messageContent = receivedMessage.Body.ToString();

Console.WriteLine(messageContent);

await receiver.CompleteMessageAsync(receivedMessage);
~~~
