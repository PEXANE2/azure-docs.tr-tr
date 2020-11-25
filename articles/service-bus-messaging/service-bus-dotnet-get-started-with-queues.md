---
title: Azure Service Bus kuyruklarında çalışmaya başlama (Azure. Messaging. ServiceBus)
description: Bu öğreticide, bir Service Bus kuyruğundan ileti göndermek ve ileti almak için bir .NET Core C# uygulaması oluşturacaksınız.
ms.topic: quickstart
ms.tgt_pltfrm: dotnet
ms.date: 11/13/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 4335c1e81ead36d14ee1794fffbdd4cc1ff72a0a
ms.sourcegitcommit: 2e9643d74eb9e1357bc7c6b2bca14dbdd9faa436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96029617"
---
# <a name="send-messages-to-and-receive-messages-from-azure-service-bus-queues-net"></a>Azure Service Bus kuyruklarından ileti gönderme ve iletileri alma (.NET)
Bu öğreticide, **Azure. Messaging. ServiceBus** paketini kullanarak bir Service Bus kuyruğuna ileti göndermek ve ileti almak için bir .NET Core konsol uygulaması oluşturacaksınız. 

> [!Important]
> Bu hızlı başlangıç, yeni Azure. Messaging. ServiceBus paketini kullanır. Eski Microsoft. Azure. ServiceBus paketini kullanan bir hızlı başlangıç için bkz. [Microsoft. Azure. ServiceBus paketini kullanarak olay gönderme ve alma](service-bus-dotnet-get-started-with-queues-legacy.md).

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2019](https://www.visualstudio.com/vs)
- Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)için kaydolabilirsiniz.
- Birlikte çalışmak için bir kuyruğunuz yoksa, bir kuyruk oluşturmak için [Service Bus kuyruğu oluşturmak üzere Azure Portal kullanma](service-bus-quickstart-portal.md) adımlarını izleyin. Service Bus ad alanınız ve oluşturduğunuz **kuyruğun** adı için **bağlantı dizesini** aklınızda edin.

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Bu hızlı başlangıç bölümünde, kuyruğa ileti göndermek için bir C# .NET Core konsol uygulaması oluşturacaksınız.

### <a name="create-a-console-application"></a>Konsol uygulaması oluşturma
Visual Studio 'Yu başlatın ve C# için yeni bir **konsol uygulaması (.NET Core)** projesi oluşturun. 

### <a name="add-the-service-bus-nuget-package"></a>Service Bus NuGet paketi ekleme

1. Yeni oluşturulan projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
1. **Gözat**'ı seçin. **[Azure. Messaging. ServiceBus](https://www.nuget.org/packages/Azure.Messaging.ServiceBus/)** araması yapın ve seçin.
1. Yüklemeyi **gerçekleştirmek için yükleme** ' yi seçin ve ardından NuGet Paket Yöneticisi ' ni kapatın.

### <a name="add-code-to-send-messages-to-the-queue"></a>Kuyruğa ileti göndermek için kod ekleme

