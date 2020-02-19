---
title: .NET kullanarak Azure Event Hubs olay gönderme veya alma (en son)
description: Bu makalede, Azure Event Hubs en son Azure. Messaging. EventHubs paketini kullanarak olayları gönderen/alan bir .NET Core uygulaması oluşturmak için İzlenecek yol sunulmaktadır.
services: event-hubs
documentationcenter: na
author: spelluru
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/11/2020
ms.author: spelluru
ms.openlocfilehash: d7d697e3ea4b1b683275d53f6e407396f474b37b
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77462029"
---
# <a name="send-events-to-and-receive-events-from-azure-event-hubs---net-core-azuremessagingeventhubs"></a>Azure Event Hubs .NET Core 'a (Azure. Messaging. EventHubs) olay gönderme ve olayları alma 
Bu hızlı başlangıçta, **Azure. Messaging. EventHubs** .NET Core kitaplığı kullanılarak Olay Hub 'ından olay gönderme ve olayları alma işlemlerinin nasıl yapılacağı gösterilir. 

> [!IMPORTANT]
> Bu hızlı başlangıç, yeni **Azure. Messaging. EventHubs** kitaplığını kullanır. Eski **Microsoft. Azure. EventHubs** kitaplığını kullanan bir hızlı başlangıç için bkz. [Microsoft. Azure. eventhubs kitaplığını kullanarak olay gönderme ve alma](event-hubs-dotnet-standard-getstarted-send.md). 



## <a name="prerequisites"></a>Önkoşullar
Azure Event Hubs 'yi yeni kullanıyorsanız, bu hızlı başlangıcı uygulamadan önce [Event Hubs genel bakış](event-hubs-about.md) bölümüne bakın. 

Bu hızlı başlangıcı tamamlayabilmeniz için aşağıdaki önkoşullara sahip olmanız gerekir:

