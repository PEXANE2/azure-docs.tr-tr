---
title: 'Hızlı başlangıç: Azure Sıra depolama kitaplığı v12 - Java'
description: Kuyruk oluşturmak ve kuyruğa ileti eklemek için Azure Queue Java v12 kitaplığını nasıl kullanacağınızı öğrenin. Ardından, sıradaki iletileri nasıl okuyup sildiğinizi öğrenirsiniz. Ayrıca bir sırayı nasıl sildiğinizi de öğreneceksiniz.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 12/4/2019
ms.service: storage
ms.subservice: queues
ms.topic: quickstart
ms.openlocfilehash: 9cfedd322db721156584844e949724ab2d104968
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78199810"
---
# <a name="quickstart-azure-queue-storage-client-library-v12-for-java"></a>Quickstart: Java için Azure Queue depolama istemcikitaplığı v12

Java için Azure Queue depolama istemcisi kitaplığı sürümü 12 ile başlayın. Azure Sıra depolama, daha sonra alma ve işleme için çok sayıda ileti depolamak için bir hizmettir. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.

Aşağıdakiler için Java için Azure Queue depolama istemcisi v12'yi kullanın:

* Bir kuyruk oluşturma
* Kuyruğa ileti ekleme
* Kuyruktaki iletilere göz atın
* Kuyruktaki bir iletiyi güncelleştirme
* Sıradaki iletileri alma ve silme
* Bir kuyruk silme

