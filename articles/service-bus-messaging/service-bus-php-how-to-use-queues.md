---
title: PHP ile Azure Servis Veri Servisi kuyrukları nasıl kullanılır?
description: Bu eğitimde, Hizmet Veri Yolu kuyruğuna ileti göndermek ve ileti almak için PHP uygulamaları oluşturmayı öğrenirsiniz.
services: service-bus-messaging
documentationcenter: php
author: axisc
manager: timlt
editor: spelluru
ms.assetid: e29c829b-44c5-4350-8f2e-39e0c380a9f2
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: quickstart
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: fcb735d81cac587c75a133ad582f2a839551dcfa
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76760700"
---
# <a name="quickstart-how-to-use-service-bus-queues-with-php"></a>Quickstart: PHP ile Servis Veri Servisi kuyrukları nasıl kullanılır?
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Bu eğitimde, Hizmet Veri Yolu kuyruğuna ileti göndermek ve ileti almak için PHP uygulamaları oluşturmayı öğrenirsiniz. 

## <a name="prerequisites"></a>Ön koşullar
1. Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) etkinleştirebilir veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)kaydolabilirsiniz.
2. Üzerinde çalışmanız gereken bir sıra yoksa, kuyruk oluşturmak için Hizmet Veri Hizmetleri sırası makalesi [oluşturmak için Azure kullan portalındaki](service-bus-quickstart-portal.md) adımları izleyin.
    1. Servis Veri Servisi **kuyruklarına**hızlı **bir genel bakış** okuyun. 
    2. Hizmet Veri Günü **ad alanı**oluşturun. 
    3. Bağlantı **dizesini**alın. 

        > [!NOTE]
        > Bu öğreticide PHP kullanarak Hizmet Veri Servisi ad alanında bir **kuyruk** oluşturacaksınız. 