- **Microsoft Azure aboneliği**. Azure Event Hubs dahil olmak üzere Azure hizmetlerini kullanmak için bir aboneliğiniz olması gerekir.  Mevcut bir Azure hesabınız yoksa, [ücretsiz deneme](https://azure.microsoft.com/free/) için kaydolabilir veya [BIR hesap oluştururken](https://azure.microsoft.com)MSDN abonesi avantajlarınızı kullanabilirsiniz.
- **Microsoft Visual Studio 2019**. Azure Event Hubs istemci kitaplığı, 8,0 ' de C# tanıtılan yeni özelliklerden yararlanmaktadır.  Kitaplığı daha eski sürümleriyle kullanmaya devam edebilirsiniz C#, ancak bazı işlevleri kullanılamaz.  Bu özellikleri etkinleştirmek için, [.NET Core 3,0 ' i hedeflemelidir](/dotnet/standard/frameworks#how-to-specify-target-frameworks) veya kullanmak istediğiniz [dil sürümünü](/dotnet/csharp/language-reference/configure-language-version#override-a-default) (8,0 veya üzeri) belirtmeniz gerekir. Visual Studio kullanıyorsanız, Visual Studio 2019 ' den önceki sürümler 8,0 proje oluşturmak C# için gereken araçlarla uyumlu değildir. Ücretsiz topluluk sürümü de dahil olmak üzere Visual Studio 2019, [buradan](https://visualstudio.microsoft.com/vs/) indirilebilir
- **Event Hubs bir ad alanı ve bir olay hub 'ı oluşturun**. İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve Olay Hub 'ı oluşturmak için [Bu makaledeki](event-hubs-create.md)yordamı izleyin. Ardından, makalenin yönergelerini izleyerek **Event Hubs ad alanı için bağlantı dizesini** alın: [bağlantı dizesi al](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Bağlantı dizesini daha sonra bu hızlı başlangıçta kullanacaksınız.

## <a name="send-events"></a>Olayları gönderme 
Bu bölümde, olayları bir olay hub 'ına göndermek için bir .NET Core konsol uygulamasının nasıl oluşturulacağı gösterilmektedir. 

### <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

1. Visual Studio 2019 ' i başlatın. 
1. **Yeni proje oluştur**' u seçin. 
1. **Yeni proje oluştur** iletişim kutusunda, aşağıdaki adımları uygulayın: Bu iletişim kutusunu görmüyorsanız menüdeki **Dosya** ' yı seçin, **Yeni**' yi seçin ve ardından **Proje**' yi seçin. 
    1. Programlama **C#** dili için seçin.
    1. Uygulamanın türü için **konsol** ' ı seçin. 
    1. Sonuçlar listesinden **konsol uygulaması (.NET Core)** seçeneğini belirleyin. 
    1. Sonra **İleri**’yi seçin. 

        ![Yeni proje iletişim kutusu](./media/getstarted-dotnet-standard-send-v2/new-send-project.png)    
1. Proje adı için **Eventhubssender** yazın, çözüm adı olarak **Eventhubsquickstart** yazın ve ardından projeyi oluşturmak için **Tamam** ' ı seçin. 

    ![C#> Konsol uygulaması](./media/getstarted-dotnet-standard-send-v2/project-solution-names.png)

### <a name="add-the-event-hubs-nuget-package"></a>Event Hubs NuGet paketini ekleme

1. Menüdeki **araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** ' nu seçin. 
1. **Azure. Messaging. EventHubs** NuGet paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd
    Install-Package Azure.Messaging.EventHubs
    ```


### <a name="write-code-to-send-messages-to-the-event-hub"></a>Olay hub'ına ileti göndermek için kod yazma

1. Aşağıdaki `using` deyimlerini **program.cs** dosyasının en üstüne ekleyin:

    ```csharp
    using System.Text;
    using System.Threading.Tasks;
    using Azure.Messaging.EventHubs;
    using Azure.Messaging.EventHubs.Producer;
    ```

2. Event Hubs bağlantı dizesi ve Olay Hub 'ı adı için `Program` sınıfına sabitler ekleyin. Köşeli ayraçlar içindeki yer tutucuları, Olay Hub 'ı oluştururken aldığınız uygun değerlerle değiştirin. `{Event Hubs namespace connection string}` dizesinin olay hub'ı dizesi değil ad alanı düzeyi bağlantı dizesi olduğundan emin olun. 

    ```csharp
    private const string connectionString = "<EVENT HUBS NAMESPACE - CONNECTION STRING>";
    private const string eventHubName = "<EVENT HUB NAME>";
    ```

3. `Main` yöntemini aşağıdaki `async Main` yöntemiyle değiştirin. Ayrıntılar için bkz. kod açıklamaları. 

    ```csharp
        static async Task Main()
        {
            // Create a producer client that you can use to send events to an event hub
            await using (var producerClient = new EventHubProducerClient(connectionString, eventHubName))
            {
                // Create a batch of events 
                using EventDataBatch eventBatch = await producerClient.CreateBatchAsync();

                // Add events to the batch. An event is a represented by a collection of bytes and metadata. 
                eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("First event")));
                eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("Second event")));
                eventBatch.TryAdd(new EventData(Encoding.UTF8.GetBytes("Third event")));

                // Use the producer client to send the batch of events to the event hub
                await producerClient.SendAsync(eventBatch);
                Console.WriteLine("A batch of 3 events has been published.");
            }
        }
    ```
5. Projeyi derleyin ve hata olmadığından emin olun.
6. Programı çalıştırın ve onay iletisini bekleyin. 
7. Azure portal, Olay Hub 'ının iletileri aldığını doğrulayabilirsiniz. **Ölçümler** bölümünde **iletiler** görünümüne geçin. Grafiği güncelleştirmek için sayfayı yenileyin. İletilerin alındığını göstermesi birkaç saniye sürebilir. 

    [![Olay Hub 'ının iletileri aldığını doğrulama](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png)](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png#lightbox)

    > [!NOTE]
    > Daha bilgilendirici açıklamalarla tüm kaynak kodu için [GitHub 'daki bu dosyaya](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/samples/Sample03_PublishAnEventBatch.cs) bakın

## <a name="receive-events"></a>Olayları alma
Bu bölümde olay işlemcisi kullanarak bir olay hub 'ından iletiler alan bir .NET Core konsol uygulamasının nasıl yazılacağı gösterilmektedir. Olay işlemcisi, bu olay hub 'larından kalıcı denetim noktaları ve paralel alma işlemlerini yöneterek Olay Hub 'larından olay almayı basitleştirir. Olay işlemcisi, belirli bir olay hub 'ı ve bir tüketici grubuyla ilişkilendirilir. Olay Hub 'ında birden çok bölümden gelen olayları alır ve bunları, sağladığınız kodu kullanarak işlemeye yönelik bir işleyici temsilcisine geçirme. 


### <a name="create-an-azure-storage-and-a-blob-container"></a>Azure depolama ve BLOB kapsayıcısı oluşturma
Bu hızlı başlangıçta, Azure Storage 'ı denetim noktası deposu olarak kullanacaksınız. Azure depolama hesabı oluşturmak için bu adımları izleyin. 

1. [Azure depolama hesabı oluşturma](/azure/storage/common/storage-account-create?tabs=azure-portal)
2. [Blob kapsayıcısı oluşturma](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [Bağlantı dizesini depolama hesabına al](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string)

    Bağlantı dizesini ve kapsayıcı adını aklınızda edin. Bunları alma kodunda kullanacaksınız. 


### <a name="create-a-project-for-the-receiver"></a>Alıcı için proje oluşturma

1. Çözüm Gezgini penceresinde **Eventhubquickstart** çözümüne sağ tıklayın, **Ekle**' nin üzerine gelin ve **Yeni proje**' yi seçin. 
1. **Konsol uygulaması (.NET Core)** seçeneğini belirleyin ve **İleri**' yi seçin. 
1. **Proje adı**olarak **Eventhubsalıcı** girin ve **Oluştur**' u seçin. 

### <a name="add-the-event-hubs-nuget-package"></a>Event Hubs NuGet paketini ekleme

1. Menüdeki **araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** ' nu seçin. 
1. **Azure. Messaging. EventHubs** NuGet paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd
    Install-Package Azure.Messaging.EventHubs
    ```
