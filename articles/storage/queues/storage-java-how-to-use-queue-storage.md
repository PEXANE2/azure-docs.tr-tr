---
title: Java-Azure Storage 'da kuyruk depolama 'yı kullanma
description: Kuyruk depolamayı kullanarak kuyruklar oluşturma ve silme hakkında bilgi edinin. Java için Azure Storage istemci kitaplığı ile ileti eklemeyi, Peek, almak ve silmek hakkında bilgi edinin.
author: mhopkins-msft
ms.custom: devx-track-java
ms.author: mhopkins
ms.date: 08/19/2020
ms.service: storage
ms.subservice: queues
ms.topic: how-to
ms.reviewer: dineshm
ms.openlocfilehash: dbb20d5331275f73945a4ad2ba3f428a3e415797
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88681762"
---
# <a name="how-to-use-queue-storage-from-java"></a>Java’dan Kuyruk depolama kullanma

Bu kılavuzda, Azure kuyruk depolama hizmetini kullanarak yaygın senaryolar için nasıl kod yapılacağı gösterilir. Örnekler Java dilinde yazılır ve [Java için Azure Depolama SDK’sı][Azure Storage SDK for Java] kullanır. Senaryolar, sıra iletilerini **ekleme**, göz **atma**, **alma**ve **silmeyi** içerir. Sıra **oluşturma** ve **silme** için kod de ele alınmıştır. Kuyruklar hakkında daha fazla bilgi için [sonraki adımlar](#next-steps) bölümüne bakın.

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java uygulaması oluşturma

# <a name="java-v12"></a>[Java V12](#tab/java)

İlk olarak, geliştirme sisteminizin [Java V12 Için Azure kuyruk depolama istemci kitaplığı](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-queue)'nda listelenen önkoşulları karşıladığını doğrulayın.

Kuyruklar adlı bir Java uygulaması oluşturmak için *-nasıl yapılır-V12*:

1. Konsol penceresinde (cmd, PowerShell veya Bash gibi), Maven kullanarak *Kuyruklar-nasıl yapılır-V12*adlı yeni bir konsol uygulaması oluşturun. "Hello World!" oluşturmak için aşağıdaki **MVN** komutunu yazın Java projesi.

   ```bash
    mvn archetype:generate \
        --define interactiveMode=n \
        --define groupId=com.queues.howto \
        --define artifactId=queues-howto-v12 \
        --define archetypeArtifactId=maven-archetype-quickstart \
        --define archetypeVersion=1.4
   ```

   ```powershell
    mvn archetype:generate `
        --define interactiveMode=n `
        --define groupId=com.queues.howto `
        --define artifactId=queues-howto-v12 `
        --define archetypeArtifactId=maven-archetype-quickstart `
        --define archetypeVersion=1.4
    ```

1. Projenin üretilme çıktısı şuna benzer görünmelidir:

    ```console
    [INFO] Scanning for projects...
    [INFO]
    [INFO] ------------------< org.apache.maven:standalone-pom >-------------------
    [INFO] Building Maven Stub Project (No POM) 1
    [INFO] --------------------------------[ pom ]---------------------------------
    [INFO]
    [INFO] >>> maven-archetype-plugin:3.1.2:generate (default-cli) > generate-sources @ standalone-pom >>>
    [INFO]
    [INFO] <<< maven-archetype-plugin:3.1.2:generate (default-cli) < generate-sources @ standalone-pom <<<
    [INFO]
    [INFO]
    [INFO] --- maven-archetype-plugin:3.1.2:generate (default-cli) @ standalone-pom ---
    [INFO] Generating project in Batch mode
    [INFO] ----------------------------------------------------------------------------
    [INFO] Using following parameters for creating project from Archetype: maven-archetype-quickstart:1.4
    [INFO] ----------------------------------------------------------------------------
    [INFO] Parameter: groupId, Value: com.queues.howto
    [INFO] Parameter: artifactId, Value: queues-howto-v12
    [INFO] Parameter: version, Value: 1.0-SNAPSHOT
    [INFO] Parameter: package, Value: com.queues.howto
    [INFO] Parameter: packageInPathFormat, Value: com/queues/howto
    [INFO] Parameter: version, Value: 1.0-SNAPSHOT
    [INFO] Parameter: package, Value: com.queues.howto
    [INFO] Parameter: groupId, Value: com.queues.howto
    [INFO] Parameter: artifactId, Value: queues-howto-v12
    [INFO] Project created from Archetype in dir: C:\queues\queues-howto-v12
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time:  6.775 s
    [INFO] Finished at: 2020-08-17T15:27:31-07:00
    [INFO] ------------------------------------------------------------------------
        ```

1. Switch to the newly created *queues-howto-v12* directory.

   ```console
   cd queues-howto-v12
   ```

### <a name="install-the-package"></a>Paketi yükler

*pom.xml* dosyasını metin düzenleyicinizde açın. Aşağıdaki bağımlılık öğesini bağımlılıklar grubuna ekleyin.

```xml
<dependency>
  <groupId>com.azure</groupId>
  <artifactId>azure-storage-queue</artifactId>
  <version>12.6.0</version>
</dependency>
```

# <a name="java-v8"></a>[Java V8](#tab/java8)

İlk olarak, geliştirme sisteminizin [Java V8 Için Azure depolama SDK 'sında](https://github.com/azure/azure-storage-java)listelenen önkoşulları karşıladığını doğrulayın. Java için Azure depolama kitaplıklarını indirme ve yükleme yönergelerini izleyin. Daha sonra bu makaledeki örnekleri kullanarak bir Java uygulaması oluşturabilirsiniz.

---

## <a name="configure-your-application-to-access-queue-storage"></a>Uygulamanızı kuyruk depolamaya erişecek şekilde yapılandırma

Aşağıdaki Import deyimlerini, kuyruklara erişmek için Azure depolama API 'Lerini kullanmak istediğiniz Java dosyasının en üstüne ekleyin:

# <a name="java-v12"></a>[Java V12](#tab/java)

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_ImportStatements":::

# <a name="java-v8"></a>[Java V8](#tab/java8)

```java
// Include the following imports to use queue APIs.
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.queue.*;
```

---

## <a name="set-up-an-azure-storage-connection-string"></a>Azure depolama bağlantı dizesi ayarlama

Azure Storage istemcisi, veri yönetimi hizmetlerine erişmek için bir depolama bağlantı dizesi kullanır. [Azure Portal](https://portal.azure.com)listelenen depolama hesabınız için bir ad ve birincil erişim anahtarı alın. Bunları bağlantı dizesindeki *AccountName* ve *accountkey* değerleri olarak kullanın. Bu örnekte bağlantı dizesini tutmak için nasıl statik bir alan bildirebileceğiniz gösterilmektedir:

# <a name="java-v12"></a>[Java V12](#tab/java)

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_ConnectionString":::

# <a name="java-v8"></a>[Java V8](#tab/java8)

```java
// Define the connection-string with your values.
final String storageConnectionString =
    "DefaultEndpointsProtocol=https;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```
Bu dizeyi *ServiceConfiguration. cscfg*adlı hizmet yapılandırma dosyasında saklayabilirsiniz. Microsoft Azure rolü içinde çalışan bir uygulama için, **Roleenvironment. getConfigurationSettings**çağırarak bağlantı dizesine erişin. *StorageConnectionString*adlı bir **ayar** öğesinden bağlantı dizesi alma örneği aşağıda verilmiştir:

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

---

Aşağıdaki örnekler, depolama bağlantı dizesini içeren bir **dize** nesneniz olduğunu varsayar.

## <a name="how-to-create-a-queue"></a>Nasıl yapılır: kuyruk oluşturma

# <a name="java-v12"></a>[Java V12](#tab/java)

Bir **Queueclient** nesnesi, bir kuyrukla etkileşim kurma işlemlerini içerir. Aşağıdaki kod bir **Queueclient** nesnesi oluşturur. Kullanmak istediğiniz kuyruğu oluşturmak için **Queueclient** nesnesini kullanın.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_CreateQueue":::

# <a name="java-v8"></a>[Java V8](#tab/java8)

**Cloudqueueclient** nesnesi, kuyruklar için başvuru nesneleri almanızı sağlar. Aşağıdaki kod bir **Cloudqueueclient** nesnesi oluşturur. (Örneğin, **cloudstorageaccount** nesneleri oluşturmanın başka yolları vardır; daha fazla bilgi için bkz. [Azure Storage Client SDK başvurusunda] **cloudstorageaccount** .)

Kullanmak istediğiniz kuyruğa bir başvuru almak için **Cloudqueueclient** nesnesini kullanın. Sıra yoksa kuyruğu oluşturabilirsiniz.

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

---

## <a name="how-to-add-a-message-to-a-queue"></a>Nasıl yapılır: bir kuyruğa ileti ekleme

# <a name="java-v12"></a>[Java V12](#tab/java)

Mevcut bir sıraya bir ileti eklemek için, **SendMessage** metodunu çağırın. İleti bir dize (UTF-8 biçiminde) ya da bir bayt dizisi olabilir. Sıraya bir dize iletisi gönderen kod aşağıda verilmiştir.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_AddMessage":::

# <a name="java-v8"></a>[Java V8](#tab/java8)

Varolan bir sıraya bir ileti yerleştirmek için ilk olarak yeni bir **CloudQueueMessage** oluşturun. Sonra **AddMessage** yöntemini çağırın. **CloudQueueMessage** bir dizeden (UTF-8 biçiminde) veya bir bayt dizisinden oluşturulabilir. Burada bir sıra (yoksa) oluşturan ve "Hello, World" iletisini ekleyen kod verilmiştir.

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

---

## <a name="how-to-peek-at-the-next-message"></a>Nasıl yapılır: bir sonraki iletiye göz atma

**PeekMessage**çağırarak bir kuyruğun ön tarafındaki iletiye göz atmayı sağlayabilirsiniz.

# <a name="java-v12"></a>[Java V12](#tab/java)

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_PeekMessage":::

# <a name="java-v8"></a>[Java V8](#tab/java8)

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

---

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Nasıl yapılır: kuyruğa alınan iletinin içeriğini değiştirme

Kuyrukta yer alan bir iletinin içeriğini değiştirebilirsiniz. İleti bir iş görevini temsil ediyorsa, durumu güncelleştirmek için bu özelliği kullanabilirsiniz. Aşağıdaki kod, yeni içerikle bir kuyruk iletisini güncelleştirir ve görünürlük zaman aşımını 30 saniye daha uzatmak için ayarlar. Görünürlük zaman aşımını genişletmek, istemciye ileti üzerinde çalışmaya devam etmesi için bir 30 saniye daha verir. Yeniden deneme sayısı da tutabilirsiniz. İleti *n* kereden fazla yeniden denenip onu silersiniz. Bu senaryo, her işlendiği zaman bir uygulama hatasını tetikleyen bir iletiye karşı koruma sağlar.

# <a name="java-v12"></a>[Java V12](#tab/java)

Aşağıdaki kod örneği, ileti sırasını arar, bir arama dizesiyle eşleşen ilk ileti içeriğini bulur, ileti içeriğini değiştirir ve çıkar.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_UpdateSearchMessage":::

# <a name="java-v8"></a>[Java V8](#tab/java8)

Aşağıdaki kod örneği, ileti sırasını araştırır, "Hello, World" ile eşleşen ilk ileti içeriğini bulur, ileti içeriğini değiştirir ve çıkar.

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

---

Aşağıdaki kod örneği, kuyruktaki yalnızca ilk görünen iletiyi güncelleştirir.

# <a name="java-v12"></a>[Java V12](#tab/java)

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_UpdateFirstMessage":::

# <a name="java-v8"></a>[Java V8](#tab/java8)

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

---

## <a name="how-to-get-the-queue-length"></a>Nasıl yapılır: kuyruk uzunluğunu alma

Bir kuyruktaki ileti sayısı ile ilgili bir tahmin alabilirsiniz.

# <a name="java-v12"></a>[Java V12](#tab/java)

**GetProperties** yöntemi, kuyruk hizmeti birkaç geçerli değeri ister. Değerlerden biri, bir kuyrukta kaç ileti olduğunu gösteren bir sayısıdır. Sayım yalnızca, isteğiniz sonunda iletiler eklenebildiğinden veya kaldırılacağından yaklaşık olur. **Getapproximatemessagecount** yöntemi, kuyruk hizmeti çağırılmadan **GetProperties**çağrısı tarafından alınan son değeri döndürür.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_GetQueueLength":::

# <a name="java-v8"></a>[Java V8](#tab/java8)

**Downloadattributes** yöntemi, kuyruk hizmeti birkaç geçerli değeri ister. Değerlerden biri, bir kuyrukta kaç ileti olduğunu gösteren bir sayısıdır. Sayım yalnızca, isteğiniz sonunda iletiler eklenebildiğinden veya kaldırılacağından yaklaşık olur. **Getapproximatemessagecount** metodu, kuyruk hizmeti çağırılmadan **downloadattributes**çağrısının aldığı son değeri döndürür.

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

---

## <a name="how-to-dequeue-the-next-message"></a>Nasıl yapılır: sonraki iletiyi sıradan çıkarma

# <a name="java-v12"></a>[Java V12](#tab/java)

Kodunuz, iki adımda bir kuyruktan bir ileti sırasını kaldırır. **ReceiveMessage**' ı çağırdığınızda, sıradaki bir sonraki iletiyi alırsınız. **ReceiveMessage** 'ten döndürülen bir ileti, bu kuyruktan gelen diğer kod okuma iletileri için görünmez hale gelir. Varsayılan olarak bu ileti 30 saniye görünmez kalır. İletiyi kuyruktan kaldırmayı tamamlaması için, **deleteMessage**' ı da çağırmanız gerekir. Kodunuz bir iletiyi işleyemezse, bu iki adımlı işlem aynı iletiyi almanızı ve yeniden denemeyi sağlar. Kodunuz, ileti işlendikten hemen sonra **deleteMessage** ' i çağırır.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_DequeueMessage":::

# <a name="java-v8"></a>[Java V8](#tab/java8)

Kodunuz, iki adımda bir kuyruktan bir ileti sırasını kaldırır. **Retrievemessage**çağırdığınızda bir kuyruktaki sonraki iletiyi alırsınız. **Retrievemessage** 'ten döndürülen bir ileti, bu kuyruktan gelen diğer kod okuma iletileri için görünmez hale gelir. Varsayılan olarak bu ileti 30 saniye görünmez kalır. İletiyi kuyruktan kaldırmayı tamamlaması için, **deleteMessage**' ı da çağırmanız gerekir. Kodunuz bir iletiyi işleyemezse, bu iki adımlı işlem aynı iletiyi almanızı ve yeniden denemeyi sağlar. Kodunuz, ileti işlendikten hemen sonra **deleteMessage** ' i çağırır.

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

---

## <a name="additional-options-for-dequeuing-messages"></a>Dequeuing iletileri için ek seçenekler

İleti alımı bir kuyruktan özelleştirmenin iki yolu vardır. İlk olarak, bir toplu ileti alın (32 'e kadar). İkincisi, daha uzun veya daha kısa bir zaman aşımı ayarlayın, böylece kodunuzun her iletiyi tamamen işlemesi için daha fazla veya daha az zaman.

# <a name="java-v12"></a>[Java V12](#tab/java)

Aşağıdaki kod örneği, bir çağrıda 20 ileti almak için **receivemessages** yöntemini kullanır. Sonra her iletiyi bir **for** döngüsü kullanarak işler. Ayrıca, her ileti için geçersiz kılma zaman aşımını beş dakikaya (300 saniye) ayarlar. Zaman aşımı, tüm iletiler için aynı anda başlar. **Receivemessages**çağrısından bu yana beş dakika geçtiğinde, silinmeyen iletiler yeniden görünür hale gelir.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_DequeueMessages":::

# <a name="java-v8"></a>[Java V8](#tab/java8)

Aşağıdaki kod örneği, tek bir çağrıda 20 ileti almak için **retrievemessages** yöntemini kullanır. Sonra her iletiyi bir **for** döngüsü kullanarak işler. Ayrıca, her ileti için geçersiz kılma zaman aşımını beş dakikaya (300 saniye) ayarlar. Zaman aşımı, tüm iletiler için aynı anda başlar. **Retrievemessages**çağrısından bu yana beş dakika geçtiğinde, silinmeyen iletiler yeniden görünür hale gelir.

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

---

## <a name="how-to-list-the-queues"></a>Nasıl yapılır: kuyrukları listeleme

# <a name="java-v12"></a>[Java V12](#tab/java)

Geçerli sıraların bir listesini almak için, bir **Queueıtem** nesneleri koleksiyonu döndüren **QueueServiceClient. listqueues ()** yöntemini çağırın.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_ListQueues":::

# <a name="java-v8"></a>[Java V8](#tab/java8)

Geçerli sıraların bir listesini almak için **Cloudqueueclient. listQueues ()** yöntemini çağırın ve bu, **cloudqueue** nesnelerinin bir koleksiyonunu döndürür.

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

---

## <a name="how-to-delete-a-queue"></a>Nasıl yapılır: kuyruğu silme

# <a name="java-v12"></a>[Java V12](#tab/java)

Bir kuyruğu ve içerdiği tüm iletileri silmek için **Queueclient** nesnesinde **Delete** yöntemini çağırın.

:::code language="java" source="~/azure-storage-snippets/queues/howto/java/java-v12/src/main/java/com/queues/howto/App.java" id="Snippet_DeleteMessageQueue":::

# <a name="java-v8"></a>[Java V8](#tab/java8)

Bir kuyruğu ve içerdiği tüm iletileri silmek için **Cloudqueue** nesnesinde **deleteifexists** yöntemini çağırın.

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

---

[!INCLUDE [storage-check-out-samples-java](../../../includes/storage-check-out-samples-java.md)]

## <a name="next-steps"></a>Sonraki adımlar

Sıra depolamanın temellerini öğrendiğinize göre, daha karmaşık depolama görevleri hakkında bilgi edinmek için bu bağlantıları izleyin.

* [Java için Azure Depolama SDK'sı][Azure Storage SDK for Java]
* [Azure Depolama İstemcisi SDK Başvurusu][Azure Storage Client SDK Reference]
* [Azure Storage Hizmetleri REST API’si][Azure Storage Services REST API]
* [Azure Depolama Ekibi Blogu][Azure Storage Team Blog]

[Azure SDK for Java]: https://github.com/azure/azure-sdk-for-java
[Azure Storage SDK for Java]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage
[Azure Depolama İstemcisi SDK Başvurusu]: https://azure.github.io/azure-sdk-for-java/storage.html
[Azure Storage Services REST API]: https://docs.microsoft.com/rest/api/storageservices/
[Azure Storage Team Blog]: https://techcommunity.microsoft.com/t5/azure-storage/bg-p/AzureStorageBlog