3. [PHP için Azure SDK](https://github.com/Azure/azure-sdk-for-php)

## <a name="create-a-php-application"></a>PHP uygulaması oluşturma
Azure Blob hizmetine erişen bir PHP uygulaması oluşturmanın tek şartı, kodunuzun içinden [PHP için Azure SDK'daki](https://github.com/Azure/azure-sdk-for-php) sınıfların başvurusudur. Uygulamanızı veya Notepad'i oluşturmak için herhangi bir geliştirme aracını kullanabilirsiniz.

> [!NOTE]
> PHP yüklemeniz in [OpenSSL uzantısı](https://php.net/openssl) da yüklü ve etkin olmalıdır.

Bu kılavuzda, yerel olarak bir PHP uygulaması içinden veya bir Azure web rolü, çalışan rolü veya web sitesi içinde çalışan kodlardan çağrılabilen hizmet özelliklerini kullanırsınız.

## <a name="get-the-azure-client-libraries"></a>Azure istemci kitaplıklarını alın
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Uygulamanızı Servis Veri Aracı'nı kullanacak şekilde yapılandırın
Servis Veri Veri Mesi apilerini kullanmak için aşağıdakileri yapın:

1. [require_once][require_once] deyimini kullanarak autoloader dosyasına başvurun.
2. Kullanabileceğiniz sınıflara başvurun.

Aşağıdaki örnek, otomatik yükleyici dosyasının nasıl `ServicesBuilder` dahil edilip sınıfa başvurulup başvurulmasını gösterir.

> [!NOTE]
> Bu örnek (ve bu makaledeki diğer örnekler), Php İstemci Kitaplıklarını Azure için Composer aracılığıyla yüklediğinizi varsayar. Kitaplıkları el ile veya ARMUT paketi olarak yüklediyseniz, **WindowsAzure.php** autoloader dosyasına başvurmanız gerekir.
> 
> 

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Aşağıdaki `require_once` örneklerde, deyim her zaman gösterilir, ancak yalnızca örneğin yürütülmesi için gerekli sınıflara başvurulmaktadır.

## <a name="set-up-a-service-bus-connection"></a>Servis Veri Servisi bağlantısı ayarlama
Servis Veri Servisi istemcisini anında atabilmek için öncelikle bu biçimde geçerli bir bağlantı dizeniz olması gerekir:

```
Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]
```

Nerede `Endpoint` genellikle biçimidir. `[yourNamespace].servicebus.windows.net`

Herhangi bir Azure hizmet istemcisi `ServicesBuilder` oluşturmak için sınıfı kullanmanız gerekir. Şunları yapabilirsiniz:

* Bağlantı dizesini doğrudan ona geçirin.
* Bağlantı dizesi için birden çok harici kaynağı denetlemek için **CloudConfigurationManager'ı (CCM)** kullanın:
  * Varsayılan olarak bir dış kaynak için destek ile birlikte gelir - çevresel değişkenler
  * `ConnectionStringSource` Sınıfı genişleterek yeni kaynaklar ekleyebilirsiniz

Burada özetlenen örnekler için bağlantı dizesi doğrudan geçirilir.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[Primary Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-queue"></a>Bir kuyruk oluşturma
`ServiceBusRestProxy` Sınıf üzerinden Servis Veri Servisi kuyrukları için yönetim işlemleri gerçekleştirebilirsiniz. Bir `ServiceBusRestProxy` nesne, onu `ServicesBuilder::createServiceBusService` yönetmek için belirteç izinleri kapsülleyen uygun bir bağlantı dizesi ile fabrika yöntemi ile oluşturulur.

Aşağıdaki `ServiceBusRestProxy` örnek, bir `ServiceBusRestProxy->createQueue` `myqueue` `MySBNamespace` hizmet ad alanı içinde adlandırılmış bir sıra oluşturmak için a ve çağrının nasıl anında oluşturulup çağırılabildiğini gösterir:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    $queueInfo = new QueueInfo("myqueue");

    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
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
> Belirli bir `listQueues` ada `ServiceBusRestProxy` sahip bir kuyruğun ad alanı içinde zaten var olup olmadığını denetlemek için nesneler üzerinde yöntemi kullanabilirsiniz.
> 
> 

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Servis Veri Servisi kuyruğuna ileti göndermek için `ServiceBusRestProxy->sendQueueMessage` uygulamanız yöntemi çağırır. Aşağıdaki kod, hizmet ad alanı `myqueue` içinde daha önce `MySBNamespace` oluşturulan kuyruğa nasıl ileti gönderilen leri gösterir.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");

    // Send message.
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
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

Hizmet Veri Servisi kuyruklarına gönderilen (ve alınan) iletiler [BrokeredMessage][BrokeredMessage] sınıfının örnekleridir. [Aracılı İleti][BrokeredMessage] nesnelerinde, özel uygulamaya özgü özellikleri tutmak için kullanılan bir dizi standart yöntem ve özellik ve rasgele uygulama verileri kümesi vardır.

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Sıra boyutundaki bu üst sınır 5 GB'dır.

## <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma

Bir kuyruktan ileti almanın en iyi yolu `ServiceBusRestProxy->receiveQueueMessage` bir yöntem kullanmaktır. Mesajlar iki farklı modda alınabilir: [*ReceiveAndDelete*](/dotnet/api/microsoft.servicebus.messaging.receivemode) ve [*PeekLock*](/dotnet/api/microsoft.servicebus.messaging.receivemode#Microsoft_ServiceBus_Messaging_ReceiveMode_PeekLock). **PeekLock** varsayılan değerdir.

[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modunu kullanırken, receive tek çekimlik bir işlemdir; diğer bir deyişle, Servis Veri Servisi kuyruktaki bir ileti için okuma isteği aldığında, iletinin tüketildiği şeklinde işaretler ve uygulamaya döndürür. [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) modu, en basit modeldir ve uygulamanın hata oluştuğunda bir iletinin işlenmemesine izin verebileceği senaryolarda en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın. Servis Veri Kurumu iletinin tüketildiği olarak işaretlenmiş olduğundan, uygulama yeniden başlatıldığında ve iletileri yeniden almaya başladığında, kilitlenmeden önce tüketilen iletiyi kaçırmış olur.

Varsayılan [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode#Microsoft_ServiceBus_Messaging_ReceiveMode_PeekLock) modunda, ileti almak iki aşamalı bir işlem haline gelir ve bu da eksik iletileri tolere edemeyen uygulamaları desteklemeyi mümkün kılar. Servis Veri Mes'i bir istek aldığında, bir sonraki iletinin tüketilmesi gerektiğini bulur, diğer tüketicilerin bu isteği almasını önlemek için kilitler ve sonra uygulamaya döndürür. Uygulama iletiyi işlemeyi bitirdikten sonra (veya gelecekteki işlemler için güvenilir bir şekilde depoladıktan) `ServiceBusRestProxy->deleteMessage`alınan iletiyi . Servis Veri Servisi `deleteMessage` aramayı gördüğünde, iletinin tüketildiği olarak işaretlenecek ve sıradan kaldıracaktır.

Aşağıdaki örnek, [PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode#Microsoft_ServiceBus_Messaging_ReceiveMode_PeekLock) modunu (varsayılan mod) kullanarak bir iletiyi nasıl alacağıve işleyin.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);

try    {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();

    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";

    /*---------------------------
        Process message here.
    ----------------------------*/

    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
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

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme

Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Alıcı uygulaması iletiyi nedense işleyemiyorsa, `unlockMessage` alınan iletideki yöntemi `deleteMessage` (yöntem yerine) çağırabilir. Bu, Servis Veri Servisi'nin sıra içindeki iletinin kilidini açmasına ve aynı alıcı uygulama veya başka bir tüketen uygulama tarafından yeniden alınmak üzere kullanılabilir hale getirmesine neden olur.

Sıra içinde kilitli bir iletiyle ilişkili bir zaman aşımı da vardır ve uygulama kilit süresi sona ermeden önce iletiyi işlemezse (örneğin, uygulama çöküyorsa), Servis Veri Servisi iletiyi otomatik olarak açar ve tekrar alınabilir.

Uygulamanın iletiyi işledikten sonra ancak `deleteMessage` istek verilmeden önce çökmesi durumunda, ileti yeniden başlatıldığında uygulamaya yeniden teslim edilir. Bu genellikle *En Az Bir Kez* işleme denir; diğer bir deyişle, her ileti en az bir kez işlenir, ancak bazı durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işleme tolere edemiyorsa, yinelenen ileti teslimini işlemek için uygulamalara ek mantık eklenmesi önerilir. Bu genellikle teslim `getMessageId` girişimleri arasında sabit kalır ileti yöntemi kullanılarak elde edilir.

> [!NOTE]
> [Servis](https://github.com/paolosalvatori/ServiceBusExplorer/)Veri Servisi Explorer ile Servis Veri Servisi kaynaklarını yönetebilirsiniz. Service Bus Explorer, kullanıcıların bir Service Bus ad alanına bağlanmasına ve ileti varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç, alma/dışa aktarma işlevselliği veya konuyu, kuyrukları, abonelikleri, geçiş hizmetlerini, bildirim hub'larını ve olay hub'larını test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Artık Servis Veri Servisi kuyruklarının temellerini öğrendiğiniz için, daha fazla bilgi için [Kuyruklar, konular ve aboneliklere][Queues, topics, and subscriptions] bakın.

Daha fazla bilgi için [PHP Geliştirici Merkezi'ni](https://azure.microsoft.com/develop/php/)de ziyaret edin.

[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[require_once]: https://php.net/require_once


