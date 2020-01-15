---
title: Node. js-Azure Event Hubs kullanarak olay gönderme ve alma
description: Bu makalede, Azure Event Hubs'dan olayları gönderen bir Node.js uygulaması oluşturmak için bir kılavuz sağlar.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 01/09/2020
ms.author: spelluru
ms.openlocfilehash: 9ea6febc781422a72ac6547338c8b21239331083
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75942522"
---
# <a name="send-events-to-or-receive-events-from-azure-event-hubs-using-nodejs"></a>Node. js kullanarak Azure Event Hubs olayları gönderme veya olayları alma 

Azure Event Hubs, saniye başına milyonlarca olayı alabilen ve işleyesağlayan büyük bir veri akışı platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı veya işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Event Hubs’a ayrıntılı bir genel bakış için bkz. [Event Hubs'a genel bakış](event-hubs-about.md) ve [Event Hubs özellikleri](event-hubs-features.md).

Bu öğreticide, Olay Hub 'ından olayları gönderme veya olayları alma için Node. js uygulamalarının nasıl oluşturulacağı açıklanmaktadır.

> [!IMPORTANT]
> Bu hızlı başlangıçta Azure Event Hubs Java betik SDK 'sının 5. sürümü kullanılmaktadır. Java betik SDK 'sının eski sürüm 2 ' yi kullanan hızlı bir başlangıç için, [Bu makaleye](event-hubs-node-get-started-send.md)bakın. SDK sürüm 2 ' yi kullanıyorsanız, kodunuzu en son sürüme geçirmeniz önerilir. Ayrıntılar için bkz. [Geçiş Kılavuzu](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/event-hubs/migrationguide.md).

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Etkin bir Azure hesabı. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- Node.js sürümü 8.x ve daha yüksek. En son LTS sürümü [ https://nodejs.org ](https://nodejs.org).
- Visual Studio Code (önerilir) veya diğer herhangi bir IDE
- **Event Hubs bir ad alanı ve bir olay hub 'ı oluşturun**. İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve Olay Hub 'ı oluşturmak için [Bu makaledeki](event-hubs-create.md)yordamı izleyin, ardından bu öğreticide aşağıdaki adımlarla devam edin. Ardından, makaledeki yönergeleri izleyerek Olay Hub 'ı ad alanı için bağlantı dizesini alın: [bağlantı dizesi al](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Bu öğreticide daha sonra'de bağlantı dizesini kullanın.


### <a name="install-npm-packages"></a>NPM paketlerini yükler
[Event Hubs için NPM paketini](https://www.npmjs.com/package/@azure/event-hubs)yüklemek için, yolunda `npm` olan bir komut istemi açın, dizini örneklerinizin olmasını istediğiniz klasörle değiştirin ve ardından bu komutu çalıştırın

```shell
npm install @azure/event-hubs
```

Alma tarafı için, iki paketi de yüklemeniz gerekir. Bu hızlı başlangıçta, programın zaten okuduğunuzu belirten olayları okuyamaması için Azure Blob depolama 'yı kalıcı denetim noktaları için kullanacaksınız. Alınan iletilerde meta verileri bir Blobun düzenli aralıklarla kontrol ediyor. Bu yaklaşım, daha sonraki bir zamanda kaldığı yerden gelen iletileri almaya devam etmek kolaylaştırır.

```shell
npm install @azure/storage-blob
```

```shell
npm install @azure/eventhubs-checkpointstore-blob
```

## <a name="send-events"></a>Olayları gönderme

Bu bölümde, Olay Hub 'ına olayları gönderen bir Node. js uygulamasının nasıl oluşturulacağı gösterilmektedir. 

1. [Visual Studio Code](https://code.visualstudio.com)gibi en sevdiğiniz düzenleyiciyi açın. 
2. `send.js` adlı bir dosya oluşturun ve içine aşağıdaki kodu yapıştırın. 

    ```javascript
    const { EventHubProducerClient } = require("@azure/event-hubs");
    
    const connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";
    const eventHubName = "EVENT HUB NAME";

    async function main() {
    
      // create a producer client to send messages to the event hub
      const producer = new EventHubProducerClient(connectionString, eventHubName);
    
      // prepare a batch of three events
      const batch = await producer.createBatch();
      batch.tryAdd({ body: "First event" });
      batch.tryAdd({ body: "Second event" });
      batch.tryAdd({ body: "Third event" });    
    
      // send the batch to the event hub
      await producer.sendBatch(batch);
    
      // close the producer client
      await producer.close();
    
      console.log("A batch of three events have been sent to the event hub");
    }

    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Koddaki **bağlantı dizesini** ve **Olay Hub 'ı ad** değerlerini değiştirmeyi unutmayın. 
5. Bu dosyayı yürütmek için `node send.js` komutunu çalıştırın. Bu, Olay Hub 'ınıza üç olay toplu işi gönderir
6. Azure portal, Olay Hub 'ının iletileri aldığını doğrulayabilirsiniz. **Ölçümler** bölümünde **iletiler** görünümüne geçin. Grafiği güncelleştirmek için sayfayı yenileyin. İletilerin alındığını göstermesi birkaç saniye sürebilir. 

    [![Olay Hub 'ının iletileri aldığını doğrulama](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png)](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png#lightbox)

    > [!NOTE]
    > Daha bilgilendirici açıklamalarla tüm kaynak kodu için [GitHub 'daki bu dosyaya](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/event-hubs/samples/javascript/sendEvents.js) bakın

Tebrikler! Olayları artık bir olay hub 'ına gönderdiniz. 


## <a name="receive-events"></a>Olayları alma 
Bu bölümde, bir Node. js uygulamasında bir Azure Blob kontrol noktası deposu kullanarak bir olay hub 'ından nasıl olay alınacağı gösterilmektedir. Bu kontrol noktaları meta verileri Azure depolama blobu, düzenli aralıklarla alınan iletiler. Bu yaklaşım, daha sonraki bir zamanda kaldığı yerden gelen iletileri almaya devam etmek kolaylaştırır.

### <a name="create-an-azure-storage-and-a-blob-container"></a>Azure depolama ve BLOB kapsayıcısı oluşturma
Bir Azure Depolama hesabını bir blob kapsayıcısı oluşturmak için bu adımları izleyin. 

1. [Azure depolama hesabı oluşturma](../storage/common/storage-quickstart-create-account.md?tabs=azure-portal)
2. [Blob kapsayıcısı oluşturma](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)
3. [Bağlantı dizesini depolama hesabına al](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string)

    Bağlantı dizesi ve kapsayıcı adı ' nı aklınızda edin. Bunları alma kodunda kullanacaksınız. 

### <a name="write-code-to-receive-events"></a>Olayları almak için kod yazma

1. [Visual Studio Code](https://code.visualstudio.com)gibi en sevdiğiniz düzenleyiciyi açın. 
2. `receive.js` adlı bir dosya oluşturun ve içine aşağıdaki kodu yapıştırın. Ayrıntılar için bkz. kod açıklamaları. 
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
      // create a blob container client and a blob checkpoint store using the client
      const containerClient = new ContainerClient(storageConnectionString, containerName);
      const checkpointStore = new BlobCheckpointStore(containerClient);
    
      // create a consumer client for the event hub by specifying the checkpoint store
      const consumerClient = new EventHubConsumerClient(consumerGroup, connectionString, eventHubName, checkpointStore);
    
      // subscribe for the events and specify handlers for processing the events and errors. 
      const subscription = consumerClient.subscribe({
          processEvents: async (events, context) => {
            for (const event of events) {
              console.log(`Received event: '${event.body}' from partition: '${context.partitionId}' and consumer group: '${context.consumerGroup}'`);
            }
            // update the checkpoint 
            await context.updateCheckpoint(events[events.length - 1]);
          },
    
          processError: async (err, context) => {
            console.log(`Error : ${err}`);
          }
        }
      );
        
      // after 30 seconds, stop processing
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
3. Kodda **aşağıdaki değerleri** belirtmeyi unutmayın: 
    - Event Hubs ad alanına bağlantı dizesi
    - Olay Hub 'ının adı
    - Azure depolama hesabına bağlantı dizesi
    - Blob kapsayıcısının adı
5. Sonra bu dosyayı yürütmek için komut isteminde komut `node receive.js` çalıştırın. Alınan olaylar hakkındaki iletileri pencerede görmeniz gerekir. 

    > [!NOTE]
    > Daha bilgilendirici açıklamalarla tüm kaynak kodu için [GitHub 'daki bu dosyaya](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/receiveEventsUsingCheckpointStore.js)bakın.

Tebrikler! Olay Hub 'ından olayları aldınız. Alıcı programı, Olay Hub 'ınızdaki varsayılan tüketici grubunun tüm bölümlerinden olayları alacaktır

## <a name="next-steps"></a>Sonraki adımlar
GitHub 'da bu örneklere göz atın:

- [JavaScript örnekleri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/javascript)
- [TypeScript örnekleri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/typescript)