1. *Program.cs*' de, `using` Sınıf bildiriminden önce, ad alanı tanımının en üstüne aşağıdaki deyimleri ekleyin:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;
    
    using Azure.Messaging.ServiceBus;
    ```

1. `Program`Sınıfında, aşağıdaki değişkenleri bildirin:

    ```csharp
        static string connectionString = "<NAMESPACE CONNECTION STRING>";
        static string queueName = "<QUEUE NAME>";
    ```

    Ad alanı için Bağlantı dizenizi değişken olarak girin `ServiceBusConnectionString` . Sıra adınızı girin.

1. `Main()`Yöntemini aşağıdaki **Async** `Main` yöntemiyle değiştirin. `SendMessagesAsync()`Kuyruğa ileti göndermek için bir sonraki adımda ekleyeceğiniz yöntemi çağırır. 

    ```csharp
    public static async Task Main(string[] args)
    {    
        const int numberOfMessages = 10;
        queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

        Console.WriteLine("======================================================");
        Console.WriteLine("Press ENTER key to exit after sending all the messages.");
        Console.WriteLine("======================================================");

        // Send messages.
        await SendMessagesAsync(numberOfMessages);

        Console.ReadKey();

        await queueClient.CloseAsync();
    }
    ```
1. Doğrudan yönteminden sonra `Main()` , `SendMessagesAsync()` tarafından belirtilen ileti sayısını gönderme işini yapan aşağıdaki yöntemi ekleyin `numberOfMessagesToSend` (Şu anda 10 ' a ayarlanır):

    ```csharp
        static async Task SendMessageAsync()
        {
            // create a Service Bus client 
            await using (ServiceBusClient client = new ServiceBusClient(connectionString))
            {
                // create a sender for the queue 
                ServiceBusSender sender = client.CreateSender(queueName);

                // create a message that we can send
                ServiceBusMessage message = new ServiceBusMessage("Hello world!");

                // send the message
                await sender.SendMessageAsync(message);
                Console.WriteLine($"Sent a single message to the queue: {queueName}");
            }
        }
    ```
1. `CreateMessages`Sınıfına bir sıra (.NET kuyruğu) oluşturmak için adlı bir yöntem ekleyin `Program` . Genellikle, bu iletileri uygulamanızın farklı bölümlerinden alırsınız. Burada, örnek iletiler için bir kuyruk oluşturacağız.

    ```csharp
        static Queue<ServiceBusMessage> CreateMessages()
        {
            // create a queue containing the messages and return it to the caller
            Queue<ServiceBusMessage> messages = new Queue<ServiceBusMessage>();
            messages.Enqueue(new ServiceBusMessage("First message"));
            messages.Enqueue(new ServiceBusMessage("Second message"));
            messages.Enqueue(new ServiceBusMessage("Third message"));
            return messages;
        }
    ```
1. Sınıfına adlı bir yöntem ekleyin `SendMessageBatchAsync` `Program` ve aşağıdaki kodu ekleyin. Bu yöntem bir ileti kuyruğu alır ve bir veya daha fazla toplu işi Service Bus kuyruğuna gönderecek şekilde hazırlar. 

    ```csharp
        static async Task SendMessageBatchAsync()
        {
            // create a Service Bus client 
            await using (ServiceBusClient client = new ServiceBusClient(connectionString))
            {
                // create a sender for the queue 
                ServiceBusSender sender = client.CreateSender(queueName);

                // get the messages to be sent to the Service Bus queue
                Queue<ServiceBusMessage> messages = CreateMessages();

                // total number of messages to be sent to the Service Bus queue
                int messageCount = messages.Count;

                // while all messages are not sent to the Service Bus queue
                while (messages.Count > 0)
                {
                    // start a new batch 
                    using ServiceBusMessageBatch messageBatch = await sender.CreateMessageBatchAsync();

                    // add the first message to the batch
                    if (messageBatch.TryAddMessage(messages.Peek()))
                    {
                        // dequeue the message from the .NET queue once the message is added to the batch
                        messages.Dequeue();
                    }
                    else
                    {
                        // if the first message can't fit, then it is too large for the batch
                        throw new Exception($"Message {messageCount - messages.Count} is too large and cannot be sent.");
                    }

                    // add as many messages as possible to the current batch
                    while (messages.Count > 0 && messageBatch.TryAddMessage(messages.Peek()))
                    {
                        // dequeue the message from the .NET queue as it has been added to the batch
                        messages.Dequeue();
                    }
        
                    // now, send the batch
                    await sender.SendMessagesAsync(messageBatch);
        
                    // if there are any remaining messages in the .NET queue, the while loop repeats 
                }

                Console.WriteLine($"Sent a batch of {messageCount} messages to the topic: {queueName}");
            }
        }
    ```
1. `Main()`Yöntemini aşağıdaki **Async** `Main` yöntemiyle değiştirin. Tek bir ileti ve sıraya toplu bir ileti göndermek için gönderme yöntemlerini çağırır. 

    ```csharp
        static async Task Main()
        {
            // send a message to the queue
            await SendMessageAsync();

            // send a batch of messages to the queue
            await SendMessageBatchAsync();
        }
    ```
5. Uygulamayı çalıştırın. Aşağıdaki iletileri görmeniz gerekir. 

    ```console
    Sent a single message to the queue: myqueue
    Sent a batch of messages to the queue: myqueue
    ```       
1. Azure portal, aşağıdaki adımları izleyin:
    1. Service Bus ad alanına gidin. 
    1. **Genel bakış** sayfasında, alt orta bölmedeki kuyruğu seçin. 
    1. **Temel** bileşenler bölümündeki değerlere dikkat edin.

    :::image type="content" source="./media/service-bus-dotnet-get-started-with-queues/sent-messages-essentials.png" alt-text="Sayı ve boyut ile alınan iletiler" lightbox="./media/service-bus-dotnet-get-started-with-queues/sent-messages-essentials.png":::

    Aşağıdaki değerlere dikkat edin:
    - Kuyruğun **etkin ileti sayısı** değeri artık **4**' dir. Bu gönderen uygulamayı iletileri almadan her çalıştırdığınızda, bu değer 4 ' ü arttırır.
    - Kuyruğun geçerli boyutu, uygulamanın sıraya ileti eklemesi her seferinde **Essentials** 'daki **geçerli** değeri arttırır.
    - Alt **ölçümler** bölümündeki **iletiler** grafiğinde, kuyruk için dört gelen ileti olduğunu görebilirsiniz. 

## <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma
Bu bölümde, kuyruktan ileti almak için kod ekleyeceksiniz.

1. `Program`İletileri ve hataları işleyen sınıfa aşağıdaki yöntemleri ekleyin. 

    ```csharp
        // handle received messages
        static async Task MessageHandler(ProcessMessageEventArgs args)
        {
            string body = args.Message.Body.ToString();
            Console.WriteLine($"Received: {body}");

            // complete the message. messages is deleted from the queue. 
            await args.CompleteMessageAsync(args.Message);
        }

        // handle any errors when receiving messages
        static Task ErrorHandler(ProcessErrorEventArgs args)
        {
            Console.WriteLine(args.Exception.ToString());
            return Task.CompletedTask;
        }
    ```
1. Sınıfına adlı bir yöntem ekleyin `ReceiveMessagesAsync` `Program` ve ileti almak için aşağıdaki kodu ekleyin. 

    ```csharp
        static async Task ReceiveMessagesAsync()
        {
            await using (ServiceBusClient client = new ServiceBusClient(connectionString))
            {
                // create a processor that we can use to process the messages
                ServiceBusProcessor processor = client.CreateProcessor(queueName, new ServiceBusProcessorOptions());

                // add handler to process messages
                processor.ProcessMessageAsync += MessageHandler;

                // add handler to process any errors
                processor.ProcessErrorAsync += ErrorHandler;

                // start processing 
                await processor.StartProcessingAsync();

                Console.WriteLine("Wait for a minute and then press any key to end the processing");
                Console.ReadKey();

                // stop processing 
                Console.WriteLine("\nStopping the receiver...");
                await processor.StopProcessingAsync();
                Console.WriteLine("Stopped receiving messages");
            }
        }
    ```
1. Yönteminden yöntemine bir çağrı ekleyin `ReceiveMessagesAsync` `Main` . `SendMessagesAsync`Yalnızca ileti alma sınamasını yapmak istiyorsanız yöntemine açıklama ekleyin. Bunu yapmazsanız, kuyruğa başka dört ileti gönderildiğini görürsünüz. 

    ```csharp
        static async Task Main()
        {
            // send a message to the queue
            await SendMessageAsync();

            // send a batch of messages to the queue
            await SendMessageBatchAsync();

            // receive message from the queue
            await ReceiveMessagesAsync();
        }
    ```

## <a name="run-the-app"></a>Uygulamayı çalıştırma
Uygulamayı çalıştırın. Bir dakika bekleyip ileti almayı durdurmak için herhangi bir tuşa basın. Aşağıdaki çıktıyı görmeniz gerekir (anahtar için ara çubuğu). 

```console
Sent a single message to the queue: myqueue
Sent a batch of messages to the queue: myqueue
Wait for a minute and then press any key to end the processing
Received: Hello world!
Received: First message in the batch
Received: Second message in the batch
Received: Third message in the batch
Received: Hello world!
Received: First message in the batch
Received: Second message in the batch
Received: Third message in the batch

Stopping the receiver...
Stopped receiving messages
```

Portalı yeniden kontrol edin. 

- **Etkin ileti sayısı** ve **geçerli** değerler artık **0**' dır.
- Alt **ölçümler** bölümündeki **iletiler** grafiğinde, sıra için sekiz gelen ileti ve sekiz giden ileti olduğunu görebilirsiniz. 

    :::image type="content" source="./media/service-bus-dotnet-get-started-with-queues/queue-messages-size-final.png" alt-text="Etkin iletiler ve alındıktan sonra Boyutlandır" lightbox="./media/service-bus-dotnet-get-started-with-queues/queue-messages-size-final.png":::

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki belgelere ve örneklere bakın:

- [.NET için Azure Service Bus istemci kitaplığı-Benioku](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/servicebus/Azure.Messaging.ServiceBus)
- [GitHub’daki örnekler](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/servicebus/Azure.Messaging.ServiceBus/samples)
- [.NET API'si başvurusu](https://docs.microsoft.com/dotnet/api/azure.messaging.servicebus?view=azure-dotnet-preview&preserve-view=true)

