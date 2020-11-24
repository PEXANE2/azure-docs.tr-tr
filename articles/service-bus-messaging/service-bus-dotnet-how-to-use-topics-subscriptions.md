---
title: Azure-mesajlaşma-ServiceBus kullanarak Azure Service Bus konularına ileti gönderme
description: Bu hızlı başlangıçta, Azure-mesajlaşma-ServiceBus paketini kullanarak Azure Service Bus konularına nasıl ileti göndereceğiniz gösterilmektedir.
ms.topic: quickstart
ms.tgt_pltfrm: dotnet
ms.date: 11/13/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 5ddfca1186025f4118013815af7bbf7f500951ce
ms.sourcegitcommit: 6a770fc07237f02bea8cc463f3d8cc5c246d7c65
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95809293"
---
# <a name="send-messages-to-an-azure-service-bus-topic-and-receive-messages-from-subscriptions-to-the-topic-net"></a>Azure Service Bus konuya ileti gönderme ve aboneliklerden konuya ileti alma (.NET)
Bu öğreticide, Service Bus konuya ileti gönderen ve konunun aboneliğinden iletileri alan bir .NET Core konsol uygulamasının nasıl oluşturulacağı gösterilmektedir. 

> [!Important]
> Bu hızlı başlangıç, yeni **Azure. Messaging. ServiceBus** paketini kullanır. Eski Microsoft. Azure. ServiceBus paketini kullanan bir hızlı başlangıç için bkz. [Microsoft. Azure. ServiceBus paketini kullanarak Ileti gönderme ve alma](service-bus-dotnet-how-to-use-topics-subscriptions-legacy.md).

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2019](https://www.visualstudio.com/vs)
- Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [Visual Studio veya MSDN abonesi avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)için kaydolabilirsiniz.
- Hızlı başlangıçtaki adımları izleyin [: konuya bir Service Bus konu ve abonelik oluşturmak için Azure Portal kullanın](service-bus-quickstart-topics-subscriptions-portal.md). Bağlantı dizesi, konu adı ve abonelik adı ' nı aklınızda edin. Bu hızlı başlangıç için yalnızca bir abonelik kullanacaksınız. 

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme
Bu bölümde, Visual Studio 'da bir .NET Core konsol uygulaması oluşturacaksınız, oluşturduğunuz konuya ileti göndermek için kod eklersiniz. 

### <a name="create-a-console-application"></a>Konsol uygulaması oluşturma
Visual Studio 'Yu başlatın ve C# için yeni bir **konsol uygulaması (.NET Core)** projesi oluşturun. 

### <a name="add-the-service-bus-nuget-package"></a>Service Bus NuGet paketi ekleme

1. Yeni oluşturulan projeye sağ tıklayın ve **NuGet Paketlerini Yönet**’i seçin.
1. **Gözat**'ı seçin. **[Azure. Messaging. ServiceBus](https://www.nuget.org/packages/Azure.Messaging.ServiceBus/)** araması yapın ve seçin.
1. Yüklemeyi **gerçekleştirmek için yükleme** ' yi seçin ve ardından NuGet Paket Yöneticisi ' ni kapatın.

### <a name="add-code-to-send-messages-to-the-topic"></a>Konuya ileti göndermek için kod ekleme 

1. Program.cs’de, ad alanı tanımının üst kısmına, sınıf bildiriminden önce aşağıdaki `using` deyimlerini ekleyin:
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using Azure.Messaging.ServiceBus;
    ```
1. `Program`Sınıfında, aşağıdaki değişkenleri bildirin:

    ```csharp
        static string connectionString = "<NAMESPACE CONNECTION STRING>";
        static string topicName = "<TOPIC NAME>";
        static string subscriptionName = "<SUBSCRIPTION NAME>";
    ```

    Aşağıdaki değerleri değiştirin:
    - `<NAMESPACE CONNECTION STRING>` Service Bus ad alanına bağlantı dizesi ile
    - `<TOPIC NAME>` konusunun adı ile
    - `<SUBSCRIPTION NAME>` Abonelik adı ile
2. Konuya bir ileti gönderen adlı adlı bir yöntem ekleyin `SendMessageToTopicAsync` . 

    ```csharp
        static async Task SendMessageToTopicAsync()
        {
            // create a Service Bus client 
            await using (ServiceBusClient client = new ServiceBusClient(connectionString))
            {
                // create a sender for the topic
                ServiceBusSender sender = client.CreateSender(topicName);
                await sender.SendMessageAsync(new ServiceBusMessage("Hello, World!"));
                Console.WriteLine($"Sent a single message to the topic: {topicName}");
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
1. Sınıfına adlı bir yöntem ekleyin `SendMessageBatchAsync` `Program` ve aşağıdaki kodu ekleyin. Bu yöntem bir ileti kuyruğu alır ve bir veya daha fazla toplu işi Service Bus konusuna gönderecek şekilde hazırlar. 

    ```csharp
        static async Task SendMessageBatchToTopicAsync()
        {
            // create a Service Bus client 
            await using (ServiceBusClient client = new ServiceBusClient(connectionString))
            {

                // create a sender for the topic 
                ServiceBusSender sender = client.CreateSender(topicName);

                // get the messages to be sent to the Service Bus topic
                Queue<ServiceBusMessage> messages = CreateMessages();

                // total number of messages to be sent to the Service Bus topic
                int messageCount = messages.Count;

                // while all messages are not sent to the Service Bus topic
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

                Console.WriteLine($"Sent a batch of {messageCount} messages to the topic: {topicName}");
            }
        }
    ```
1. `Main()`Yöntemini aşağıdaki **Async** `Main` yöntemiyle değiştirin. Tek bir ileti ve konuya bir toplu ileti göndermek için gönderme yöntemlerini çağırır.  

    ```csharp
        static async Task Main()
        {
            // send a message to the topic
            await SendMessageToTopicAsync();

            // send a batch of messages to the topic
            await SendMessageBatchToTopicAsync();
        }
    ```
5. Uygulamayı çalıştırın. Aşağıdaki çıkışı görmeniz gerekir:

    ```console
    Sent a single message to the topic: mytopic
    Sent a batch of 3 messages to the topic: mytopic
    ```
1. Azure portal, aşağıdaki adımları izleyin:
    1. Service Bus ad alanına gidin. 
    1. **Genel bakış** sayfasında, alt orta bölmedeki **konular** sekmesine geçin ve Service Bus konusunu seçin. Aşağıdaki örnekte, bu `mytopic` .
    
        :::image type="content" source="./media/service-bus-dotnet-how-to-use-topics-subscriptions/select-topic.png" alt-text="Konu seçin":::
    1. **Service Bus konu** sayfasında, alt **ölçümler** bölümündeki **iletiler** grafiğinde, konu için dört gelen ileti olduğunu görebilirsiniz. Değeri görmüyorsanız, birkaç dakika bekleyin ve güncelleştirilmiş grafiği görmek için sayfayı yenileyin. 

        :::image type="content" source="./media/service-bus-dotnet-how-to-use-topics-subscriptions/sent-messages-essentials.png" alt-text="Konuya gönderilen iletiler" lightbox="./media/service-bus-dotnet-how-to-use-topics-subscriptions/sent-messages-essentials.png":::
    4. Alt bölmedeki aboneliği seçin. Aşağıdaki örnekte **S1**' dir. **Service Bus abonelik** sayfasında **etkin ileti sayısını** **4** olarak görürsünüz. Abonelik, konuya gönderdiğiniz dört iletiyi aldı, ancak henüz hiç alıcı çekmedi. 
    
        :::image type="content" source="./media/service-bus-dotnet-how-to-use-topics-subscriptions/subscription-page.png" alt-text="Abonelikte alınan iletiler" lightbox="./media/service-bus-dotnet-how-to-use-topics-subscriptions/subscription-page.png":::
    

## <a name="receive-messages-from-a-subscription"></a>Bir abonelikten ileti alma

1. `Program`İletileri ve hataları işleyen sınıfa aşağıdaki yöntemleri ekleyin. 

    ```csharp
        static async Task MessageHandler(ProcessMessageEventArgs args)
        {
            string body = args.Message.Body.ToString();
            Console.WriteLine($"Received: {body} from subscription: {subscriptionName}");

            // complete the message. messages is deleted from the queue. 
            await args.CompleteMessageAsync(args.Message);
        }

        static Task ErrorHandler(ProcessErrorEventArgs args)
        {
            Console.WriteLine(args.Exception.ToString());
            return Task.CompletedTask;
        }
    ```
1. Sınıfına aşağıdaki yöntemi ekleyin `ReceiveMessagesFromSubscriptionAsync` `Program` .

    ```csharp
        static async Task ReceiveMessagesFromSubscriptionAsync()
        {
            await using (ServiceBusClient client = new ServiceBusClient(connectionString))
            {
                // create a processor that we can use to process the messages
                ServiceBusProcessor processor = client.CreateProcessor(topicName, subscriptionName, new ServiceBusProcessorOptions());

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
1. Yöntemine bir çağrı ekleyin `ReceiveMessagesFromSubscriptionAsync` `Main` . `SendMessagesToTopicAsync`Yalnızca ileti alma sınamasını yapmak istiyorsanız yöntemine açıklama ekleyin. Bunu yapmazsanız, konuya başka dört ileti gönderildiğini görürsünüz. 

    ```csharp
        static async Task Main()
        {
            // send a message to the topic
            await SendMessageToTopicAsync();

            // send a batch of messages to the topic
            await SendMessageBatchToTopicAsync();

            // receive messages from the subscription
            await ReceiveMessagesFromSubscriptionAsync();
        }
    ```
## <a name="run-the-app"></a>Uygulamayı çalıştırma
Uygulamayı çalıştırın. Bir dakika bekleyip ileti almayı durdurmak için herhangi bir tuşa basın. Aşağıdaki çıktıyı görmeniz gerekir (anahtar için ara çubuğu). 

```console
Sent a single message to the topic: mytopic
Sent a batch of 3 messages to the topic: mytopic
Wait for a minute and then press any key to end the processing
Received: Hello, World! from subscription: mysub
Received: First message from subscription: mysub
Received: Second message from subscription: mysub
Received: Third message from subscription: mysub
Received: Hello, World! from subscription: mysub
Received: First message from subscription: mysub
Received: Second message from subscription: mysub
Received: Third message from subscription: mysub

Stopping the receiver...
Stopped receiving messages
```

Portalı yeniden kontrol edin. 

- **Service Bus konu** sayfasındaki **iletiler** grafiğinde, sekiz gelen ileti ve sekiz giden ileti görürsünüz. Bu numaraları görmüyorsanız, birkaç dakika bekleyin ve güncelleştirilmiş grafiği görmek için sayfayı yenileyin. 

    :::image type="content" source="./media/service-bus-dotnet-how-to-use-topics-subscriptions/messages-size-final.png" alt-text="Gönderilen ve alınan iletiler" lightbox="./media/service-bus-dotnet-how-to-use-topics-subscriptions/messages-size-final.png":::
- **Service Bus abonelik** sayfasında **etkin ileti sayısını** sıfır olarak görürsünüz. Bunun nedeni, alıcı bu abonelikten ileti almış ve iletileri tamamlamıştır. 

    :::image type="content" source="./media/service-bus-dotnet-how-to-use-topics-subscriptions/subscription-page-final.png" alt-text="Son sırada abonelikte etkin ileti sayısı" lightbox="./media/service-bus-dotnet-how-to-use-topics-subscriptions/subscription-page-final.png":::
    


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki belgelere ve örneklere bakın:

- [.NET için Azure Service Bus istemci kitaplığı-Benioku](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/servicebus/Azure.Messaging.ServiceBus)
- [GitHub’daki örnekler](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/servicebus/Azure.Messaging.ServiceBus/samples)
- [.NET API'si başvurusu](https://docs.microsoft.com/dotnet/api/azure.messaging.servicebus?view=azure-dotnet-preview&preserve-view=true)

