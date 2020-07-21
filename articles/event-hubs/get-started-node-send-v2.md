---
title: JavaScript kullanarak Azure Event Hubs olay gönderme veya alma (en son)
description: Bu makalede, Azure Event Hubs en son Azure/Olay-Hub sürümü 5 paketini kullanarak olayları gönderen/alan bir JavaScript uygulaması oluşturmaya yönelik bir yol sunulmaktadır.
ms.topic: quickstart
ms.date: 06/23/2020
ms.openlocfilehash: 7ec97d07843a9844387ad1038c075ed55e304a2c
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86521896"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-by-using-javascript--azureevent-hubs-version-5"></a>JavaScript kullanarak Olay Hub 'larına olay gönderme veya olayları alma (Azure/Olay-Hub sürüm 5)
Bu hızlı başlangıçta, **Azure/Event-Hub 5** JavaScript paketini kullanarak Olay Hub 'ından olayları gönderme ve olayları alma işlemlerinin nasıl yapılacağı gösterilir. 

> [!IMPORTANT]
> Bu hızlı başlangıç, en son Azure/Olay-Hub sürümü 5 paketini kullanır. Eski Azure/Olay-Hub 'ları sürüm 2 paketini kullanan hızlı bir başlangıç için bkz. [Azure/Event-hub sürüm 2 kullanarak olay gönderme ve alma](event-hubs-node-get-started-send.md). 

## <a name="prerequisites"></a>Önkoşullar
Azure Event Hubs 'yi yeni kullanıyorsanız, bu hızlı başlangıcı uygulamadan önce [Event Hubs genel bakış](event-hubs-about.md) bölümüne bakın. 

Bu hızlı başlangıcı tamamlayabilmeniz için aşağıdaki önkoşullara sahip olmanız gerekir:

