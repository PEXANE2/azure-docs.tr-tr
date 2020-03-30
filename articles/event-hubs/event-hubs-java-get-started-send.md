---
title: Java kullanarak Azure Etkinlik Hub'larından etkinlik gönderme veya alma (eski)
description: Bu makalede, eski azure olay hub'ları paketini kullanarak Azure Etkinlik Hub'larına/azure Etkinlik Hub'larına etkinlik gönderen/alan bir Java uygulaması oluşturma nın bir bölümü sağlanmaktadır.
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.workload: core
ms.topic: quickstart
ms.date: 02/11/2020
ms.author: spelluru
ms.openlocfilehash: 2c9baa4c0e048419ece09b954cee1af21b1f0cc1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77158018"
---
# <a name="use-java-to-send-events-to-or-receive-events-from-azure-event-hubs-azure-eventhubs"></a>Azure Etkinlik Hub'larına etkinlik göndermek veya etkinlikler almak için Java'yı kullanın (azure-eventhubs)

Bu hızlı başlangıç, **azure-eventhubs** Java paketini kullanarak bir etkinlik hub'ına olayları nasıl göndereceğinizi ve olay merkezinden nasıl alınarak alınabildiğini gösterir.

> [!WARNING]
> Bu hızlı başlangıç, eski **azure-eventhub'ları** ve **azure-eventhubs-eph** paketlerini kullanır. En son **azure-messaging-eventhubs** paketini kullanan hızlı bir başlangıç için, [azure-messaging-eventhubs kullanarak etkinlik gönder ve al'a](get-started-java-send-v2.md)bakın. Uygulamanızı eski paketi kullanmaktan yenisine taşımak [için azure-eventhub'lardan azure-mesajlaşma-eventhub'lara geçiş kılavuzuna](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/eventhubs/azure-messaging-eventhubs/migration-guide.md)bakın. 


## <a name="prerequisites"></a>Ön koşullar

Azure Etkinlik Hub'larında yeniyseniz, bu hızlı başlangıcı yapmadan önce [Etkinlik Hub'larına genel bakış](event-hubs-about.md) bakın. 

Bu hızlı başlangıcı tamamlamak için aşağıdaki ön koşullara ihtiyacınız vardır:

- **Microsoft Azure aboneliği.** Azure Etkinlik Hub'ları da dahil olmak üzere Azure hizmetlerini kullanmak için bir aboneliğe ihtiyacınız vardır.  Varolan bir Azure hesabınız yoksa, [ücretsiz](https://azure.microsoft.com/free/) deneme sürümüne kaydolabilir veya [bir hesap oluştururken](https://azure.microsoft.com)MSDN abone avantajlarınızı kullanabilirsiniz.
- Java geliştirme ortamı. Bu quickstart [Eclipse](https://www.eclipse.org/)kullanır.
- **Olay Hub'ları ad alanı ve olay hub'ı oluşturun.** İlk adım, Olay Hub türünden bir ad alanı oluşturmak ve uygulamanızın etkinlik merkeziyle iletişim kurmak için ihtiyaç duyduğu yönetim kimlik bilgilerini elde etmek için [Azure portalını](https://portal.azure.com) kullanmaktır. Ad alanı ve olay hub'ı oluşturmak için [bu makaledeki](event-hubs-create.md)yordamı izleyin. Ardından, makaledeki yönergeleri izleyerek olay hub'ı için erişim anahtarının değerini alın: [Bağlantı dizesini alın.](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) Bu hızlı başlatmada daha sonra yazdığınız koddaki erişim anahtarını kullanırsınız. Varsayılan anahtar adı: **RootManageSharedAccessKey**.

## <a name="send-events"></a>Olayları gönderme 
Bu bölümde, olaylara bir etkinlik hub'ı göndermek için java uygulamasının nasıl oluşturulacağı gösterilmektedir. 

> [!NOTE]
> Bu hızlı başlangıcı [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend)’dan örnek olarak indirebilir, `EventHubConnectionString` ve `EventHubName` dizelerini olay hub’ınızdaki değerlerle değiştirebilir ve çalıştırabilirsiniz. Alternatif olarak, kendi oluşturmak için bu quickstart adımları izleyebilirsiniz.

### <a name="add-reference-to-azure-event-hubs-library"></a>Azure Etkinlik Hub'ları kitaplığına başvuru ekleme

Etkinlik Hub'ları için Java istemci kitaplığı [Maven Merkez Deposu'ndan](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22)Maven projelerinde kullanılabilir. Maven proje dosyanızda aşağıdaki bağımlılık bildirimini kullanarak bu kitaplık başvuru yapabilirsiniz:

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>2.2.0</version>
</dependency>
```

Farklı yapı ortamları türleri için, [Maven Central Repository'den](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22)en son yayımlanan JAR dosyalarını açıkça edinebilirsiniz.  

Basit bir olay yayımcısı için, Olay Hub'ları istemci sınıfları için *com.microsoft.azure.eventhubs* paketini ve Azure Servis Veri Gönderi ileti istemcisi ile paylaşılan ortak özel durumlar gibi yardımcı program sınıfları için *com.microsoft.azure.servicebus* paketini aktarın. 

### <a name="write-code-to-send-messages-to-the-event-hub"></a>Olay hub'ına ileti göndermek için kod yazma

Aşağıdaki örnek için önce en sevdiğiniz Java geliştirme ortamında bir konsol/kabuk uygulaması için yeni bir Maven projesi oluşturun. Adlı `SimpleSend`bir sınıf ekleyin ve sınıfa aşağıdaki kodu ekleyin:

```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
import com.microsoft.azure.eventhubs.EventData;
import com.microsoft.azure.eventhubs.EventHubClient;
import com.microsoft.azure.eventhubs.EventHubException;

import java.io.IOException;
import java.nio.charset.Charset;
import java.time.Instant;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;

public class SimpleSend {

    public static void main(String[] args)
            throws EventHubException, ExecutionException, InterruptedException, IOException {
            
            
    }
 }
```

### <a name="construct-connection-string"></a>Bağlantı dizesi oluşturma

Olay Hub'ları istemci örneğine geçmek için bir bağlantı dize değeri oluşturmak için ConnectionStringBuilder sınıfını kullanın. Yer tutucuları ad alanı ve olay hub'ını oluşturduğunuzda elde ettiğiniz değerlerle değiştirin:

```java
        final ConnectionStringBuilder connStr = new ConnectionStringBuilder()
                .setNamespaceName("<EVENTHUB NAMESPACE") 
                .setEventHubName("EVENT HUB")
                .setSasKeyName("RootManageSharedAccessKey")
                .setSasKey("SHARED ACCESS KEY");
```

### <a name="write-code-to-send-events"></a>Olayları göndermek için kod yazma

Bir dizeyi UTF-8 bayt kodlamasına dönüştürerek tekil bir olay oluşturun. Ardından, bağlantı dizesinden yeni bir Olay Hub'ları istemci örneği oluşturun ve iletiyi gönderin:   

```java 
        final Gson gson = new GsonBuilder().create();

        // The Executor handles all asynchronous tasks and this is passed to the EventHubClient instance.
        // This enables the user to segregate their thread pool based on the work load.
        // This pool can then be shared across multiple EventHubClient instances.
        // The following sample uses a single thread executor, as there is only one EventHubClient instance,
        // handling different flavors of ingestion to Event Hubs here.
        final ScheduledExecutorService executorService = Executors.newScheduledThreadPool(4);

        // Each EventHubClient instance spins up a new TCP/SSL connection, which is expensive.
        // It is always a best practice to reuse these instances. The following sample shows this.
        final EventHubClient ehClient = EventHubClient.createSync(connStr.toString(), executorService);


        try {
            for (int i = 0; i < 10; i++) {

                String payload = "Message " + Integer.toString(i);
                byte[] payloadBytes = gson.toJson(payload).getBytes(Charset.defaultCharset());
                EventData sendEvent = EventData.create(payloadBytes);

                // Send - not tied to any partition
                // Event Hubs service will round-robin the events across all Event Hubs partitions.
                // This is the recommended & most reliable way to send to Event Hubs.
                ehClient.sendSync(sendEvent);
            }

            System.out.println(Instant.now() + ": Send Complete...");
            System.out.println("Press Enter to stop.");
            System.in.read();
        } finally {
            ehClient.closeSync();
            executorService.shutdown();
        }

``` 

Programı oluşturun ve çalıştırın ve hata olmadığından emin olun.

Tebrikler! Bir olay hub'ına ileti gönderdiniz.

### <a name="appendix-how-messages-are-routed-to-eventhub-partitions"></a>Ek: İletiler EventHub bölümlerine nasıl yönlendirilir?

İletiler tüketiciler tarafından alınmadan önce, öncelikle yayıncılar tarafından bölümlere yayımlanmalıdır. İletiler com.microsoft.azure.eventhubs.EventHubClient nesnesindeki sendSync() yöntemini kullanarak olay hub'ına eşzamanlı olarak yayımlandığında, ileti belirli bir bölüme gönderilebilir veya kullanılabilir tüm bölümlere round-robin bir şekilde dağıtılabilir bölüm anahtarının belirtilip belirtilmediğine bagIn.

Bölüm anahtarını temsil eden bir dize belirtildiğinde, olayı hangi bölüme göndereceğini belirlemek için anahtar işlenir.

Bölme anahtarı ayarlanmadığında, iletiler kullanılabilir tüm bölümlere yuvarlatılır

```java
// Serialize the event into bytes
byte[] payloadBytes = gson.toJson(messagePayload).getBytes(Charset.defaultCharset());

// Use the bytes to construct an {@link EventData} object
EventData sendEvent = EventData.create(payloadBytes);

// Transmits the event to event hub without a partition key
// If a partition key is not set, then we will round-robin to all topic partitions
eventHubClient.sendSync(sendEvent);

//  the partitionKey will be hash'ed to determine the partitionId to send the eventData to.
eventHubClient.sendSync(sendEvent, partitionKey);

// close the client at the end of your program
eventHubClient.closeSync();

```

## <a name="receive-events"></a>Olayları alma
Bu öğreticideki kod, tam çalışma uygulamasını görmek için incelediğiniz [GitHub'daki EventProcessorSample kodunu](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/EventProcessorSample)temel alınarak temel alabilirsiniz.

### <a name="receive-messages-with-eventprocessorhost-in-java"></a>Java’da EventProcessorHost bulunan iletiler alma

**EventProcessorHost,** bu Olay Hub'larından kalıcı denetim noktalarını ve paralel alımlarını yöneterek Olay Hub'larından olay alma kolaylaştırılabilen bir Java sınıfıdır. EventProcessorHost’u kullanarak, farklı düğümlerde barındırıldığında bile birden çok alıcı arasında olayları bölebilirsiniz. Bu örnek, tek alıcı için EventProcessorHost’un nasıl kullanıldığını göstermektedir.

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

EventProcessorHost'u kullanmak için bir [Azure Depolama hesabı][Azure Depolama hesabı] olması gerekir:

1. [Azure portalında](https://portal.azure.com)oturum açın ve ekranın sol tarafında **kaynak oluştur'u** seçin.
2. **Depolama'yı**seçin, ardından **Depolama hesabını**seçin. Depolama **hesabı oluştur** penceresinde, depolama hesabı için bir ad yazın. Alanların geri kalanını tamamlayın, istediğiniz bölgeyi seçin ve sonra **Oluştur'u**seçin.
   
    ![Azure portalında depolama hesabı oluşturma](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-azure-storage-account.png)

3. Yeni oluşturulan depolama hesabını seçin ve ardından **Erişim Tuşları'nı**seçin:
   
    ![Azure portalında erişim anahtarlarınızı alın](./media/event-hubs-dotnet-framework-getstarted-receive-eph/select-azure-storage-access-keys.png)

    Key1 değerini geçici bir konuma kopyalayın. Daha sonra bu öğreticide kullanacaksınız.

### <a name="create-a-java-project-using-the-eventprocessor-host"></a>EventProcessor Ana Bilgisayarını kullanarak Java projesi oluşturma

Olay Hub'ları için Java istemci kitaplığı Maven projelerinde [Kullanılabilir Maven Merkezi Deposu'ndan](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22)kullanılabilir ve Maven proje dosyanızda aşağıdaki bağımlılık bildirimi kullanılarak başvurulabilir: 

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>2.2.0</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>2.4.0</version>
</dependency>
```

Farklı yapı ortamları türleri için, [Maven Central Repository'den](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22)en son yayımlanan JAR dosyalarını açıkça edinebilirsiniz.

1. Aşağıdaki örnek için önce en sevdiğiniz Java geliştirme ortamında bir konsol/kabuk uygulaması için yeni bir Maven projesi oluşturun. Sınıfın adı `ErrorNotificationHandler`.     
   
    ```java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   
    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```
2. `EventProcessorSample` adlı yeni bir sınıf oluşturmak için aşağıdaki kodu kullanın. Yer tutucuları olay merkezi ve depolama hesabını oluşturduğunuzda kullanılan değerlerle değiştirin:
   
   ```java
   package com.microsoft.azure.eventhubs.samples.eventprocessorsample;

   import com.microsoft.azure.eventhubs.ConnectionStringBuilder;
   import com.microsoft.azure.eventhubs.EventData;
   import com.microsoft.azure.eventprocessorhost.CloseReason;
   import com.microsoft.azure.eventprocessorhost.EventProcessorHost;
   import com.microsoft.azure.eventprocessorhost.EventProcessorOptions;
   import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   import com.microsoft.azure.eventprocessorhost.IEventProcessor;
   import com.microsoft.azure.eventprocessorhost.PartitionContext;

   import java.util.concurrent.ExecutionException;
   import java.util.function.Consumer;

   public class EventProcessorSample
   {
       public static void main(String args[]) throws InterruptedException, ExecutionException
       {
           String consumerGroupName = "$Default";
           String namespaceName = "----NamespaceName----";
           String eventHubName = "----EventHubName----";
           String sasKeyName = "----SharedAccessSignatureKeyName----";
           String sasKey = "----SharedAccessSignatureKey----";
           String storageConnectionString = "----AzureStorageConnectionString----";
           String storageContainerName = "----StorageContainerName----";
           String hostNamePrefix = "----HostNamePrefix----";
        
           ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder()
                .setNamespaceName(namespaceName)
                .setEventHubName(eventHubName)
                .setSasKeyName(sasKeyName)
                .setSasKey(sasKey);
        
           EventProcessorHost host = new EventProcessorHost(
                EventProcessorHost.createHostName(hostNamePrefix),
                eventHubName,
                consumerGroupName,
                eventHubConnectionString.toString(),
                storageConnectionString,
                storageContainerName);
        
           System.out.println("Registering host named " + host.getHostName());
           EventProcessorOptions options = new EventProcessorOptions();
           options.setExceptionNotification(new ErrorNotificationHandler());

           host.registerEventProcessor(EventProcessor.class, options)
           .whenComplete((unused, e) ->
           {
               if (e != null)
               {
                   System.out.println("Failure while registering: " + e.toString());
                   if (e.getCause() != null)
                   {
                       System.out.println("Inner exception: " + e.getCause().toString());
                   }
               }
           })
           .thenAccept((unused) ->
           {
               System.out.println("Press enter to stop.");
               try 
               {
                   System.in.read();
               }
               catch (Exception e)
               {
                   System.out.println("Keyboard read failed: " + e.toString());
               }
           })
           .thenCompose((unused) ->
           {
               return host.unregisterEventProcessor();
           })
           .exceptionally((e) ->
           {
               System.out.println("Failure while unregistering: " + e.toString());
               if (e.getCause() != null)
               {
                   System.out.println("Inner exception: " + e.getCause().toString());
               }
               return null;
           })
           .get(); // Wait for everything to finish before exiting main!
        
           System.out.println("End of sample");
       }
    ```
3. Aşağıdaki kodu kullanarak `EventProcessor`adında bir sınıf daha oluşturun:
   
    ```java
    public static class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;

        // OnOpen is called when a new event processor instance is created by the host. 
        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }

        // OnClose is called when an event processor instance is being shut down. 
        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
        
        // onError is called when an error occurs in EventProcessorHost code that is tied to this partition, such as a receiver failure.
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }

        // onEvents is called when events are received on this partition of the Event Hub. 
        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> events) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got event batch");
            int eventCount = 0;
            for (EventData data : events)
            {
                try
                {
                    System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                            data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBytes(), "UTF8"));
                    eventCount++;
                    
                    // Checkpointing persists the current position in the event stream for this partition and means that the next
                    // time any host opens an event processor on this event hub+consumer group+partition combination, it will start
                    // receiving at the event after this one. 
                    this.checkpointBatchingCount++;
                    if ((checkpointBatchingCount % 5) == 0)
                    {
                        System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                            data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                        // Checkpoints are created asynchronously. It is important to wait for the result of checkpointing
                        // before exiting onEvents or before creating the next checkpoint, to detect errors and to ensure proper ordering.
                        context.checkpoint(data).get();
                    }
                }
                catch (Exception e)
                {
                    System.out.println("Processing failed for an event: " + e.toString());
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + eventCount + " for host " + context.getOwner());
        }
    }
    ```

Bu öğretici, EventProcessorHost’un tek bir örneğini kullanır. İş imasını artırmak için, tercihen ayrı makinelerde birden çok EventProcessorHost örneği çalıştırmanızı öneririz.  Fazlalık da sağlar. Böyle durumlarda, alınan olayların yük dengesi için çeşitli örnekler otomatik olarak birbirleriyle koordine olurlar. Birden çok alıcının her birinin *tüm* olayları işlemesini istiyorsanız **ConsumerGroup** kavramını kullanmalısınız. Olaylar farklı makinelerden alındığında, dağıtıldıkları makineleri (veya rolleri) temel alan EventProcessorHost örnekleri için ad belirtmek yararlı olabilir.

### <a name="publishing-messages-to-eventhub"></a>İletileri EventHub'a Yayımlama

İletiler tüketiciler tarafından alınmadan önce, öncelikle yayıncılar tarafından bölümlere yayımlanmalıdır. İletiler com.microsoft.azure.eventhubs.EventHubClient nesnesindeki sendSync() yöntemini kullanarak olay hub'ına eşzamanlı olarak yayımlandığında, iletinin belirli bir bölüme gönderilebildiğini veya kullanılabilir tüm bölümlere dağıtılabileceğini belirtmekte yarar vardır. bölüm anahtarının belirtilip belirtilmediğine bagısa bir yuvarlanmandı.

Bölüm anahtarını temsil eden bir dize belirtildiğinde, olayı hangi bölüme göndereceğini belirlemek için anahtar haşlanır.

Bölme anahtarı ayarlanmadığında, iletiler kullanılabilir tüm bölümlere yuvarlanır

```java
// Serialize the event into bytes
byte[] payloadBytes = gson.toJson(messagePayload).getBytes(Charset.defaultCharset());

// Use the bytes to construct an {@link EventData} object
EventData sendEvent = EventData.create(payloadBytes);

// Transmits the event to event hub without a partition key
// If a partition key is not set, then we will round-robin to all topic partitions
eventHubClient.sendSync(sendEvent);

//  the partitionKey will be hash'ed to determine the partitionId to send the eventData to.
eventHubClient.sendSync(sendEvent, partitionKey);

```

### <a name="implementing-a-custom-checkpointmanager-for-eventprocessorhost-eph"></a>EventProcessorHost (EPH) için Özel CheckpointManager uygulama

API, varsayılan uygulamanın kullanım servis talebinizle uyumlu olmadığı senaryolar için özel denetim noktası yöneticinizi uygulamak için bir mekanizma sağlar.

Varsayılan denetim noktası yöneticisi blob depolama kullanır, ancak kendi uygulamanızla EPH tarafından kullanılan denetim noktası yöneticisigeçersiz kılarsanız, denetim noktası yöneticisi uygulamanızı yedeklemek istediğiniz herhangi bir depoyu kullanabilirsiniz.

com.microsoft.azure.eventprocessorhost.ICheckpointManager arabirimini uygulayan bir sınıf oluşturun

Denetim noktası yöneticisinin özel uygulamanızı kullanın (com.microsoft.azure.eventprocessorhost.ICheckpointManager)

Uygulamanızda, varsayılan denetim mekanizmasını geçersiz kılabilir ve kendi veri deponuza (SQL Server, CosmosDB ve Redis için Azure Önbelleği gibi) dayalı olarak kendi denetim noktalarımızı uygulayabilirsiniz. Denetim noktası yöneticisi uygulamanızı destekleyen mağazanın, tüketici grubu için olayları işleyen tüm EPH örneklerine erişebiliyor olmasını öneririz.

Ortamınızda bulunan herhangi bir veri deposunı kullanabilirsiniz.

com.microsoft.azure.eventprocessorhost.EventProcessorHost sınıfı, EventProcessorHost'unuzun denetim noktası yöneticisini geçersiz kılmanızı sağlayan iki oluşturucu sağlar.


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleleri okuyun: 

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Azure Event Hubs'ın özellikleri ve terminolojisi](event-hubs-features.md)
- [Event Hubs ile ilgili SSS](event-hubs-faq.md)

