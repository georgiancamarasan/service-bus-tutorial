# Process Queue Messages

1. Install package:

~~~
Azure.Messaging.ServiceBus
~~~

1. Code example of sending a message:

~~~ CSharp
await using var client = new ServiceBusClient(connectionString);

var processor = client.CreateProcessor(queueName, new ServiceBusProcessorOptions());
// var processor = client.CreateProcessor(topicName, subscriptionName, new ServiceBusProcessorOptions());


try
{
    processor.ProcessMessageAsync += MessageHandler;
    processor.ProcessErrorAsync += ErrorHandler;

    await processor.StartProcessingAsync();

    Console.WriteLine("Press anyy key to end processing");
    Console.ReadKey();

    Console.WriteLine("\nStopping the receiver...");
    await processor.StopProcessingAsync();
    Console.WriteLine("Stopped receiving messages");
}
finally
{
    await processor.DisposeAsync();
    await client.DisposeAsync();
}

static async Task MessageHandler(ProcessMessageEventArgs args)
{
    string messageContent = args.Message.Body.ToString();
    Console.WriteLine(messageContent);
    await args.CompleteMessageAsync(args.Message);
}

static Task ErrorHandler(ProcessErrorEventArgs args)
{
    Console.WriteLine(args.Exception.ToString());
    return Task.CompletedTask;
}
~~~
