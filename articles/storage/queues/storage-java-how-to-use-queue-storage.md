---
title: Java'dan Sıra depolama nasıl kullanılır - Azure Depolama
description: Kuyrukoluşturmak ve silmek için Kuyruk depolama alanını nasıl kullanacağınızı ve Java için Azure Depolama istemci kitaplığıyla ileti eklemeyi, almayı ve silmeyi öğrenin.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 12/08/2016
ms.service: storage
ms.subservice: queues
ms.topic: conceptual
ms.reviewer: cbrooks
ms.openlocfilehash: 7658b8541e7a79a5e547a6649b35681446e34b0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067140"
---
# <a name="how-to-use-queue-storage-from-java"></a>Java’dan Kuyruk depolama kullanma

[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

Bu kılavuz, Azure Kuyruk depolama hizmetini kullanarak sık karşılaşılan senaryoları nasıl gerçekleştireceğinizi gösterir. Örnekler Java dilinde yazılır ve [Java için Azure Depolama SDK’sı][Azure Storage SDK for Java] kullanır. Kapsanan senaryolar arasında kuyruk iletileri **ekleme,** **gözetleme,** **alma**ve **silme** nin yanı sıra kuyruk **oluşturma** ve **silme** yer alır. Kuyruklar hakkında daha fazla bilgi için [Sonraki adımlar](#next-steps) bölümüne bakın.

> [!NOTE]
> Android cihazlarda Azure Depolama kullanan geliştiriciler için bir SDK mevcuttur. Daha fazla bilgi için bkz. [Android için Azure Depolama SDK’sı][Azure Storage SDK for Android].

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java uygulaması oluşturma

Bu kılavuzda, bir Java uygulaması içinde yerel olarak veya Azure'daki bir web uygulaması içinde çalışan kodda çalıştırılabilen depolama özelliklerini kullanırsınız.

Bunu yapmak için Java Geliştirme Kiti'ni (JDK) yüklemeniz ve Azure aboneliğinizde bir Azure depolama hesabı oluşturmanız gerekir. Bunu yaptıktan sonra, geliştirme sisteminizin GitHub'daki Java deposu için Azure Depolama [SDK'sında][Azure Storage SDK for Java] listelenen minimum gereksinimleri ve bağımlılıkları karşıladığını doğrulamanız gerekir. Sisteminiz bu gereksinimleri karşılıyorsa, sisteminizdeki Java için Azure Depolama Kitaplıklarını indirme ve yükleme yönergelerini bu depodan takip edebilirsiniz. Bu görevleri tamamladıktan sonra, bu makaledeki örnekleri kullanan bir Java uygulaması oluşturabilirsiniz.

## <a name="configure-your-application-to-access-queue-storage"></a>Uygulamanızı sıra depolamaalanına erişmek için yapılandırın

Kuyruklara erişmek için Azure depolama API'lerini kullanmak istediğiniz Java dosyasının en üstüne aşağıdaki içe aktarma ekstrelerini ekleyin:

```java
// Include the following imports to use queue APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.queue.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a>Azure depolama bağlantı dizesi ayarlama

Azure depolama istemcisi, veri yönetimi hizmetlerine erişmek üzere uç noktaları ve kimlik bilgilerini depolamak için bir depolama bağlantı dizesi kullanır. Bir istemci uygulamada çalışırken, *AccountName* ve *AccountKey* için [Azure portalında](https://portal.azure.com) listelenen depolama hesabınızın adını ve depolama hesabı Birincil erişim anahtarını kullanarak depolama bağlantı dizesini aşağıdaki biçimde sağlamanız gerekir. Bu örnekte bağlantı dizesini tutmak için nasıl statik bir alan bildirebileceğiniz gösterilmektedir:

```java
// Define the connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

Microsoft Azure'da bir rol içinde çalışan bir uygulamada, bu dize hizmet yapılandırma dosyasında depolanabilir, *ServiceConfiguration.cscfg*, ve **RoleEnvironment.getConfigurationSettings** yöntemine yapılan bir çağrıyla erişilebilir. Yapılandırma dosyasındaki *StorageConnectionString* adlı bir **Ayar** öğesinden bağlantı dizesini alma örneği aşağıda verilmiştir:

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

Aşağıdaki örnekler, depolama bağlantı dizesini almak için bu iki yöntemden birini kullandığınızı varsayar.

## <a name="how-to-create-a-queue"></a>Nasıl yapılı: Sıra oluşturma
**CloudQueueClient** nesnesi, kuyruklar için başvuru nesneleri almanızı sağlar. Aşağıdaki kod bir **CloudQueueClient** nesnesi oluşturur. (Not: **CloudStorageAccount** nesneleri oluşturmanın ek yolları vardır; daha fazla bilgi için [Azure Depolama İstemci SDK Başvurusu'nda] **CloudStorageAccount'a** bakın .)

Kullanmak istediğiniz kuyruğa başvuru almak için **CloudQueueClient** nesnesini kullanın. Yoksa sırayı oluşturabilirsiniz.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

   // Create the queue client.
   CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

   // Retrieve a reference to a queue.
   CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Create the queue if it doesn't already exist.
   queue.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-a-message-to-a-queue"></a>Nasıl yapilir: Kuyruğa ileti ekleme
Varolan bir sıraya bir ileti yerleştirmek için ilk olarak yeni bir **CloudQueueMessage** oluşturun. Ardından **addMessage** yöntemini arayın. **CloudQueueMessage** bir dizeden (UTF-8 biçiminde) veya bir bayt dizisinden oluşturulabilir. Burada bir sıra oluşturan kod (yoksa) ve "Merhaba, Dünya" iletisi ekler.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.createIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.addMessage(message);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-peek-at-the-next-message"></a>Nasıl yapılsın: Bir sonraki iletiye göz atın
**PeekMessage'ı**arayarak sıranın önündeki iletiye göz atabilirsiniz.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Peek at the next message.
    CloudQueueMessage peekedMessage = queue.peekMessage();

    // Output the message value.
    if (peekedMessage != null)
    {
      System.out.println(peekedMessage.getMessageContentAsString());
   }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Nasıl yapilir: Sıralı iletinin içeriğini değiştirme
Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. Eğer ileti bir iş görevini temsil ediyorsa, bu özelliği kullanarak iş görevinin durumunu güncelleştirebilirsiniz. Aşağıdaki kod kuyruk iletisini yeni içeriklerle güncelleştirir ve görünürlük zaman aşımını 60 saniye daha uzatır. Görünürlük zaman anına kadar genişletmek, iletiyle ilişkili çalışma durumunu kaydeder ve istemciye ileti üzerinde çalışmaya devam etmesi için bir dakika daha verir. Bir işleme adımı donanım veya yazılım arızasından dolayı başarısız olursa baştan başlamanıza gerek kalmadan kuyruk iletilerindeki çok adımlı iş akışlarını izlemek için bu yöntemi kullanabilirsiniz. Genellikle bir yeniden deneme sayacı tutmanı gerekir ve bir ileti *n* seferden daha fazla yeniden denenirse, silebilirsiniz. Bu, her işlendiğinde bir uygulama hatası tetikleyen bir iletiye karşı koruma sağlar.

Aşağıdaki kod örneği ileti kuyruğunda arama lar, içerik için "Merhaba, Dünya" ile eşleşen ilk iletiyi bulur, ardından ileti içeriğini ve çıkışları değiştirir.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // The maximum number of messages that can be retrieved is 32.
    final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

    // Loop through the messages in the queue.
    for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
    {
        // Check for a specific string.
        if (message.getMessageContentAsString().equals("Hello, World"))
        {
            // Modify the content of the first matching message.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 30 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 30, updateFields, null, null);
            break;
        }
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

Alternatif olarak, aşağıdaki kod örneği yalnızca kuyruktaki ilk görünür iletiyi güncelleştirir.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage message = queue.retrieveMessage();

    if (message != null)
    {
        // Modify the message content.
        message.setMessageContent("Updated contents.");
        // Set it to be visible in 60 seconds.
        EnumSet<MessageUpdateFields> updateFields =
            EnumSet.of(MessageUpdateFields.CONTENT,
            MessageUpdateFields.VISIBILITY);
        // Update the message.
        queue.updateMessage(message, 60, updateFields, null, null);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-get-the-queue-length"></a>Nasıl yapılır: Sıra uzunluğunu alma
Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz. **DownloadAttributes** yöntemi, kuyrukta kaç ileti olduğunu saymak da dahil olmak üzere çeşitli geçerli değerler için Sıra hizmeti ister. Sıra hizmeti isteğinize yanıt verdikten sonra iletiler eklenebileceği veya kaldırılabildiği için sayım yalnızca yaklaşık olarak dır. **getApproximateMessageCount** yöntemi, Çağrı'nın **GelenE'yi indirmesi**için alınan son değeri Sıra hizmetini aramadan döndürür.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
       CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

   // Download the approximate message count from the server.
    queue.downloadAttributes();

    // Retrieve the newly cached approximate message count.
    long cachedMessageCount = queue.getApproximateMessageCount();

    // Display the queue length.
    System.out.println(String.format("Queue length: %d", cachedMessageCount));
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-dequeue-the-next-message"></a>Nasıl yapılsın: Bir sonraki iletide sırayı silme
Kodunuz iki adımda bir kuyruktaki bir iletiyi sıradan uzaklar. **retrieveMessage'ı**aradiğinizde, bir sonraki iletiyi kuyrukta alırsınız. **retrieveMessage'dan** döndürülen ileti, bu kuyruktan gelen diğer kod okuma iletileri için görünmez olur. Varsayılan olarak bu ileti 30 saniye görünmez kalır. İletiyi kuyruktan kaldırmayı bitirmek için **deleteMessage'ı**da aramanız gerekir. Bir iletinin iki adımlı kaldırılma süreci, donanım veya yazılım arızasından dolayı kodunuzun bir iletiyi işleyememesi durumunda kodunuzun başka bir örneğinin aynı iletiyi alıp yeniden denemesini sağlar. Kodunuz ileti işlendikten hemen sonra **deleteMessage'ı** çağırır.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve the first visible message in the queue.
    CloudQueueMessage retrievedMessage = queue.retrieveMessage();

    if (retrievedMessage != null)
    {
        // Process the message in less than 30 seconds, and then delete the message.
        queue.deleteMessage(retrievedMessage);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="additional-options-for-dequeuing-messages"></a>İletileri sıraya alma için ek seçenekler
İletilerin bir kuyruktan alınma şeklini iki yöntemle özelleştirebilirsiniz. İlk olarak toplu iletiler alabilirsiniz (en fazla 32). İkinci olarak daha uzun veya daha kısa bir görünmezlik süresi ayarlayarak kodunuzun her iletiyi tamamen işlemesi için daha az veya daha fazla zaman tanıyabilirsiniz.

Aşağıdaki kod örneği, tek bir aramada 20 ileti almak için **retrieveMessages** yöntemini kullanır. Daha sonra her iletiyi **for** döngüsü kullanarak işler. Ayrıca, görünmezlik zaman anına her ileti için beş dakika (300 saniye) olarak ayarlar. Tüm iletiler için beş dakika aynı anda başlar, bu nedenle **İletileri alma**çağrısından bu yana beş dakika geçtiğinde, silinmemiş iletiler yeniden görünür hale gelir.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
        // Do processing for all messages in less than 5 minutes,
        // deleting each message after processing.
        queue.deleteMessage(message);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-the-queues"></a>Nasıl yapılı: Kuyrukları listele etme
Geçerli kuyrukların listesini almak için **CloudQueueClient.listQueues()** yöntemini arayabilirsiniz ve bu yöntem **CloudQueue** nesnelerinden oluşan bir koleksiyon döndürecek.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient =
        storageAccount.createCloudQueueClient();

    // Loop through the collection of queues.
    for (CloudQueue queue : queueClient.listQueues())
    {
        // Output each queue name.
        System.out.println(queue.getName());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-queue"></a>Nasıl yapılı: Sırayı silme
Bir kuyruğ ve içinde bulunan tüm iletileri silmek için **CloudQueue** nesnesindeki **deleteIfExists** yöntemini arayın.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.getQueueReference("myqueue");

    // Delete the queue if it exists.
    queue.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="next-steps"></a>Sonraki adımlar
Artık sıra depolamanın temellerini öğrendiğiniz için, daha karmaşık depolama görevleri hakkında bilgi edinmek için bu bağlantıları izleyin.

* [Java için Azure Depolama SDK'sı][Azure Storage SDK for Java]
* [Azure Depolama İstemcisi SDK Başvurusu][Azure Storage Client SDK Reference]
* [Azure Storage Hizmetleri REST API’si][Azure Storage Services REST API]
* [Azure Depolama Ekibi Blogu][Azure Storage Team Blog]

[Azure SDK for Java]: https://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Depolama İstemcisi SDK Başvurusu]: https://javadoc.io/doc/com.microsoft.azure/azure-core/0.8.0/index.html
[Azure Storage Services REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: https://blogs.msdn.com/b/windowsazurestorage/
