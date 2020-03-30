---
title: PHP ile Azure Hizmet Veri Meskeni konuları nasıl kullanılır?
description: Bu eğitimde, bir PHP uygulamasından Azure Hizmet Veri Yolu konularını ve aboneliklerini nasıl kullanacağınızı öğrenirsiniz.
services: service-bus-messaging
documentationcenter: php
author: axisc
manager: timlt
editor: spelluru
ms.assetid: faaa4bbd-f6ef-42ff-aca7-fc4353976449
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: quickstart
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: 92f25f4bdac4942478c93f717c81eadd2c2f5b4a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76760683"
---
# <a name="quickstart-how-to-use-service-bus-topics-and-subscriptions-with-php"></a>Quickstart: PHP ile Servis Veri Servisi konuları ve abonelikleri nasıl kullanılır?

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu makalede, Hizmet Veri Servisi konuları ve abonelikleri nasıl kullanacağınızı gösterir. Örnekler PHP ile yazılır ve [PHP için Azure SDK'yı](https://github.com/Azure/azure-sdk-for-php)kullanın. Kapsanan senaryolar şunlardır:

- Konu ve abonelik oluşturma 
- Abonelik filtreleri oluşturma 
- Bir konuya ileti gönderme 
- Abonelikten ileti alma
- Konuları ve abonelikleri silme

## <a name="prerequisites"></a>Ön koşullar
1. Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Visual Studio [veya MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) etkinleştirebilir veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)kaydolabilirsiniz.
2. Quickstart'taki adımları izleyin: Hizmet Veri Yolunda niçin [bir konu oluşturmak için Azure portalını kullanın ve bir](service-bus-quickstart-topics-subscriptions-portal.md) Hizmet Veri Servisi ad **alanı** oluşturmak ve bağlantı **dizesini**almak için konuya abonelikler oluşturun.

    > [!NOTE]
    > Bu hızlı başlangıçta **PHP'yi** kullanarak bir **konu** ve konuya **abonelik** oluşturacaksınız. 

