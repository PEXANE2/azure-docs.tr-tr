---
title: Node.js ile Azure/Service-Bus konuları ve abonelikleri kullanın
description: "Hızlı başlangıç: Azure 'da Node.js uygulamadan Service Bus konuları ve abonelikleri nasıl kullanacağınızı öğrenin."
author: spelluru
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 06/23/2020
ms.author: spelluru
ms.custom: devx-track-javascript
ms.openlocfilehash: bbb0f530d95c78b8b5da178ee1544830ac3f7132
ms.sourcegitcommit: d18a59b2efff67934650f6ad3a2e1fe9f8269f21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88660605"
---
# <a name="quickstart-how-to-use-service-bus-topics-and-subscriptions-with-nodejs-and-the-azureservice-bus-package"></a>Hızlı başlangıç: Node.js ve Azure/Service-Bus paketiyle Service Bus konuları ve abonelikleri kullanma
Bu öğreticide, bir Service Bus konuya ileti göndermek ve yeni paketi kullanarak bir Service Bus aboneliğinden ileti almak için Node.js bir program yazmayı öğreneceksiniz [@azure/service-bus](https://www.npmjs.com/package/@azure/service-bus) . Bu paket, [rest çalışma zamanı API 'leri Service Bus](/rest/api/servicebus/service-bus-runtime-rest)kullanılan daha eski [Azure-SB](https://www.npmjs.com/package/azure-sb) paketi olan daha hızlı [AMQP 1,0 protokolünü](service-bus-amqp-overview.md) kullanır. Örnekler JavaScript 'e yazılır.

## <a name="prerequisites"></a>Önkoşullar
- Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)için kaydolabilirsiniz.
- Birlikte çalışmak için bir konu ve aboneliğiniz yoksa, bunları oluşturmak için [Service Bus konu ve abonelik oluşturmak üzere Azure Portal kullanma](service-bus-quickstart-topics-subscriptions-portal.md) adımlarını izleyin. Service Bus örneğiniz için bağlantı dizesini ve oluşturduğunuz konunun ve aboneliğin adlarını göz önünde bulabilirsiniz. Bu değerleri örneklerde kullanacağız.