[API başvuru belgeleri](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-queue/12.0.0/index.html) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-queue) | [Paketi (Maven)](https://mvnrepository.com/artifact/com.azure/azure-storage-queue) | [Örnekleri](https://docs.microsoft.com/azure/storage/common/storage-samples-java?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#queue-samples)

## <a name="prerequisites"></a>Ön koşullar

* [Java Geliştirme Kiti (JDK)](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable) sürüm 8 veya üzeri
* [Apaçi Maven](https://maven.apache.org/download.cgi)
* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* Azure depolama hesabı - [bir depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)

## <a name="setting-up"></a>Ayarlama

Bu bölüm, Java için Azure Queue depolama istemcisi kitaplığı v12 ile çalışmak üzere bir proje hazırlamakonusunda size yol sunar.

### <a name="create-the-project"></a>Proje oluşturma

*Kuyruklar-quickstart-v12*adlı bir Java uygulaması oluşturun.

1. Konsol penceresinde (cmd, PowerShell veya Bash gibi), ad *kuyrukları-quickstart-v12*ile yeni bir konsol uygulaması oluşturmak için Maven kullanın. "Merhaba dünya!" oluşturmak için aşağıdaki **mvn** komutunu yazın. Java projesi.

   ```console
   mvn archetype:generate -DgroupId=com.queues.quickstart \
                          -DartifactId=queues-quickstart-v12 \
                          -DarchetypeArtifactId=maven-archetype-quickstart \
                          -DarchetypeVersion=1.4 \
                          -DinteractiveMode=false
   ```

1. Proje oluşturma çıktısı şuna benzer:

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
    [INFO] Parameter: groupId, Value: com.queues.quickstart
    [INFO] Parameter: artifactId, Value: queues-quickstart-v12
    [INFO] Parameter: version, Value: 1.0-SNAPSHOT
    [INFO] Parameter: package, Value: com.queues.quickstart
    [INFO] Parameter: packageInPathFormat, Value: com/queues/quickstart
    [INFO] Parameter: version, Value: 1.0-SNAPSHOT
    [INFO] Parameter: package, Value: com.queues.quickstart
    [INFO] Parameter: groupId, Value: com.queues.quickstart
    [INFO] Parameter: artifactId, Value: queues-quickstart-v12
    [INFO] Project created from Archetype in dir: C:\quickstarts\queues\queues-quickstart-v12
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time:  6.394 s
    [INFO] Finished at: 2019-12-03T09:58:35-08:00
    [INFO] ------------------------------------------------------------------------
    ```

1. Yeni oluşturulan *kuyruklar-quickstart-v12* dizinine geçin.

   ```console
   cd queues-quickstart-v12
   ```

### <a name="install-the-package"></a>Paketi yükleyin

Metin düzenleyicinizdeki *pom.xml* dosyasını açın. Bağımlılıklar grubuna aşağıdaki bağımlılık öğesini ekleyin.

```xml
<dependency>
  <groupId>com.azure</groupId>
  <artifactId>azure-storage-queue</artifactId>
  <version>12.0.1</version>
</dependency>
```

### <a name="set-up-the-app-framework"></a>Uygulama çerçevesini ayarlama

Proje dizininden:

1. */src/main/java/com/queues/quickstart* dizinine gidin
1. Editörünüzde *App.java* dosyasını açın
1. İfadeyi `System.out.println("Hello world!");` silme
1. `import` Yönergeekleme

İşte kod:

```java
package com.queues.quickstart;

/**
 * Azure queue storage v12 SDK quickstart
 */
import com.azure.storage.queue.*;
import com.azure.storage.queue.models.*;
import java.io.*;
import java.time.*;

public class App
{
    public static void main( String[] args ) throws IOException
    {
    }
}
```

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="object-model"></a>Nesne modeli

Azure Kuyruk depolama, çok sayıda iletiyi depolamaya yönelik bir hizmettir. Bir sıra iletisi 64 KB boyutuna kadar olabilir. Bir sıra, bir depolama hesabının toplam kapasite sınırına kadar milyonlarca ileti içerebilir. Kuyruklar genellikle eşzamanlı olarak işlemek için bir çalışma biriktirme listesi oluşturmak için kullanılır. Sıra depolama üç tür kaynak sunar:

* Depolama hesabı
* Depolama hesabında bir sıra
* Sıra içindeki iletiler

Aşağıdaki diyagramda bu kaynaklar arasındaki ilişki gösterilmektedir.

![Sıra depolama mimarisi diyagramı](./media/storage-queues-introduction/queue1.png)

Bu kaynaklarla etkileşimde kalmak için aşağıdaki Java sınıflarını kullanın:

* [QueueClientBuilder](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-queue/12.0.0/com/azure/storage/queue/QueueClientBuilder.html): `QueueClientBuilder` Sınıf bir `QueueClient` nesneyi yapılandırır ve anında alır.
* [QueueServiceClient](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-queue/12.0.0/com/azure/storage/queue/QueueServiceClient.html): `QueueServiceClient` Depolama hesabınızdaki tüm kuyrukları yönetmenize olanak tanır.
* [QueueClient](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-queue/12.0.0/com/azure/storage/queue/QueueClient.html): `QueueClient` Sınıf, tek bir sırayı ve iletilerini yönetmenize ve işlemenize olanak tanır.
* [QueueMessageItem](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-queue/12.0.0/com/azure/storage/queue/models/QueueMessageItem.html): `QueueMessageItem` Sınıf, bir sırada [receiveMessages'ı](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-queue/12.0.0/com/azure/storage/queue/QueueClient.html#receiveMessages-java.lang.Integer-) ararken döndürülen tek tek nesneleri temsil eder.

## <a name="code-examples"></a>Kod örnekleri

Bu örnek kod parçacıkları, Java için Azure Kuyruk depolama istemcisi kitaplığıyla aşağıdaki işlemleri nasıl yapacağınızı gösterir:

* [Bağlantı dizesini alma](#get-the-connection-string)
* [Bir kuyruk oluşturma](#create-a-queue)
* [Kuyruğa ileti ekleme](#add-messages-to-a-queue)
* [Kuyruktaki iletilere göz atın](#peek-at-messages -in-a-queue)
* [Kuyruktaki bir iletiyi güncelleştirme](#update-a-message-in-a-queue)
* [Sıradaki iletileri alma ve silme](#receive-and-delete-messages-from-a-queue)
* [Bir kuyruk silme](#delete-a-queue)

### <a name="get-the-connection-string"></a>Bağlantı dizesini alma

Aşağıdaki kod depolama hesabının bağlantı dizesini alır. Bağlantı dizesi, [depolama bağlantı dizesi](#configure-your-storage-connection-string) bölümüne Yapılandırıldığında oluşturulan ortam değişkeni depolanır.

Bu kodu yöntemin `main` içine ekleyin:

```java
System.out.println("Azure Queues storage v12 - Java quickstart sample\n");

// Retrieve the connection string for use with the application. The storage
// connection string is stored in an environment variable on the machine
// running the application called AZURE_STORAGE_CONNECTION_STRING. If the environment variable
// is created after the application is launched in a console or with
// Visual Studio, the shell or application needs to be closed and reloaded
// to take the environment variable into account.
String connectStr = System.getenv("AZURE_STORAGE_CONNECTION_STRING");
```

### <a name="create-a-queue"></a>Bir kuyruk oluşturma

Yeni sıra için bir ad belirleyin. Aşağıdaki kod, benzersiz olduğundan emin olmak için sıra adına bir GUID değeri ekler.

> [!IMPORTANT]
> Sıra adları yalnızca küçük harfler, sayılar ve tireler içerebilir ve bir harf veya sayıyla başlamalıdır. Her kısa çizginin önünde ve arkasında kısa çizgi dışında bir karakter bulunmalıdır. Ad da 3 ve 63 karakter uzunluğunda olmalıdır. Sıraları adlandırma hakkında daha fazla bilgi için [Bkz.](/rest/api/storageservices/naming-queues-and-metadata)


[QueueClient](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-queue/12.0.0/com/azure/storage/queue/QueueClient.html) sınıfının bir örneğini oluşturun. Ardından, depolama hesabınızdaki sırayı oluşturmak için [oluşturma](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-queue/12.0.0/com/azure/storage/queue/QueueClient.html#create--) yöntemini arayın.

Bu kodu `main` yöntemin sonuna ekleyin:

```java
// Create a unique name for the queue
String queueName = "quickstartqueues-" + java.util.UUID.randomUUID();

System.out.println("Creating queue: " + queueName);

// Instantiate a QueueClient which will be
// used to create and manipulate the queue
QueueClient queueClient = new QueueClientBuilder()
                                .connectionString(connectStr)
                                .queueName(queueName)
                                .buildClient();

// Create the queue
queueClient.create();
```

### <a name="add-messages-to-a-queue"></a>Kuyruğa ileti ekleme

Aşağıdaki kod snippet [sendMessage](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-queue/12.0.0/com/azure/storage/queue/QueueClient.html#sendMessage-java.lang.String-) yöntemini çağırarak sıraya iletiekler. Ayrıca, `sendMessage` bir çağrıdan döndürülen bir [SendMessageResult'ı](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-queue/12.0.0/com/azure/storage/queue/models/SendMessageResult.html) da kaydeder. Sonuç, iletiyi daha sonra programda güncelleştirmek için kullanılır.

Bu kodu `main` yöntemin sonuna ekleyin:

```java
System.out.println("\nAdding messages to the queue...");

// Send several messages to the queue
queueClient.sendMessage("First message");
queueClient.sendMessage("Second message");

// Save the result so we can update this message later
SendMessageResult result = queueClient.sendMessage("Third message");
```

### <a name="peek-at-messages-in-a-queue"></a>Kuyruktaki iletilere göz atın

PeekMessages yöntemini arayarak kuyruktaki [iletilere göz atın.](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-queue/12.0.0/com/azure/storage/queue/QueueClient.html#peekMessages-java.lang.Integer-java.time.Duration-com.azure.core.util.Context-) Yöntem, `peelkMessages` sıranın önünden bir veya daha fazla ileti alır, ancak iletinin görünürlüğünü değiştirmez.

Bu kodu `main` yöntemin sonuna ekleyin:

```java
System.out.println("\nPeek at the messages in the queue...");

// Peek at messages in the queue
queueClient.peekMessages(10, null, null).forEach(
    peekedMessage -> System.out.println("Message: " + peekedMessage.getMessageText()));
```

### <a name="update-a-message-in-a-queue"></a>Kuyruktaki bir iletiyi güncelleştirme

UpdateMessage yöntemini çağırarak iletinin içeriğini [güncelleştirin.](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-queue/12.0.0/com/azure/storage/queue/QueueClient.html#updateMessage-java.lang.String-java.lang.String-java.lang.String-java.time.Duration-) Yöntem, `updateMessage` iletinin görünürlük zaman anına ve içeriğini değiştirebilir. İleti içeriği 64 KB boyutuna kadar olan UTF-8 kodlanmış bir dize olmalıdır. İleti için yeni içerikle birlikte, kodda daha önce `SendMessageResult` kaydedilen içeriği kullanarak ileti kimliğini ve pop makbuzu'nu iletin. İleti kimliği ve pop makbuzu hangi iletiyi güncelleştireceklerini tanımlar.

```java
System.out.println("\nUpdating the third message in the queue...");

// Update a message using the result that
// was saved when sending the message
queueClient.updateMessage(result.getMessageId(),
                          result.getPopReceipt(), 
                          "Third message has been updated",
                          Duration.ofSeconds(1));
```

### <a name="receive-and-delete-messages-from-a-queue"></a>Sıradaki iletileri alma ve silme

Daha önce eklenen iletileri [receiveMessages](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-queue/12.0.0/com/azure/storage/queue/QueueClient.html#receiveMessages-java.lang.Integer-java.time.Duration-java.time.Duration-com.azure.core.util.Context-) yöntemini arayarak indirin. Örnek kod, iletileri alındıktan ve işlendikten sonra da sıradan siler. Bu durumda, işleme yalnızca konsolda ileti yi görüntüler.

Uygulama, iletileri almadan önce `System.console().readLine();` arayarak kullanıcı girişi için duraklatAbilir ve siler. [Azure portalınızda](https://portal.azure.com) kaynakların silinmeden önce doğru oluşturulduğunu doğrulayın. Açıkça silinmeyen iletiler, bunları işlemek için başka bir şans için kuyrukta tekrar görünür hale gelir.

Bu kodu `main` yöntemin sonuna ekleyin:

```java
System.out.println("\nPress Enter key to receive messages and delete them from the queue...");
System.console().readLine();

// Get messages from the queue
queueClient.receiveMessages(10).forEach(
    // "Process" the message
    receivedMessage -> {
        System.out.println("Message: " + receivedMessage.getMessageText());

        // Let the service know we're finished with
        // the message and it can be safely deleted.
        queueClient.deleteMessage(receivedMessage.getMessageId(), receivedMessage.getPopReceipt());
    }
);
```

### <a name="delete-a-queue"></a>Bir kuyruk silme

Aşağıdaki kod, [sil](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-queue/12.0.0/com/azure/storage/queue/QueueClient.html#delete--) yöntemini kullanarak sırayı silerek oluşturulan kaynakları temizler.

Bu kodu `main` yöntemin sonuna ekleyin:

```java
System.out.println("\nPress Enter key to delete the queue...");
System.console().readLine();

// Clean up
System.out.println("Deleting queue: " + queueClient.getQueueName());
queueClient.delete();

System.out.println("Done");
```

## <a name="run-the-code"></a>Kodu çalıştırma

Bu uygulama, Azure kuyruğuna üç ileti oluşturur ve ekler. Kod, kuyruktaki iletileri listeler, ardından sırayı silmeden önce alır ve siler.

Konsol pencerenizde, uygulama dizininize gidin ve ardından uygulamayı oluşturun ve çalıştırın.

```console
mvn compile
```

Sonra, paketi oluşturun.

```console
mvn package
```

Uygulamayı çalıştırmak `mvn` için aşağıdaki komutu çalıştırın.

```console
mvn exec:java -Dexec.mainClass="com.queues.quickstart.App" -Dexec.cleanupDaemonThreads=false
```

Uygulamanın çıktısı aşağıdaki örneğe benzer:

```output
Azure Queues storage v12 - Java quickstart sample

Adding messages to the queue...

Peek at the messages in the queue...
Message: First message
Message: Second message
Message: Third message

Updating the third message in the queue...

Press Enter key to receive messages and delete them from the queue...

Message: First message
Message: Second message
Message: Third message has been updated

Press Enter key to delete the queue...

Deleting queue: quickstartqueues-fbf58f33-4d5a-41ac-ac0e-1a05d01c7003
Done
```

Uygulama ileti almadan önce durakladığında, [Azure portalındaki](https://portal.azure.com)depolama hesabınızı kontrol edin. İletilerin sırada olduğunu doğrulayın.

İletileri almak ve silmek için **Enter** tuşuna basın. İstendiğinde, sırayı silmek ve demoyu bitirmek için **Enter** tuşuna tekrar basın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Java kodunu kullanarak kuyruk oluşturmayı ve ona ileti eklemeyi öğrendiniz. Ardından iletileri gözetlemeyi, almayı ve silmeyi öğrendiniz. Son olarak, ileti kuyruğunun nasıl silinir öğrenilir.

Öğreticiler, örnekler, hızlı başlangıçlar ve diğer belgeler için şu adresi ziyaret edin:

> [!div class="nextstepaction"]
> [Java bulut geliştiricileri için Azure](https://docs.microsoft.com/azure/java/)

* Daha fazla Azure Kuyruğu depolama örneği uygulaması görmek için [Azure Queue depolama alanı SDK v12 Java istemci kitaplığı örneklerine](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-queue/src/samples/java/com/azure/storage/queue)devam edin.
