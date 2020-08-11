---
title: Node.js 'de Azure/Service-Bus kuyruklarını kullanma
description: Yeni paketi kullanarak bir Service Bus kuyruğuna ileti göndermek ve ileti almak için bir NodeJS programı yazmayı öğrenin @azure/service-bus .
author: spelluru
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 06/23/2020
ms.author: spelluru
ms.custom: devx-track-javascript
ms.openlocfilehash: c082eff4f70b0255b7701340ac6e8b377223c0b5
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88076872"
---
# <a name="quickstart-how-to-use-service-bus-queues-with-nodejs-and-the-azureservice-bus-package"></a>Hızlı başlangıç: Node.js ve Azure/Service-Bus paketiyle Service Bus kuyruklarını kullanma
Bu öğreticide, yeni paketi kullanarak Service Bus kuyruğuna ileti göndermek ve ileti almak için bir NodeJS programı yazmayı öğreneceksiniz [@azure/service-bus](https://www.npmjs.com/package/@azure/service-bus) . Bu paket, [rest çalışma zamanı API 'leri Service Bus](/rest/api/servicebus/service-bus-runtime-rest)kullanılan daha eski [Azure-SB](https://www.npmjs.com/package/azure-sb) paketi olan daha hızlı [AMQP 1,0 protokolünü](service-bus-amqp-overview.md) kullanır. Örnekler JavaScript 'e yazılır.

## <a name="prerequisites"></a>Önkoşullar
- Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)için kaydolabilirsiniz.
- Birlikte çalışmak için bir kuyruğunuz yoksa, bir kuyruk oluşturmak için [Service Bus kuyruğu oluşturmak üzere Azure Portal kullanma](service-bus-quickstart-portal.md) adımlarını izleyin. Service Bus örneğiniz için bağlantı dizesini ve oluşturduğunuz kuyruğun adını aklınızda edin. Bu değerleri örneklerde kullanacağız.