- **Microsoft Azure aboneliği**. Azure Event Hubs dahil olmak üzere Azure hizmetlerini kullanmak için bir aboneliğiniz olması gerekir.  Mevcut bir Azure hesabınız yoksa, [ücretsiz deneme](https://azure.microsoft.com/free/) için kaydolabilir veya [BIR hesap oluştururken](https://azure.microsoft.com)MSDN abonesi avantajlarınızı kullanabilirsiniz.
- Sürüm 8. x veya üzeri Node.js. En son [uzun süreli destek (LTS) sürümünü](https://nodejs.org)indirin.  
- Visual Studio Code (önerilir) veya diğer bir tümleşik geliştirme ortamı (IDE).  
- Etkin bir Event Hubs ad alanı ve Olay Hub 'ı. Bunları oluşturmak için aşağıdaki adımları uygulayın: 

   1. [Azure Portal](https://portal.azure.com), *Event Hubs*türünde bir ad alanı oluşturun ve ardından uygulamanızın Olay Hub 'ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. 
   1. Ad alanı ve Olay Hub 'ı oluşturmak için [hızlı başlangıç: Azure Portal kullanarak bir olay hub 'ı oluşturma](event-hubs-create.md)' daki yönergeleri izleyin.
   1. Bu hızlı başlangıçtaki yönergeleri izleyerek devam edin. 
   1. Olay Hub 'ı ad alanınız için bağlantı dizesini almak için [bağlantı dizesi al](event-hubs-get-connection-string.md#get-connection-string-from-the-portal)' daki yönergeleri izleyin. Daha sonra bu hızlı başlangıçta kullanılacak bağlantı dizesini kaydedin.
- **Event Hubs bir ad alanı ve bir olay hub 'ı oluşturun**. İlk adım, Event Hubs türünde bir ad alanı oluşturmak için [Azure Portal](https://portal.azure.com) ve uygulamanızın Olay Hub 'ı ile iletişim kurması için gereken yönetim kimlik bilgilerini elde etmek için kullanılır. Bir ad alanı ve Olay Hub 'ı oluşturmak için [Bu makaledeki](event-hubs-create.md)yordamı izleyin. Ardından, makalenin yönergelerini izleyerek **Event Hubs ad alanı için bağlantı dizesini** alın: [bağlantı dizesi al](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Bağlantı dizesini daha sonra bu hızlı başlangıçta kullanacaksınız.

### <a name="install-the-npm-package"></a>NPM paketini yükler
[Event Hubs Için düğüm paketi Yöneticisi (NPM) paketini](https://www.npmjs.com/package/@azure/event-hubs)yüklemek için, yolunda *NPM* olan bir komut istemi açın, dizini, örneklerinizi tutmak istediğiniz klasörle değiştirin ve ardından şu komutu çalıştırın:

```shell
npm install @azure/event-hubs
```

Alma tarafı için, iki paketi de yüklemeniz gerekir. Bu hızlı başlangıçta, programın zaten okuduğunuzdan olayları okuyamaması için kontrol noktalarını kalıcı hale getirmek üzere Azure Blob depolama alanını kullanırsınız. Alınan iletilerde meta veri denetim noktalarını bir Blobun düzenli aralıklarla gerçekleştirir. Bu yaklaşım, daha sonra kaldığınız yerden ileti almaya devam etmeyi kolaylaştırır.

Aşağıdaki komutları çalıştırın:

```shell
npm install @azure/storage-blob
```

```shell
npm install @azure/eventhubs-checkpointstore-blob
```

## <a name="send-events"></a>Olayları gönderme

Bu bölümde, olayları bir olay hub 'ına gönderen bir JavaScript uygulaması oluşturacaksınız.

1. [Visual Studio Code](https://code.visualstudio.com)gibi en sevdiğiniz düzenleyiciyi açın.
1. *send.js*adlı bir dosya oluşturun ve içine aşağıdaki kodu yapıştırın:

    ```javascript
    const { EventHubProducerClient } = require("@azure/event-hubs");

    const connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";
    const eventHubName = "EVENT HUB NAME";

    async function main() {

      // Create a producer client to send messages to the event hub.
      const producer = new EventHubProducerClient(connectionString, eventHubName);

      // Prepare a batch of three events.
      const batch = await producer.createBatch();
      batch.tryAdd({ body: "First event" });
      batch.tryAdd({ body: "Second event" });
      batch.tryAdd({ body: "Third event" });    

      // Send the batch to the event hub.
      await producer.sendBatch(batch);

      // Close the producer client.
      await producer.close();

      console.log("A batch of three events have been sent to the event hub");
    }

    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
1. Kodda, aşağıdakileri değiştirmek için gerçek değerleri kullanın:
    * `EVENT HUBS NAMESPACE CONNECTION STRING` 
    * `EVENT HUB NAME`
1. `node send.js`Bu dosyayı yürütmek için ' i çalıştırın. Bu komut, Olay Hub 'ınıza üç olay toplu işi gönderir.
1. Azure portal, Olay Hub 'ının iletileri aldığını doğrulayın. **Ölçümler** bölümünde **iletiler** görünümü ' ne geçin. Grafiği güncelleştirmek için sayfayı yenileyin. İletilerin alındığını göstermesi birkaç saniye sürebilir.

    [![Olay Hub 'ının iletileri aldığını doğrulama](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png)](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png#lightbox)

    > [!NOTE]
    > Ek bilgilendirme açıklamaları dahil olmak üzere, tüm kaynak kodu için [GitHub sendEvents.js sayfasına](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/event-hubs/samples/javascript/sendEvents.js)gidin.

Tebrikler! Olayları artık bir olay hub 'ına gönderdiniz.


## <a name="receive-events"></a>Olayları alma
Bu bölümde bir JavaScript uygulamasında bir Azure Blob depolama denetim noktası deposu kullanarak bir olay hub 'ından olay alırsınız. Bir Azure depolama blobunda düzenli aralıklarla alınan iletilerde meta veri denetim noktaları gerçekleştirir. Bu yaklaşım, daha sonra kaldığınız yerden ileti almaya devam etmeyi kolaylaştırır.

> [!NOTE]
> Azure Stack Hub üzerinde çalıştırıyorsanız, bu platform Azure 'da genel kullanıma sunulan farklı bir Depolama Blobu SDK sürümü destekleyebilir. Örneğin, [Azure Stack hub sürümü 2002 üzerinde](/azure-stack/user/event-hubs-overview)çalıştırıyorsanız, depolama hizmeti için en yüksek sürüm 2017-11-09 ' dir. Bu durumda, bu bölümdeki adımların yanı sıra Storage Service API sürüm 2017-11-09 ' i hedeflemek için de kod eklemeniz gerekecektir. Belirli bir depolama API sürümünün nasıl hedeflenecek hakkında bir örnek için bkz. GitHub 'da [JavaScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/javascript/receiveEventsWithApiSpecificStorage.js) ve [TypeScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/typescript/src/receiveEventsWithApiSpecificStorage.ts) örnekleri. Azure Stack hub 'ında desteklenen Azure depolama hizmeti sürümleri hakkında daha fazla bilgi için lütfen [Azure Stack hub depolama: farklar ve konular](/azure-stack/user/azure-stack-acs-differences)bölümüne bakın.


### <a name="create-an-azure-storage-account-and-a-blob-container"></a>Azure depolama hesabı ve BLOB kapsayıcısı oluşturma
Bir Azure depolama hesabı ve içinde bir blob kapsayıcısı oluşturmak için aşağıdaki işlemleri yapın:

1. [Azure depolama hesabı oluşturma](../storage/common/storage-account-create.md?tabs=azure-portal)  
2. [Depolama hesabında bir blob kapsayıcısı oluşturma](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)  
3. [Bağlantı dizesini depolama hesabına al](../storage/common/storage-configure-connection-string.md)

Alma kodunda daha sonra kullanmak için bağlantı dizesini ve kapsayıcı adını kaydettiğinizden emin olun.

### <a name="write-code-to-receive-events"></a>Olayları almak için kod yazma

1. [Visual Studio Code](https://code.visualstudio.com)gibi en sevdiğiniz düzenleyiciyi açın.
1. *receive.js*adlı bir dosya oluşturun ve içine aşağıdaki kodu yapıştırın:

    ```javascript
    const { EventHubConsumerClient } = require("@azure/event-hubs");
    const { ContainerClient } = require("@azure/storage-blob");    
    const { BlobCheckpointStore } = require("@azure/eventhubs-checkpointstore-blob");

    const connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";    
    const eventHubName = "EVENT HUB NAME";
    const consumerGroup = "$Default"; // name of the default consumer group
    const storageConnectionString = "AZURE STORAGE CONNECTION STRING";
    const containerName = "BLOB CONTAINER NAME";

    async function main() {
      // Create a blob container client and a blob checkpoint store using the client.
      const containerClient = new ContainerClient(storageConnectionString, containerName);
      const checkpointStore = new BlobCheckpointStore(containerClient);

      // Create a consumer client for the event hub by specifying the checkpoint store.
      const consumerClient = new EventHubConsumerClient(consumerGroup, connectionString, eventHubName, checkpointStore);

      // Subscribe to the events, and specify handlers for processing the events and errors.
      const subscription = consumerClient.subscribe({
          processEvents: async (events, context) => {
            for (const event of events) {
              console.log(`Received event: '${event.body}' from partition: '${context.partitionId}' and consumer group: '${context.consumerGroup}'`);
            }
            // Update the checkpoint.
            await context.updateCheckpoint(events[events.length - 1]);
          },

          processError: async (err, context) => {
            console.log(`Error : ${err}`);
          }
        }
      );

      // After 30 seconds, stop processing.
      await new Promise((resolve) => {
        setTimeout(async () => {
          await subscription.close();
          await consumerClient.close();
          resolve();
        }, 30000);
      });
    }

    main().catch((err) => {
      console.log("Error occurred: ", err);
    });    
    ```
1. Kodda, aşağıdaki değerleri değiştirmek için gerçek değerleri kullanın:
    - `EVENT HUBS NAMESPACE CONNECTION STRING`
    - `EVENT HUB NAME`
    - `AZURE STORAGE CONNECTION STRING`
    - `BLOB CONTAINER NAME`
1. `node receive.js`Bu dosyayı yürütmek için bir komut isteminde çalıştırın. Pencerede alınan olaylar hakkında iletiler görüntülenmelidir.

    > [!NOTE]
    > Ek bilgilendirme açıklamaları dahil olmak üzere, tüm kaynak kodu için [GitHub receiveEventsUsingCheckpointStore.js sayfasına](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/javascript/receiveEventsUsingCheckpointStore.js)gidin.

Tebrikler! Artık olay hub 'ınızdan olayları aldınız. Alıcı programı, Olay Hub 'ında varsayılan tüketici grubunun tüm bölümlerinden olayları alır.

## <a name="next-steps"></a>Sonraki adımlar
GitHub 'da bu örneklere göz atın:

- [JavaScript örnekleri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/javascript)
- [TypeScript örnekleri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/typescript)
