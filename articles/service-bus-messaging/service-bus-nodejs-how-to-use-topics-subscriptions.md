---
title: Azure/Service-Bus Node. js paketiyle Azure Service Bus konular kullanma
description: Azure 'da Azure/Service-Bus paketini kullanarak bir Node. js uygulamasından Azure 'da Service Bus konuları ve abonelikleri nasıl kullanacağınızı öğrenin.
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
ms.openlocfilehash: c85b63b4a56e74b0fef9a122ec995b4106496cbe
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78330455"
---
# <a name="quickstart-how-to-use-service-bus-topics-and-subscriptions-with-nodejs-and-the-azure-sb-package"></a>Hızlı başlangıç: node. js ve Azure-SB paketiyle Service Bus konuları ve abonelikleri kullanma
Bu öğreticide, [Azure-SB](https://www.npmjs.com/package/azure-sb) paketini kullanarak bir Service Bus konusuna ileti göndermek ve Service Bus aboneliğinden ileti almak için Node. js uygulamaları oluşturmayı öğreneceksiniz. Örnekler JavaScript 'te yazılır ve `azure-sb` paketi dahili olarak kullanan Node. js [Azure modülünü](https://www.npmjs.com/package/azure) kullanır.

> [!IMPORTANT]
> [Azure-SB](https://www.npmjs.com/package/azure-sb) PAKETI [Service Bus Rest çalışma zamanı API 'lerini](/rest/api/servicebus/service-bus-runtime-rest)kullanır. Daha hızlı [@azure/service-bus](https://www.npmjs.com/package/@azure/service-bus) [AMQP 1,0 protokolünü](service-bus-amqp-overview.md)kullanan yeni paketini kullanarak daha hızlı bir deneyim edinebilirsiniz. 
> 
> Yeni paket hakkında daha fazla bilgi edinmek için bkz. [Node. js ve @azure/service-bus Package ile Service Bus konuları ve abonelikleri kullanma](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-nodejs-how-to-use-topics-subscriptions-new-package), aksi takdirde okumaya devam etme, [Azure](https://www.npmjs.com/package/azure) paketini nasıl kullanacağınızı öğrenmek için.

Burada ele alınan senaryolar şunlardır:

- Konular ve abonelikler oluşturma 
- Abonelik filtreleri oluşturma 
- Konuya ileti gönderme 
- Bir abonelikten ileti alma
- Konuları ve abonelikleri silme 

Konular ve abonelikler hakkında daha fazla bilgi için, [sonraki adımlar](#next-steps) bölümüne bakın.

## <a name="prerequisites"></a>Ön koşullar
- Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [Visual Studio veya MSDN abonesi avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)için kaydolabilirsiniz.
- Hızlı başlangıç içindeki adımları izleyin: bir Service Bus **ad alanı** oluşturmak ve **bağlantı dizesini**almak için [Azure Portal Service Bus konu başlığı ve abonelikleri oluşturmak üzere kullanın](service-bus-quickstart-topics-subscriptions-portal.md) .

    > [!NOTE]
    > Bu hızlı başlangıçta **Node. js** kullanarak konuya bir **Konu** ve bir **abonelik** oluşturacaksınız. 

## <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma
Boş bir Node. js uygulaması oluşturun. Node. js uygulaması oluşturma hakkında yönergeler için bkz. [Node. js uygulaması oluşturma ve bir Azure Web sitesine], [Node. js bulut hizmetini][Node.js Cloud Service] Windows PowerShell kullanarak veya WebMatrix ile Web sitesi oluşturma ve dağıtma.

## <a name="configure-your-application-to-use-service-bus"></a>Uygulamanızı kullanmak için yapılandırma Service Bus
Service Bus kullanmak için Node. js Azure paketini indirin. Bu paket, Service Bus REST hizmetleriyle iletişim kuran bir kitaplıklar kümesi içerir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Paketi edinmek için düğüm Paket Yöneticisi 'Ni (NPM) kullanın
1. **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (Unix) gibi bir komut satırı arabirimi açın.
2. Örnek uygulamanızı oluşturduğunuz klasöre gidin.
3. Komut penceresine **NPM Azure Install** yazın ve bu, aşağıdaki çıkışa neden olmalıdır:

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
3. Bir **\_düğüm modülleri** klasörünün oluşturulduğunu doğrulamak için **ls** komutunu el ile çalıştırabilirsiniz. Bu klasörün içinde, Service Bus konularına erişmeniz gereken kitaplıkları içeren **Azure** paketini bulun.

### <a name="import-the-module"></a>Modülü içeri aktar
Not defteri veya başka bir metin düzenleyicisi kullanarak, uygulamanın **Server. js** dosyasının en üstüne aşağıdakileri ekleyin:

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Service Bus bağlantısını ayarlama
Azure modülü, önceki adımda elde ettiğiniz `AZURE_SERVICEBUS_CONNECTION_STRING` bağlantı dizesinin ortam değişkenini okur, "kimlik bilgilerini alın." Bu ortam değişkeni ayarlanmamışsa, çağırırken `createServiceBusService`hesap bilgilerini belirtmeniz gerekir.

Bir Azure bulut hizmeti için ortam değişkenlerini ayarlamayla ilgili bir örnek için bkz. [ortam değişkenlerini ayarlama](../container-instances/container-instances-environment-variables.md#azure-cli-example).



## <a name="create-a-topic"></a>Konu başlığı oluşturma
**Servicebusservice** nesnesi konularda çalışmanıza olanak sağlar. Aşağıdaki kod bir **Servicebusservice** nesnesi oluşturur. Azure modülünü içeri aktarma ifadesinden sonra, **Server. js** dosyasının en üstüne yakın bir şekilde ekleyin:

```javascript
var serviceBusService = azure.createServiceBusService();
```

`createTopicIfNotExists` **Servicebusservice** nesnesinde çağrı yaparsanız, belirtilen konu (varsa) döndürülür veya belirtilen ada sahip yeni bir konu oluşturulur. Aşağıdaki kod, adlı `createTopicIfNotExists` `MyTopic`konuyu oluşturmak veya bu konuya bağlanmak için kullanır:

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

`createTopicIfNotExists` Yöntemi Ayrıca, ileti Süresi canlı veya en büyük konu boyutu gibi varsayılan konu ayarlarını geçersiz kılmanızı sağlayan ek seçenekleri de destekler. 

Aşağıdaki örnek, en büyük konu boyutunu bir dakikadan kısa bir süre içinde 5 GB 'a ayarlar:

```javascript
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Filtreler
İsteğe bağlı filtreleme işlemleri, **Servicebusservice**kullanılarak gerçekleştirilen işlemlere uygulanabilir. Filtreleme işlemleri, günlüğe kaydetme, otomatik yeniden deneme vb. içerebilir. Filtreler imzaya sahip bir yöntemi uygulayan nesnelerdir:

```javascript
function handle (requestOptions, next)
```

İstek seçeneklerinde ön işleme gerçekleştirildikten sonra yöntemi çağırır `next`ve aşağıdaki imzayla bir geri çağırma işlemi geçirir:

```javascript
function (returnObject, finalCallback, next)
```

Bu geri çağırmada ve `returnObject` (istekten sunucuya olan yanıt) işlendikten sonra geri çağırma işlemi, diğer filtreleri işlemeye devam etmek için bir sonraki (varsa) veya hizmet çağrısını bitirmek için Invoke `finalCallback` olmalıdır.

Yeniden deneme mantığı uygulayan iki filtre (**ExponentialRetryPolicyFilter** ve **LinearRetryPolicyFilter**), Node.js için Azure SDK’sına dahil edilir. Aşağıdaki kod, **üs Alretrypolicyfilter**kullanan bir **servicebusservice** nesnesi oluşturur:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a>Abonelikleri oluşturma
Konu abonelikleri **Servicebusservice** nesnesiyle de oluşturulur. Abonelikler adlandırılır ve aboneliğin sanal kuyruğuna teslim edilen ileti kümesini kısıtlayan isteğe bağlı bir filtreye sahip olabilir.

> [!NOTE]
> Varsayılan olarak, abonelikler ya da ilişkili oldukları konu tamamlanana kadar kalıcıdır. Uygulamanız bir abonelik oluşturmak için mantık içeriyorsa, öncelikle `getSubscription` yöntemi kullanılarak aboneliğin mevcut olup olmadığını denetlemelidir.
>
> Otomatik [Deleteonıdle özelliği](https://docs.microsoft.com/javascript/api/@azure/arm-servicebus/sbsubscription?view=azure-node-latest#autodeleteonidle)ayarlanarak aboneliklerin otomatik olarak silinmesini sağlayabilirsiniz.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma
**Matchall** filtresi, bir abonelik oluşturulduğunda kullanılan varsayılan filtredir. **MatchAll** filtresini kullandığınızda konu başlığında yayımlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnek AllMessages adlı bir abonelik oluşturur ve varsayılan **Matchall** filtresini kullanır.

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Filtre içeren abonelik oluşturma
Ayrıca, bir konuya gönderilen iletilerin, belirli bir konu aboneliği içinde gösterilmesi gerektiğini kapsamlarına izin veren filtreler de oluşturabilirsiniz.

Abonelikler tarafından desteklenen en esnek filtre türü, SQL92 alt kümesini uygulayan **Sqlfilter**' dir. SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. Bir SQL filtresiyle kullanılabilecek ifadeler hakkında daha fazla ayrıntı için [sqlfilter. SqlExpression][SqlFilter.SqlExpression] söz dizimini gözden geçirin.

Bir aboneliğe, `createRule` **servicebusservice** nesnesinin yöntemi kullanılarak Filtreler eklenebilir. Bu yöntem, mevcut bir aboneliğe yeni filtreler eklemenize olanak tanır.

> [!NOTE]
> Varsayılan filtre tüm yeni aboneliklere otomatik olarak uygulandığından, ilk olarak varsayılan filtreyi kaldırmanız gerekir veya **Matchall** , belirtebileceğiniz diğer filtreleri geçersiz kılar. `deleteRule` **Servicebusservice** nesnesinin yöntemini kullanarak varsayılan kuralı kaldırabilirsiniz.
>
>

Aşağıdaki örnek, yalnızca 3 ' ten `HighMessages` büyük özel `messagenumber` bir özelliği olan iletileri seçen **sqlfilter** ile adlı bir abonelik oluşturur:

```javascript
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'HighMessages',
            'HighMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Benzer şekilde, aşağıdaki örnekte, yalnızca 3 ' `LowMessages` ten küçük veya buna eşit bir `messagenumber` özelliği olan iletileri seçen **sqlfilter** ile adlı bir abonelik oluşturulur:

```javascript
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages',
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME,
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic',
            'LowMessages',
            'LowMessageFilter',
            ruleOptions,
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

`MyTopic`' A bir ileti gönderildiğinde, `AllMessages` konu aboneliğine abone olan alıcıların alıcıya teslim edilir ve `HighMessages` ve `LowMessages` konu aboneliklerine (ileti içeriğine bağlı olarak) abone olan alıcılar için seçmeli olarak dağıtılır.

## <a name="how-to-send-messages-to-a-topic"></a>Konuya ileti gönderme
Service Bus bir konuya ileti göndermek için uygulamanızın `sendTopicMessage` **servicebusservice** nesnesinin yöntemini kullanması gerekir.
Service Bus konularına gönderilen iletiler **aracılı Edmessage** nesneleridir.
**Brokeredmessage** nesneleri, bir dizi standart Özellik ( `Label` ve `TimeToLive`gibi), uygulamaya özgü özel özellikleri tutmak için kullanılan bir sözlük ve bir dize verileri gövdesi vardır. Bir uygulama, `sendTopicMessage` bir dize değeri öğesine geçirerek ileti gövdesini ayarlayabilir ve gerekli standart özellikler varsayılan değerlere göre doldurulur.

Aşağıdaki örnek, öğesine `MyTopic`beş sınama iletisinin nasıl gönderileceğini gösterir. Her `messagenumber` iletinin Özellik değeri, döngünün yinelemesinde farklılık gösterir (Bu özellik hangi aboneliklerin bu uygulamayı alacağını belirler):

```javascript
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Bir konuda tutulan ileti sayısında bir sınır yoktur, ancak bir konu tarafından tutulan iletilerin toplam boyutu için bir sınır vardır. Bu konu başlığı boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir.

## <a name="receive-messages-from-a-subscription"></a>Bir abonelikten ileti alma
İleti, `receiveSubscriptionMessage` **servicebusservice** nesnesinde yöntemi kullanılarak bir abonelikten alınır. Varsayılan olarak, iletiler okunduklarında abonelikten silinir. Bununla birlikte, okumak (Peek) için `isPeekLock` isteğe bağlı parametresini **true** olarak ayarlayabilir ve iletiyi aboneliğden silmeden kilitleyemezsiniz.

İletiyi alma işleminin bir parçası olarak okuma ve silmenin varsayılan davranışı en basit modeldir ve bir hata olduğunda bir uygulamanın bir iletiyi işlememesinin kabul edebildiği senaryolar için en iyi sonucu verir. Bu davranışı anlamak için, tüketicinin alma isteğini yaptığı ve işlemeden önce çöktüğü bir senaryo düşünün. Service Bus ileti tüketildiği gibi işaretlendiğinden, uygulama yeniden başlatıldığında ve iletileri yeniden kullanmaya başladığında, kilitlenme öncesinde tüketilen iletiyi kaçırmıştır.

`isPeekLock` Parametresi **true**olarak ayarlanırsa, alma işlemi iki aşamalı bir işlem haline gelir ve bu, kaçırılan iletileri kabul edemediği uygulamaları desteklemeyi olanaklı kılar. Service Bus bir istek aldığında, kullanmak üzere bir sonraki iletiyi bulur, diğer tüketicilerin bunu almasını engellemek için onu kilitler ve uygulamaya döndürür.
Uygulama iletiyi işledikten sonra (veya gelecekteki işleme için güvenilir bir şekilde saklarsa), **deleteMessage** yöntemini çağırarak alma işleminin ikinci aşamasını tamamlar ve bir parametre olarak silinecek iletiyi geçirir. **DeleteMessage** yöntemi iletiyi tüketilen şekilde işaretler ve abonelikten kaldırır.

Aşağıdaki örnek, iletilerinin kullanılarak `receiveSubscriptionMessage`nasıl alınıp işlenebileceğinizi gösterir. Örnek öncelikle ' LowMessages ' aboneliğinden bir ileti alır ve siler ve ardından ' HighMessages ' aboneliğinden true olarak ayarla ' yı kullanarak `isPeekLock` bir ileti alır. Ardından, şunu kullanarak `deleteMessage`iletiyi siler:

```javascript
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        })
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bir alıcı uygulaması iletiyi bir nedenden dolayı işleyebilirse, `unlockMessage` **servicebusservice** nesnesinde yöntemi çağırabilir. Bu yöntem Service Bus, aboneliğin içindeki iletinin kilidini açıp yeniden alınmasını sağlar. Bu örnekte, aynı uygulama ya da başka bir uygulama tarafından kullanılıyor.

Ayrıca, abonelik içinde kilitlenen bir iletiyle ilişkili bir zaman aşımı da vardır. Uygulama, kilit zaman aşımı dolmadan önce iletiyi işleyemezse (örneğin, uygulama kilitlenirse), Service Bus otomatik olarak iletinin kilidini açar ve yeniden alınmak üzere kullanılabilir hale gelir.

İleti işlendikten sonra uygulamanın çöktüğü durumda, ancak `deleteMessage` Yöntem çağrılmadan önce, yeniden başlatıldığında ileti uygulamaya yeniden gönderilir. Bu davranış genellikle *en az bir kez işleme*olarak adlandırılır. Diğer bir deyişle, her ileti en az bir kez işlenir, ancak bazı durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işleme kabul edemeyebilir, yinelenen ileti teslimini işlemek için uygulamanıza mantık eklemeniz gerekir. İletinin **MessageID** özelliğini kullanabilirsiniz. Bu, teslim girişimleri arasında sabitde kalır.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme
[Oto Deleteonıdle özelliği](https://docs.microsoft.com/javascript/api/@azure/arm-servicebus/sbsubscription?view=azure-node-latest#autodeleteonidle) ayarlanmadıkça konular ve abonelikler kalıcıdır ve [Azure Portal][Azure portal] veya program aracılığıyla açıkça silinmemelidir.
Aşağıdaki örnek, adlı `MyTopic`konunun nasıl silineceğini göstermektedir:

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir. Ayrıca, abonelikler bağımsız olarak da silinebilir. Aşağıdaki örnek, `HighMessages` `MyTopic` konusunun adlı bir aboneliğin nasıl silineceğini göstermektedir:

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

> [!NOTE]
> Service Bus kaynaklarını [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/)ile yönetebilirsiniz. Service Bus gezgin, kullanıcıların bir Service Bus ad alanına bağlanmasına ve mesajlaşma varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç içeri/dışarı aktarma işlevselliği gibi gelişmiş özellikler ya da konu, kuyruk, abonelik, geçiş Hizmetleri, Bildirim Hub 'ları ve Olay Hub 'larını test etme yeteneği sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Service Bus konuların temellerini öğrendiğinize göre, daha fazla bilgi edinmek için bu bağlantıları izleyin.

* Bkz. [Kuyruklar, konular ve abonelikler][Queues, topics, and subscriptions].
* [SqlFilter][SqlFilter] için API başvurusu
* GitHub 'daki [düğüm deposu Için Azure SDK 'sını][Azure SDK for Node] ziyaret edin.

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /javascript/api/@azure/arm-servicebus/sqlfilter?view=azure-node-latest
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Bir Node. js uygulamasını bir Azure Web sitesinde oluşturma ve dağıtma]: ../app-service/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md

