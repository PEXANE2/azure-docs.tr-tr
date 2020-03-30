---
title: JavaScript kullanarak Azure Etkinlik Hub'larından etkinlik gönderme veya alma (eski)
description: Bu makalede, eski azure/olay hub'ları sürüm 2 paketini kullanarak Azure Etkinlik Hub'larına/azure Etkinlik Hub'larına etkinlik gönderen/alan bir JavaScript uygulaması oluşturmak için bir iz bilgililik sağlanmaktadır.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/15/2020
ms.author: spelluru
ms.openlocfilehash: 0a4b76bd1febca864cab6060fbdbd96dd0061cff
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77162625"
---
# <a name="quickstart-send-events-to-or-receive-events-from-azure-event-hubs-using-javascript-azureevent-hubs-version-2"></a>Hızlı başlangıç: JavaScript kullanarak Azure Etkinlik Hub'larına@azure/event-hubs etkinlik gönderme veya alma (sürüm 2)
Bu hızlı başlangıç, azure/event hub'ları sürüm 2 JavaScript paketini kullanarak olayları bir etkinlik merkezine göndermek ve bir olay merkezinden almak için JavaScript uygulamalarının nasıl oluşturuluğa gösteriş gösterir. 

> [!WARNING]
> Bu hızlı başlatma, eski azure/event hub'ları sürüm 2 paketini kullanır. Paketin en son sürüm **5'ini** kullanan hızlı bir başlangıç için [azure/eventhubs sürüm 5'i kullanarak etkinlik gönder ve al'](get-started-node-send-v2.md)a bakın. Uygulamanızı eski paketi kullanmaktan yenisine taşımak için [azure/eventhubs sürüm 1'den sürüm 5'e geçiş kılavuzuna](https://github.com/Azure/azure-sdk-for-js/blob/master/sdk/eventhub/event-hubs/migrationguide.md)bakın. 


## <a name="prerequisites"></a>Ön koşullar

Azure Etkinlik Hub'larında yeniyseniz, bu hızlı başlangıcı yapmadan önce [Etkinlik Hub'larına genel bakış](event-hubs-about.md) bakın. 

Bu hızlı başlangıcı tamamlamak için aşağıdaki ön koşullara ihtiyacınız vardır:

- **Microsoft Azure aboneliği.** Azure Etkinlik Hub'ları da dahil olmak üzere Azure hizmetlerini kullanmak için bir aboneliğe ihtiyacınız vardır.  Varolan bir Azure hesabınız yoksa, [ücretsiz](https://azure.microsoft.com/free/) deneme sürümüne kaydolabilir veya [bir hesap oluştururken](https://azure.microsoft.com)MSDN abone avantajlarınızı kullanabilirsiniz.
- Düğüm.js sürüm 8.x ve üstü. En son LTS [https://nodejs.org](https://nodejs.org)sürümünü buradan indirin.
- Visual Studio Code (önerilir) veya başka bir IDE
- **Olay Hub'ları ad alanı ve olay hub'ı oluşturun.** İlk adım, Olay Hub türünden bir ad alanı oluşturmak ve uygulamanızın etkinlik merkeziyle iletişim kurmak için ihtiyaç duyduğu yönetim kimlik bilgilerini elde etmek için [Azure portalını](https://portal.azure.com) kullanmaktır. Bir ad alanı ve olay hub'ı oluşturmak için, [bu makaledeki](event-hubs-create.md)yordamı izleyin , ardından bu öğreticide aşağıdaki adımları uygulayın. Daha sonra, makaledeki yönergeleri izleyerek olay göbeği ad alanı için bağlantı dizesini alın: [Bağlantı dizesini alın.](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) Bağlantı dizesini daha sonra bu öğreticide kullanırsınız.


### <a name="install-npm-package"></a>npm paketini yükleyin
[Olay Hub'ları için npm paketini](https://www.npmjs.com/package/@azure/event-hubs/v/2.1.0)yüklemek için, yoluna çıkan bir komut istemi `npm` açın, dizini örneklerinizi almak istediğiniz klasöre değiştirin ve bu komutu çalıştırın

```shell
npm install @azure/event-hubs@2
```

[Olay İşlemcisi Ana Bilgisayar için npm paketini](https://www.npmjs.com/package/@azure/event-processor-host)yüklemek için aşağıdaki komutu çalıştırmak yerine

```shell
npm install @azure/event-processor-host
```

## <a name="send-events"></a>Olayları gönderme

Bu bölümde, olayları bir olay merkezine gönderen bir JavaScript uygulamasının nasıl oluşturulacağı gösterilmektedir. 

> [!NOTE]
> Bu hızlı başlangıcı [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client)’dan örnek olarak indirebilir, `EventHubConnectionString` ve `EventHubName` dizelerini olay hub’ınızdaki değerlerle değiştirebilir ve çalıştırabilirsiniz. Alternatif olarak bu öğreticideki adımları izleyerek kendi çözümünüzü de oluşturabilirsiniz.

1. [Visual Studio Code](https://code.visualstudio.com)gibi en sevdiğiniz düzenleyiciyi açın. 
2. Çağrılan `send.js` bir dosya oluşturun ve aşağıdaki kodu içine yapıştırın. Makaledeki yönergeleri izleyerek olay göbeği ad alanı için bağlantı dizesini alın: [Bağlantı dizesini alın.](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) 

    ```javascript
    const { EventHubClient } = require("@azure/event-hubs@2");

    // Connection string - primary key of the Event Hubs namespace. 
    // For example: Endpoint=sb://myeventhubns.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    const connectionString = "Endpoint=sb://<EVENT HUBS NAMESPACE NAME>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<SHARED ACCESS KEY>";

    // Name of the event hub. For example: myeventhub
    const eventHubsName = "<EVENT HUB NAME>";

    async function main() {
      const client = EventHubClient.createFromConnectionString(connectionString, eventHubsName);

      for (let i = 0; i < 100; i++) {
        const eventData = {body: `Event ${i}`};
        console.log(`Sending message: ${eventData.body}`);
        await client.send(eventData);
      }

      await client.close();
    }

    main().catch(err => {
      console.log("Error occurred: ", err);
    });
    ```
3. Yukarıdaki koda bağlantı dizesini ve Olay Hub'ınızın adını girin
4. Ardından bu `node send.js` dosyayı çalıştırmak için komut isteminde komutu çalıştırın. Bu, Etkinlik Merkezinize 100 etkinlik gönderir

Tebrikler! Şimdi olayları bir etkinlik merkezine gönderdiniz.


## <a name="receive-events"></a>Olayları alma

Bu bölümde, bir olay hub'ındaki varsayılan tüketici grubunun tek bir bölümünden olayları alan bir JavaScript uygulamasınasıl oluşturulabileceğiniz gösterilmektedir. 

1. [Visual Studio Code](https://code.visualstudio.com)gibi en sevdiğiniz düzenleyiciyi açın. 
2. Çağrılan `receive.js` bir dosya oluşturun ve aşağıdaki kodu içine yapıştırın.
    ```javascript
    const { EventHubClient, delay } = require("@azure/event-hubs@2");

    // Connection string - primary key of the Event Hubs namespace. 
    // For example: Endpoint=sb://myeventhubns.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    const connectionString = "Endpoint=sb://<EVENT HUBS NAMESPACE NAME>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<SHARED ACCESS KEY>";

    // Name of the event hub. For example: myeventhub
    const eventHubsName = "<EVENT HUB NAME>";

    async function main() {
      const client = EventHubClient.createFromConnectionString(connectionString, eventHubsName);
      const allPartitionIds = await client.getPartitionIds();
      const firstPartitionId = allPartitionIds[0];

      const receiveHandler = client.receive(firstPartitionId, eventData => {
        console.log(`Received message: ${eventData.body} from partition ${firstPartitionId}`);
      }, error => {
        console.log('Error when receiving message: ', error)
      });

      // Sleep for a while before stopping the receive operation.
      await delay(15000);
      await receiveHandler.stop();

      await client.close();
    }

    main().catch(err => {
      console.log("Error occurred: ", err);
    });
    ```
3. Yukarıdaki koda bağlantı dizesini ve Olay Hub'ınızın adını girin.
4. Ardından bu `node receive.js` dosyayı çalıştırmak için komut isteminde komutu çalıştırın. Bu, Olay Hub'ınızdaki varsayılan tüketici grubunun bölümlerinden birinden olaylar alır

Tebrikler! Şimdi olay merkezinden olaylar aldınız.

## <a name="receive-events-using-event-processor-host"></a>Olay İşlemcisi Konağı kullanarak olay alma

Bu bölümde, bir JavaScript uygulamasında Azure [EventProcessorHost](event-hubs-event-processor-host.md) kullanarak bir etkinlik merkezinden olayların nasıl alınılsüreceğini gösterilmektedir. EventProcessorHost (EPH), bir etkinlik merkezinin tüketici grubundaki tüm bölümlerde alıcılar oluşturarak olayları bir etkinlik merkezinden verimli bir şekilde almanıza yardımcı olur. Bir Azure Depolama Blob'unda düzenli aralıklarla alınan iletilerle ilgili meta verileri kontrol eder. Bu yaklaşım, daha sonra kaldığınız yerden ileti almaya devam etmeyi kolaylaştırır.

1. [Visual Studio Code](https://code.visualstudio.com)gibi en sevdiğiniz düzenleyiciyi açın. 
2. Çağrılan `receiveAll.js` bir dosya oluşturun ve aşağıdaki kodu içine yapıştırın.
    ```javascript
    const { EventProcessorHost, delay } = require("@azure/event-processor-host");

    // Connection string - primary key of the Event Hubs namespace. 
    // For example: Endpoint=sb://myeventhubns.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    const eventHubConnectionString = "Endpoint=sb://<EVENT HUBS NAMESPACE NAME>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<SHARED ACCESS KEY>";

    // Name of the event hub. For example: myeventhub
    const eventHubName = "<EVENT HUB NAME>";

    // Azure Storage connection string
    const storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=<STORAGE ACCOUNT NAME>;AccountKey=<STORAGE ACCOUNT KEY>;EndpointSuffix=core.windows.net";

    async function main() {
      const eph = EventProcessorHost.createFromConnectionString(
        "my-eph",
        storageConnectionString,
        "my-storage-container-name",
        eventHubConnectionString,
        {
          eventHubPath: eventHubName,
          onEphError: (error) => {
            console.log("[%s] Error: %O", error);
          }
        }
      );


      eph.start((context, eventData) => {
        console.log(`Received message: ${eventData.body} from partition ${context.partitionId}`);
      }, error => {
        console.log('Error when receiving message: ', error)
      });

      // Sleep for a while before stopping the receive operation.
      await delay(15000);
      await eph.stop();
    }

    main().catch(err => {
      console.log("Error occurred: ", err);
    });

    ```
3. Yukarıdaki koda bağlantı dizesini ve Olay Hub'ınızın adını ve bağlantı dizesini ve Azure Blob Depolama bağlantı dizesini girin
4. Ardından bu `node receiveAll.js` dosyayı çalıştırmak için komut isteminde komutu çalıştırın.

Tebrikler! Şimdi Olay İşlemci Ana Bilgisayar'ı kullanarak olay merkezinden olaylar aldınız. Bu, Olay Hub'ınızdaki varsayılan tüketici grubunun tüm bölümlerinden olaylar alır

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleleri okuyun:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Azure Event Hubs'ın özellikleri ve terminolojisi](event-hubs-features.md)
- [Event Hubs ile ilgili SSS](event-hubs-faq.md)
- GitHub'daki Olay [Hub'ları](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples) ve [Olay İşlemci Ana Bilgisayar](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-processor-host/samples) için diğer JavaScript örneklerine göz atın