1. **Azure. Messaging. EventHubs. Processor** NuGet paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd
    Install-Package Azure.Messaging.EventHubs.Processor
    ```    

### <a name="update-the-main-method"></a>Main metodunu güncelleştirme 

1. **Program.cs** dosyasının en üstüne aşağıdaki `using` deyimlerini ekleyin.

    ```csharp
    using System.Text;
    using System.Threading.Tasks;
    using Azure.Storage.Blobs;
    using Azure.Messaging.EventHubs;
    using Azure.Messaging.EventHubs.Consumer;
    using Azure.Messaging.EventHubs.Processor;
    ```
1. Event Hubs bağlantı dizesi ve Olay Hub 'ı adı için `Program` sınıfına sabitler ekleyin. Köşeli ayraçlar içindeki yer tutucuları, Olay Hub 'ı oluştururken aldığınız uygun değerlerle değiştirin. Köşeli ayraçlar içindeki yer tutucuları, Olay Hub 'ı ve depolama hesabını oluştururken aldığınız uygun değerlerle değiştirin (erişim anahtarları-birincil bağlantı dizesi). `{Event Hubs namespace connection string}` dizesinin olay hub'ı dizesi değil ad alanı düzeyi bağlantı dizesi olduğundan emin olun.

    ```csharp
        private const string ehubNamespaceConnectionString = "<EVENT HUBS NAMESPACE - CONNECTION STRING>";
        private const string eventHubName = "<EVENT HUB NAME>";
        private const string blobStorageConnectionString = "<AZURE STORAGE CONNECTION STRING>";
        private const string blobContainerName = "<BLOB CONTAINER NAME>";
    ```
3. `Main` yöntemini aşağıdaki `async Main` yöntemiyle değiştirin. Ayrıntılar için bkz. kod açıklamaları. 

    ```csharp
        static async Task Main()
        {
            // Read from the default consumer group: $Default
            string consumerGroup = EventHubConsumerClient.DefaultConsumerGroupName;

            // Create a blob container client that the event processor will use 
            BlobContainerClient storageClient = new BlobContainerClient(blobStorageConnectionString, blobContainerName);

            // Create an event processor client to process events in the event hub
            EventProcessorClient processor = new EventProcessorClient(storageClient, consumerGroup, ehubNamespaceConnectionString, eventHubName);

            // Register handlers for processing events and handling errors
            processor.ProcessEventAsync += ProcessEventHandler;
            processor.ProcessErrorAsync += ProcessErrorHandler;

            // Start the processing
            await processor.StartProcessingAsync();

            // Wait for 10 seconds for the events to be processed
            await Task.Delay(TimeSpan.FromSeconds(10));

            // Stop the processing
            await processor.StopProcessingAsync();
        }    
    ```
1. Şimdi, sınıfına aşağıdaki olay ve hata işleyicisi yöntemlerini ekleyin. 

    ```csharp
        static Task ProcessEventHandler(ProcessEventArgs eventArgs)
        { 
            // Write the body of the event to the console window
            Console.WriteLine("\tRecevied event: {0}", Encoding.UTF8.GetString(eventArgs.Data.Body.ToArray())); 
            return Task.CompletedTask; 
        }

        static Task ProcessErrorHandler(ProcessErrorEventArgs eventArgs)
        {
            // Write details about the error to the console window
            Console.WriteLine($"\tPartition '{ eventArgs.PartitionId}': an unhandled exception was encountered. This was not expected to happen.");
            Console.WriteLine(eventArgs.Exception.Message);
            return Task.CompletedTask;
        }    
    ```
1. Projeyi derleyin ve hata olmadığından emin olun.

    > [!NOTE]
    > Daha bilgilendirici açıklamalarla tüm kaynak kodu için [GitHub 'daki bu dosyaya](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs.Processor/samples/Sample01_HelloWorld.cs)bakın.
6. Alıcı uygulamasını çalıştırın. 
1. Olayın alındığını belirten bir ileti görmeniz gerekir. 

    ![Alınan olay](./media/getstarted-dotnet-standard-send-v2/event-received.png)

    Bu olaylar, gönderen programını çalıştırarak, daha önce olay hub 'ına gönderdiğiniz üç olaydır. 


## <a name="next-steps"></a>Sonraki adımlar
GitHub 'daki örneklere göz atın. 

- [GitHub 'daki Event Hubs örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs/samples)
- [GitHub 'daki olay işlemcisi örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs.Processor/samples)
- [Rol tabanlı erişim denetimi (RBAC) örneği](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Azure.Messaging.EventHubs/ManagedIdentityWebApp)
