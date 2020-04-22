---
title: .NET (en son) kullanarak Azure Etkinlik Hub'larından etkinlik gönderme veya alma
description: Bu makale, en son Azure.Messaging.EventHubs paketini kullanarak Azure Etkinlik Hub'larına/Azure Etkinlik Hub'larına etkinlik gönderen/alan bir .NET Core uygulaması oluşturmak için bir iz bilgililik sağlar.
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
ms.date: 04/20/2020
ms.author: spelluru
ms.openlocfilehash: bc8ec6cf4845eb100af5dcd80101f17102d2b7ac
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81682152"
---
# <a name="send-events-to-and-receive-events-from-azure-event-hubs---net-core-azuremessagingeventhubs"></a>Azure Etkinlik Hub'larından etkinlik gönderme ve alma - .NET Core (Azure.Messaging.EventHubs) 
Bu hızlı başlangıç, **Azure.Messaging.EventHubs** .NET Core kitaplığını kullanarak bir etkinlik merkezine olayları nasıl göndereceğinizi ve bir olay merkezinden nasıl alınarak alınabildiğini gösterir. 

> [!IMPORTANT]
> Bu hızlı başlangıç, yeni **Azure.Messaging.EventHubs** kitaplığını kullanır. Eski **Microsoft.Azure.EventHubs** kitaplığını kullanan hızlı bir başlangıç için, [Microsoft.Azure.EventHubs kitaplığını kullanarak etkinlik gönder ve al'a](event-hubs-dotnet-standard-getstarted-send.md)bakın. 



## <a name="prerequisites"></a>Ön koşullar
Azure Etkinlik Hub'larında yeniyseniz, bu hızlı başlangıcı yapmadan önce [Etkinlik Hub'larına genel bakış](event-hubs-about.md) konusubakın. 

Bu hızlı başlangıcı tamamlamak için aşağıdaki ön koşullara ihtiyacınız vardır:

- **Microsoft Azure aboneliği.** Azure Etkinlik Hub'ları da dahil olmak üzere Azure hizmetlerini kullanmak için bir aboneliğe ihtiyacınız vardır.  Varolan bir Azure hesabınız yoksa, [ücretsiz](https://azure.microsoft.com/free/) deneme sürümüne kaydolabilir veya [bir hesap oluştururken](https://azure.microsoft.com)MSDN abone avantajlarınızı kullanabilirsiniz.
- **Microsoft Visual Studio 2019**. Azure Etkinlik Hub'ları istemci kitaplığı, C# 8.0'da tanıtılan yeni özelliklerden yararlanıyor.  Kitaplığı hala C#'ın eski sürümleriyle kullanabilirsiniz, ancak bazı işlevleri kullanılamaz.  Bu özellikleri etkinleştirmek için [.NET Core 3.0'ı hedeflemeniz](/dotnet/standard/frameworks#how-to-specify-target-frameworks) veya kullanmak istediğiniz dil sürümünü (8.0 veya üzeri) [belirtmeniz](/dotnet/csharp/language-reference/configure-language-version#override-a-default) gerekir. Visual Studio kullanıyorsanız, Visual Studio 2019'dan önceki sürümler C# 8.0 projeleri oluşturmak için gereken araçlarla uyumlu değildir. Visual Studio 2019, ücretsiz Topluluk sürümü de dahil olmak üzere, [buradan](https://visualstudio.microsoft.com/vs/) indirebilirsiniz
- **Olay Hub'ları ad alanı ve olay hub'ı oluşturun.** İlk adım, Olay Hub türünden bir ad alanı oluşturmak ve uygulamanızın etkinlik merkeziyle iletişim kurmak için ihtiyaç duyduğu yönetim kimlik bilgilerini elde etmek için [Azure portalını](https://portal.azure.com) kullanmaktır. Ad alanı ve olay hub'ı oluşturmak için [bu makaledeki](event-hubs-create.md)yordamı izleyin. Ardından, makaledeki yönergeleri izleyerek **Olay Hub'ları ad alanının bağlantı dizesini** alın: [Bağlantı dizesini alın.](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) Bağlantı dizesini daha sonra bu hızlı başlatmada kullanırsınız.

## <a name="send-events"></a>Olayları gönderme 
Bu bölümde, olayları bir olay merkezine göndermek için bir .NET Core konsol uygulaması nasıl oluşturulabileceğiniz gösterilmektedir. 

### <a name="create-a-console-application"></a>Konsol uygulaması oluşturma

1. Visual Studio 2019'u başlatın. 
1. **Yeni bir proje oluştur'u**seçin. 
1. Yeni **bir proje** iletişim kutusu oluştur'da aşağıdaki adımları yapın: Bu iletişim kutusunu görmüyorsanız, menüde **Dosya'yı** seçin, **Yeni'yi**seçin ve ardından **Project'i**seçin. 
    1. Programlama dili için **C#** seçeneğini belirleyin.
    1. Uygulama türü için **Konsol'u** seçin. 
    1. Sonuç listesinden **Konsol Uygulaması'nı (.NET Core)** seçin. 
    1. Ardından **İleri'yi**seçin. 

        ![Yeni proje iletişim kutusu](./media/getstarted-dotnet-standard-send-v2/new-send-project.png)    
1. Proje adı için **EventHubsSender'ı,** çözüm adı için **EventHubsQuickStart'ı** girin ve projeyi oluşturmak için **Tamam'ı** seçin. 

    ![C# > Konsol uygulaması](./media/getstarted-dotnet-standard-send-v2/project-solution-names.png)

### <a name="add-the-event-hubs-nuget-package"></a>Event Hubs NuGet paketini ekleme

1. Menüden **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu'nu** seçin. 
1. **Azure.Messaging.EventHubs** NuGet paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd
    Install-Package Azure.Messaging.EventHubs
    ```


### <a name="write-code-to-send-messages-to-the-event-hub"></a>Olay hub'ına ileti göndermek için kod yazma

1. `using` **Program.cs** dosyasının üst bölümüne aşağıdaki ifadeleri ekleyin:

    ```csharp
    using System.Text;
    using System.Threading.Tasks;
    using Azure.Messaging.EventHubs;
    using Azure.Messaging.EventHubs.Producer;
    ```

2. Olay Hub'ları `Program` bağlantı dizesi ve olay hub adı için sınıfa sabitler ekleyin. Yer tutucuları parantez içinde olay merkezini oluştururken sahip olduğunuz uygun değerlerle değiştirin. `{Event Hubs namespace connection string}` dizesinin olay hub'ı dizesi değil ad alanı düzeyi bağlantı dizesi olduğundan emin olun. 

    ```csharp
    private const string connectionString = "<EVENT HUBS NAMESPACE - CONNECTION STRING>";
    private const string eventHubName = "<EVENT HUB NAME>";
    ```

3. Yöntemi `Main` aşağıdaki `async Main` yöntemle değiştirin. Ayrıntılar için kod açıklamalarına bakın. 

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
5. Projeyi oluşturun ve hata olmadığından emin olun.
6. Programı çalıştırın ve onay iletisini bekleyin. 
7. Azure portalında, olay merkezinin iletileri aldığını doğrulayabilirsiniz. **Ölçümler** bölümünde **İletiler** görünümüne geçin. Grafiği güncellemek için sayfayı yenileyin. İletilerin alındığını göstermesi birkaç saniye sürebilir. 

    [![Olay merkezinin iletileri aldığını doğrulama](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png)](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png#lightbox)

    > [!NOTE]
    > Daha fazla bilgi içeren tam kaynak kodu için [GitHub'daki bu dosyaya](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/samples/Sample03_PublishAnEventBatch.cs) bakın

## <a name="receive-events"></a>Olayları alma
Bu bölümde, olay işlemcisi kullanarak bir olay merkezinden ileti alan bir .NET Core konsol uygulamasının nasıl yazılalış ları gösterilmektedir. Olay işlemcisi, kalıcı denetim noktalarını ve bu olay merkezlerinden paralel almaları yöneterek olay merkezlerinden olay hub'larından olay alma almayı kolaylaştırır. Olay işlemcisi, belirli bir olay Hub'ı ve tüketici grubuyla ilişkilidir. Olay merkezindeki birden çok bölümden olayları alır ve bunları sağladığınız kodu kullanarak işlemek üzere işleyici temsilcisine aktarıyor. 


> [!NOTE]
> Azure Stack Hub'da çalışıyorsanız, bu platform Depolama Blob SDK'nın Azure'da bulunanlardan farklı bir sürümünü destekleyebilir. Örneğin, Azure Yığını [Hub sürümü 2002'de](https://docs.microsoft.com/azure-stack/user/event-hubs-overview)çalışıyorsanız, Depolama hizmeti için kullanılabilir en yüksek sürüm 2017-11-09 sürümüdür. Bu durumda, bu bölümdeki aşağıdaki adımların yanı sıra, Depolama hizmeti API sürümünü 2017-11-09'u hedeflemek için kod eklemeniz gerekir. Belirli bir Depolama API sürümünün nasıl hedeflenilene ilişkin bir örnek için, [bu örneğe GitHub'da](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs.Processor/samples/Sample10_RunningWithDifferentStorageVersion.cs)bakın. Azure Yığını Hub'ında desteklenen Azure Depolama hizmeti sürümleri hakkında daha fazla bilgi için lütfen [Azure Yığın Hub depolama alanına bakın: Farklılıklar ve dikkat edilmesi gerekenler.](https://docs.microsoft.com/azure-stack/user/azure-stack-acs-differences)

### <a name="create-an-azure-storage-and-a-blob-container"></a>Azure Depolama ve blob kapsayıcısı oluşturma
Bu hızlı başlangıçta, denetim noktası deposu olarak Azure Depolama'yı kullanırsınız. Bir Azure Depolama hesabı oluşturmak için aşağıdaki adımları izleyin. 

1. [Azure Depolama hesabı oluşturma](/azure/storage/common/storage-account-create?tabs=azure-portal)
2. [Blob kapsayıcısı oluşturma](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [Bağlantı dizesini depolama hesabına alma](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string)

    Bağlantı dizesini ve kapsayıcı adını not edin. Bunları alma kodunda kullanırsınız. 


### <a name="create-a-project-for-the-receiver"></a>Alıcı için bir proje oluşturma

1. Çözüm Gezgini penceresinde **EventHubQuickStart** çözümünü sağ tıklatın, **Ekle'ye**işaret edin ve **Yeni Proje'yi**seçin. 
1. **Konsol Uygulaması (.NET Core)** seçeneğini seçin ve **İleri'yi**seçin. 
1. **Proje adı**için **EventHubsReceiver** girin ve **Oluştur'u**seçin. 

### <a name="add-the-event-hubs-nuget-package"></a>Event Hubs NuGet paketini ekleme

1. Menüden **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu'nu** seçin. 
1. **Azure.Messaging.EventHubs** NuGet paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd
    Install-Package Azure.Messaging.EventHubs
    ```
1. **Azure.Messaging.EventHubs.Processor** NuGet paketini yüklemek için aşağıdaki komutu çalıştırın:

    ```cmd
    Install-Package Azure.Messaging.EventHubs.Processor
    ```    

### <a name="update-the-main-method"></a>Ana yöntemi güncelleştirme 

1. aşağıdaki `using` ifadeleri **Program.cs** dosyasının üst kısmında ekleyin.

    ```csharp
    using System.Text;
    using System.Threading.Tasks;
    using Azure.Storage.Blobs;
    using Azure.Messaging.EventHubs;
    using Azure.Messaging.EventHubs.Consumer;
    using Azure.Messaging.EventHubs.Processor;
    ```
1. Olay Hub'ları `Program` bağlantı dizesi ve olay hub adı için sınıfa sabitler ekleyin. Yer tutucuları parantez içinde olay merkezini oluştururken sahip olduğunuz uygun değerlerle değiştirin. Yer tutucuları, olay hub'ını ve depolama hesabını oluştururken sahip olduğunuz uygun değerlerle (erişim tuşları - birincil bağlantı dizesi) değiştirin. `{Event Hubs namespace connection string}` dizesinin olay hub'ı dizesi değil ad alanı düzeyi bağlantı dizesi olduğundan emin olun.

    ```csharp
        private const string ehubNamespaceConnectionString = "<EVENT HUBS NAMESPACE - CONNECTION STRING>";
        private const string eventHubName = "<EVENT HUB NAME>";
        private const string blobStorageConnectionString = "<AZURE STORAGE CONNECTION STRING>";
        private const string blobContainerName = "<BLOB CONTAINER NAME>";
    ```
3. Yöntemi `Main` aşağıdaki `async Main` yöntemle değiştirin. Ayrıntılar için kod açıklamalarına bakın. 

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
1. Şimdi, sınıfa aşağıdaki olay ve hata işleyici yöntemlerini ekleyin. 

    ```csharp
        static async Task ProcessEventHandler(ProcessEventArgs eventArgs)
        {
            // Write the body of the event to the console window
            Console.WriteLine("\tRecevied event: {0}", Encoding.UTF8.GetString(eventArgs.Data.Body.ToArray()));

            // Update checkpoint in the blob storage so that the app receives only new events the next time it's run
            await eventArgs.UpdateCheckpointAsync(eventArgs.CancellationToken);
        }

        static Task ProcessErrorHandler(ProcessErrorEventArgs eventArgs)
        {
            // Write details about the error to the console window
            Console.WriteLine($"\tPartition '{ eventArgs.PartitionId}': an unhandled exception was encountered. This was not expected to happen.");
            Console.WriteLine(eventArgs.Exception.Message);
            return Task.CompletedTask;
        }    
    ```
1. Projeyi oluşturun ve hata olmadığından emin olun.

    > [!NOTE]
    > Daha fazla bilgi içeren tam kaynak kodu için [GitHub'daki bu dosyaya](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs.Processor/samples/Sample01_HelloWorld.cs)bakın.
6. Alıcı uygulamasını çalıştırın. 
1. Olayın alındığına dair bir ileti görmeniz gerekir. 

    ![Alınan olay](./media/getstarted-dotnet-standard-send-v2/event-received.png)

    Bu olaylar, gönderen programı çalıştırarak daha önce etkinlik merkezine gönderdiğiniz üç olaydır. 


## <a name="next-steps"></a>Sonraki adımlar
GitHub'daki örneklere göz atın. 

- [GitHub'da Etkinlik Hub'ları örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs/samples)
- [GitHub'da olay işlemci örnekleri](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/eventhub/Azure.Messaging.EventHubs.Processor/samples)
- [Rol tabanlı erişim kontrolü (RBAC) örneği](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Azure.Messaging.EventHubs/ManagedIdentityWebApp)