> [!NOTE]
> - Bu öğretici, [NodeJS](https://nodejs.org/)kullanarak kopyalayıp çalıştırabileceğiniz örneklerle çalışır. Node.js uygulamasının nasıl oluşturulacağı hakkında yönergeler için, bkz. [Azure Web sitesinde bir Node.js uygulaması oluşturma ve dağıtma](../app-service/quickstart-nodejs.md)veya [Windows PowerShell kullanarak bulut hizmetiNode.js](../cloud-services/cloud-services-nodejs-develop-deploy-app.md).
> - Yeni [@azure/service-bus](https://www.npmjs.com/package/@azure/service-bus) paket henüz sıra oluşturmayı desteklemiyor. [@azure/arm-servicebus](https://www.npmjs.com/package/@azure/arm-servicebus)Program aracılığıyla onları oluşturmak istiyorsanız lütfen paketi kullanın.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Paketi yüklemek için Düğüm Paket Yöneticisi’ni (NPM) kullanma
Service Bus için NPM paketini yüklemek için, yolunda bulunan bir komut istemi açın `npm` , dizini örneklerinizin olmasını istediğiniz klasörle değiştirin ve ardından bu komutu çalıştırın.

```bash
npm install @azure/service-bus
```

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Service Bus kuyrukla etkileşim kurmak [Servicebusclient](/javascript/api/@azure/service-bus/servicebusclient) sınıfının örneğini oluşturmaya ve [queueclient](/javascript/api/@azure/service-bus/queueclient) sınıfının örneğini oluşturmak için kullanılmasına başlar. Kuyruk istemcisini aldıktan sonra, ileti göndermek için bir gönderici oluşturabilir ve bunun üzerinde [Send](/javascript/api/@azure/service-bus/sender#send-sendablemessageinfo-) veya [sendbatch](/javascript/api/@azure/service-bus/sender#sendbatch-sendablemessageinfo---) yöntemini kullanabilirsiniz.

1. [Visual Studio Code](https://code.visualstudio.com/) gibi en sevdiğiniz düzenleyiciyi açın
2. Adlı bir dosya oluşturun `send.js` ve içine aşağıdaki kodu yapıştırın. Bu kod, kuyruğunuza 10 ileti gönderir.

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
3. Yukarıdaki kodda, kuyruğunuzun bağlantı dizesini ve adını girin.
4. Sonra `node send.js` bu dosyayı yürütmek için komut isteminde komutunu çalıştırın.

Tebrikler! Service Bus kuyruğuna ileti gönderdiniz.

İletiler, `label` gönderme sırasında ayarlayabileceğiniz ve gibi bazı standart özelliklere sahiptir `messageId` . Herhangi bir özel özellik ayarlamak istiyorsanız, `userProperties` özel verilerinizin anahtar-değer çiftlerini tutabilecek bir JSON nesnesi olan öğesini kullanın.

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Bir kuyrukta tutulan ileti sayısında bir sınır yoktur ancak bir sıranın tuttuğu iletilerin toplam boyutunun bir üst sınırı vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotalar hakkında daha fazla bilgi için bkz. [Service Bus kotaları](service-bus-quotas.md).

## <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma
Service Bus kuyrukla etkileşim kurmak [Servicebusclient](/javascript/api/@azure/service-bus/servicebusclient) sınıfının örneğini oluşturmaya ve [queueclient](/javascript/api/@azure/service-bus/queueclient) sınıfının örneğini oluşturmak için kullanılmasına başlar. Sıra istemcisini aldıktan sonra, bir alıcı oluşturabilir ve ileti almak için üzerinde [Receivemessages](/javascript/api/@azure/service-bus/receiver#receivemessages-number--undefined---number-) veya [registermessagehandler](/javascript/api/@azure/service-bus/receiver#registermessagehandler-onmessage--onerror--messagehandleroptions-) metodunu kullanabilirsiniz.

1. [Visual Studio Code](https://code.visualstudio.com/) gibi en sevdiğiniz düzenleyiciyi açın
2. Adlı bir dosya oluşturun `recieve.js` ve içine aşağıdaki kodu yapıştırın. Bu kod, kuyruğunuzdan 10 ileti almaya çalışır. Aldığınız gerçek sayı, sıradaki ileti sayısına ve ağ gecikmesine bağlıdır.

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
3. Yukarıdaki kodda, kuyruğunuzun bağlantı dizesini ve adını girin.
4. Sonra `node receiveMessages.js` bu dosyayı yürütmek için komut isteminde komutunu çalıştırın.

Tebrikler! Service Bus sırasından ileti aldınız.

[Createreceiver](/javascript/api/@azure/service-bus/queueclient#createreceiver-receivemode-) yöntemi, `ReceiveMode` [Receiveanddelete](message-transfers-locks-settlement.md#settling-receive-operations) ve [PeekLock](message-transfers-locks-settlement.md#settling-receive-operations)değerlerini içeren bir Enum olan ' ı alır. [settle your messages](message-transfers-locks-settlement.md#settling-receive-operations) `PeekLock` `complete()` `abandon()` İleti üzerinde,, `defer()` veya yöntemlerinden birini kullanarak modunu kullanırsanız, iletilerinizi kapatmanız gerektiğini unutmayın `deadletter()` .

> [!NOTE]
> Service Bus kaynaklarını [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/)ile yönetebilirsiniz. Service Bus gezgin, kullanıcıların bir Service Bus ad alanına bağlanmasına ve mesajlaşma varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç içeri/dışarı aktarma işlevselliği gibi gelişmiş özellikler ya da konu, kuyruk, abonelik, geçiş Hizmetleri, Bildirim Hub 'ları ve Olay Hub 'larını test etme yeteneği sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın.
- [Kuyruklar, konu başlıkları ve abonelikler](service-bus-queues-topics-subscriptions.md)
- [GitHub üzerinde Service Bus için diğer NodeJS örneklerini](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/servicebus/service-bus/samples/javascript) kullanıma al
- [Node.js Geliştirici Merkezi](https://azure.microsoft.com/develop/nodejs/)