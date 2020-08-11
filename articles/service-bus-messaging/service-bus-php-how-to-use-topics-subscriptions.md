---
title: PHP ile Azure Service Bus konuları kullanma
description: Bu öğreticide, bir PHP uygulamasından Azure Service Bus konuları ve abonelikleri kullanmayı öğreneceksiniz.
ms.devlang: PHP
ms.topic: quickstart
ms.date: 06/23/2020
ms.openlocfilehash: 706f523fdfb3c710bb16b048cfc68ce98875adb1
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88066211"
---
# <a name="quickstart-how-to-use-service-bus-topics-and-subscriptions-with-php"></a>Hızlı başlangıç: PHP ile Service Bus konuları ve abonelikleri kullanma

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Bu makalede, Service Bus konularının ve aboneliklerinin nasıl kullanılacağı gösterilmektedir. Örnekler PHP 'de yazılır ve [php Için Azure SDK 'sını](https://github.com/Azure/azure-sdk-for-php)kullanır. Kapsanan senaryolar şunlardır:

- Konular ve abonelikler oluşturma 
- Abonelik filtreleri oluşturma 
- Konuya ileti gönderme 
- Bir abonelikten ileti alma
- Konuları ve abonelikleri silme

## <a name="prerequisites"></a>Önkoşullar
1. Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [Visual Studio veya MSDN abonesi avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)için kaydolabilirsiniz.
2. Hızlı başlangıç içindeki adımları izleyin: bir Service Bus **ad alanı** oluşturmak ve **bağlantı dizesini**almak için [Azure Portal Service Bus konu başlığı ve abonelikleri oluşturmak üzere kullanın](service-bus-quickstart-topics-subscriptions-portal.md) .

    > [!NOTE]
    > Bu hızlı başlangıçta **php** 'yi kullanarak konu için bir **Konu** ve bir **abonelik** oluşturacaksınız. 

## <a name="create-a-php-application"></a>PHP uygulaması oluşturma
Azure Blob hizmetine erişen bir PHP uygulaması oluşturmaya yönelik tek gereksinim, kodunuzun içinden [php Için Azure SDK 'sının](https://github.com/Azure/azure-sdk-for-php) sınıflarına başvurmalıdır. Uygulamanızı veya Not defterini oluşturmak için herhangi bir geliştirme aracı kullanabilirsiniz.

> [!NOTE]
> PHP yüklemenizin Ayrıca [OpenSSL uzantısının](https://php.net/openssl) yüklü ve etkin olması gerekir.
> 
> 

Bu makalede, yerel olarak bir PHP uygulaması içinde veya bir Azure Web rolü, çalışan rolü veya Web sitesi içinde çalışan kodda çağrılabilen hizmet özelliklerinin nasıl kullanılacağı açıklanır.

## <a name="get-the-azure-client-libraries"></a>Azure istemci kitaplıklarını al

### <a name="install-via-composer"></a>Besteci aracılığıyla Install
1. Projenizin kökünde **composer.js** adlı bir dosya oluşturun ve bu dosyaya aşağıdaki kodu ekleyin:
   
    ```json
    {
      "require": {
        "microsoft/windowsazure": "*"
      }
    }
    ```
2. Proje kökünde **[besteci. phar] [besteci-phar]** öğesini indirin.
3. Bir komut istemi açın ve proje kökünde aşağıdaki komutu yürütün
   
    ```
    php composer.phar install
    ```

## <a name="configure-your-application-to-use-service-bus"></a>Uygulamanızı kullanmak için yapılandırma Service Bus
Service Bus API 'Lerini kullanmak için:

1. [Require_once][require-once] ifadesini kullanarak otomatik yükleyici dosyasına başvurun.
2. Kullanabileceğiniz tüm sınıflara başvurun.

Aşağıdaki örnek, otomatik yükleyici dosyasının nasıl ekleneceğini ve **Servicebusservice** sınıfına nasıl başvurululacağını gösterir.

> [!NOTE]
> Bu örnek (ve bu makaledeki diğer örnekler), Oluşturucu aracılığıyla Azure için PHP Istemci kitaplıklarını yüklediğinizi varsayar. Kitaplıkları el ile veya bir PEAR paketi olarak yüklediyseniz, **windowsazure. php** otomatik yükleyici dosyasına başvurmanız gerekir.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Aşağıdaki örneklerde, `require_once` deyimleri her zaman gösteriliyor, ancak yalnızca yürütülecek örnek için gereken sınıflara başvurulur.

## <a name="set-up-a-service-bus-connection"></a>Service Bus bağlantısını ayarlama
Service Bus istemcisinin örneğini oluşturmak için, önce bu biçimde geçerli bir bağlantı dizesine sahip olmanız gerekir:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Burada `Endpoint` genellikle biçimindedir `https://[yourNamespace].servicebus.windows.net` .

Herhangi bir Azure hizmeti istemcisi oluşturmak için sınıfını kullanmanız gerekir `ServicesBuilder` . Seçenekleriniz şunlardır:

* Bağlantı dizesini doğrudan ona geçirin.
* Bağlantı dizesinin birden çok dış kaynağını denetlemek için **Cloudconfigurationmanager (CCM)** kullanın:
  * Varsayılan olarak, bir dış kaynak-ortam değişkeni desteğiyle birlikte gelir.
  * `ConnectionStringSource` sınıfını genişleterek yeni kaynaklar ekleyebilirsiniz.

Burada özetlenen örnekler için bağlantı dizesi doğrudan geçirilir.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Konu başlığı oluşturma
Sınıfı aracılığıyla Service Bus konular için yönetim işlemlerini gerçekleştirebilirsiniz `ServiceBusRestProxy` . Bir `ServiceBusRestProxy` nesne, `ServicesBuilder::createServiceBusService` kendisini yönetmek için belirteç izinlerini kapsülleyen uygun bir bağlantı dizesiyle, Factory yöntemi aracılığıyla oluşturulur.

Aşağıdaki örnek, bir `ServiceBusRestProxy` `ServiceBusRestProxy->createTopic` `mytopic` ad alanı içinde adlı bir konu oluşturmak için ve çağrısının nasıl örneklendirilecek gösterilmektedir `MySBNamespace` :

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
> `listTopics` `ServiceBusRestProxy` Bir hizmet ad alanı içinde belirtilen bir ada sahip bir konunun zaten var olup olmadığını denetlemek için nesneleri üzerinde yöntemini kullanabilirsiniz.
> 
> 

## <a name="create-a-subscription"></a>Abonelik oluşturma
Konu abonelikleri, yöntemiyle de oluşturulur `ServiceBusRestProxy->createSubscription` . Abonelikler adlandırılır ve aboneliğin sanal kuyruğuna gönderilen ileti kümesini sınırlayan isteğe bağlı bir filtre içerebilir.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Varsayılan (MatchAll) filtreyle abonelik oluşturma
Yeni bir abonelik oluşturulduğunda bir filtre belirtilmemişse, **Matchall** filtresi (varsayılan) kullanılır. **Matchall** filtresi kullanıldığında, konuya yayınlanan tüm iletiler aboneliğin sanal kuyruğuna yerleştirilir. Aşağıdaki örnek adlı bir abonelik oluşturur `mysubscription` ve varsayılan **Matchall** filtresini kullanır.

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
Bir konu başlığına gönderilen iletilerden hangilerinin belirli bir konu başlığı aboneliğinde görüneceğini belirlemenize olanak sağlayan filtreler de ayarlayabilirsiniz. Abonelikler tarafından desteklenen en esnek filtre türü, SQL92 alt kümesini uygulayan [Sqlfilter](/dotnet/api/microsoft.servicebus.messaging.sqlfilter)' dir. SQL filtreleri, konu başlığında yayımlanan iletilerin özelliklerinde çalışır. SqlFilters hakkında daha fazla bilgi için bkz. [Sqlfilter. SqlExpression özelliği][sqlfilter].

> [!NOTE]
> Bir abonelikteki her kural, gelen iletileri bağımsız olarak işler ve sonuç iletilerini aboneliğe ekler. Ayrıca, her yeni abonelik, konudan tüm iletileri aboneliğe ekleyen bir filtreye sahip bir varsayılan **kural** nesnesine sahiptir. Yalnızca Filtrenizle eşleşen iletileri almak için varsayılan kuralı kaldırmanız gerekir. Yöntemini kullanarak varsayılan kuralı kaldırabilirsiniz `ServiceBusRestProxy->deleteRule` .
> 
> 

Aşağıdaki örnekte, `HighMessages` yalnızca 3 ' ten büyük özel bir özelliği olan iletileri seçen **sqlfilter** ile adlı bir abonelik oluşturulur `MessageNumber` . İletilere özel özellikler ekleme hakkında bilgi için bkz. [bir konuya Ileti gönderme](#send-messages-to-a-topic) .

```php
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Bu kod için ek bir ad alanı kullanılması gerekir: `WindowsAzure\ServiceBus\Models\SubscriptionInfo` .

Benzer şekilde, aşağıdaki örnek, `LowMessages` `SqlFilter` yalnızca `MessageNumber` 3 ' ten küçük veya buna eşit bir özelliği olan iletileri seçen, ile adlı bir abonelik oluşturur.

```php
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Şimdi, konuya bir ileti gönderildiğinde `mytopic` , bu, her zaman aboneliğe abone olan alıcılar için dağıtılır `mysubscription` ve, ve aboneliklerine abone olan alıcılar için `HighMessages` `LowMessages` (ileti içeriğine bağlı olarak) seçmeli olarak dağıtılır.

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme
Service Bus bir konuya ileti göndermek için, uygulamanız `ServiceBusRestProxy->sendTopicMessage` yöntemini çağırır. Aşağıdaki kod, `mytopic` hizmet ad alanı içinde daha önce oluşturulan konuya nasıl ileti gönderileceğini gösterir `MySBNamespace` .

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

Service Bus konu başlıklarına gönderilen iletiler, [BrokeredMessage][BrokeredMessage] sınıfının örnekleridir. [Brokeredmessage][BrokeredMessage] nesneleri, bir dizi standart özellik ve yöntemin yanı sıra uygulamaya özgü özel özellikleri tutmak için kullanılabilen özellikleri de vardır. Aşağıdaki örnek, `mytopic` daha önce oluşturulan konuya beş test iletisinin nasıl gönderileceğini gösterir. `setProperty`Yöntemi, her iletiye özel bir özellik eklemek için kullanılır `MessageNumber` . `MessageNumber`Özellik değeri her bir ileti üzerinde değişir (Bu değeri, [abonelik oluşturma](#create-a-subscription) bölümünde gösterildiği gibi, hangi aboneliklerin alacağını öğrenmek için kullanabilirsiniz):

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

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Konu başlığında tutulan ileti sayısına ilişkin bir sınır yoktur ancak konu başlığı tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Konu boyutu için bu üst sınır 5 GB 'dir. Kotalar hakkında daha fazla bilgi için bkz. [Service Bus kotaları][Service Bus quotas].

## <a name="receive-messages-from-a-subscription"></a>Bir abonelikten ileti alma
Bir abonelikten ileti almanın en iyi yolu bir `ServiceBusRestProxy->receiveSubscriptionMessage` yöntemi kullanmaktır. İletiler iki farklı modda alınabilir: [ *Receiveanddelete* ve *PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode). **PeekLock** varsayılan değerdir.

[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modunu kullanırken alma işlemi tek aşamalıdır. Service Bus abonelikte bir iletiye yönelik okuma isteği aldığında, iletiyi kullanılıyor olarak işaretler ve uygulamaya döndürür. [Receiveanddelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) * modu en basit modeldir ve bir hata oluştuğunda bir uygulamanın bir iletiyi işlememesi için en iyisi senaryolar için geçerlidir. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Service Bus ileti tüketildiği gibi işaretlendiğinden, uygulama yeniden başlatıldığında ve iletileri yeniden kullanmaya başladığında, kilitlenme öncesinde tüketilen iletiyi kaçırmıştır.

Varsayılan [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) modunda bir ileti almak iki aşamalı bir işlem haline gelir, bu da eksik iletilere izin verilmeyen uygulamaları desteklemeyi olanaklı kılar. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan (veya gelecekteki işlemler için güvenilir bir şekilde depolar), alınan iletiyi ' a geçirerek alma işleminin ikinci aşamasını tamamlar `ServiceBusRestProxy->deleteMessage` . Service Bus `deleteMessage` çağrısı gördüğünde, iletiyi tüketildiği gibi işaretler ve kuyruktan kaldırır.

Aşağıdaki örnek, [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) Mode (varsayılan mod) kullanarak bir iletinin nasıl alınacağını ve işleyeceğini gösterir. 

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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Nasıl yapılır: uygulama kilitlenmelerini ve okunamaz iletileri işleme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bir alıcı uygulamasının iletiyi bazı nedenlerle işleyemeyeceği takdirde, `unlockMessage` alınan ileti üzerinde yöntemi çağırabilir ( `deleteMessage` yöntemi yerine). Service Bus, kuyruktaki iletinin kilidini açmak ve aynı uygulama ya da başka bir uygulama tarafından yeniden alınmak üzere kullanılabilir hale gelmesine neden olur.

Ayrıca, kuyruk içinde kilitlenen bir iletiyle ilişkili bir zaman aşımı vardır. uygulama, kilit zaman aşımı dolmadan önce iletiyi işleyemezse (örneğin, uygulama çökerse) Service Bus, otomatik olarak iletinin kilidini açar ve yeniden alınmak üzere kullanılabilir hale gelir.

İleti işlendikten sonra, ancak istek verilmeden önce uygulamanın çöktüğü durumda `deleteMessage` , yeniden başlatıldığında ileti uygulamaya yeniden gönderilir. Bu tür işleme genellikle *en az bir kez* işleme olarak adlandırılır; diğer bir deyişle, her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işleme tolerans vermezse, uygulama geliştiricilerinin yinelenen ileti teslimini işlemek için uygulamalara ek mantık eklemesi gerekir. Genellikle ileti yöntemi kullanılarak elde edilir `getMessageId` ve bu da teslim girişimleri arasında sabit kalır.

## <a name="delete-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri silme
Bir konuyu veya aboneliği silmek için `ServiceBusRestProxy->deleteTopic` `ServiceBusRestProxy->deleteSubscripton` sırasıyla veya yöntemlerini kullanın. Bir konu başlığı silindiğinde bu konu başlığıyla kaydedilen tüm abonelikler de silinir.

Aşağıdaki örnek, adlı konunun `mytopic` ve kayıtlı aboneliklerinin nasıl silineceğini gösterir.

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

`deleteSubscription`Yöntemini kullanarak bir aboneliği bağımsız olarak silebilirsiniz:

```php
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

> [!NOTE]
> Service Bus kaynaklarını [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/)ile yönetebilirsiniz. Service Bus gezgin, kullanıcıların bir Service Bus ad alanına bağlanmasına ve mesajlaşma varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç içeri/dışarı aktarma işlevselliği gibi gelişmiş özellikler ya da konu, kuyruk, abonelik, geçiş Hizmetleri, Bildirim Hub 'ları ve Olay Hub 'larını test etme yeteneği sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [Kuyruklar, konular ve abonelikler][Queues, topics, and subscriptions].

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: /dotnet/api/microsoft.servicebus.messaging.sqlfilter
[require-once]: https://php.net/require_once
[Service Bus quotas]: service-bus-quotas.md