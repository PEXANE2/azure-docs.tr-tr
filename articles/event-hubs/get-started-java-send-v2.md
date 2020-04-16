---
title: Java kullanarak Azure Etkinlik Hub'larından etkinlik gönderme veya alma (en son)
description: Bu makale, en son azure ileti-eventhubs paketini kullanarak Azure Etkinlik Hub'larına/azure Etkinlik Hub'larına etkinlik gönderen/alan bir Java uygulaması oluşturma nın bir walkthrough'u sağlar.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 02/11/2020
ms.author: spelluru
ms.openlocfilehash: 824244c0c3247e5a218c1551dd95de6e1d6e1007
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419230"
---
# <a name="use-java-to-send-events-to-or-receive-events-from-azure-event-hubs-azure-messaging-eventhubs"></a>Azure Etkinlik Hub'larına etkinlik göndermek veya bu etkinliklerden (azure-mesajlaşma-eventhub'lar) almak için Java'yı kullanın
Bu hızlı başlangıç, **azure-messaging-eventhubs** Java paketini kullanarak bir etkinlik hub'ına olayları nasıl göndereceğinizi ve bir olay merkezinden nasıl alınarak alınabildiğini gösterir.

> [!IMPORTANT]
> Bu hızlı başlatma, yeni **azure-messaging-eventhubs paketini** kullanır. Eski **azure-eventhub'ları** ve **azure-eventhubs-eph** paketlerini kullanan hızlı bir başlangıç için, [azure-eventhubs ve azure-eventhubs-eph kullanarak etkinlik gönder ve alma](event-hubs-java-get-started-send.md)konusuna bakın. 


## <a name="prerequisites"></a>Ön koşullar
Azure Etkinlik Hub'larında yeniyseniz, bu hızlı başlangıcı yapmadan önce [Etkinlik Hub'larına genel bakış](event-hubs-about.md) konusubakın. 

Bu hızlı başlangıcı tamamlamak için aşağıdaki ön koşullara ihtiyacınız vardır:

- **Microsoft Azure aboneliği.** Azure Etkinlik Hub'ları da dahil olmak üzere Azure hizmetlerini kullanmak için bir aboneliğe ihtiyacınız vardır.  Varolan bir Azure hesabınız yoksa, [ücretsiz](https://azure.microsoft.com/free/) deneme sürümüne kaydolabilir veya [bir hesap oluştururken](https://azure.microsoft.com)MSDN abone avantajlarınızı kullanabilirsiniz.
- Java geliştirme ortamı. Bu quickstart [Eclipse](https://www.eclipse.org/)kullanır. Java Geliştirme Kiti (JDK) sürüm 8 veya üzeri gereklidir. 
- **Olay Hub'ları ad alanı ve olay hub'ı oluşturun.** İlk adım, Olay Hub türünden bir ad alanı oluşturmak ve uygulamanızın etkinlik merkeziyle iletişim kurmak için ihtiyaç duyduğu yönetim kimlik bilgilerini elde etmek için [Azure portalını](https://portal.azure.com) kullanmaktır. Ad alanı ve olay hub'ı oluşturmak için [bu makaledeki](event-hubs-create.md)yordamı izleyin. Ardından, makaledeki yönergeleri izleyerek **Olay Hub'ları ad alanının bağlantı dizesini** alın: [Bağlantı dizesini alın.](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) Bağlantı dizesini daha sonra bu hızlı başlatmada kullanırsınız.

## <a name="send-events"></a>Olayları gönderme 
Bu bölümde, olaylara bir etkinlik hub'ı göndermek için java uygulamasının nasıl oluşturulacağı gösterilmektedir. 

### <a name="add-reference-to-azure-event-hubs-library"></a>Azure Etkinlik Hub'ları kitaplığına başvuru ekleme

Etkinlik Hub'ları için Java istemci kitaplığı [Maven Merkezi Deposu'nda](https://search.maven.org/search?q=a:azure-messaging-eventhubs)kullanılabilir. Maven proje dosyanızda aşağıdaki bağımlılık bildirimini kullanarak bu kitaplık başvuru yapabilirsiniz:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-messaging-eventhubs</artifactId>
    <version>5.0.1</version>
</dependency>
```

### <a name="write-code-to-send-messages-to-the-event-hub"></a>Olay hub'ına ileti göndermek için kod yazma

Aşağıdaki örnek için önce en sevdiğiniz Java geliştirme ortamında bir konsol/kabuk uygulaması için yeni bir Maven projesi oluşturun. Adlı `Sender`bir sınıf ekleyin ve sınıfa aşağıdaki kodu ekleyin:

```java
import com.azure.messaging.eventhubs.*;
import static java.nio.charset.StandardCharsets.UTF_8;

public class Sender {
       public static void main(String[] args) {
    }
}
```

### <a name="connection-string-and-event-hub"></a>Bağlantı dizesi ve olay hub'ı
Bu kod, Olay Hub'ları istemcisi oluşturmak için Olay Hub'ları ad alanına bağlantı dizesini ve olay merkezinin adını kullanır. 

```java
String connectionString = "<CONNECTION STRING to EVENT HUBS NAMESPACE>";
String eventHubName = "<EVENT HUB NAME>";
```

### <a name="create-an-event-hubs-producer-client"></a>Olay Hub'ları Üretici istemcisi oluşturma 
Bu kod, olay hub'ına olay oluşturmak/göndermek için kullanılan bir üretici istemci nesnesi oluşturur. 

```java
EventHubProducerClient producer = new EventHubClientBuilder()
    .connectionString(connectionString, eventHubName)
    .buildProducerClient();
```

### <a name="prepare-a-batch-of-events"></a>Bir dizi etkinlik hazırlama
Bu kod bir dizi olay hazırlar. 

```java
EventDataBatch batch = producer.createBatch();
batch.tryAdd(new EventData("First event"));
batch.tryAdd(new EventData("Second event"));
batch.tryAdd(new EventData("Third event"));
batch.tryAdd(new EventData("Fourth event"));
batch.tryAdd(new EventData("Fifth event"));
```

### <a name="send-the-batch-of-events-to-the-event-hub"></a>Etkinlik toplu larını etkinlik merkezine gönderme
Bu kod, önceki adımda hazırladığınız olaylar toplu işlerini olay merkezine gönderir. Gönderme işleminde aşağıdaki kod blokları. 

```java
producer.send(batch);
```

### <a name="close-and-cleanup"></a>Yakın ve temizleme
Bu kod yapımcıyı kapatır. 

```java
producer.close();
```
### <a name="complete-code-to-send-events"></a>Olayları göndermek için kodu tamamlayın
Olay merkezine olay göndermek için tam kod aşağıda veda edilmiştir. 

```java
import com.azure.messaging.eventhubs.*;

public class Sender {
    public static void main(String[] args) {
        final String connectionString = "EVENT HUBS NAMESPACE CONNECTION STRING";
        final String eventHubName = "EVENT HUB NAME";

        // create a producer using the namespace connection string and event hub name
        EventHubProducerClient producer = new EventHubClientBuilder()
            .connectionString(connectionString, eventHubName)
            .buildProducerClient();

        // prepare a batch of events to send to the event hub    
        EventDataBatch batch = producer.createBatch();
        batch.tryAdd(new EventData("First event"));
        batch.tryAdd(new EventData("Second event"));
        batch.tryAdd(new EventData("Third event"));
        batch.tryAdd(new EventData("Fourth event"));
        batch.tryAdd(new EventData("Fifth event"));

        // send the batch of events to the event hub
        producer.send(batch);

        // close the producer
        producer.close();
    }
}
```

Programı oluşturun ve hata olmadığından emin olun. Alıcı programını çalıştırdıktan sonra bu programı çalıştıracaksınız. 

## <a name="receive-events"></a>Olayları alma
Bu öğreticideki kod, tam çalışma uygulamasını görmek için incelediğiniz [GitHub'daki EventProcessorClient örneğine](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs/EventProcessorClientSample.java)dayanır.

> [!NOTE]
> Azure Stack Hub'da çalışıyorsanız, bu platform Depolama Blob SDK'nın Azure'da bulunanlardan farklı bir sürümünü destekleyebilir. Örneğin, Azure Yığını [Hub sürümü 2002'de](https://docs.microsoft.com/azure-stack/user/event-hubs-overview)çalışıyorsanız, Depolama hizmeti için kullanılabilir en yüksek sürüm 2017-11-09 sürümüdür. Bu durumda, bu bölümdeki aşağıdaki adımların yanı sıra, Depolama hizmeti API sürümünü 2017-11-09'u hedeflemek için kod eklemeniz gerekir. Belirli bir Depolama API sürümünün nasıl hedeflenilene ilişkin bir örnek için, [bu örneğe GitHub'da](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs-checkpointstore-blob/src/samples/java/com/azure/messaging/eventhubs/checkpointstore/blob/EventProcessorWithCustomStorageVersion.java)bakın. Azure Yığını Hub'ında desteklenen Azure Depolama hizmeti sürümleri hakkında daha fazla bilgi için lütfen [Azure Yığın Hub depolama alanına bakın: Farklılıklar ve dikkat edilmesi gerekenler.](https://docs.microsoft.com/azure-stack/user/azure-stack-acs-differences)

### <a name="create-a-java-project"></a>Java projesi oluşturma

Olay Hub'ları için Java istemci kitaplığı Maven projelerinde [Kullanılabilir Maven Merkezi Deposu'ndan](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22)kullanılabilir ve Maven proje dosyanızda aşağıdaki bağımlılık bildirimi kullanılarak başvurulabilir: 

```xml
<dependencies>
    <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-messaging-eventhubs</artifactId>
        <version>5.0.1</version>
    </dependency>
</dependencies>
```

1. `Receiver` adlı yeni bir sınıf oluşturmak için aşağıdaki kodu kullanın. Yer tutucuları olay merkezi ve depolama hesabını oluşturduğunuzda kullanılan değerlerle değiştirin:
   
   ```java
     import com.azure.messaging.eventhubs.*;
     import com.azure.messaging.eventhubs.models.ErrorContext;
     import com.azure.messaging.eventhubs.models.EventContext;
     import java.util.concurrent.TimeUnit;
     import java.util.function.Consumer;
    
     public class Receiver {
    
         final static String connectionString = "<EVENT HUBS NAMESPACE - CONNECTION STRING>";
         final static String eventHubName = "<EVENT HUB NAME>";
         
         public static void main(String[] args) throws Exception {
    
             // function to process events
             Consumer<EventContext> processEvent = eventContext  -> {
                 System.out.print("Received event: ");
                 // print the body of the event
                 System.out.println(eventContext.getEventData().getBodyAsString());
                 eventContext.updateCheckpoint();
             };
    
             // function to process errors
             Consumer<ErrorContext> processError = errorContext -> {
                 // print the error message
                 System.out.println(errorContext.getThrowable().getMessage());
             };
    
            
             EventProcessorClient eventProcessorClient = new EventProcessorClientBuilder()
                     .connectionString(connectionString, eventHubName)
                     .processEvent(processEvent)
                     .processError(processError)
                     .consumerGroup(EventHubClientBuilder.DEFAULT_CONSUMER_GROUP_NAME)
                     .checkpointStore(new InMemoryCheckpointStore())
                     .buildEventProcessorClient();
    
             System.out.println("Starting event processor");
             eventProcessorClient.start();
    
             System.out.println("Press enter to stop.");
             System.in.read();
    
             System.out.println("Stopping event processor");
             eventProcessorClient.stop();
             System.out.println("Event processor stopped.");
    
             System.out.println("Exiting process");
         }
     }
    ```
    
2. **InMemoryCheckpointStore.java** dosyasını [GitHub'dan](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs/InMemoryCheckpointStore.java)indirin ve projenize ekleyin. 
3. Programı oluşturun ve hata olmadığından emin olun. 

## <a name="run-the-applications"></a>Uygulamaları çalıştırma
1. Önce **alıcı** uygulamasını çalıştırın.
1. Ardından, gönderen uygulamasını **çalıştırın.** 
1. **Alıcı** uygulama penceresinde, gönderen uygulama tarafından yayınlanan olayları gördüğünüzü onaylayın.
1. Uygulamayı durdurmak için alıcı uygulama penceresinde **ENTER** tuşuna basın. 

## <a name="next-steps"></a>Sonraki adımlar
[GitHub'daki Java SDK örneklerine](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs) göz atın

