---
title: PHP'den Sıra depolama nasıl kullanılır - Azure Depolama
description: Kuyrukoluşturmak ve silmek ve ileti eklemek, almak ve silmek için Azure Kuyruk depolama hizmetini nasıl kullanacağınızı öğrenin. Örnekler PHP ile yazılır.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 01/11/2018
ms.service: storage
ms.subservice: queues
ms.topic: conceptual
ms.reviewer: cbrooks
ms.openlocfilehash: 692c943e48c08771b5f1c60b66412270081cf0e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72302961"
---
# <a name="how-to-use-queue-storage-from-php"></a>PHP’den Kuyruk depolama kullanma

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

Bu kılavuz, Azure Sıra depolama hizmetini kullanarak sık karşılaşılan senaryoları nasıl gerçekleştireceklerini gösterir. Örnekler PHP için Azure [Depolama İstemci Kitaplığı'ndan][download]sınıflar aracılığıyla yazılır. Kapsanan senaryolar arasında sıra iletileri ekleme, gözetleme, alma ve silmenin yanı sıra kuyruk oluşturma ve silme yer alır.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>PHP uygulaması oluşturma

Azure Sıra depolama alanına erişen bir PHP uygulaması oluşturmanın tek şartı, [kodunuzdaphp için Azure Depolama İstemci Kitaplığı'ndaki][download] sınıfların başvurusudur. Uygulamanızı oluşturmak için Not Defteri dahil olmak üzere tüm geliştirme araçlarını kullanabilirsiniz.

Bu kılavuzda, yerel olarak bir PHP uygulaması içinde veya Azure'daki bir web uygulamasında çalışan kodda çağrılabilen Sıra depolama hizmeti özelliklerini kullanırsınız.

## <a name="get-the-azure-client-libraries"></a>Azure İstemci Kitaplıklarını Alın

### <a name="install-via-composer"></a>Composer ile yükle

1. Projenizin kökünde **composer.json** adlı bir dosya oluşturun ve projeye aşağıdaki kodu ekleyin:
   
    ```json
    {
      "require": {
        "microsoft/azure-storage-queue": "*"
      }
    }
    ```
2. Proje kökünde **[composer.phar'ı][composer-phar]** indirin.
3. Komut istemini açın ve proje kökünüzde aşağıdaki komutu çalıştırın
   
    ```
    php composer.phar install
    ```

Alternatif olarak, kaynak kodu klonlamak için GitHub'daki [Azure Depolama PHP İstemci Kitaplığı'na][download] gidin.

## <a name="configure-your-application-to-access-queue-storage"></a>Uygulamanızı Sıra depolama alanına erişmek için yapılandırın

Azure Sıra depolama alanı için API'leri kullanmak için şunları yapmanız gerekir:

1. [require_once] deyimini kullanarak autoloader dosyasına başvurun.
2. Kullanabileceğiniz sınıflara başvurun.

Aşağıdaki örnek, autoloader dosyasının nasıl dahil edilebildiğini ve **QueueRestProxy** sınıfına nasıl başvurulup başvurulmasını gösterir.

```php
require_once 'vendor/autoload.php';
use MicrosoftAzure\Storage\Queue\QueueRestProxy;
```

Aşağıdaki örneklerde, `require_once` deyim her zaman gösterilir, ancak yalnızca örneğin yürütülmesi için gerekli olan sınıflarbaşvurur.

## <a name="set-up-an-azure-storage-connection"></a>Azure depolama bağlantısı ayarlama

Azure Sıra depolama istemcisini anında oluşturmak için öncelikle geçerli bir bağlantı dizeniz olması gerekir. Sıra hizmeti bağlantı dizesi biçimi aşağıdaki gibidir.

Canlı hizmete erişmek için:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Emülatör depolamasına erişmek için:

```php
UseDevelopmentStorage=true
```

Bir Azure Kuyruk hizmeti istemcisi oluşturmak için **QueueRestProxy** sınıfını kullanmanız gerekir. Aşağıdaki tekniklerden birini kullanabilirsiniz:

* Bağlantı dizesini doğrudan ona geçirin.
* Bağlantı dizesini depolamak için Web Uygulamanızda ortam değişkenlerini kullanın. Bağlantı dizelerini yapılandırmak için [Azure web uygulaması yapılandırma ayarları](../../app-service/configure-common.md) belgesine bakın.
Burada özetlenen örnekler için bağlantı dizesi doğrudan geçirilir.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";
$queueClient = QueueRestProxy::createQueueService($connectionString);
```

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

**QueueRestProxy nesnesi** **createQueue** yöntemini kullanarak bir kuyruk oluşturmanızı sağlar. Sıra oluştururken, kuyrukta seçenekleri ayarlayabilirsiniz, ancak bunu yapmak gerekli değildir. (Aşağıdaki örnekte, meta verilerin kuyruğa nasıl ayarlanılabilen bir örnek gösterilmektedir.)

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\CreateQueueOptions;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

// OPTIONAL: Set queue metadata.
$createQueueOptions = new CreateQueueOptions();
$createQueueOptions->addMetaData("key1", "value1");
$createQueueOptions->addMetaData("key2", "value2");

try    {
    // Create queue.
    $queueClient->createQueue("myqueue", $createQueueOptions);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [!NOTE]
> Meta veri anahtarları için servis talebi duyarlılığına güvenmemelisiniz. Tüm anahtarlar servisten küçük harfle okunur.
> 
> 

## <a name="add-a-message-to-a-queue"></a>Kuyruğa ileti ekleme

Kuyruğa ileti eklemek için **QueueRestProxy >createMessage'ı**kullanın. Yöntem sıra adını, ileti metnini ve ileti seçeneklerini (isteğe bağlı) alır.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\CreateMessageOptions;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

try    {
    // Create message.
    $queueClient->createMessage("myqueue", "Hello World!");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="peek-at-the-next-message"></a>Sonraki iletiye gözatın

**QueueRestProxy->peekMessages'ı**arayarak kuyruktan çıkarmadan kuyruğun önündeki iletiye (veya iletilere) göz atabilirsiniz. Varsayılan olarak, **peekMessage** yöntemi tek bir ileti döndürür, ancak **PeekMessagesOptions->setNumberOfMessages** yöntemini kullanarak bu değeri değiştirebilirsiniz.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\PeekMessagesOptions;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

// OPTIONAL: Set peek message options.
$message_options = new PeekMessagesOptions();
$message_options->setNumberOfMessages(1); // Default value is 1.

try    {
    $peekMessagesResult = $queueClient->peekMessages("myqueue", $message_options);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$messages = $peekMessagesResult->getQueueMessages();

// View messages.
$messageCount = count($messages);
if($messageCount <= 0){
    echo "There are no messages.<br />";
}
else{
    foreach($messages as $message)    {
        echo "Peeked message:<br />";
        echo "Message Id: ".$message->getMessageId()."<br />";
        echo "Date: ".date_format($message->getInsertionDate(), 'Y-m-d')."<br />";
        echo "Message text: ".$message->getMessageText()."<br /><br />";
    }
}
```

## <a name="de-queue-the-next-message"></a>Sonraki iletiyi sıradan çıkarmak

Kodunuz iki adımda bir iletiyi kuyruktan kaldırır. İlk olarak, **queueRestProxy->listMessages'ı**arayarak, iletiyi kuyruktan okunan diğer kodlara görünmez yapar. Varsayılan olarak bu ileti 30 saniye görünmez kalır. (İleti bu zaman diliminde silinmezse, kuyrukta tekrar görünür hale gelir.) İletiyi kuyruktan kaldırmayı bitirmek için **QueueRestProxy->deleteMessage'ı**aramanız gerekir. İletiyi kaldırma işlemi, kodunuz donanım veya yazılım hatası nedeniyle bir iletiyi işlemezse, kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. Kodunuz ileti işlendikten hemen sonra **deleteMessage'ı** çağırır.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

// Get message.
$listMessagesResult = $queueClient->listMessages("myqueue");
$messages = $listMessagesResult->getQueueMessages();
$message = $messages[0];

/* ---------------------
    Process message.
   --------------------- */

// Get message ID and pop receipt.
$messageId = $message->getMessageId();
$popReceipt = $message->getPopReceipt();

try    {
    // Delete message.
    $queueClient->deleteMessage("myqueue", $messageId, $popReceipt);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="change-the-contents-of-a-queued-message"></a>Kuyruğa alınan iletinin içeriğini değiştirme

**QueueRestProxy->updateMessage'ı**arayarak kuyruktaki iletinin içeriğini yerinde değiştirebilirsiniz. Eğer ileti bir iş görevini temsil ediyorsa, bu özelliği kullanarak iş görevinin durumunu güncelleştirebilirsiniz. Aşağıdaki kod, sıra iletisini yeni içeriklerle güncelleştirir ve görünürlük zaman aşmasını 60 saniye daha uzatacak şekilde ayarlar. Bu, iletiyle ilişkili çalışma durumunu kaydeder ve istemciye ileti üzerinde çalışmaya devam etmesi için bir dakika daha verir. Bir işleme adımı donanım veya yazılım arızasından dolayı başarısız olursa baştan başlamanıza gerek kalmadan kuyruk iletilerindeki çok adımlı iş akışlarını izlemek için bu yöntemi kullanabilirsiniz. Genellikle bir yeniden deneme sayacı tutmanı gerekir ve bir ileti *n* seferden daha fazla yeniden denenirse, silebilirsiniz. Bu, her işlendiğinde bir uygulama hatası tetikleyen bir iletiye karşı koruma sağlar.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Get message.
$listMessagesResult = $queueClient->listMessages("myqueue");
$messages = $listMessagesResult->getQueueMessages();
$message = $messages[0];

// Define new message properties.
$new_message_text = "New message text.";
$new_visibility_timeout = 5; // Measured in seconds.

// Get message ID and pop receipt.
$messageId = $message->getMessageId();
$popReceipt = $message->getPopReceipt();

try    {
    // Update message.
    $queueClient->updateMessage("myqueue",
                                $messageId,
                                $popReceipt,
                                $new_message_text,
                                $new_visibility_timeout);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="additional-options-for-de-queuing-messages"></a>İletileri sıraya alma için ek seçenekler

Bir kuyruktan ileti alma özelleştirebilirsiniz iki yolu vardır. İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak, kodunuzun her iletiyi tam olarak işlemesi için daha fazla veya daha az zaman tanıyan daha uzun veya daha kısa bir görünürlük zaman aşımı ayarlayabilirsiniz. Aşağıdaki kod örneği, tek bir aramada 16 ileti almak için **getMessages** yöntemini kullanır. Daha sonra bir **for** döngüsü kullanarak her iletiyi işler. Ayrıca her ileti için görünmezlik zaman aşımı beş dakika olarak ayarlanır.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;
use MicrosoftAzure\Storage\Queue\Models\ListMessagesOptions;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

// Set list message options.
$message_options = new ListMessagesOptions();
$message_options->setVisibilityTimeoutInSeconds(300);
$message_options->setNumberOfMessages(16);

// Get messages.
try{
    $listMessagesResult = $queueClient->listMessages("myqueue",
                                                     $message_options);
    $messages = $listMessagesResult->getQueueMessages();

    foreach($messages as $message){

        /* ---------------------
            Process message.
        --------------------- */

        // Get message Id and pop receipt.
        $messageId = $message->getMessageId();
        $popReceipt = $message->getPopReceipt();

        // Delete message.
        $queueClient->deleteMessage("myqueue", $messageId, $popReceipt);
    }
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="get-queue-length"></a>Sıra uzunluğunu alma

Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. **QueueRestProxy->getQueueMetadata** yöntemi, kuyruk hizmetinden kuyrukla ilgili meta verileri döndürmesini ister. Döndürülen nesnede **getApproximateMessageCount** yöntemini çağırmak, kuyrukta kaç ileti olduğunu niçin saydığını sağlar. Sıra hizmeti isteğinize yanıt verdikten sonra iletiler eklenebileceği veya kaldırılabildiği için sayım yalnızca yaklaşık olarak dır.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

try    {
    // Get queue metadata.
    $queue_metadata = $queueClient->getQueueMetadata("myqueue");
    $approx_msg_count = $queue_metadata->getApproximateMessageCount();
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

echo $approx_msg_count;
```

## <a name="delete-a-queue"></a>Bir kuyruk silme

Bir sırayı ve tüm iletileri silmek için **QueueRestProxy->deleteQueue** yöntemini arayın.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Queue\QueueRestProxy;
use MicrosoftAzure\Storage\Common\Exceptions\ServiceException;

$connectionString = "DefaultEndpointsProtocol=http;AccountName=<accountNameHere>;AccountKey=<accountKeyHere>";

// Create queue REST proxy.
$queueClient = QueueRestProxy::createQueueService($connectionString);

try    {
    // Delete queue.
    $queueClient->deleteQueue("myqueue");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://msdn.microsoft.com/library/azure/dd179446.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Sıra depolama alanının temellerini öğrendiğiniz için, daha karmaşık depolama görevleri hakkında bilgi edinmek için aşağıdaki bağlantıları izleyin:

* Azure [Depolama PHP İstemci Kitaplığı için API Başvurusu'nu](https://azure.github.io/azure-storage-php/) ziyaret edin
* Gelişmiş [Sıra örneğine](https://github.com/Azure/azure-storage-php/blob/master/samples/QueueSamples.php)bakın.

Daha fazla bilgi için [PHP Geliştirici Merkezi'ne](https://azure.microsoft.com/develop/php/)de bakın.

[download]: https://github.com/Azure/azure-storage-php
[require_once]: https://www.php.net/manual/en/function.require-once.php
[Azure Portal]: https://portal.azure.com
[composer-phar]: https://getcomposer.org/composer.phar
