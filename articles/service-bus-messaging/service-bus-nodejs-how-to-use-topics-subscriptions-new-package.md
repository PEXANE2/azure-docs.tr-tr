---
title: Node.js ile azure/servis veri meskeni konularını ve abonelikleri kullanma
description: "Hızlı başlangıç: Bir Node.js uygulamasından Azure'daki Hizmet Veri Yolu konularını ve aboneliklerini nasıl kullanacağınızı öğrenin."
services: service-bus-messaging
documentationcenter: nodejs
author: axisc
manager: timlt
editor: spelluru
ms.assetid: b9f5db85-7b6c-4cc7-bd2c-bd3087c99875
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 01/16/2020
ms.author: aschhab
ms.openlocfilehash: 6088b4c54ed16c5ef46d2c0671e619884cad29d4
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78330626"
---
# <a name="quickstart-how-to-use-service-bus-topics-and-subscriptions-with-nodejs-and-the-azureservice-bus-package"></a>Quickstart: Node.js ve azure/servis-veri-servis paketi ile Servis Veri Servisi konuları ve abonelikleri nasıl kullanılır?
Bu eğitimde, Bir Hizmet Veri Yolu konusuna ileti göndermek ve yeni [@azure/service-bus](https://www.npmjs.com/package/@azure/service-bus) paketi kullanarak bir Servis Veri yolu aboneliğinden ileti almak için bir Düğüm.js programı yazmayı öğrenirsiniz. Bu paket daha hızlı [AMQP 1.0 protokolünü](service-bus-amqp-overview.md) kullanırken, eski [azure-sb](https://www.npmjs.com/package/azure-sb) paketi [Service Bus REST çalışma zamanı API'lerini](/rest/api/servicebus/service-bus-runtime-rest)kullansın. Örnekler JavaScript ile yazılmıştır.

## <a name="prerequisites"></a>Ön koşullar
- Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) etkinleştirebilir veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)kaydolabilirsiniz.
- Üzerinde çalışabileceğiniz bir konu ve aboneliğiniz yoksa, bunları oluşturmak için bir Hizmet Veri Servisi konuları ve abonelikler makalesi [oluşturmak için Azure'u kullan portalındaki](service-bus-quickstart-topics-subscriptions-portal.md) adımları izleyin. Hizmet Veri Servisi örneğinizin bağlantı dizesini ve oluşturduğunuz konu nun ve aboneliğin adlarını not edin. Bu değerleri örneklerde kullanacağız.

> [!NOTE]
> - Bu öğretici, [Nodejs](https://nodejs.org/)kullanarak kopyalayıp çalıştırabileceğiniz örneklerle çalışır. Bir Düğüm.js uygulamasının nasıl oluşturulacağına ilişkin talimatlar için [bkz.](../app-service/app-service-web-get-started-nodejs.md) [Node.js Cloud Service using Windows PowerShell](../cloud-services/cloud-services-nodejs-develop-deploy-app.md)
> - Yeni [@azure/service-bus](https://www.npmjs.com/package/@azure/service-bus) paket henüz topcis ve aboneliklerin oluşturulmasını desteklemiyor. Programlı [@azure/arm-servicebus](https://www.npmjs.com/package/@azure/arm-servicebus) bir şekilde oluşturmak istiyorsanız lütfen paketi kullanın.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Paketi yüklemek için Düğüm Paket Yöneticisi’ni (NPM) kullanma
Servis Veri Yolu için npm paketini yüklemek için, yoluna çıkan bir komut istemi `npm` açın, dizini örneklerinizi almak istediğiniz klasöre değiştirin ve bu komutu çalıştırın.

```bash
npm install @azure/service-bus
```

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme
Bir Servis Veri Servisi konusuyla etkileşim kurmak [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) sınıfını anlık olarak kullanmak ve [TopicClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/topicclient) sınıfını anında kullanmakla başlar. Konu istemcisini aldıktan sonra, bir gönderen oluşturabilir ve ileti göndermek için üzerinde [gönder](https://docs.microsoft.com/javascript/api/%40azure/service-bus/sender#send-sendablemessageinfo-) veya [gönderyöntemini](https://docs.microsoft.com/javascript/api/@azure/service-bus/sender#sendbatch-sendablemessageinfo---) kullanabilirsiniz.

1. [Visual Studio Code](https://code.visualstudio.com/) gibi en sevdiğiniz düzenleyiciyi açın
2. Çağrılan `send.js` bir dosya oluşturun ve aşağıdaki kodu içine yapıştırın. Bu kod, konunuza 10 ileti gönderir.

    ```javascript
    const { ServiceBusClient } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const topicName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const topicClient = sbClient.createTopicClient(topicName);
      const sender = topicClient.createSender();
      
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

            await topicClient.close();
          } finally {
            await sbClient.close();
          }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Yukarıdaki koda bağlantı dizesini ve konunuzun adını girin.
4. Ardından bu `node send.js` dosyayı çalıştırmak için komut isteminde komutu çalıştırın. 

Tebrikler! Servis Veri Servisi kuyruğuna ileti gönderdiniz.

İletiler gibi `label` bazı standart `messageId` özelliklere sahiptir ve gönderirken ayarlayabilirsiniz. Özel özellikler ayarlamak istiyorsanız, özel `userProperties`verilerinizin anahtar değeri çiftlerini tutabilen bir json nesnesi olan nesneyi kullanın.

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Bir konuda tutulan ileti sayısında bir sınır yoktur, ancak bir konu tarafından tutulan iletilerin toplam boyutunda bir sınır vardır. Bu konu başlığı boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotalar hakkında daha fazla bilgi için [Servis Otobüsü kotalarına](service-bus-quotas.md)bakın.

## <a name="receive-messages-from-a-subscription"></a>Abonelikten ileti alma
Service Bus aboneliğiyle etkileşim kurmak, [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) sınıfını anlık olarak kullanmak ve [Aboneliği İstemlemi](https://docs.microsoft.com/javascript/api/%40azure/service-bus/subscriptionclient) sınıfını anında kullanmakla başlar. Abonelik istemcisine sahip olduğunuzda, bir alıcı oluşturabilir ve ileti almak için [iletileri almak](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#receivemessages-number--undefined---number-) veya [registerMessageHandler](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#registermessagehandler-onmessage--onerror--messagehandleroptions-) yöntemini kullanabilirsiniz.

1. [Visual Studio Code](https://code.visualstudio.com/) gibi en sevdiğiniz düzenleyiciyi açın
2. Çağrılan `recieve.js` bir dosya oluşturun ve aşağıdaki kodu içine yapıştırın. Bu kod, aboneliğinizden 10 ileti almaya çalışır. Aldığınız gerçek sayım, abonelikteki ileti sayısına ve ağ gecikme durumuna bağlıdır.

    ```javascript
    const { ServiceBusClient, ReceiveMode } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const topicName = ""; 
    const subscriptionName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const subscriptionClient = sbClient.createSubscriptionClient(topicName, subscriptionName);
      const receiver = subscriptionClient.createReceiver(ReceiveMode.receiveAndDelete);
      
      try {
        const messages = await receiver.receiveMessages(10);
        console.log("Received messages:");
        console.log(messages.map(message => message.body));
        
        await subscriptionClient.close();
      } finally {
        await sbClient.close();
      }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Yukarıdaki koda bağlantı dizesini ve konu ve aboneliğinizin adlarını girin.
4. Ardından bu `node receiveMessages.js` dosyayı çalıştırmak için komut isteminde komutu çalıştırın.

Tebrikler! Bir Servis Veri Servisi aboneliğinden ileti ler aldınız.

[CreateReceiver](https://docs.microsoft.com/javascript/api/%40azure/service-bus/subscriptionclient#createreceiver-receivemode-) yöntemi `ReceiveMode` [receiveAndDelete](message-transfers-locks-settlement.md#settling-receive-operations) ve [PeekLock](message-transfers-locks-settlement.md#settling-receive-operations)değerleri ile bir enum alır. İletideki `PeekLock` herhangi `complete()`bir yöntemi `abandon()` `defer()` `deadletter()` kullanarak modu kullanıyorsanız [iletilerinizi kapatmayı](message-transfers-locks-settlement.md#settling-receive-operations) unutmayın.

## <a name="subscription-filters-and-actions"></a>Abonelik filtreleri ve eylemleri
Service Bus, gelen iletileri aboneye filtrelemenize ve özelliklerini nida edebilmenize olanak tanıyan [aboneliklerle ilgili filtreleri](topic-filters.md)ve eylemleri destekler.

Bir örnek `SubscriptionClient` aldıktan sonra, filtreleri ve eylemleri denetlemek için abonelik kurallarını almak, eklemek ve kaldırmak için aşağıdaki yöntemleri kullanabilirsiniz.

- getRules
- Addrule
- removeRule

Her aboneliğin, gelen tüm iletilere izin vermek için gerçek filtreyi kullanan bir varsayılan kuralı vardır. Yeni bir kural eklediğinizde, yeni kuralınızdaki filtrenin çalışması için varsayılan filtreyi kaldırmayı unutmayın. Bir aboneliğin kuralları yoksa, ileti almaz.

> [!NOTE]
> [Servis](https://github.com/paolosalvatori/ServiceBusExplorer/)Veri Servisi Explorer ile Servis Veri Servisi kaynaklarını yönetebilirsiniz. Service Bus Explorer, kullanıcıların bir Service Bus ad alanına bağlanmasına ve ileti varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç, alma/dışa aktarma işlevselliği veya konuyu, kuyrukları, abonelikleri, geçiş hizmetlerini, bildirim hub'larını ve olay hub'larını test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın.

- [Kuyruklar, konular ve abonelikler](service-bus-queues-topics-subscriptions.md)
- [GitHub'daki Servis Otobüsü için diğer Nodejs örneklerini](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/servicebus/service-bus/samples/javascript) ödeme
- [Node.js Geliştirici Merkezi](https://azure.microsoft.com/develop/nodejs/)


