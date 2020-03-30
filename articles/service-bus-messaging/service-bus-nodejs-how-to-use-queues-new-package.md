---
title: Düğüm.js'de azure/servis veri-servis kuyrukları nasıl kullanılır?
description: Yeni @azure/service-bus paketi kullanarak Servis Veri Yolu kuyruğuna ileti göndermek ve ileti almak için Bir Nodejs programı yazmayı öğrenin.
services: service-bus-messaging
documentationcenter: nodejs
author: axisc
editor: spelluru
ms.assetid: a87a00f9-9aba-4c49-a0df-f900a8b67b3f
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 01/27/2020
ms.author: aschhab
ms.openlocfilehash: c2e24e9dea2c8463294c85f04c9e4d7d2da17261
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78330660"
---
# <a name="quickstart-how-to-use-service-bus-queues-with-nodejs-and-the-azureservice-bus-package"></a>Quickstart: Node.js ve azure/servis veri günü paketi ile Servis Veri Servisi kuyrukları nasıl kullanılır?
Bu eğitimde, yeni [@azure/service-bus](https://www.npmjs.com/package/@azure/service-bus) paketi kullanarak Bir Hizmet Veri Yolu kuyruğuna ileti göndermek ve ileti almak için bir Nodejs programı yazmayı öğrenirsiniz. Bu paket daha hızlı [AMQP 1.0 protokolünü](service-bus-amqp-overview.md) kullanırken, eski [azure-sb](https://www.npmjs.com/package/azure-sb) paketi [Service Bus REST çalışma zamanı API'lerini](/rest/api/servicebus/service-bus-runtime-rest)kullansın. Örnekler JavaScript ile yazılmıştır.

## <a name="prerequisites"></a>Ön koşullar
- Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) etkinleştirebilir veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)kaydolabilirsiniz.
- Üzerinde çalışmanız gereken bir sıra yoksa, kuyruk oluşturmak için Hizmet Veri Hizmetleri sırası makalesi [oluşturmak için Azure kullan portalındaki](service-bus-quickstart-portal.md) adımları izleyin. Hizmet Veri Servisi örneğinizin bağlantı dizesini ve oluşturduğunuz kuyruğun adını not edin. Bu değerleri örneklerde kullanacağız.

