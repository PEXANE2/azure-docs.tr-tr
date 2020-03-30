---
title: Hizmet Veri Servisi kuyruğu oluşturmak için Azure portalını kullanın
description: Bu hızlı başlangıçta, Azure portalını kullanarak Hizmet Veri Yolu kuyruğu oluşturmayı öğrenirsiniz. Ardından, sıraya ileti göndermek ve sıradan ileti almak için örnek bir istemci uygulaması kullanırsınız.
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: 0c88db652efe54a497af094160fb426be7c43d16
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240649"
---
# <a name="quickstart-use-azure-portal-to-create-a-service-bus-queue"></a>Hızlı başlangıç: Hizmet Veri Servisi kuyruğu oluşturmak için Azure portalını kullanma
Bu hızlı başlangıçta, mesajlaşma ad alanı ve o ad alanı içinde bir kuyruk oluşturmak ve söz konusu ad alanında yetkilendirme kimlik bilgilerini almak için [Azure portalı][Azure portal] kullanarak bir Service Bus kuyruğuna nasıl ileti gönderileceği ve Service Bus kuyruğundan nasıl ileti alınacağı açıklanmaktadır. Daha sonra yordam, [.NET Standard kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) kullanılarak bu kuyruktan nasıl ileti gönderilip alınacağını gösterir.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için şunları yüklediğinizden emin olun:

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][] oluşturabilirsiniz.
- [Visual Studio 2017 Güncelleştirme 3 (sürüm 15.3, 26730.01)](https://www.visualstudio.com/vs) veya sonraki sürümler. Bir sıraya ileti gönderen ve ileti alan bir örnek oluşturmak için Visual Studio'u kullanırsınız. Örnek PowerShell kullanarak oluşturduğunuz sırayı test etmektir. 
- [NET Core SDK](https://www.microsoft.com/net/download/windows), sürüm 2.0 veya sonraki sürümler.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="send-and-receive-messages"></a>İleti alma ve gönderme

> [!NOTE]
> İleti göndermek ve almak için bu bölümde kullanılan örnek bir .NET örneğidir. Örneklerin diğer programlama dillerini kullanarak ileti göndermesi/alması için [Servis Veri Servisi örneklerine](service-bus-samples.md)bakın. 
> 
> Çeşitli programlama dillerini kullanarak ileti gönderme/alma yla ilgili adım adım talimatlar için aşağıdaki hızlı başlangıçlara bakın:
> - [.NET](service-bus-dotnet-get-started-with-queues.md)
> - [Java](service-bus-java-how-to-use-queues.md)
> - [Azure/servis-veri-servis paketi kullanarak düğüm.js](service-bus-nodejs-how-to-use-queues-new-package.md)
> - [Azure-sb paketini kullanarak düğüm.js](service-bus-nodejs-how-to-use-queues.md)
> - [PHP](service-bus-php-how-to-use-queues.md)
> - [Python](service-bus-python-how-to-use-queues.md)
> - [Ruby](service-bus-ruby-how-to-use-queues.md)

Ad alanı ve kuyruğun sağlanmasının ve gerekli kimlik bilgilerine sahip olmanızın ardından ileti gönderip almaya hazırsınız demektir. [Bu GitHub örnek klasöründeki](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/TopicFilters) kodu inceleyebilirsiniz.

Kodu çalıştırmak için aşağıdakileri yapın:

1. [Service Bus GitHub deposunu](https://github.com/Azure/azure-service-bus/) aşağıdaki komutu çalıştırarak kopyalayın:

   ```
   git clone https://github.com/Azure/azure-service-bus.git
   ```
3. Örnek `azure-service-bus\samples\DotNet\GettingStarted\BasicSendReceiveQuickStart\BasicSendReceiveQuickStart` klasörüne gidin.
4. Yönetim kimlik bilgilerini alma bölümünde aldığınız bağlantı dizesi ve kuyruk adını kopyalayın.
5.  Komut isteminde aşağıdaki komutu yazın:

    ```
    dotnet build
    ```
6.  `bin\Debug\netcoreapp2.0` klasörüne gidin.
7.  Programı çalıştırmak için aşağıdaki komutu yazın. `myConnectionString` yerine daha önce aldığınız değeri ve `myQueueName` yerine oluşturduğunuz kuyruğun adını koymayı unutmayın:

    ```shell
    dotnet BasicSendReceiveQuickStart.dll -ConnectionString "myConnectionString" -QueueName "myQueueName"
    ``` 
8. Kuyruğa 10 ileti gönderildiğini ve ardından bunların kuyruktan alındığını gözlemleyin:

   ![program çıktısı](./media/service-bus-quickstart-portal/dotnet.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubu, ad alanı ve kuyruğu kaldırmak için portalı kullanabilirsiniz.

## <a name="understand-the-sample-code"></a>Örnek kodu anlama

Bu bölümde, örnek kodun yaptıkları hakkında daha fazla ayrıntı bulunmaktadır. 

### <a name="get-connection-string-and-queue"></a>Bağlantı dizesini ve kuyruğu alma

Bağlantı dizesi ve sıra `Main()` adı yönteme komut satırı bağımsız değişkenleri olarak geçirilir. `Main()`, bu değerleri tutmak için iki dize değişkeni bildirir:

```csharp
static void Main(string[] args)
{
    string ServiceBusConnectionString = "";
    string QueueName = "";

    for (int i = 0; i < args.Length; i++)
    {
        var p = new Program();
        if (args[i] == "-ConnectionString")
        {
            Console.WriteLine($"ConnectionString: {args[i+1]}");
            ServiceBusConnectionString = args[i + 1]; 
        }
        else if(args[i] == "-QueueName")
        {
            Console.WriteLine($"QueueName: {args[i+1]}");
            QueueName = args[i + 1];
        }                
    }

    if (ServiceBusConnectionString != "" && QueueName != "")
        MainAsync(ServiceBusConnectionString, QueueName).GetAwaiter().GetResult();
    else
    {
        Console.WriteLine("Specify -Connectionstring and -QueueName to execute the example.");
        Console.ReadKey();
    }                            
}
```
 
`Main()` yöntemi daha sonra zaman uyumsuz ileti döngüsünü (`MainAsync()`) başlatır.

### <a name="message-loop"></a>İleti döngüsü

MainAsync() yöntemi komut satırı bağımsız değişkenleri ile bir sıra istemcisi `RegisterOnMessageHandlerAndReceiveMessages()`oluşturur, adlı bir alıcı ileti işleyicisi çağırır ve ileti kümesi gönderir:

```csharp
static async Task MainAsync(string ServiceBusConnectionString, string QueueName)
{
    const int numberOfMessages = 10;
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);

    Console.WriteLine("======================================================");
    Console.WriteLine("Press any key to exit after receiving all the messages.");
    Console.WriteLine("======================================================");

    // Register QueueClient's MessageHandler and receive messages in a loop
    RegisterOnMessageHandlerAndReceiveMessages();

    // Send Messages
    await SendMessagesAsync(numberOfMessages);

    Console.ReadKey();

    await queueClient.CloseAsync();
}
```

`RegisterOnMessageHandlerAndReceiveMessages()` yöntemi, basit bir şekilde birkaç ileti işleyicisi seçeneği ayarlar, daha sonra kuyruk istemcisinin `RegisterMessageHandler()` yöntemini çağırarak alma işlemini başlatır:

```csharp
static void RegisterOnMessageHandlerAndReceiveMessages()
{
    // Configure the MessageHandler Options in terms of exception handling, number of concurrent messages to deliver etc.
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        // Maximum number of Concurrent calls to the callback `ProcessMessagesAsync`, set to 1 for simplicity.
        // Set it according to how many messages the application wants to process in parallel.
        MaxConcurrentCalls = 1,

        // Indicates whether MessagePump should automatically complete the messages after returning from User Callback.
        // False below indicates the Complete will be handled by the User Callback as in `ProcessMessagesAsync` below.
        AutoComplete = false
    };

    // Register the function that will process messages
    queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
} 
```

### <a name="send-messages"></a>İleti gönderme

İleti oluşturma ve gönderme işlemleri, `SendMessagesAsync()` yönteminde gerçekleşir:

```csharp
static async Task SendMessagesAsync(int numberOfMessagesToSend)
{
    try
    {
        for (var i = 0; i < numberOfMessagesToSend; i++)
        {
            // Create a new message to send to the queue
            string messageBody = $"Message {i}";
            var message = new Message(Encoding.UTF8.GetBytes(messageBody));

            // Write the body of the message to the console
            Console.WriteLine($"Sending message: {messageBody}");

            // Send the message to the queue
            await queueClient.SendAsync(message);
        }
    }
    catch (Exception exception)
    {
        Console.WriteLine($"{DateTime.Now} :: Exception: {exception.Message}");
    }
}
```

### <a name="process-messages"></a>İletileri işleme

`ProcessMessagesAsync()` yöntemi, iletilerin alınmasını onaylar, işler ve tamamlar:

```csharp
static async Task ProcessMessagesAsync(Message message, CancellationToken token)
{
    // Process the message
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");

    // Complete the message so that it is not received again.
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
}
```
> [!NOTE]
> [Servis](https://github.com/paolosalvatori/ServiceBusExplorer/)Veri Servisi Explorer ile Servis Veri Servisi kaynaklarını yönetebilirsiniz. Service Bus Explorer, kullanıcıların bir Service Bus ad alanına bağlanmasına ve ileti varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç, alma/dışa aktarma işlevselliği veya konuyu, kuyrukları, abonelikleri, geçiş hizmetlerini, bildirim hub'larını ve olay hub'larını test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir Service Bus alan adı ve bir kuyruktan ileti gönderip almak için gereken diğer kaynakları oluşturdunuz. İleti göndermek ve almak için kod yazma hakkında daha fazla bilgi edinmek **için, İleti gönder ve iletileri al** bölümündeki öğreticilere devam edin. 

> [!div class="nextstepaction"]
> [İleti alma ve gönderme](service-bus-dotnet-get-started-with-queues.md)


[ücretsiz bir hesap]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[Azure portal]: https://portal.azure.com/

[service-bus-flow]: ./media/service-bus-quickstart-portal/service-bus-flow.png
