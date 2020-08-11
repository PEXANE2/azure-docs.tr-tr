---
title: Azure/Service-Bus Node.js paketiyle Azure Service Bus konuları kullanma
description: Azure 'da Azure/Service-Bus paketini kullanarak bir Node.js uygulamadan Azure 'daki Service Bus konu ve abonelikleri nasıl kullanacağınızı öğrenin.
author: spelluru
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 08/09/2020
ms.author: spelluru
ms.custom: devx-track-javascript
ms.openlocfilehash: fa1f6738628ed96e386186a579569170bfaac3ee
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88066959"
---
# <a name="quickstart-how-to-use-service-bus-topics-and-subscriptions-with-nodejs-and-the-azure-sb-package"></a>Hızlı başlangıç: Node.js ve Azure-SB paketiyle Service Bus konuları ve abonelikleri kullanma
Bu öğreticide, Service Bus bir konuya ileti göndermek ve [Azure-SB](https://www.npmjs.com/package/azure-sb) paketini kullanarak bir Service Bus aboneliğinden ileti almak için Node.js uygulamalar oluşturmayı öğreneceksiniz. Örnekler JavaScript 'te yazılır ve paketi dahili olarak kullanan Node.js [Azure modülünü](https://www.npmjs.com/package/azure) kullanır `azure-sb` .

> [!IMPORTANT]
> [Azure-SB](https://www.npmjs.com/package/azure-sb) PAKETI [Service Bus Rest çalışma zamanı API 'lerini](/rest/api/servicebus/service-bus-runtime-rest)kullanır. [@azure/service-bus](https://www.npmjs.com/package/@azure/service-bus)Daha hızlı [amqp 1,0 protokolünü](service-bus-amqp-overview.md)kullanan yeni paketini kullanarak daha hızlı bir deneyim edinebilirsiniz. 
> 
> Yeni paket hakkında daha fazla bilgi edinmek için bkz. [Service Bus konuları ve abonelikleri Node.js ve @azure/service-bus paketiyle kullanma](./service-bus-nodejs-how-to-use-topics-subscriptions-new-package.md), aksi takdirde okumaya devam etme, [Azure](https://www.npmjs.com/package/azure) paketini nasıl kullanacağınızı öğrenmek için.

Burada ele alınan senaryolar şunlardır:

- Konular ve abonelikler oluşturma 
- Abonelik filtreleri oluşturma 
- Konuya ileti gönderme 
- Bir abonelikten ileti alma
- Konuları ve abonelikleri silme 

Konular ve abonelikler hakkında daha fazla bilgi için, [sonraki adımlar](#next-steps) bölümüne bakın.

## <a name="prerequisites"></a>Önkoşullar
- Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [Visual Studio veya MSDN abonesi avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)için kaydolabilirsiniz.
- Hızlı başlangıç içindeki adımları izleyin: bir Service Bus **ad alanı** oluşturmak ve **bağlantı dizesini**almak için [Azure Portal Service Bus konu başlığı ve abonelikleri oluşturmak üzere kullanın](service-bus-quickstart-topics-subscriptions-portal.md) .

    > [!NOTE]
    > Bu hızlı başlangıçta **Node.js** kullanarak konu için bir **Konu** ve bir **abonelik** oluşturacaksınız. 

## <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma
Boş bir Node.js uygulaması oluşturun. Node.js uygulaması oluşturma hakkında yönergeler için bkz. [Azure Web sitesine Node.js uygulama oluşturma ve dağıtma], Windows PowerShell kullanarak [bulut hizmetiNode.js][Node.js Cloud Service] veya WebMatrix ile Web sitesi kullanma.

## <a name="configure-your-application-to-use-service-bus"></a>Uygulamanızı kullanmak için yapılandırma Service Bus
Service Bus kullanmak için Node.js Azure paketini indirin. Bu paket, Service Bus REST hizmetleriyle iletişim kuran bir kitaplıklar kümesi içerir.

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
3. Bir **düğüm \_ modülleri** klasörünün oluşturulduğunu doğrulamak için **ls** komutunu el ile çalıştırabilirsiniz. Bu klasörün içinde, Service Bus konularına erişmeniz gereken kitaplıkları içeren **Azure** paketini bulun.

### <a name="import-the-module"></a>Modülü içeri aktar
Not defteri veya başka bir metin düzenleyicisi kullanarak, uygulamanın **server.js** dosyasının en üstüne aşağıdakileri ekleyin:

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Service Bus bağlantısını ayarlama
Azure modülü, `AZURE_SERVICEBUS_CONNECTION_STRING` [önkoşulların](#prerequisites)bir parçası olarak elde ettiğiniz bağlantı dizesinin ortam değişkenini okur. Bağlantı dizesini yeniden alma yönergelerine ihtiyacınız varsa, bkz. [bağlantı dizesini alma](service-bus-quickstart-topics-subscriptions-portal.md#get-the-connection-string). Bu ortam değişkeni ayarlanmamışsa, çağırırken hesap bilgilerini belirtmeniz gerekir `createServiceBusService` .

Bir Azure bulut hizmeti için ortam değişkenlerini ayarlamayla ilgili bir örnek için bkz. [ortam değişkenlerini ayarlama](../container-instances/container-instances-environment-variables.md#azure-cli-example).



## <a name="create-a-topic"></a>Konu başlığı oluşturma
**Servicebusservice** nesnesi konularda çalışmanıza olanak sağlar. Aşağıdaki kod bir **Servicebusservice** nesnesi oluşturur. Azure modülünü içeri aktarma ifadesinden sonra **server.js** dosyanın üst kısmına yakın bir şekilde ekleyin:

```javascript
var serviceBusService = azure.createServiceBusService();
```

`createTopicIfNotExists` **Servicebusservice** nesnesinde çağrı yaparsanız, belirtilen konu (varsa) döndürülür veya belirtilen ada sahip yeni bir konu oluşturulur. Aşağıdaki kod, `createTopicIfNotExists` adlı konuyu oluşturmak veya bu konuya bağlanmak için kullanır `MyTopic` :

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

`createTopicIfNotExists`Yöntemi Ayrıca, ileti Süresi canlı veya en büyük konu boyutu gibi varsayılan konu ayarlarını geçersiz kılmanızı sağlayan ek seçenekleri de destekler. 

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

### <a name="filters"></a>FilTReleri
İsteğe bağlı filtreleme işlemleri, **Servicebusservice**kullanılarak gerçekleştirilen işlemlere uygulanabilir. Filtreleme işlemleri, günlüğe kaydetme, otomatik yeniden deneme vb. içerebilir. Filtreler imzaya sahip bir yöntemi uygulayan nesnelerdir:

```javascript
function handle (requestOptions, next)
```

İstek seçeneklerinde ön işleme gerçekleştirildikten sonra yöntemi çağırır `next` ve aşağıdaki imzayla bir geri çağırma işlemi geçirir:

```javascript
function (returnObject, finalCallback, next)
```

Bu geri çağırmada ve `returnObject` (istekten sunucuya olan yanıt) işlendikten sonra geri çağırma işlemi, diğer filtreleri işlemeye devam etmek için bir sonraki (varsa) veya `finalCallback` hizmet çağrısını bitirmek için Invoke olmalıdır.

Yeniden deneme mantığı uygulayan iki filtre (**ExponentialRetryPolicyFilter** ve **LinearRetryPolicyFilter**), Node.js için Azure SDK’sına dahil edilir. Aşağıdaki kod, **üs Alretrypolicyfilter**kullanan bir **servicebusservice** nesnesi oluşturur:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a>Abonelik oluşturma
Konu abonelikleri **Servicebusservice** nesnesiyle de oluşturulur. Abonelikler adlandırılır ve aboneliğin sanal kuyruğuna teslim edilen ileti kümesini kısıtlayan isteğe bağlı bir filtreye sahip olabilir.

> [!NOTE]
> Varsayılan olarak, abonelikler ya da ilişkili oldukları konu tamamlanana kadar kalıcıdır. Uygulamanız bir abonelik oluşturmak için mantık içeriyorsa, öncelikle yöntemi kullanılarak aboneliğin mevcut olup olmadığını denetlemelidir `getSubscription` .
>
> Otomatik [Deleteonıdle özelliği](/javascript/api/@azure/arm-servicebus/sbsubscription?view=azure-node-latest#autodeleteonidle)ayarlanarak aboneliklerin otomatik olarak silinmesini sağlayabilirsiniz.

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

Bir aboneliğe, `createRule` **Servicebusservice** nesnesinin yöntemi kullanılarak Filtreler eklenebilir. Bu yöntem, mevcut bir aboneliğe yeni filtreler eklemenize olanak tanır.

> [!NOTE]
> Varsayılan filtre tüm yeni aboneliklere otomatik olarak uygulandığından, ilk olarak varsayılan filtreyi kaldırmanız gerekir veya **Matchall** , belirtebileceğiniz diğer filtreleri geçersiz kılar. `deleteRule` **Servicebusservice** nesnesinin yöntemini kullanarak varsayılan kuralı kaldırabilirsiniz.
>
>

Aşağıdaki örnek, `HighMessages` yalnızca 3 ' ten büyük özel bir özelliği olan iletileri seçen **sqlfilter** ile adlı bir abonelik oluşturur `messagenumber` :

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

Benzer şekilde, aşağıdaki örnekte, `LowMessages` yalnızca 3 ' ten küçük veya buna eşit bir özelliği olan iletileri seçen **sqlfilter** ile adlı bir abonelik oluşturulur `messagenumber` :

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

' A bir ileti gönderildiğinde `MyTopic` , konu aboneliğine abone olan alıcıların alıcıya teslim edilir `AllMessages` ve `HighMessages` ve `LowMessages` Konu aboneliklerine (ileti içeriğine bağlı olarak) abone olan alıcılar için seçmeli olarak dağıtılır.

## <a name="how-to-send-messages-to-a-topic"></a>Konuya ileti gönderme
Service Bus bir konuya ileti göndermek için uygulamanızın `sendTopicMessage` **Servicebusservice** nesnesinin yöntemini kullanması gerekir.
Service Bus konularına gönderilen iletiler **aracılı Edmessage** nesneleridir.
**Brokeredmessage** nesneleri, bir dizi standart Özellik ( `Label` ve gibi `TimeToLive` ), uygulamaya özgü özel özellikleri tutmak için kullanılan bir sözlük ve bir dize verileri gövdesi vardır. Bir uygulama, bir dize değeri öğesine geçirerek ileti gövdesini ayarlayabilir `sendTopicMessage` ve gerekli standart özellikler varsayılan değerlere göre doldurulur.

Aşağıdaki örnek, öğesine beş sınama iletisinin nasıl gönderileceğini gösterir `MyTopic` . `messagenumber`Her iletinin Özellik değeri, döngünün yinelemesinde farklılık gösterir (Bu özellik hangi aboneliklerin bu uygulamayı alacağını belirler):

```javascript
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (var i = 0; i < 5; i++) {
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
İleti, `receiveSubscriptionMessage` **servicebusservice** nesnesinde yöntemi kullanılarak bir abonelikten alınır. Varsayılan olarak, iletiler okunduklarında abonelikten silinir. Bununla birlikte, `isPeekLock` okumak (Peek) için isteğe bağlı parametresini **true** olarak ayarlayabilir ve iletiyi aboneliğden silmeden kilitleyemezsiniz.

İletiyi alma işleminin bir parçası olarak okuma ve silmenin varsayılan davranışı en basit modeldir ve bir hata olduğunda bir uygulamanın bir iletiyi işlememesinin kabul edebildiği senaryolar için en iyi sonucu verir. Bu davranışı anlamak için, tüketicinin alma isteğini yaptığı ve işlemeden önce çöktüğü bir senaryo düşünün. Service Bus ileti tüketildiği gibi işaretlendiğinden, uygulama yeniden başlatıldığında ve iletileri yeniden kullanmaya başladığında, kilitlenme öncesinde tüketilen iletiyi kaçırmıştır.

`isPeekLock`Parametresi **true**olarak ayarlanırsa, alma işlemi iki aşamalı bir işlem haline gelir ve bu, kaçırılan iletileri kabul edemediği uygulamaları desteklemeyi olanaklı kılar. Service Bus bir istek aldığında, kullanmak üzere bir sonraki iletiyi bulur, diğer tüketicilerin bunu almasını engellemek için onu kilitler ve uygulamaya döndürür.
Uygulama iletiyi işledikten sonra (veya gelecekteki işleme için güvenilir bir şekilde saklarsa), **deleteMessage** yöntemini çağırarak alma işleminin ikinci aşamasını tamamlar ve bir parametre olarak silinecek iletiyi geçirir. **DeleteMessage** yöntemi iletiyi tüketilen şekilde işaretler ve abonelikten kaldırır.

Aşağıdaki örnek, iletilerinin kullanılarak nasıl alınıp işlenebileceğinizi gösterir `receiveSubscriptionMessage` . Örnek öncelikle ' LowMessages ' aboneliğinden bir ileti alır ve siler ve ardından ' HighMessages ' aboneliğinden true olarak ayarla ' yı kullanarak bir ileti alır `isPeekLock` . Ardından, şunu kullanarak iletiyi siler `deleteMessage` :

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
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bir alıcı uygulaması iletiyi bir nedenden dolayı işleyebilirse, `unlockMessage` **Servicebusservice** nesnesinde yöntemi çağırabilir. Bu yöntem Service Bus, aboneliğin içindeki iletinin kilidini açıp yeniden alınmasını sağlar. Bu örnekte, aynı uygulama ya da başka bir uygulama tarafından kullanılıyor.

Ayrıca, abonelik içinde kilitlenen bir iletiyle ilişkili bir zaman aşımı da vardır. Uygulama, kilit zaman aşımı dolmadan önce iletiyi işleyemezse (örneğin, uygulama kilitlenirse), Service Bus otomatik olarak iletinin kilidini açar ve yeniden alınmak üzere kullanılabilir hale gelir.

İleti işlendikten sonra uygulamanın çöktüğü durumda `deleteMessage` , ancak yöntem çağrılmadan önce, yeniden başlatıldığında ileti uygulamaya yeniden gönderilir. Bu davranış genellikle *en az bir kez işleme*olarak adlandırılır. Diğer bir deyişle, her ileti en az bir kez işlenir, ancak bazı durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işleme kabul edemeyebilir, yinelenen ileti teslimini işlemek için uygulamanıza mantık eklemeniz gerekir. İletinin **MessageID** özelliğini kullanabilirsiniz. Bu, teslim girişimleri arasında sabitde kalır.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme
[Oto Deleteonıdle özelliği](/javascript/api/@azure/arm-servicebus/sbsubscription?view=azure-node-latest#autodeleteonidle) ayarlanmadıkça konular ve abonelikler kalıcıdır ve [Azure Portal][Azure portal] veya program aracılığıyla açıkça silinmemelidir.
Aşağıdaki örnek, adlı konunun nasıl silineceğini göstermektedir `MyTopic` :

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir. Ayrıca, abonelikler bağımsız olarak da silinebilir. Aşağıdaki örnek, konusunun adlı bir aboneliğin nasıl silineceğini göstermektedir `HighMessages` `MyTopic` :

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
[Azure Web sitesinde Node.js uygulaması oluşturma ve dağıtma]: ../app-service/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md