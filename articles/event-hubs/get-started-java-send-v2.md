---
title: Java kullanarak Azure Event Hubs olay gönderme veya alma (en son)
description: Bu makalede, Azure Event Hubs en son Azure-Messaging-eventhubs paketini kullanarak olayları gönderen/alan bir Java uygulaması oluşturmaya yönelik izlenecek yol sunulmaktadır.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 01/15/2020
ms.author: spelluru
ms.openlocfilehash: 2e406cfd84642056bcc97190a3100f7e05d3c828
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77137942"
---
# <a name="use-java-to-send-events-to-or-receive-events-from-azure-event-hubs-azure-messaging-eventhubs"></a>Azure Event Hubs (Azure-Messaging-eventhubs) olay göndermek veya olayları almak için Java 'Yı kullanma
Bu hızlı başlangıçta, Azure Event Hubs olaylar göndermek veya almak için Java uygulamalarının nasıl oluşturulacağı gösterilmektedir. 

Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Event Hubs’a ayrıntılı bir genel bakış için bkz. [Event Hubs'a genel bakış](event-hubs-about.md) ve [Event Hubs özellikleri](event-hubs-features.md).

> [!IMPORTANT]
> Bu hızlı başlangıç, yeni **Azure-Messaging-eventhubs** paketini kullanır. Eski **Azure-eventhubs** ve **Azure-eventhubs-EPH** paketlerini kullanan bir hızlı başlangıç için, [Bu makaleye](event-hubs-java-get-started-send.md)bakın. 


## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Etkin bir Azure hesabı. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- Java geliştirme ortamı. Bu öğretici, [tutulma](https://www.eclipse.org/)kullanır. Java Development Kit (JDK) sürümü 8 veya üzeri gereklidir. 
- **Event Hubs bir ad alanı ve bir olay hub 'ı oluşturun**. İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve Olay Hub 'ı oluşturmak için [Bu makaledeki](event-hubs-create.md)yordamı izleyin. Ardından, makaledeki yönergeleri izleyerek Olay Hub 'ı için erişim anahtarı değerini alın: [bağlantı dizesi al](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Bu öğreticinin ilerleyen bölümlerinde yazdığınız kod erişim anahtarını kullanın. Varsayılan anahtar adı: **RootManageSharedAccessKey**.

## <a name="send-events"></a>Olayları gönderme 
Bu bölümde, Olay Hub 'ına olay göndermek için bir Java uygulaması oluşturma gösterilmektedir. 

### <a name="add-reference-to-azure-event-hubs-library"></a>Azure Event Hubs kitaplığına bir başvuru ekleyin

Event Hubs için Java istemci kitaplığı, [Maven merkezi deposundaki](https://search.maven.org/search?q=a:azure-messaging-eventhubs)Maven projelerinde kullanıma sunulmuştur. Bu kitaplığa, Maven proje dosyanızda aşağıdaki bağımlılık bildirimini kullanarak başvurabilirsiniz:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-messaging-eventhubs</artifactId>
    <version>5.0.1</version>
</dependency>
```

### <a name="write-code-to-send-messages-to-the-event-hub"></a>Olay hub'ına ileti göndermek için kod yazma

Aşağıdaki örnek için önce en sevdiğiniz Java geliştirme ortamında bir konsol/kabuk uygulaması için yeni bir Maven projesi oluşturun. `SimpleSend`adlı bir sınıf ekleyin ve aşağıdaki kodu sınıfına ekleyin:

```java
import com.azure.messaging.eventhubs.*;
import static java.nio.charset.StandardCharsets.UTF_8;

public class Sender {
    public static void main(String[] args) {
    }
}
```

### <a name="connection-string-and-event-hub"></a>Bağlantı dizesi ve Olay Hub 'ı
Bu kod, Event Hubs istemci derlemek için Event Hubs ad alanına ve Olay Hub 'ının adına bağlantı dizesini kullanır. 

```java
String connectionString = "<CONNECTION STRING to EVENT HUBS NAMESPACE>";
String eventHubName = "<EVENT HUB NAME>";
```

### <a name="create-an-event-hubs-producer-client"></a>Event Hubs üreticisi istemcisi oluşturma 
Bu kod, Olay Hub 'ına olay oluşturmak/göndermek için kullanılan bir üretici istemci nesnesi oluşturur. 

```java
EventHubProducerClient producer = new EventHubClientBuilder()
    .connectionString(connectionString, eventHubName)
    .buildProducerClient();
```

### <a name="prepare-a-batch-of-events"></a>Olay toplu işlemini hazırlama
Bu kod, olay toplu işlemini hazırlar. 

```java
EventDataBatch batch = producer.createBatch();
batch.tryAdd(new EventData("First event"));
batch.tryAdd(new EventData("Second event"));
batch.tryAdd(new EventData("Third event"));
batch.tryAdd(new EventData("Fourth event"));
batch.tryAdd(new EventData("Fifth event"));
```

### <a name="send-the-batch-of-events-to-the-event-hub"></a>Olay toplu işlemini Olay Hub 'ına gönderme
Bu kod, önceki adımda hazırladığınız olay toplu işlemini Olay Hub 'ına gönderir. Gönderme işleminde aşağıdaki kod blokları vardır. 

```java
producer.send(batch);
```

### <a name="close-and-cleanup"></a>Kapat ve temizle
Bu kod, üreticiyi kapatır. 

```java
producer.close();
```
### <a name="complete-code-to-send-events"></a>Olayları göndermek için kodu doldurun
Olayları Olay Hub 'ına göndermek için kodun tamamı aşağıda verilmiştir. 

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
Bu öğreticideki kod, [GitHub 'Daki Eventprocessorclient örneğine](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs/EventProcessorClientSample.java)dayalıdır ve bu, tam çalışma uygulamasını görmek için inceleyebilirsiniz.

### <a name="create-a-java-project"></a>Java projesi oluşturma

Event Hubs için Java istemci kitaplığı, Maven [Merkezi deposundaki](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22)Maven projelerinde kullanılabilir ve Maven proje dosyanızda aşağıdaki bağımlılık bildirimi kullanılarak başvurulabilirler: 

```xml
<dependencies>
    <dependency>
        <groupId>com.azure</groupId>
        <artifactId>azure-messaging-eventhubs</artifactId>
        <version>5.0.1</version>
    </dependency>
</dependencies>
```

1. `Receiver` adlı yeni bir sınıf oluşturmak için aşağıdaki kodu kullanın. Yer tutucuları olay hub'ı ve depolama hesabı oluştururken kullandığınız değerlerle değiştirin:
   
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
    
2. **Inmemorycheckpointstore. Java** dosyasını [GitHub](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs)'dan indirin ve projenize ekleyin. 
3. Programı oluşturun ve hata olmadığından emin olun. 

## <a name="run-the-applications"></a>Uygulamaları çalıştırma
1. Önce **alıcı** uygulamasını çalıştırın.
1. Ardından, **Gönderen** uygulamasını çalıştırın. 
1. **Alıcı** uygulaması penceresinde, gönderen uygulama tarafından yayınlanan olayları görtığınızdan emin olun.
1. Uygulamayı durdurmak için alıcı uygulaması penceresinde **ENTER** tuşuna basın. 

## <a name="next-steps"></a>Sonraki adımlar
[GitHub 'Da Java SDK örneklerine](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/eventhubs/azure-messaging-eventhubs/src/samples/java/com/azure/messaging/eventhubs) göz atın