> [!NOTE]
> - Bu öğretici, [Nodejs](https://nodejs.org/)kullanarak kopyalayıp çalıştırabileceğiniz örneklerle çalışır. Bir Düğüm.js uygulamasının nasıl oluşturulacağına ilişkin talimatlar için [bkz.](../app-service/app-service-web-get-started-nodejs.md) [Node.js cloud service using Windows PowerShell](../cloud-services/cloud-services-nodejs-develop-deploy-app.md)
> - Yeni [@azure/service-bus](https://www.npmjs.com/package/@azure/service-bus) paket henüz kuyruk oluşturulmasını desteklemiyor. Programlı [@azure/arm-servicebus](https://www.npmjs.com/package/@azure/arm-servicebus) bir şekilde oluşturmak istiyorsanız lütfen paketi kullanın.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Paketi yüklemek için Düğüm Paket Yöneticisi’ni (NPM) kullanma
Servis Veri Yolu için npm paketini yüklemek için, yoluna çıkan bir komut istemi `npm` açın, dizini örneklerinizi almak istediğiniz klasöre değiştirin ve bu komutu çalıştırın.

```bash
npm install @azure/service-bus
```

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Servis Veri Servisi kuyruğuyla etkileşim kurmak [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) sınıfını anlık olarak kullanmak ve [QueueClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/queueclient) sınıfını anında kullanmakla başlar. Sıra istemcisine sahip olduktan sonra, bir gönderen oluşturabilir ve ileti göndermek için üzerinde [gönder](https://docs.microsoft.com/javascript/api/%40azure/service-bus/sender#send-sendablemessageinfo-) veya [gönderyöntemini](https://docs.microsoft.com/javascript/api/@azure/service-bus/sender#sendbatch-sendablemessageinfo---) kullanabilirsiniz.

1. [Visual Studio Code](https://code.visualstudio.com/) gibi en sevdiğiniz düzenleyiciyi açın
2. Çağrılan `send.js` bir dosya oluşturun ve aşağıdaki kodu içine yapıştırın. Bu kod, kuyruğunuza 10 ileti gönderir.

    ```javascript
    const { ServiceBusClient } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const queueName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const queueClient = sbClient.createQueueClient(queueName);
      const sender = queueClient.createSender();
      
      try {
        for (let i = 0; i < 10; i++) {
          const message= {
            body: `Hello world! ${i}`,
            label: `test`,
            userProperties: {
                myCustomPropertyName: `my custom property value ${i}`
           }
          };
          console.log(`Sending message: ${message.body}`);
          await sender.send(message);
        }
        
        await queueClient.close();
      } finally {
        await sbClient.close();
      }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Yukarıdaki koda bağlantı dizesini ve sıranızın adını girin.
4. Ardından bu `node send.js` dosyayı çalıştırmak için komut isteminde komutu çalıştırın.

Tebrikler! Servis Veri Servisi kuyruğuna ileti gönderdiniz.

İletiler gibi `label` bazı standart `messageId` özelliklere sahiptir ve gönderirken ayarlayabilirsiniz. Özel özellikler ayarlamak istiyorsanız, özel `userProperties`verilerinizin anahtar değeri çiftlerini tutabilen bir json nesnesi olan nesneyi kullanın.

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Kuyrukta tutulan ileti sayısında bir sınır yoktur, ancak bir kuyruk tarafından tutulan iletilerin toplam boyutunda bir kapak vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotalar hakkında daha fazla bilgi için [Servis Otobüsü kotalarına](service-bus-quotas.md)bakın.

## <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma
Servis Veri Servisi kuyruğuyla etkileşim kurmak [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) sınıfını anlık olarak kullanmak ve [QueueClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/queueclient) sınıfını anında kullanmakla başlar. Sıra istemcisine sahip olduğunuzda, bir alıcı oluşturabilir ve ileti almak için [iletileri almak](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#receivemessages-number--undefined---number-) veya [registerMessageHandler](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#registermessagehandler-onmessage--onerror--messagehandleroptions-) yöntemini kullanabilirsiniz.

1. [Visual Studio Code](https://code.visualstudio.com/) gibi en sevdiğiniz düzenleyiciyi açın
2. Çağrılan `recieve.js` bir dosya oluşturun ve aşağıdaki kodu içine yapıştırın. Bu kod, kuyruğunuzdan 10 ileti almaya çalışır. Aldığınız gerçek sayı, kuyruktaki ileti sayısına ve ağ gecikmedurumuna bağlıdır.

    ```javascript
    const { ServiceBusClient, ReceiveMode } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const queueName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const queueClient = sbClient.createQueueClient(queueName);
      const receiver = queueClient.createReceiver(ReceiveMode.receiveAndDelete);
      try {
        const messages = await receiver.receiveMessages(10)
        console.log("Received messages:");
        console.log(messages.map(message => message.body));
         
        await queueClient.close();
      } finally {
        await sbClient.close();
      }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Yukarıdaki koda bağlantı dizesini ve sıranızın adını girin.
4. Ardından bu `node receiveMessages.js` dosyayı çalıştırmak için komut isteminde komutu çalıştırın.

Tebrikler! Servis Veri Servisi kuyruğundan iletiler aldınız.

[CreateReceiver](https://docs.microsoft.com/javascript/api/%40azure/service-bus/queueclient#createreceiver-receivemode-) yöntemi `ReceiveMode` [receiveAndDelete](message-transfers-locks-settlement.md#settling-receive-operations) ve [PeekLock](message-transfers-locks-settlement.md#settling-receive-operations)değerleri ile bir enum alır. İletideki `PeekLock` herhangi `complete()`bir yöntemi `abandon()` `defer()` `deadletter()` kullanarak modu kullanıyorsanız [iletilerinizi kapatmayı](message-transfers-locks-settlement.md#settling-receive-operations) unutmayın.

> [!NOTE]
> [Servis](https://github.com/paolosalvatori/ServiceBusExplorer/)Veri Servisi Explorer ile Servis Veri Servisi kaynaklarını yönetebilirsiniz. Service Bus Explorer, kullanıcıların bir Service Bus ad alanına bağlanmasına ve ileti varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç, alma/dışa aktarma işlevselliği veya konuyu, kuyrukları, abonelikleri, geçiş hizmetlerini, bildirim hub'larını ve olay hub'larını test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın.
- [Kuyruklar, konular ve abonelikler](service-bus-queues-topics-subscriptions.md)
- [GitHub'daki Servis Otobüsü için diğer Nodejs örneklerini](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/servicebus/service-bus/samples/javascript) ödeme
- [Node.js Geliştirici Merkezi](https://azure.microsoft.com/develop/nodejs/)

