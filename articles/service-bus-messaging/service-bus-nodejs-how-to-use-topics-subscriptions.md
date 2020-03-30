---
title: Azure/servis veri meskeni Node.js paketiyle Azure Hizmet Veri Servisi konularını kullanma
description: Azure/servis veri yolu paketini kullanarak Bir Düğüm.js uygulamasından Azure'daki Hizmet Veri Yolu konularını ve aboneliklerini nasıl kullanacağınızı öğrenin.'
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
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78330455"
---
# <a name="quickstart-how-to-use-service-bus-topics-and-subscriptions-with-nodejs-and-the-azure-sb-package"></a>Quickstart: Node.js ve azure-sb paketi ile Hizmet Veri Servisi konuları ve abonelikleri nasıl kullanılır?
Bu eğitimde, bir Hizmet Veri Yolu konusuna ileti göndermek ve [azure-sb](https://www.npmjs.com/package/azure-sb) paketini kullanarak bir Servis Veri Yolu aboneliğinden ileti almak için Node.js uygulamalarını nasıl oluşturabileceğinizi öğrenirsiniz. Örnekler JavaScript ile yazılır ve paketi dahili olarak kullanan Düğüm.js [Azure modüllerini](https://www.npmjs.com/package/azure) `azure-sb` kullanır.

> [!IMPORTANT]
> [Azure-sb](https://www.npmjs.com/package/azure-sb) [paketi, Service Bus REST çalışma zamanı API'lerini](/rest/api/servicebus/service-bus-runtime-rest)kullanır. Daha hızlı [@azure/service-bus](https://www.npmjs.com/package/@azure/service-bus) [AMQP 1.0 protokolünü](service-bus-amqp-overview.md)kullanan yeni paketi kullanarak daha hızlı bir deneyim elde edebilirsiniz. 
> 
> Yeni paket hakkında daha fazla bilgi edinmek için [Node.js ve paketi @azure/service-bus yle Hizmet Veri Yolu konularını ve aboneliklerini nasıl kullanacağınızı](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-nodejs-how-to-use-topics-subscriptions-new-package)öğrenin , aksi takdirde [azure](https://www.npmjs.com/package/azure) paketinin nasıl kullanılacağını görmek için okumaya devam edin.

Burada kapsanan senaryolar şunlardır:

- Konu ve abonelik oluşturma 
- Abonelik filtreleri oluşturma 
- Bir konuya ileti gönderme 
- Abonelikten ileti alma
- Konuları ve abonelikleri silme 

Konular ve abonelikler hakkında daha fazla bilgi için [Sonraki adımlar bölümüne](#next-steps) bakın.

## <a name="prerequisites"></a>Ön koşullar
- Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Visual Studio [veya MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) etkinleştirebilir veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)kaydolabilirsiniz.
- Quickstart'taki adımları izleyin: Hizmet Veri Yolunda niçin [bir konu oluşturmak için Azure portalını kullanın ve bir](service-bus-quickstart-topics-subscriptions-portal.md) Hizmet Veri Servisi ad **alanı** oluşturmak ve bağlantı **dizesini**almak için konuya abonelikler oluşturun.

    > [!NOTE]
    > Bu hızlı başlangıçta **Düğüm.js'yi** kullanarak bir **konu** ve konuya **abonelik** oluşturacaksınız. 

## <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma
Boş bir Düğüm.js uygulaması oluşturun. Bir [Node.js Cloud Service][Node.js Cloud Service] [Düğüm.js]uygulaması oluşturma yla ilgili talimatlar için bkz.

## <a name="configure-your-application-to-use-service-bus"></a>Uygulamanızı Servis Veri Aracı'nı kullanacak şekilde yapılandırın
Hizmet Veri Servisi'ni kullanmak için Node.js Azure paketini indirin. Bu paket, Servis Veri Servisi REST hizmetleriyle iletişim kuran bir dizi kitaplık içerir.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Paketi almak için Düğüm Paket Yöneticisi'ni (NPM) kullanın
1. **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (Unix) gibi bir komut satırı arabirimi açın.
2. Örnek uygulamanızı oluşturduğunuz klasöre gidin.
3. Komut penceresine **npm yükleme azure** yazın, bu da aşağıdaki çıktıyla sonuçlanır:

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
3. Düğüm **\_modülleri** klasörü oluşturulduğunu doğrulamak için **lS** komutunu el ile çalıştırabilirsiniz. Bu klasörün içinde, Hizmet Veri Servisi konularına erişmek için gereken kitaplıkları içeren **azure** paketini bulun.

### <a name="import-the-module"></a>Modülü alma
Not Defteri'ni veya başka bir metin düzenleyicisini kullanarak, uygulamanın **server.js** dosyasının üst bölümüne aşağıdakileri ekleyin:

```javascript
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Servis Veri Servisi bağlantısı ayarlama
Azure modülü, önceki `AZURE_SERVICEBUS_CONNECTION_STRING` adımdan elde ettiğiniz bağlantı dizesinin ortam değişkenini okur: "Kimlik bilgilerini edinin." Bu ortam değişkeni ayarlanmazsa, 'yi ararken `createServiceBusService`hesap bilgilerini belirtmeniz gerekir.

Azure Bulut Hizmeti için ortam değişkenlerini ayarlama örneği [için](../container-instances/container-instances-environment-variables.md#azure-cli-example)bkz.



## <a name="create-a-topic"></a>Konu başlığı oluşturma
**ServiceBusService nesnesi** konularla çalışmanızı sağlar. Aşağıdaki kod bir **ServiceBusService nesnesi** oluşturur. Azure modülünün içe aktarılması için bildirimden sonra **server.js** dosyasının üst bölümüne ekleyin:

```javascript
var serviceBusService = azure.createServiceBusService();
```

`createTopicIfNotExists` **ServiceBusService** nesnesini çağırırsanız, belirtilen konu döndürülür (varsa) veya belirtilen ada sahip yeni bir konu oluşturulur. Aşağıdaki kod, `createTopicIfNotExists` adlı `MyTopic`konuyu oluşturmak veya bağlanmak için kullanır:

```javascript
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

Yöntem, `createTopicIfNotExists` ileti süresi veya maksimum konu boyutu gibi varsayılan konu ayarlarını geçersiz kılmanızı sağlayan ek seçenekleri de destekler. 

Aşağıdaki örnek, bir dakika yaşamak için bir süre ile 5 GB maksimum konu boyutunu ayarlar:

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
İsteğe bağlı filtreleme işlemleri **ServiceBusService**kullanılarak gerçekleştirilen işlemlere uygulanabilir. Filtreleme işlemleri günlüğe kaydetmeyi, otomatik olarak yeniden denemeyi vb. içerebilir. Filtreler, imzaile birlikte bir yöntem uygulayan nesnelerdir:

```javascript
function handle (requestOptions, next)
```

İstek seçenekleri üzerinde ön işleme yaptıktan `next`sonra, yöntem çağırır ve aşağıdaki imza ile bir geri arama geçer:

```javascript
function (returnObject, finalCallback, next)
```

Bu geri aramada ve `returnObject` (istekten sunucuya gelen yanıt) işledikten sonra, geri aramanın diğer filtreleri işlemeye `finalCallback` devam etmek için sonraki (varsa) çağırması veya hizmet çağrısını sona erdirmek için çağrıda bulunması gerekir.

Yeniden deneme mantığı uygulayan iki filtre (**ExponentialRetryPolicyFilter** ve **LinearRetryPolicyFilter**), Node.js için Azure SDK’sına dahil edilir. Aşağıdaki **kod, ÜstelRetryPolicyFilter**kullanan bir **ServiceBusService** nesnesi oluşturur:

```javascript
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="create-subscriptions"></a>Abonelikleri oluşturma
Konu abonelikleri **ServiceBusService** nesnesi ile de oluşturulur. Abonelikler adlandırılmış ve aboneliğin sanal kuyruğa teslim edilen ileti kümesini kısıtlayan isteğe bağlı bir filtreye sahip olabilir.

> [!NOTE]
> Varsayılan olarak, abonelikler, onlar veya ilişkili oldukları konu silinene kadar kalıcıdır. Uygulamanız bir abonelik oluşturmak için mantık içeriyorsa, önce aboneliğin var olup olmadığını `getSubscription` yöntemi kullanarak denetlemelidir.
>
> [AutoDeleteOnIdle özelliğini](https://docs.microsoft.com/javascript/api/@azure/arm-servicebus/sbsubscription?view=azure-node-latest#autodeleteonidle)ayarlayarak aboneliklerin otomatik olarak silinmesini sağlayabilirsiniz.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma
**MatchAll** filtresi, abonelik oluşturulduğunda kullanılan varsayılan filtredir. **MatchAll** filtresini kullandığınızda konu başlığında yayımlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnekte AllMessages adında bir abonelik oluşturulur ve varsayılan **MatchAll** filtresi ni kullanır.

```javascript
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Filtre içeren abonelik oluşturma
Ayrıca, bir konuya gönderilen iletilerin belirli bir konu aboneliği içinde gösterilmesini kapsamını genişletmenize olanak tanıyan filtreler de oluşturabilirsiniz.

Abonelikler tarafından desteklenen en esnek filtre türü, SQL92 alt kümesini uygulayan **SqlFilter'dir.** SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. SQL filtresi ile kullanılabilecek ifadeler hakkında daha fazla bilgi için [SqlFilter.SqlExpression][SqlFilter.SqlExpression] sözdizimini inceleyin.

Filtreler `createRule` **ServiceBusService** nesnesinin yöntemini kullanarak aboneye eklenebilir. Bu yöntem, varolan bir aboneye yeni filtreler eklemenize olanak tanır.

> [!NOTE]
> Varsayılan filtre tüm yeni aboneliklere otomatik olarak uygulandığından, önce varsayılan filtreyi kaldırmanız gerekir veya **MatchAll** belirtebileceğin diğer filtreleri geçersiz kılar. `deleteRule` **ServiceBusService** nesnesinin yöntemini kullanarak varsayılan kuralı kaldırabilirsiniz.
>
>

Aşağıdaki örnek, yalnızca 3'ten büyük özel `HighMessages` `messagenumber` bir özel liğe sahip iletileri seçen bir **SqlFilter** ile adında bir abonelik oluşturur:

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

Benzer şekilde, aşağıdaki örnek, yalnızca `LowMessages` 3'ten az veya eşit bir `messagenumber` özelliği olan iletileri seçen bir **SqlFilter** ile adında bir abonelik oluşturur:

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

Bir `MyTopic`ileti şimdi gönderildiğinde, `AllMessages` konu aboneliğine abone olan alıcılara teslim edilir ve seçici olarak `HighMessages` konu `LowMessages` aboneliklerine abone olan alıcılara teslim edilir (ileti içeriğine bağlı olarak).

## <a name="how-to-send-messages-to-a-topic"></a>Bir konuya ileti gönderme
Bir Hizmet Veri Servisi konusuna ileti göndermek için, uygulamanızın `sendTopicMessage` **ServiceBusService** nesnesinin yöntemini kullanması gerekir.
Hizmet Veri Mestomu konularına gönderilen iletiler **Aracılı İleti** nesneleridir.
**Aracılı İleti** nesnelerinde, özel uygulamaya özgü `Label` `TimeToLive`özellikleri tutmak için kullanılan bir sözlük ve dize verileri gövdesi gibi standart özellikler kümesi vardır. Bir uygulama bir dize değeri geçirerek iletinin `sendTopicMessage` gövdesini ayarlayabilir ve gerekli standart özellikler varsayılan değerlere göre doldurulur.

Aşağıdaki örnek, `MyTopic`''ye beş test iletisi nasıl gönderilebildiğini gösterir Her `messagenumber` iletinin özellik değeri döngü yinelemeye göre değişir (bu özellik hangi aboneliklerin alacağını belirler):

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

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Bir konuda tutulan ileti lerin sayısında bir sınır yoktur, ancak bir konu tarafından tutulan iletilerin toplam boyutunda bir sınır vardır. Bu konu başlığı boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir.

## <a name="receive-messages-from-a-subscription"></a>Abonelikten ileti alma
İletiler `receiveSubscriptionMessage` **ServiceBusService** nesnesindeki yöntem kullanılarak bir abonelikten alınır. Varsayılan olarak, iletiler okundukça abonelikten silinir. Ancak, isteğe bağlı parametreyi `isPeekLock` okumak için **doğru** (peek) olarak ayarlayabilir ve iletiyi abonelikten silmeden kilitleyebilirsiniz.

Alma işleminin bir parçası olarak iletiyi okuma ve silme nin varsayılan davranışı en basit modeldir ve bir uygulamanın bir hata olduğunda iletiişlememeye tahammül edemediği senaryolar için en iyi şekilde çalışır. Bu davranışı anlamak için, tüketicinin alma isteğini sorunları ve işlemeden önce çöktüğün bir senaryoyu düşünün. Servis Veri Kurumu iletinin tüketildiği olarak işaretlendiğinden, uygulama yeniden başlatıldığında ve iletileri yeniden almaya başladığında, kilitlenmeden önce tüketilen iletiyi kaçırmıştır.

`isPeekLock` Parametre **doğru**olarak ayarlanırsa, receive iki aşamalı bir işlem haline gelir ve bu da kaçırılan iletileri tolere edemeyen uygulamaları desteklemeyi mümkün kılar. Servis Veri Mes'i bir istek aldığında, bir sonraki iletinin tüketilmesigerektiğini bulur, diğer tüketicilerin almasını önlemek için kilitler ve uygulamaya döndürür.
Uygulama iletiyi işledikten sonra (veya gelecekteki işlemler için güvenilir bir şekilde depoladıktan) **deleteMessage** yöntemini arayarak alma işleminin ikinci aşamasını tamamlar ve iletiyi parametre olarak silmek için geçirir. **DeleteMessage** yöntemi, iletinin tüketildiği şekilde işaretler ve abonelikten kaldırır.

Aşağıdaki örnek, iletilerin nasıl alınabileceğini ve `receiveSubscriptionMessage`nasıl işlenebileceğini gösterir. Örnek önce 'LowMessages' aboneliğinden bir ileti alır ve siler, sonra da 'HighMessages' aboneliğinden doğru `isPeekLock` ayarlı bir ileti alır. Daha sonra aşağıdakileri `deleteMessage`kullanarak iletiyi siler:

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
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bir alıcı uygulaması iletiyi nedense işleyemiyorsa, `unlockMessage` **ServiceBusService** nesnesindeki yöntemi çağırabilir. Bu yöntem, Hizmet Veri Servisi'nin abonelik içindeki iletinin kilidini açmasına ve yeniden alınmak üzere kullanılabilir hale getirmesine neden olur. Bu durumda, aynı tüketen uygulama veya başka bir tüketen uygulama tarafından.

Abonelik içinde kilitli bir iletiyle ilişkili bir zaman ası da vardır. Uygulama kilit süresi dolmadan önce iletiyi işlemeyi başaramazsa (örneğin, uygulama çöküyorsa), Servis Veri Servisi iletinin kilidini otomatik olarak açar ve yeniden alınmak üzere kullanılabilir hale getirir.

Uygulama, iletiyi işledikten sonra kilitlenir, ancak `deleteMessage` yöntem çağrılmadan önce, ileti yeniden başlatıldığında uygulamaya yeniden teslim edilir. Bu davranış genellikle *En Az Bir Kez İşleme*denir. Diğer bir deyişle, her ileti en az bir kez işlenir, ancak bazı durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işleme tolere edemiyorsa, yinelenen ileti teslimişlemek için uygulamanıza mantık eklemeniz gerekir. İletinin, teslim girişimleri arasında sabit kalan **MessageId** özelliğini kullanabilirsiniz.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme
Konular ve abonelikler, [autoDeleteOnIdle özelliği](https://docs.microsoft.com/javascript/api/@azure/arm-servicebus/sbsubscription?view=azure-node-latest#autodeleteonidle) ayarlanmadıkça kalıcıdır ve [Azure portalı][Azure portal] veya programlı olarak açıkça silinmelidir.
Aşağıdaki örnek, adlı konunun nasıl `MyTopic`silinir gösteriş olduğunu gösterir:

```javascript
serviceBusService.deleteTopic('MyTopic', function (error) {
    if (error) {
        console.log(error);
    }
});
```

Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir. Ayrıca, abonelikler bağımsız olarak da silinebilir. Aşağıdaki örnek, `HighMessages` `MyTopic` adındaki bir aboneliğin nasıl silinir olduğunu gösterir:

```javascript
serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
    if(error) {
        console.log(error);
    }
});
```

> [!NOTE]
> [Servis](https://github.com/paolosalvatori/ServiceBusExplorer/)Veri Servisi Explorer ile Servis Veri Servisi kaynaklarını yönetebilirsiniz. Service Bus Explorer, kullanıcıların bir Service Bus ad alanına bağlanmasına ve ileti varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç, alma/dışa aktarma işlevselliği veya konuyu, kuyrukları, abonelikleri, geçiş hizmetlerini, bildirim hub'larını ve olay hub'larını test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Artık Servis Veri Yolu konularının temellerini öğrendiğiniz için, daha fazla bilgi edinmek için bu bağlantıları takip edin.

* [Bkz. Kuyruklar, konular ve abonelikler.][Queues, topics, and subscriptions]
* [SqlFilter][SqlFilter] için API başvurusu
* GitHub'daki Düğüm deposu [için Azure SDK'sını][Azure SDK for Node] ziyaret edin.

[Azure SDK for Node]: https://github.com/Azure/azure-sdk-for-node
[Azure portal]: https://portal.azure.com
[SqlFilter.SqlExpression]: service-bus-messaging-sql-filter.md
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /javascript/api/@azure/arm-servicebus/sqlfilter?view=azure-node-latest
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Bir Azure Web Sitesi'ne Bir Düğüm.js uygulaması oluşturma ve dağıtma]: ../app-service/app-service-web-get-started-nodejs.md
[Node.js Cloud Service with Storage]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md