> [!NOTE]
> - Bu öğretici, [NodeJS](https://nodejs.org/)kullanarak kopyalayıp çalıştırabileceğiniz örneklerle çalışır. Node.js uygulamasının nasıl oluşturulacağı hakkında yönergeler için, bkz. [Azure Web sitesinde bir Node.js uygulaması oluşturma ve dağıtma](../app-service/quickstart-nodejs.md)veya [ Windows PowerShell kullanarak bulut hizmetiNode.js](../cloud-services/cloud-services-nodejs-develop-deploy-app.md).
> - Yeni [@azure/service-bus](https://www.npmjs.com/package/@azure/service-bus) paket henüz topcıs ve abonelik oluşturmayı desteklemiyor. [@azure/arm-servicebus](https://www.npmjs.com/package/@azure/arm-servicebus)Program aracılığıyla onları oluşturmak istiyorsanız lütfen paketi kullanın.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Paketi yüklemek için Düğüm Paket Yöneticisi’ni (NPM) kullanma
Service Bus için NPM paketini yüklemek için, yolunda bulunan bir komut istemi açın `npm` , dizini örneklerinizin olmasını istediğiniz klasörle değiştirin ve ardından bu komutu çalıştırın.

```bash
npm install @azure/service-bus
```

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme
Service Bus konusuyla etkileşim kurmak [Servicebusclient](/javascript/api/@azure/service-bus/servicebusclient) sınıfının örneğini oluşturma ve bunu [topicclient](/javascript/api/@azure/service-bus/topicclient) sınıfının örneğini oluşturmak için kullanma ile başlar. Konu istemciye sahip olduktan sonra, ileti göndermek için bir gönderici oluşturabilir ve üzerinde [Send](/javascript/api/@azure/service-bus/sender#send-sendablemessageinfo-) ya da [sendbatch](/javascript/api/@azure/service-bus/sender#sendbatch-sendablemessageinfo---) metodunu kullanabilirsiniz.

1. [Visual Studio Code](https://code.visualstudio.com/) gibi en sevdiğiniz düzenleyiciyi açın
2. Adlı bir dosya oluşturun `send.js` ve içine aşağıdaki kodu yapıştırın. Bu kod, konuya 10 ileti gönderir.

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
3. Yukarıdaki koda sitenizin bağlantı dizesini ve adını girin.
4. Sonra `node send.js` bu dosyayı yürütmek için komut isteminde komutunu çalıştırın. 

Tebrikler! Service Bus kuyruğuna ileti gönderdiniz.

İletiler, `label` gönderme sırasında ayarlayabileceğiniz ve gibi bazı standart özelliklere sahiptir `messageId` . Herhangi bir özel özellik ayarlamak istiyorsanız, `userProperties` özel verilerinizin anahtar-değer çiftlerini tutabilecek bir JSON nesnesi olan öğesini kullanın.

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Bir konuda tutulan ileti sayısında sınır yoktur, ancak bir konu tarafından tutulan iletilerin toplam boyutu için bir sınır vardır. Bu konu başlığı boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotalar hakkında daha fazla bilgi için bkz. [Service Bus kotaları](service-bus-quotas.md).

## <a name="receive-messages-from-a-subscription"></a>Bir abonelikten ileti alma
Service Bus abonelikle etkileşim kurmak, [Servicebusclient](/javascript/api/@azure/service-bus/servicebusclient) sınıfının örneğini oluşturma ve bunu kullanarak [subscriptionclient](/javascript/api/@azure/service-bus/subscriptionclient) sınıfının örneğini oluşturmaya başlar. Abonelik istemcisini aldıktan sonra, bir alıcı oluşturabilir ve ileti almak için üzerinde [Receivemessages](/javascript/api/@azure/service-bus/receiver#receivemessages-number--undefined---number-) ya da [registermessagehandler](/javascript/api/@azure/service-bus/receiver#registermessagehandler-onmessage--onerror--messagehandleroptions-) metodunu kullanabilirsiniz.

1. [Visual Studio Code](https://code.visualstudio.com/) gibi en sevdiğiniz düzenleyiciyi açın
2. Adlı bir dosya oluşturun `receive.js` ve içine aşağıdaki kodu yapıştırın. Bu kod, aboneliğinizden 10 ileti almaya çalışır. Aldığınız gerçek sayı, abonelik ve ağ gecikmesi içindeki ileti sayısına bağlıdır.

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
3. Yukarıdaki koda konu ve aboneliğinizin bağlantı dizesini ve adlarını girin.
4. Sonra `node receive.js` bu dosyayı yürütmek için komut isteminde komutunu çalıştırın.

Tebrikler! Service Bus aboneliğinden yalnızca ileti aldınız.

[Createreceiver](/javascript/api/@azure/service-bus/subscriptionclient#createreceiver-receivemode-) yöntemi, `ReceiveMode` [Receiveanddelete](message-transfers-locks-settlement.md#settling-receive-operations) ve [PeekLock](message-transfers-locks-settlement.md#settling-receive-operations)değerlerini içeren bir Enum olan ' ı alır. [settle your messages](message-transfers-locks-settlement.md#settling-receive-operations) `PeekLock` `complete()` `abandon()` İleti üzerinde,, `defer()` veya yöntemlerinden birini kullanarak modunu kullanırsanız, iletilerinizi kapatmanız gerektiğini unutmayın `deadletter()` .

## <a name="subscription-filters-and-actions"></a>Abonelik filtreleri ve eylemleri
Service Bus, [aboneliklerdeki filtreleri ve eylemleri](topic-filters.md)destekler, bu da gelen iletileri bir aboneliğe filtrelemenizi ve özelliklerini düzenlemenizi sağlar.

Bir örneğine sahip olduktan sonra, `SubscriptionClient` filtre ve eylemleri denetlemek için abonelikte kuralları almak, eklemek ve kaldırmak için aşağıdaki yöntemleri kullanabilirsiniz.

- getRules
- addRule
- removeRule

Her aboneliğin, tüm gelen iletilere izin vermek için doğru filtreyi kullanan bir varsayılan kuralı vardır. Yeni bir kural eklediğinizde, yeni kuraldaki filtrenin çalışması için varsayılan filtreyi kaldırmayı unutmayın. Bir aboneliğin kuralları yoksa, hiçbir ileti almaz.

> [!NOTE]
> Service Bus kaynaklarını [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/)ile yönetebilirsiniz. Service Bus gezgin, kullanıcıların bir Service Bus ad alanına bağlanmasına ve mesajlaşma varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç içeri/dışarı aktarma işlevselliği gibi gelişmiş özellikler ya da konu, kuyruk, abonelik, geçiş Hizmetleri, Bildirim Hub 'ları ve Olay Hub 'larını test etme yeteneği sağlar. 

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın.

- [Kuyruklar, konu başlıkları ve abonelikler](service-bus-queues-topics-subscriptions.md)
- [GitHub üzerinde Service Bus için diğer NodeJS örneklerini](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/servicebus/service-bus/samples/javascript) kullanıma al
- [Node.js Geliştirici Merkezi](https://azure.microsoft.com/develop/nodejs/)
