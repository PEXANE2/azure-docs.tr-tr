---
title: Azure-SB paketini kullanarak Node.js Azure Service Bus kuyruklarını kullanma
description: Azure-SB paketini kullanarak bir Azure Service Bus kuyruğundan ileti göndermek ve ileti almak için Node.js uygulamalar oluşturmayı öğrenin.
author: spelluru
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 06/23/2020
ms.author: spelluru
ms.custom: seo-javascript-september2019, seo-javascript-october2019, devx-track-javascript
ms.openlocfilehash: 076233c52074bca646c072e9a1abe63b15412c1b
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87430635"
---
# <a name="quickstart-use-service-bus-queues-in-azure-with-nodejs-and-the-azure-sb-package"></a>Hızlı başlangıç: Azure 'da Node.js ve Azure-SB paketiyle Service Bus kuyrukları kullanma
Bu öğreticide, [Azure-SB](https://www.npmjs.com/package/azure-sb) paketini kullanarak bir Azure Service Bus kuyruğundan ileti göndermek ve ileti almak için Node.js uygulamalar oluşturmayı öğreneceksiniz. Örnekler JavaScript 'te yazılır ve Azure-SB paketini dahili olarak kullanan Node.js [Azure modülünü](https://www.npmjs.com/package/azure) kullanır.

> [!IMPORTANT]
> [Azure-SB](https://www.npmjs.com/package/azure-sb) PAKETI [Service Bus Rest çalışma zamanı API 'lerini](/rest/api/servicebus/service-bus-runtime-rest)kullanır. Yeni kullanarak daha hızlı bir deneyim edinebilirsiniz [@azure/service-bus](https://www.npmjs.com/package/@azure/service-bus) . Bu, daha hızlı [amqp 1,0 protokolünü](service-bus-amqp-overview.md)kullanır. 
> 
> Yeni paket hakkında daha fazla bilgi edinmek için bkz. [Service Bus kuyruklarını Node.js ve @azure/service-bus paketiyle kullanma](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-nodejs-how-to-use-queues-new-package), aksi takdirde okumaya devam etme, [Azure](https://www.npmjs.com/package/azure) paketini nasıl kullanacağınızı öğrenmek için.

## <a name="prerequisites"></a>Önkoşullar
- Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)için kaydolabilirsiniz.
- Birlikte çalışmak için bir kuyruğunuz yoksa, bir kuyruk oluşturmak için [Service Bus kuyruğu oluşturmak üzere Azure Portal kullanma](service-bus-quickstart-portal.md) adımlarını izleyin.
    1. Service Bus **kuyruklara**hızlı **genel bakış** konusunu okuyun. 
    2. Service Bus **ad alanı**oluşturun. 
    3. **Bağlantı dizesini**alın. 

        > [!NOTE]
        > Bu öğreticide Node.js kullanarak Service Bus ad alanında bir **sıra** oluşturacaksınız. 
 

## <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma
Boş bir Node.js uygulaması oluşturun. Node.js uygulamasının nasıl oluşturulacağı hakkında yönergeler için, bkz. [Azure Web sitesinde bir Node.js uygulaması oluşturma ve dağıtma][Create and deploy a Node.js application to an Azure Website]veya Windows PowerShell kullanarak [bulut hizmetiNode.js][Node.js Cloud Service] .

## <a name="configure-your-application-to-use-service-bus"></a>Uygulamanızı kullanmak için yapılandırma Service Bus
Azure Service Bus kullanmak için Node.js Azure paketini indirip kullanın. Bu paket, Service Bus REST hizmetleriyle iletişim kuran bir kitaplıklar kümesi içerir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Paketi edinmek için düğüm Paket Yöneticisi 'Ni (NPM) kullanın
1. Örnek uygulamanızı oluşturduğunuz **c: \\ node \\ sbqueues \\ WebRole1** klasörüne gitmek için **Windows PowerShell for Node.js** komut penceresini kullanın.
2. Komut penceresine **NPM 'yi yüklemek** için aşağıdaki örneğe benzer bir çıktı ile sonuçlanır:

    ```
    azure@0.7.5 node_modules\azure
        ├── dateformat@1.0.2-1.2.3
        ├── xmlbuilder@0.4.2
        ├── node-uuid@1.2.0
        ├── mime@1.2.9
        ├── underscore@1.4.4
        ├── validator@1.1.1
        ├── tunnel@0.0.2
        ├── wns@0.5.3
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```
3. Bir **node_modules** klasörünün oluşturulduğunu doğrulamak için **ls** komutunu kendiniz çalıştırabilirsiniz. Bu klasörün içinde, Service Bus kuyruklara erişmeniz gereken kitaplıkları içeren **Azure** paketini bulun.

### <a name="import-the-module"></a>Modülü içeri aktar
Not defteri veya başka bir metin düzenleyicisi kullanarak, uygulamanın **server.js** dosyasının en üstüne aşağıdakileri ekleyin:

```javascript
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Azure Service Bus bağlantısını ayarlama
Azure modülü, `AZURE_SERVICEBUS_CONNECTION_STRING` Service Bus bağlanmak için gereken bilgileri almak için ortam değişkenini okur. Bu ortam değişkeni ayarlanmamışsa, çağırırken hesap bilgilerini belirtmeniz gerekir `createServiceBusService` .

Bir Azure Web sitesi için [Azure Portal][Azure portal] ortam değişkenlerini ayarlamaya ilişkin bir örnek için, bkz. [Node.js Web Application with Storage][Node.js Web Application with Storage].

## <a name="create-a-queue"></a>Bir kuyruk oluşturma
**Servicebusservice** nesnesi Service Bus kuyruklarıyla çalışmanıza olanak sağlar. Aşağıdaki kod bir **Servicebusservice** nesnesi oluşturur. Azure modülünü içeri aktarma ifadesinden sonra **server.js** dosyanın üst kısmına yakın bir şekilde ekleyin:

```javascript
var serviceBusService = azure.createServiceBusService();
```

`createQueueIfNotExists` **Servicebusservice** nesnesine çağırarak, belirtilen sıra döndürülür (varsa) veya belirtilen ada sahip yeni bir sıra oluşturulur. Aşağıdaki kod, `createQueueIfNotExists` adlı sırayı oluşturmak veya bu kuyruğa bağlanmak için kullanır `myqueue` :

```javascript
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

`createServiceBusService`Yöntemi Ayrıca, ileti Süresi canlı veya en yüksek sıra boyutu gibi varsayılan sıra ayarlarını geçersiz kılmanızı sağlayan ek seçenekleri de destekler. Aşağıdaki örnek, en büyük sıra boyutunu 5 GB ve yaşam süresi (TTL) değeri olarak 1 dakika olarak ayarlar:

```javascript
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### <a name="filters"></a>Filtreler
İsteğe bağlı filtreleme işlemleri, **Servicebusservice**kullanılarak gerçekleştirilen işlemlere uygulanabilir. Filtreleme işlemleri, günlüğe kaydetme, otomatik yeniden deneme vb. içerebilir. Filtreler imzaya sahip bir yöntemi uygulayan nesnelerdir:

```javascript
function handle (requestOptions, next)
```

İstek seçeneklerinde ön işlemesini gerçekleştirdikten sonra, yöntemi çağırmalıdır `next` ve aşağıdaki imzayla bir geri çağırma gerçekleşmelidir:

```javascript
function (returnObject, finalCallback, next)
```

Bu geri `returnObject` çağırmada ve (istekten sunucuya olan yanıt) işlendikten sonra geri çağırma, `next` diğer filtreleri işlemeye devam etmek için varsa veya `finalCallback` hizmet çağrısını sonlandıran Invoke çağrısı yapmanız gerekir.

Yeniden deneme mantığını uygulayan iki filtre, Node.js için Azure SDK ile birlikte gelir `ExponentialRetryPolicyFilter` `LinearRetryPolicyFilter` . Aşağıdaki kod `ServiceBusService` , şunu kullanan bir nesnesi oluşturur `ExponentialRetryPolicyFilter` :

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Service Bus kuyruğuna ileti göndermek için, uygulamanız `sendQueueMessage` **Servicebusservice** nesnesinde yöntemini çağırır. Service Bus sıralara gönderilen iletiler (ve öğesinden alınan), **Brokeredmessage** nesneleridir ve standart bir özellikler kümesi ( **etiket** ve **TimeToLive**gibi), uygulamaya özgü özel özellikleri tutmak için kullanılan bir sözlük ve rastgele uygulama verileri gövdesi vardır. Bir uygulama ileti olarak bir dize geçirerek ileti gövdesini ayarlayabilir. Gerekli standart özellikler varsayılan değerlerle doldurulur.

Aşağıdaki örnek, kullanılarak adlı sıraya bir test iletisinin nasıl gönderileceğini göstermektedir `myqueue` `sendQueueMessage` :

```javascript
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Bir kuyrukta tutulan ileti sayısında bir sınır yoktur ancak bir sıranın tuttuğu iletilerin toplam boyutunun bir üst sınırı vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotalar hakkında daha fazla bilgi için bkz. [Service Bus kotaları][Service Bus quotas].

## <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma
İleti, `receiveQueueMessage` **servicebusservice** nesnesinde yöntemi kullanılarak bir kuyruktan alınır. Varsayılan olarak, iletiler, okunan sırada kuyruktan silinir; Bununla birlikte, isteğe bağlı parametresini true olarak ayarlayarak iletiyi, kuyruktan silmeden onu okuyabilir (Peek) ve kilitleyebilir `isPeekLock` . **true**

İletiyi alma işleminin bir parçası olarak okuma ve silmenin varsayılan davranışı en basit modeldir ve bir hata oluştuğunda bir uygulamanın bir iletiyi işlememesinin kabul edebildiği senaryolar için en iyi sonucu verir. Bu davranışı anlamak için, tüketicinin alma isteğini yaptığı ve işlemeden önce çöktüğü bir senaryo düşünün. Service Bus ileti tüketildiği gibi işaretleneceği için, uygulama yeniden başlatıldığında ve iletileri yeniden kullanmaya başladığında, kilitlenmeden önce tüketilen ileti kaçırılmış olur.

`isPeekLock`Parametresi **true**olarak ayarlanırsa, alma işlemi iki aşamalı bir işlem olur ve bu da eksik iletileri kabul edemediği uygulamaları desteklemeye olanak sağlar. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan (veya gelecekteki işlemler için güvenilir bir şekilde depolar), yöntemi çağırarak alma işleminin ikinci aşamasını tamamlar `deleteMessage` ve bir parametre olarak silinecek iletiyi sağlar. `deleteMessage`Yöntemi iletiyi tüketildiği gibi işaretler ve kuyruktan kaldırır.

Aşağıdaki örnek kullanarak iletilerin nasıl alınacağını ve işleyeceğini gösterir `receiveQueueMessage` . Örnek öncelikle bir iletiyi alır ve siler ve ardından `isPeekLock` **doğru**olarak ayarla ' yı kullanarak bir ileti alır, ardından şunu kullanarak iletiyi siler `deleteMessage` :

```javascript
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bir alıcı uygulaması iletiyi bir nedenden dolayı işleyebilirse, `unlockMessage` **Servicebusservice** nesnesinde yöntemi çağırabilir. Service Bus, kuyruktaki iletinin kilidini açmasına ve aynı uygulama ya da başka bir uygulama tarafından yeniden alınabileceğinden neden olur.

Ayrıca, kuyruk içinde kilitlenen bir iletiyle ilişkili bir zaman aşımı da vardır ve uygulamanın kilit zaman aşımı dolmadan önce iletiyi işleyemezse (örneğin, uygulama çökerse), Service Bus otomatik olarak iletinin kilidini açar ve yeniden alınmak üzere kullanılabilir hale gelir.

İleti işlendikten sonra, ancak yöntem çağrılmadan önce uygulamanın çöktüğü durumda `deleteMessage` , yeniden başlatıldığında ileti uygulamaya yeniden gönderilir. Bu yaklaşım genellikle *en az bir kez işleme*olarak adlandırılır, diğer bir deyişle her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işleme kabul edememesi durumunda, uygulama geliştiricilerinin yinelenen ileti teslimini işlemek için uygulamasına ek mantık eklemesi gerekir. Genellikle iletinin **MessageID** özelliği kullanılarak elde edilir ve bu, teslim girişimleri arasında sabit kalacak.

> [!NOTE]
> Service Bus kaynaklarını [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/)ile yönetebilirsiniz. Service Bus gezgin, kullanıcıların bir Service Bus ad alanına bağlanmasına ve mesajlaşma varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç içeri/dışarı aktarma işlevselliği gibi gelişmiş özellikler ya da konu, kuyruk, abonelik, geçiş Hizmetleri, Bildirim Hub 'ları ve Olay Hub 'larını test etme yeteneği sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Kuyruklar hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın.

* [Kuyruklar, konu başlıkları ve abonelikler][Queues, topics, and subscriptions]
* GitHub 'da [düğüm deposu Için Azure SDK][Azure SDK for Node]
* [Node.js Geliştirici Merkezi](https://azure.microsoft.com/develop/nodejs/)

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com

[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Create and deploy a Node.js application to an Azure Website]: ../app-service/app-service-web-get-started-nodejs.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-how-to-use-nodejs.md
[Service Bus quotas]: service-bus-quotas.md
