---
title: Node. js kullanarak Azure Event Hubs olay gönderme veya alma (en son)
description: Bu makalede, Azure Event Hubs en son Azure/Olay-Hub sürümü 5 paketini kullanarak olayları gönderen/alan bir Node. js uygulaması oluşturmaya yönelik bir yol sunulmaktadır.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/30/2020
ms.author: spelluru
ms.openlocfilehash: b523e4a7b463564cbfeb407c91b7bb05317f8166
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76906377"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-by-using-nodejs--azureevent-hubs-version-5"></a>Node. js kullanarak Olay Hub 'larına olay gönderme veya olayları alma (Azure/Olay-Hub sürüm 5)

Azure Event Hubs, saniyede milyonlarca olayı alabilen ve işleyesağlayan büyük bir veri akışı platformu ve olay alma hizmetidir. Olay Hub 'ları dağıtılmış yazılım ve cihazlar tarafından üretilen olayları, verileri veya Telemetriyi işleyebilir ve saklayabilir. Bir olay hub 'ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı veya toplu işlem/depolama bağdaştırıcısı kullanılarak dönüştürülebilir ve depolanabilir. Daha fazla bilgi için bkz. [Event Hubs genel bakış](event-hubs-about.md) ve [Event Hubs özellikleri](event-hubs-features.md).

Bu hızlı başlangıç, bir olay hub 'ından olay gönderebilen veya olayları alabilen Node. js uygulamalarının nasıl oluşturulacağını açıklar.

> [!IMPORTANT]
> Bu hızlı başlangıçta Azure Event Hubs JavaScript SDK 'sının 5. sürümü kullanılmaktadır. JavaScript SDK 'sının 2. sürümünü kullanan hızlı bir başlangıç için, [Bu makaleye](event-hubs-node-get-started-send.md)bakın. 

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlayabilmeniz için aşağıdaki önkoşullara sahip olmanız gerekir:

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).  
- Node. js sürüm 8. x veya üzeri. En son [uzun süreli destek (LTS) sürümünü](https://nodejs.org)indirin.  
- Visual Studio Code (önerilir) veya diğer bir tümleşik geliştirme ortamı (IDE).  
- Etkin bir Event Hubs ad alanı ve Olay Hub 'ı. Bunları oluşturmak için aşağıdaki adımları uygulayın: 

   1. [Azure Portal](https://portal.azure.com), *Event Hubs*türünde bir ad alanı oluşturun ve ardından uygulamanızın Olay Hub 'ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. 
   1. Ad alanı ve Olay Hub 'ı oluşturmak için [hızlı başlangıç: Azure Portal kullanarak bir olay hub 'ı oluşturma](event-hubs-create.md)' daki yönergeleri izleyin.
   1. Bu hızlı başlangıçtaki yönergeleri izleyerek devam edin. 
   1. Olay Hub 'ı ad alanınız için bağlantı dizesini almak için [bağlantı dizesi al](event-hubs-get-connection-string.md#get-connection-string-from-the-portal)' daki yönergeleri izleyin. Daha sonra bu hızlı başlangıçta kullanılacak bağlantı dizesini kaydedin.

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

Bu bölümde, Olay Hub 'ına olayları gönderen bir Node. js uygulaması oluşturacaksınız.

1. [Visual Studio Code](https://code.visualstudio.com)gibi en sevdiğiniz düzenleyiciyi açın.
1. *Send. js*adlı bir dosya oluşturun ve içine aşağıdaki kodu yapıştırın:

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
1. Bu dosyayı yürütmek için `node send.js` çalıştırın. Bu komut, Olay Hub 'ınıza üç olay toplu işi gönderir.
1. Azure portal, Olay Hub 'ının iletileri aldığını doğrulayın. **Ölçümler** bölümünde **iletiler** görünümü ' ne geçin. Grafiği güncelleştirmek için sayfayı yenileyin. İletilerin alındığını göstermesi birkaç saniye sürebilir.

    [![Olay Hub 'ının iletileri aldığını doğrulama](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png)](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png#lightbox)

    > [!NOTE]
    > Ek bilgilendirme açıklamaları dahil olmak üzere, tüm kaynak kodu için [GitHub sendEvents. js sayfasına](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/event-hubs/samples/javascript/sendEvents.js)gidin.

Tebrikler! Olayları artık bir olay hub 'ına gönderdiniz.


## <a name="receive-events"></a>Olayları alma
Bu bölümde, bir Node. js uygulamasında bir Azure Blob depolama denetim noktası deposu kullanarak bir olay hub 'ından olay alırsınız. Bir Azure depolama blobunda düzenli aralıklarla alınan iletilerde meta veri denetim noktaları gerçekleştirir. Bu yaklaşım, daha sonra kaldığınız yerden ileti almaya devam etmeyi kolaylaştırır.

### <a name="create-an-azure-storage-account-and-a-blob-container"></a>Azure depolama hesabı ve BLOB kapsayıcısı oluşturma
Bir Azure depolama hesabı ve içinde bir blob kapsayıcısı oluşturmak için aşağıdaki işlemleri yapın:

1. [Azure depolama hesabı oluşturma](../storage/common/storage-account-create.md?tabs=azure-portal)  
2. [Depolama hesabında bir blob kapsayıcısı oluşturma](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)  
3. [Bağlantı dizesini depolama hesabına al](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string)

Alma kodunda daha sonra kullanmak için bağlantı dizesini ve kapsayıcı adını kaydettiğinizden emin olun.

### <a name="write-code-to-receive-events"></a>Olayları almak için kod yazma

1. [Visual Studio Code](https://code.visualstudio.com)gibi en sevdiğiniz düzenleyiciyi açın.
1. *Receive. js*adlı bir dosya oluşturun ve içine aşağıdaki kodu yapıştırın:

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
1. Bu dosyayı yürütmek için bir komut isteminde `node receive.js` çalıştırın. Pencerede alınan olaylar hakkında iletiler görüntülenmelidir.

    > [!NOTE]
    > Ek bilgilendirme açıklamaları dahil olmak üzere, tüm kaynak kodu için [GitHub receiveEventsUsingCheckpointStore. js sayfasına](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/receiveEventsUsingCheckpointStore.js)gidin.

Tebrikler! Artık olay hub 'ınızdan olayları aldınız. Alıcı programı, Olay Hub 'ında varsayılan tüketici grubunun tüm bölümlerinden olayları alır.

## <a name="next-steps"></a>Sonraki adımlar
GitHub 'da bu örneklere göz atın:

- [JavaScript örnekleri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/javascript)
- [TypeScript örnekleri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/typescript)
