---
title: JavaScript kullanarak Azure Etkinlik Hub'larından etkinlik gönderme veya alma (en son)
description: Bu makalede, en son azure/olay hub'ları sürüm 5 paketini kullanarak Azure Etkinlik Hub'larına/azure Etkinlik Hub'larına etkinlik gönderen/alan bir JavaScript uygulaması oluşturmak için bir iz bilgililik sağlanmaktadır.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/30/2020
ms.author: spelluru
ms.openlocfilehash: 063f2e1005db177420da7f81b799ab01fd212d7e
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478480"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-by-using-javascript--azureevent-hubs-version-5"></a>JavaScript kullanarak etkinlik merkezlerine etkinlik gönderme veya olay hub'ları (azure/olay hub'ları sürüm 5)
Bu hızlı başlangıç, **azure/event hub'ları sürüm 5** JavaScript paketini kullanarak bir etkinlik merkezine olayları nasıl göndereceğinizi ve bir olay merkezinden etkinlik alacağını gösterir. 

> [!IMPORTANT]
> Bu hızlı başlatma, en son azure/event hub'ları sürüm 5 paketini kullanır. Eski azure/event hub'ları sürüm 2 paketini kullanan hızlı bir başlangıç için [azure/event hub'ları sürüm 2'yi kullanarak olayları gönder ve al](event-hubs-node-get-started-send.md)bakın. 

## <a name="prerequisites"></a>Ön koşullar
Azure Etkinlik Hub'larında yeniyseniz, bu hızlı başlangıcı yapmadan önce [Etkinlik Hub'larına genel bakış](event-hubs-about.md) bakın. 

Bu hızlı başlangıcı tamamlamak için aşağıdaki ön koşullara ihtiyacınız vardır:

- **Microsoft Azure aboneliği.** Azure Etkinlik Hub'ları da dahil olmak üzere Azure hizmetlerini kullanmak için bir aboneliğe ihtiyacınız vardır.  Varolan bir Azure hesabınız yoksa, [ücretsiz](https://azure.microsoft.com/free/) deneme sürümüne kaydolabilir veya [bir hesap oluştururken](https://azure.microsoft.com)MSDN abone avantajlarınızı kullanabilirsiniz.
- Düğüm.js sürüm 8.x veya daha sonra. En son [uzun vadeli destek (LTS) sürümünü](https://nodejs.org)indirin.  
- Visual Studio Code (önerilir) veya diğer tümleşik geliştirme ortamı (IDE).  
- Etkin Olay Hub'ları ad alanı ve olay hub'ı. Bunları oluşturmak için aşağıdaki adımları yapın: 

   1. Azure [portalında,](https://portal.azure.com) *Olay Hub'ları*türünden bir ad alanı oluşturun ve ardından uygulamanızın olay hub'ı ile iletişim kurmak için ihtiyaç duyduğu yönetim kimlik bilgilerini edinin. 
   1. Ad alanı ve etkinlik hub'ını oluşturmak için Quickstart' taki yönergeleri [izleyin: Azure portalını kullanarak bir etkinlik merkezi oluşturun.](event-hubs-create.md)
   1. Bu hızlı başlangıçtaki talimatları izleyerek devam edin. 
   1. Olay Hub ad alanınız için bağlantı dizesini almak için [Bağlantı Dizesi Al'daki](event-hubs-get-connection-string.md#get-connection-string-from-the-portal)yönergeleri izleyin. Daha sonra bu hızlı başlatmada kullanmak üzere bağlantı dizesini kaydedin.
- **Olay Hub'ları ad alanı ve olay hub'ı oluşturun.** İlk adım, Olay Hub türünden bir ad alanı oluşturmak ve uygulamanızın etkinlik merkeziyle iletişim kurmak için ihtiyaç duyduğu yönetim kimlik bilgilerini elde etmek için [Azure portalını](https://portal.azure.com) kullanmaktır. Ad alanı ve olay hub'ı oluşturmak için [bu makaledeki](event-hubs-create.md)yordamı izleyin. Ardından, makaledeki yönergeleri izleyerek **Olay Hub'ları ad alanının bağlantı dizesini** alın: [Bağlantı dizesini alın.](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) Bağlantı dizesini daha sonra bu hızlı başlatmada kullanırsınız.

### <a name="install-the-npm-package"></a>npm paketini yükleyin
[Olay Hub'ları için Düğüm Paket Yöneticisi (npm) paketini](https://www.npmjs.com/package/@azure/event-hubs)yüklemek için, yolunda *npm* olan bir komut istemi açın, dizini örneklerinizi tutmak istediğiniz klasöre değiştirin ve sonra bu komutu çalıştırın:

```shell
npm install @azure/event-hubs
```

Alıcı taraf için iki paket daha yüklemeniz gerekir. Bu hızlı başlangıçta, programın zaten okuduğu olayları okumaması için denetim noktalarını sürdürmek için Azure Blob depolama alanını kullanırsınız. Alınan iletilerde meta veri denetim noktalarını düzenli aralıklarla bir blob içinde gerçekleştirir. Bu yaklaşım, daha sonra kaldığınız yerden ileti almaya devam etmeyi kolaylaştırır.

Aşağıdaki komutları çalıştırın:

```shell
npm install @azure/storage-blob
```

```shell
npm install @azure/eventhubs-checkpointstore-blob
```

## <a name="send-events"></a>Olayları gönderme

Bu bölümde, olayları bir olay hub'ına gönderen bir JavaScript uygulaması oluşturursunuz.

1. [Visual Studio Code](https://code.visualstudio.com)gibi en sevdiğiniz düzenleyiciyi açın.
1. *send.js*adlı bir dosya oluşturun ve aşağıdaki kodu yapıştırın:

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
1. Bu `node send.js` dosyayı çalıştırmak için çalıştırın. Bu komut, olay merkezinize üç olaydan oluşan bir toplu iş gönderir.
1. Azure portalında, olay merkezinin iletileri aldığını doğrulayın. **Ölçümler** bölümünde, **İletiler** görünümüne geçin. Grafiği güncellemek için sayfayı yenileyin. İletilerin alındığını göstermesi birkaç saniye sürebilir.

    [![Olay merkezinin iletileri aldığını doğrulama](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png)](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png#lightbox)

    > [!NOTE]
    > Ek bilgi açıklamaları da dahil olmak üzere tam kaynak kodu için [GitHub sendEvents.js sayfasına](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/event-hubs/samples/javascript/sendEvents.js)gidin.

Tebrikler! Şimdi olayları bir etkinlik merkezine gönderdiniz.


## <a name="receive-events"></a>Olayları alma
Bu bölümde, javascript uygulamasında bir Azure Blob depolama denetim noktası deposu kullanarak bir etkinlik merkezinden olay hub'ından olay merkeziyle etkinlik alırsınız. Alınan iletilerde meta veri denetim noktalarını bir Azure Depolama örneğinde düzenli aralıklarla gerçekleştirir. Bu yaklaşım, daha sonra kaldığınız yerden ileti almaya devam etmeyi kolaylaştırır.

> [!NOTE]
> Azure Stack Hub'da çalışıyorsanız, bu platform Depolama Blob SDK'nın Azure'da bulunanlardan farklı bir sürümünü destekleyebilir. Örneğin, Azure Yığını [Hub sürümü 2002'de](https://docs.microsoft.com/azure-stack/user/event-hubs-overview)çalışıyorsanız, Depolama hizmeti için kullanılabilir en yüksek sürüm 2017-11-09 sürümüdür. Bu durumda, bu bölümdeki aşağıdaki adımların yanı sıra, Depolama hizmeti API sürümünü 2017-11-09'u hedeflemek için kod eklemeniz gerekir. Belirli bir Depolama API sürümünü niçin hedefleneceksiniz hakkında bir örnek için, GitHub'da [JavaScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/javascript/receiveEventsWithApiSpecificStorage.js) ve [TypeScript](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/typescript/src/receiveEventsWithApiSpecificStorage.ts) örneklerine bakın. Azure Yığını Hub'ında desteklenen Azure Depolama hizmeti sürümleri hakkında daha fazla bilgi için lütfen [Azure Yığın Hub depolama alanına bakın: Farklılıklar ve dikkat edilmesi gerekenler.](https://docs.microsoft.com/azure-stack/user/azure-stack-acs-differences)


### <a name="create-an-azure-storage-account-and-a-blob-container"></a>Azure depolama hesabı ve blob kapsayıcısı oluşturma
Bir Azure depolama hesabı ve içinde bir blob kapsayıcısı oluşturmak için aşağıdaki işlemleri yapın:

1. [Azure depolama hesabı oluşturma](../storage/common/storage-account-create.md?tabs=azure-portal)  
2. [Depolama hesabında bir blob kapsayıcısı oluşturma](../storage/blobs/storage-quickstart-blobs-portal.md#create-a-container)  
3. [Bağlantı dizesini depolama hesabına alma](../storage/common/storage-configure-connection-string.md?#view-and-copy-a-connection-string)

Daha sonra kullanım için bağlantı dizesini ve kapsayıcı adını alma koduna kaydettiğinden emin olun.

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
1. Bu `node receive.js` dosyayı çalıştırmak için bir komut istemi çalıştırın. Pencere, alınan olaylarla ilgili iletileri görüntülemelidir.

    > [!NOTE]
    > Ek bilgi açıklamaları da dahil olmak üzere tam kaynak kodu için [GitHub receiveEventsUsingCheckpointStore.js sayfasına](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/eventhubs-checkpointstore-blob/samples/receiveEventsUsingCheckpointStore.js)gidin.

Tebrikler! Artık etkinlik merkezinizden etkinlikler aldınız. Alıcı programı, olay merkezindeki varsayılan tüketici grubunun tüm bölümlerinden olaylar alır.

## <a name="next-steps"></a>Sonraki adımlar
GitHub'daki şu örneklere göz atın:

- [JavaScript örnekleri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/javascript)
- [TypeScript örnekleri](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples/typescript)
