---
title: Azure-sb paketini kullanarak Düğüm.js'deki Azure Hizmet Veri Servisi kuyruklarını kullanma
description: Azure-sb paketini kullanarak Azure Hizmet Veri Yolu kuyruğuna ileti göndermek ve ileti almak için Node.js uygulamalarını nasıl oluşturup alacağınızı öğrenin.
services: service-bus-messaging
documentationcenter: nodejs
author: axisc
manager: timlt
editor: spelluru
ms.assetid: a87a00f9-9aba-4c49-a0df-f900a8b67b3f
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 01/27/2020
ms.author: aschhab
ms.custom: seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: 7ee3939c1a1b450f2458267ab0b70e3924a4869b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78330609"
---
# <a name="quickstart-use-service-bus-queues-in-azure-with-nodejs-and-the-azure-sb-package"></a>Hızlı başlangıç: Node.js ve azure-sb paketi yle Azure'da Servis Veri Servisi kuyruklarını kullanma
Bu eğitimde, [azure-sb](https://www.npmjs.com/package/azure-sb) paketini kullanarak Azure Hizmet Veri Yolu kuyruğuna ileti göndermek ve ileti almak için Node.js uygulamalarını nasıl oluşturabileceğinizi öğrenirsiniz. Örnekler JavaScript ile yazılır ve azure-sb paketini dahili olarak kullanan Node.js [Azure modüllerini](https://www.npmjs.com/package/azure) kullanır.

> [!IMPORTANT]
> [Azure-sb](https://www.npmjs.com/package/azure-sb) [paketi, Service Bus REST çalışma zamanı API'lerini](/rest/api/servicebus/service-bus-runtime-rest)kullanır. Daha hızlı [@azure/service-bus](https://www.npmjs.com/package/@azure/service-bus) [AMQP 1.0 protokolünü](service-bus-amqp-overview.md)kullanan yeni yi kullanarak daha hızlı bir deneyim yaşayabilirsiniz. 
> 
> Yeni paket hakkında daha fazla bilgi edinmek için [Node.js ve @azure/service-bus paketi olan Servis Veri Yolu kuyruklarını nasıl kullanacağınızı](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-nodejs-how-to-use-queues-new-package)öğrenin , aksi takdirde [azure](https://www.npmjs.com/package/azure) paketinin nasıl kullanılacağını görmek için okumaya devam edin.

## <a name="prerequisites"></a>Ön koşullar
- Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) etkinleştirebilir veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)kaydolabilirsiniz.
- Üzerinde çalışmanız gereken bir sıra yoksa, kuyruk oluşturmak için Hizmet Veri Hizmetleri sırası makalesi [oluşturmak için Azure kullan portalındaki](service-bus-quickstart-portal.md) adımları izleyin.
    1. Servis Veri Servisi **kuyruklarına**hızlı **bir genel bakış** okuyun. 
    2. Hizmet Veri Günü **ad alanı**oluşturun. 
    3. Bağlantı **dizesini**alın. 

        > [!NOTE]
        > Bu öğreticide Node.js kullanarak Hizmet Veri Servisi ad alanında bir **kuyruk** oluşturacaksınız. 
 

## <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma
Boş bir Düğüm.js uygulaması oluşturun. Bir Düğüm.js uygulamasının nasıl oluşturulacağına ilişkin talimatlar için [bkz.][Create and deploy a Node.js application to an Azure Website] [Node.js Cloud Service][Node.js Cloud Service]

## <a name="configure-your-application-to-use-service-bus"></a>Uygulamanızı Servis Veri Aracı'nı kullanacak şekilde yapılandırın
Azure Hizmet Veri Servisi'ni kullanmak için Node.js Azure paketini indirin ve kullanın. Bu paket, Servis Veri Servisi REST hizmetleriyle iletişim kuran bir dizi kitaplık içerir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Paketi almak için Düğüm Paket Yöneticisi'ni (NPM) kullanın
1. **C:\\node\\sbqueues\\WebRole1** klasörüne gitmek için **Düğüm.js** komut penceresini kullanın.
2. Komut penceresine **npm yükleme azure** yazın, bu da aşağıdaki örneğe benzer çıktıyla sonuçlanır:

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
3. Bir **node_modules** klasörünün oluşturulduğunu doğrulamak için **ls** komutunu kendiniz çalıştırabilirsiniz. Bu klasörün içinde, Hizmet Veri Servisi kuyruklarına erişmek için gereken kitaplıkları içeren **azure** paketini bulun.

### <a name="import-the-module"></a>Modülü alma
Not Defteri'ni veya başka bir metin düzenleyicisini kullanarak, uygulamanın **server.js** dosyasının üst bölümüne aşağıdakileri ekleyin:

```javascript
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Azure Hizmet Veri Servisi bağlantısı ayarlama
Azure modülü, Servis `AZURE_SERVICEBUS_CONNECTION_STRING` Veri Servisi'ne bağlanmak için gereken bilgileri elde etmek için ortam değişkenini okur. Bu ortam değişkeni ayarlanmıyorsa, 'yi ararken `createServiceBusService`hesap bilgilerini belirtmeniz gerekir.

Azure web sitesi için [Azure portalında][Azure portal] ortam değişkenlerini ayarlama örneği için Bkz. [Depolama ile Node.js Web Uygulaması.][Node.js Web Application with Storage]

## <a name="create-a-queue"></a>Bir kuyruk oluşturma
**ServiceBusService nesnesi,** Service Bus queues ile çalışmanızı sağlar. Aşağıdaki kod bir **ServiceBusService nesnesi** oluşturur. Azure modülünün içe aktarılaması için bildirimden sonra **server.js** dosyasının üst bölümüne ekleyin:

```javascript
var serviceBusService = azure.createServiceBusService();
```

`createQueueIfNotExists` **ServiceBusService** nesnesini arayarak, belirtilen sıra döndürülür (varsa) veya belirtilen ada sahip yeni bir sıra oluşturulur. Aşağıdaki kod, `createQueueIfNotExists` adlı `myqueue`kuyruğa oluşturmak veya bağlanmak için kullanır:

```javascript
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

Yöntem, `createServiceBusService` ileti süresi veya maksimum sıra boyutu gibi varsayılan sıra ayarlarını geçersiz kılmanızı sağlayan ek seçenekleri de destekler. Aşağıdaki örnekte, en fazla sıra boyutu 5 GB ve 1 dakikalık bir yaşam süresi (TTL) değeri olarak ayarlar:

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
İsteğe bağlı filtreleme işlemleri **ServiceBusService**kullanılarak gerçekleştirilen işlemlere uygulanabilir. Filtreleme işlemleri günlüğe kaydetmeyi, otomatik olarak yeniden denemeyi vb. içerebilir. Filtreler, imzaile birlikte bir yöntem uygulayan nesnelerdir:

```javascript
function handle (requestOptions, next)
```

İstek seçenekleri üzerinde ön işleme yaptıktan sonra, `next`yöntem çağırmalıdır , aşağıdaki imza ile bir geri arama geçen:

```javascript
function (returnObject, finalCallback, next)
```

Bu geri aramada ve `returnObject` (istekten sunucuya gelen yanıt) işledikten sonra, geri aramanın diğer filtreleri işlemeye devam etmek için varsa çağırması `next` veya hizmet çağrısını sona erdirene çağrıda bulunması `finalCallback`gerekir.

Yeniden deneme mantığını uygulayan iki filtre, `ExponentialRetryPolicyFilter` Düğüm.js için Azure SDK'ya dahildir ve `LinearRetryPolicyFilter`. Aşağıdaki kod kullanan `ServiceBusService` bir nesne `ExponentialRetryPolicyFilter`oluşturur:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Hizmet Veri Servisi kuyruğuna ileti göndermek için, `sendQueueMessage` uygulamanız **ServiceBusService** nesnesindeki yöntemi çağırır. Hizmet Veri Hizmetleri sıralarına gönderilen (ve alınan) Iletiler **Aracılı İleti** nesneleridir ve özel uygulamaya özgü özellikleri tutmak için kullanılan bir sözlük ve rasgele uygulama verileri gövdesi olan bir dizi standart özelliklere **(Label** ve **TimeToLive**gibi) sahiptir. Bir uygulama ileti olarak bir dize geçirerek iletinin gövdesini ayarlayabilirsiniz. Gerekli standart özellikler varsayılan değerlerle doldurulur.

Aşağıdaki örnek, aşağıdaki kullanarak adlı `myqueue` kuyruğa `sendQueueMessage`nasıl bir test iletisi gönderilen gösterir:

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

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısında bir sınır yoktur, ancak bir kuyruk tarafından tutulan iletilerin toplam boyutunda bir kapak vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotalar hakkında daha fazla bilgi için [Servis Otobüsü kotalarına][Service Bus quotas]bakın.

## <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma
İletiler `receiveQueueMessage` **ServiceBusService** nesnesindeki yöntemi kullanarak bir kuyruktan alınır. Varsayılan olarak, iletiler okundukça kuyruktan silinir; ancak, isteğe bağlı parametreyi `isPeekLock` **doğru**ayarlayarak iletiyi sıradan silmeden okuyabilir (peek) ve kilitleyebilirsiniz.

Alma işleminin bir parçası olarak iletiyi okuma ve silme nin varsayılan davranışı en basit modeldir ve bir hata oluştuğunda bir iletiyi işlememeye tahammül edemediği senaryolar için en iyi şekilde çalışır. Bu davranışı anlamak için, tüketicinin alma isteğini sorunları ve işlemeden önce çöktüğün bir senaryoyu düşünün. Servis Veri Kurumu iletinin tüketildiği olarak işaretlenmiş olduğundan, uygulama yeniden başlatıldığında ve iletileri yeniden tüketmeye başladığında, kilitlenmeden önce tüketilen iletiyi kaçırmış olur.

`isPeekLock` Parametre **doğru**olarak ayarlanırsa, receive iki aşamalı bir işlem haline gelir ve bu da eksik iletileri tolere edemeyen uygulamaları desteklemeyi mümkün kılar. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi bitirdikten (veya gelecekteki işlemler için güvenilir bir şekilde depoladıktan) yöntemi arayarak `deleteMessage` ve iletinin parametre olarak silineceğini sağlayarak alma işleminin ikinci aşamasını tamamlar. Yöntem, `deleteMessage` iletinin tüketildiğini işaretler ve sıradan kaldırır.

Aşağıdaki örnek, iletilerin nasıl alınıp `receiveQueueMessage`işlenirken yapılacağını gösterir. Örnek önce bir iletiyi alır ve siler, sonra **true**ayarını `deleteMessage`kullanarak `isPeekLock` bir ileti alır, sonra aşağıdakileri kullanarak iletiyi siler:

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
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bir alıcı uygulaması iletiyi nedense işleyemiyorsa, `unlockMessage` **ServiceBusService** nesnesindeki yöntemi çağırabilir. Hizmet Veri Servisi'nin sıra içindeki iletiyi açmasına ve aynı alıcı uygulama veya başka bir tüketen uygulama tarafından yeniden alınmaya hazır hale getirmesine neden olur.

Sıra içinde kilitli bir iletiyle ilişkili bir zaman aşımı da vardır ve uygulama kilit süresi sona ermeden önce iletiyi işlemezse (örneğin, uygulama çöküyorsa), Servis Veri Servisi iletiyi otomatik olarak açar ve tekrar alınabilir.

Uygulamanın iletiyi işledikten sonra ancak `deleteMessage` yöntem çağrılmadan önce çökmesi durumunda, ileti yeniden başlatıldığında uygulamaya yeniden teslim edilir. Bu yaklaşım genellikle *En Az Bir Kez İşleme*olarak adlandırılır, yani, her ileti en az bir kez işlenir, ancak bazı durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işleme tolere edemiyorsa, uygulama geliştiricileri yinelenen ileti teslimişlemek için kendi uygulama ek mantık eklemelidir. Genellikle iletinin **MessageId** özelliği kullanılarak elde edilmiştir ve bu özellik teslim girişimleri arasında sabit kalır.

> [!NOTE]
> [Servis](https://github.com/paolosalvatori/ServiceBusExplorer/)Veri Servisi Explorer ile Servis Veri Servisi kaynaklarını yönetebilirsiniz. Service Bus Explorer, kullanıcıların bir Service Bus ad alanına bağlanmasına ve ileti varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç, alma/dışa aktarma işlevselliği veya konuyu, kuyrukları, abonelikleri, geçiş hizmetlerini, bildirim hub'larını ve olay hub'larını test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Kuyruklar hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın.

* [Kuyruklar, konular ve abonelikler][Queues, topics, and subscriptions]
* GitHub'da Düğüm deposu [için Azure SDK][Azure SDK for Node]
* [Node.js Geliştirici Merkezi](https://azure.microsoft.com/develop/nodejs/)

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com

[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Create and deploy a Node.js application to an Azure Website]: ../app-service/app-service-web-get-started-nodejs.md
[Node.js Web Application with Storage]:../cosmos-db/table-storage-how-to-use-nodejs.md
[Service Bus quotas]: service-bus-quotas.md