## <a name="create-a-php-application"></a>PHP uygulaması oluşturma
Azure Blob hizmetine erişen bir PHP uygulaması oluşturmanın tek şartı, kodunuz içinden [PHP için Azure SDK'daki](https://github.com/Azure/azure-sdk-for-php) sınıflara başvurmaktır. Uygulamanızı veya Notepad'i oluşturmak için herhangi bir geliştirme aracını kullanabilirsiniz.

> [!NOTE]
> PHP yüklemeniz in [OpenSSL uzantısı](https://php.net/openssl) da yüklü ve etkin olmalıdır.
> 
> 

Bu makalede, bir PHP uygulaması içinde yerel olarak veya Bir Azure web rolü, çalışan rolü veya web sitesi içinde çalışan kod içinde çağrılabilen hizmet özelliklerinin nasıl kullanılacağı açıklanmaktadır.

## <a name="get-the-azure-client-libraries"></a>Azure istemci kitaplıklarını alın

### <a name="install-via-composer"></a>Composer ile yükle
1. Projenizin kökünde **composer.json** adlı bir dosya oluşturun ve projeye aşağıdaki kodu ekleyin:
   
    ```json
    {
      "require": {
        "microsoft/windowsazure": "*"
      }
    }
    ```
2. Proje **kökünüzde [composer.phar][composer-phar]** indirin.
3. Komut istemini açın ve proje kökünüzde aşağıdaki komutu çalıştırın
   
    ```
    php composer.phar install
    ```

## <a name="configure-your-application-to-use-service-bus"></a>Uygulamanızı Servis Veri Aracı'nı kullanacak şekilde yapılandırın
Servis Veri Servisi API'lerini kullanmak için:

1. [require_once][require-once] deyimini kullanarak autoloader dosyasına başvurun.
2. Kullanabileceğiniz sınıflara başvurun.

Aşağıdaki örnek, autoloader dosyasının nasıl ekleştirilebildiğini ve **ServiceBusService** sınıfına nasıl başvurulup başvurulmasını gösterir.

> [!NOTE]
> Bu örnek (ve bu makaledeki diğer örnekler), Php İstemci Kitaplıklarını Azure için Composer aracılığıyla yüklediğinizi varsayar. Kitaplıkları el ile veya ARMUT paketi olarak yüklediyseniz, **WindowsAzure.php** autoloader dosyasına başvurmanız gerekir.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Aşağıdaki örneklerde, `require_once` deyim her zaman gösterilir, ancak yalnızca örneğin yürütülmesi için gerekli sınıflara başvurulmaktadır.

## <a name="set-up-a-service-bus-connection"></a>Servis Veri Servisi bağlantısı ayarlama
Servis Veri Servisi istemcisini anında atabilmek için öncelikle bu biçimde geçerli bir bağlantı dizeniz olması gerekir:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Nerede `Endpoint` genellikle biçimidir. `https://[yourNamespace].servicebus.windows.net`

Herhangi bir Azure hizmet istemcisi `ServicesBuilder` oluşturmak için sınıfı kullanmanız gerekir. Şunları yapabilirsiniz:

* Bağlantı dizesini doğrudan ona geçirin.
* Bağlantı dizesi için birden çok harici kaynağı denetlemek için **CloudConfigurationManager'ı (CCM)** kullanın:
  * Varsayılan olarak bir dış kaynak için destek ile birlikte gelir - çevresel değişkenler.
  * `ConnectionStringSource` sınıfını genişleterek yeni kaynaklar ekleyebilirsiniz.

Burada özetlenen örnekler için bağlantı dizesi doğrudan geçirilir.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Konu başlığı oluşturma
Hizmet Veri Mes'i konuları için `ServiceBusRestProxy` yönetim işlemlerini sınıf üzerinden gerçekleştirebilirsiniz. Bir `ServiceBusRestProxy` nesne, onu `ServicesBuilder::createServiceBusService` yönetmek için belirteç izinleri kapsülleyen uygun bir bağlantı dizesi ile fabrika yöntemi ile oluşturulur.

Aşağıdaki örnek, ad `ServiceBusRestProxy` alanı içinde `ServiceBusRestProxy->createTopic` `mytopic` `MySBNamespace` adlandırılmış bir konu oluşturmak için a ve çağrının nasıl anında oluşturulup çağırılabildiğini gösterir:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try {
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [!NOTE]
> Belirli bir `listTopics` ada `ServiceBusRestProxy` sahip bir konunun bir hizmet ad alanı içinde zaten var olup olmadığını denetlemek için nesnelerüzerinde yöntemi kullanabilirsiniz.
> 
> 

## <a name="create-a-subscription"></a>Abonelik oluşturma
Konu abonelikleri de `ServiceBusRestProxy->createSubscription` yöntemle oluşturulur. Abonelikler adlandırılır ve aboneliğin sanal kuyruğuna gönderilen ileti kümesini sınırlayan isteğe bağlı bir filtre içerebilir.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma
Yeni bir abonelik oluşturulduğunda filtre belirtilmezse, **MatchAll** filtresi (varsayılan) kullanılır. **MatchAll** filtresi kullanıldığında, konuya göre yayınlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnek, adı verilen `mysubscription` bir abonelik oluşturur ve varsayılan **MatchAll** filtresini kullanır.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Filtre içeren abonelik oluşturma
Bir konu başlığına gönderilen iletilerden hangilerinin belirli bir konu başlığı aboneliğinde görüneceğini belirlemenize olanak sağlayan filtreler de ayarlayabilirsiniz. Abonelikler tarafından desteklenen en esnek filtre türü, SQL92 alt kümesini uygulayan [SqlFilter'dir.](/dotnet/api/microsoft.servicebus.messaging.sqlfilter) SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. SqlFilters hakkında daha fazla bilgi için [sqlfilter.SqlExpression Özelliği'ne][sqlfilter]bakın.

> [!NOTE]
> Abonelikteki her kural gelen iletileri bağımsız olarak işler ve sonuç iletilerini aboneye ekler. Ayrıca, her yeni aboneliğin, ilgili den etme deki tüm iletileri aboneye ekleyen bir filtreye sahip varsayılan bir **Kural** nesnesi vardır. Yalnızca filtrenizle eşleşen iletileri almak için varsayılan kuralı kaldırmanız gerekir. `ServiceBusRestProxy->deleteRule` Yöntemi kullanarak varsayılan kuralı kaldırabilirsiniz.
> 
> 

Aşağıdaki örnek, yalnızca 3'ten büyük özel `HighMessages` `MessageNumber` bir özel liğe sahip iletileri seçen bir **SqlFilter** ile adında bir abonelik oluşturur. İletilere özel özellikler ekleme hakkında bilgi almak için [bir konuya ileti gönder'](#send-messages-to-a-topic) e bakın.

```php
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Bu kod ek bir ad alanı `WindowsAzure\ServiceBus\Models\SubscriptionInfo`kullanımını gerektirir: .

Benzer şekilde, aşağıdaki örnek, yalnızca `LowMessages` 3'ten az veya eşit `MessageNumber` bir özelliği olan iletileri seçen bir `SqlFilter` abonelik oluşturur.

```php
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Şimdi, `mytopic` konuya bir ileti gönderildiğinde, her zaman `mysubscription` abone olan alıcılara teslim edilir ve seçici olarak abone `HighMessages` `LowMessages` olan alıcılara ve aboneliklere (ileti içeriğine bağlı olarak) teslim edilir.

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme
Servis Veri Gönderi konusuna ileti göndermek için `ServiceBusRestProxy->sendTopicMessage` uygulamanız yöntemi çağırır. Aşağıdaki kod, hizmet ad alanı `mytopic` içinde daha önce `MySBNamespace` oluşturulan konuya nasıl ileti gönderilen leri gösterir.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");

    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Service Bus konu başlıklarına gönderilen iletiler, [BrokeredMessage][BrokeredMessage] sınıfının örnekleridir. [Aracılı İleti][BrokeredMessage] nesnelerinin standart özellikleri ve yöntemlerinin yanı sıra özel uygulamaya özgü özellikleri tutmak için kullanılabilecek özellikler kümesi vardır. Aşağıdaki örnek, daha önce oluşturulan `mytopic` konuya beş test iletisi göndermenin nasıl yapılacağını gösterir. Yöntem, `setProperty` her iletiye özel`MessageNumber`bir özellik ( ) eklemek için kullanılır. Özellik `MessageNumber` değeri her iletide değişir [(abonelik oluştur](#create-a-subscription) bölümünde gösterildiği gibi, hangi aboneliklerin alacağını belirlemek için bu değeri kullanabilirsiniz):

```php
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);

    // Set custom property.
    $message->setProperty("MessageNumber", $i);

    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Konu başlığında tutulan ileti sayısına ilişkin bir sınır yoktur ancak konu başlığı tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Konu boyutundaki bu üst sınır 5 GB'dır. Kotalar hakkında daha fazla bilgi için [Servis Otobüsü kotalarına][Service Bus quotas]bakın.

## <a name="receive-messages-from-a-subscription"></a>Abonelikten ileti alma
Abonelikten ileti almanın en iyi yolu bir `ServiceBusRestProxy->receiveSubscriptionMessage` yöntem kullanmaktır. Mesajlar iki farklı modda alınabilir: [ *ReceiveAndDelete* ve *PeekLock*](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode). **PeekLock** varsayılan değerdir.

[ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) modunu kullanırken alma işlemi tek aşamalıdır. Service Bus abonelikte bir iletiye yönelik okuma isteği aldığında, iletiyi kullanılıyor olarak işaretler ve uygulamaya döndürür. [ReceiveAndDelete](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) * modu en basit modeldir ve bir uygulamanın bir hata oluştuğunda bir iletiyi işlememeye tahammül edebileceği senaryolar için en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Servis Veri Kurumu iletinin tüketildiği olarak işaretlendiğinden, uygulama yeniden başlatıldığında ve iletileri yeniden almaya başladığında, kilitlenmeden önce tüketilen iletiyi kaçırmıştır.

Varsayılan [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) modunda, ileti almak iki aşamalı bir işlem haline gelir ve bu da eksik iletileri tolere edemeyen uygulamaları desteklemeyi mümkün kılar. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi bitirdikten sonra (veya gelecekteki işlemler için güvenilir bir şekilde depoladıktan) `ServiceBusRestProxy->deleteMessage`alınan iletiyi . Servis Veri Servisi `deleteMessage` aramayı gördüğünde, iletinin tüketildiğini işaretler ve sıradan kaldırır.

Aşağıdaki örnek, [PeekLock](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.receivemode) modunu (varsayılan mod) kullanarak bir iletiyi nasıl alacağıve işleyin. 

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Nasıl gidilir: uygulama kilitlenmeleri ve okunamayan iletileri işleme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulaması iletiyi nedense işleyemiyorsa, `unlockMessage` alınan iletideki yöntemi `deleteMessage` (yöntem yerine) çağırabilir. Servis Veri Servisi'nin sıra içindeki iletinin kilidini açmasına ve aynı alıcı uygulama veya başka bir tüketen uygulama tarafından yeniden alınmaya hazır hale getirilmesine neden olur.

Sıra içinde kilitli bir iletiyle ilişkili bir zaman aşımı da vardır ve uygulama kilit süresi sona ermeden önce iletiyi işlemezse (örneğin, uygulama çöküyorsa), Servis Veri Servisi iletiyi otomatik olarak açar ve tekrar alınabilir.

Uygulamanın iletiyi işledikten sonra ancak `deleteMessage` istek verilmeden önce çökmesi durumunda, ileti yeniden başlatıldığında uygulamaya yeniden teslim edilir. Bu tür işleme genellikle *en az bir kez* işleme denir; diğer bir deyişle, her ileti en az bir kez işlenir, ancak bazı durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işleme tolere edemiyorsa, uygulama geliştiricileri yinelenen ileti teslimişlemek için uygulamalara ek mantık eklemelidir. Genellikle teslim girişimleri `getMessageId` arasında sabit kalır ileti yöntemi kullanılarak elde edilir.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme
Bir konuyu veya aboneliği silmek `ServiceBusRestProxy->deleteTopic` için `ServiceBusRestProxy->deleteSubscripton` sırasıyla yöntemleri veya yöntemleri kullanın. Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir.

Aşağıdaki örnek, adlı `mytopic` bir konunun ve kayıtlı aboneliklerinin nasıl silinir olduğunu gösterir.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try {
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // https://docs.microsoft.com/rest/api/storageservices/Common-REST-API-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

`deleteSubscription` Yöntemi kullanarak, aboneliği bağımsız olarak silebilirsiniz:

```php
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

> [!NOTE]
> [Servis](https://github.com/paolosalvatori/ServiceBusExplorer/)Veri Servisi Explorer ile Servis Veri Servisi kaynaklarını yönetebilirsiniz. Service Bus Explorer, kullanıcıların bir Service Bus ad alanına bağlanmasına ve ileti varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç, alma/dışa aktarma işlevselliği veya konuyu, kuyrukları, abonelikleri, geçiş hizmetlerini, bildirim hub'larını ve olay hub'larını test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için [Sıralar, konular ve abonelikler][Queues, topics, and subscriptions]konusuna bakın.

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[require-once]: https://php.net/require_once
[Service Bus quotas]: service-bus-quotas.md
