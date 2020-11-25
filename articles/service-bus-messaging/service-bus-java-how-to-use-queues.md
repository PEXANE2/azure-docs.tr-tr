---
title: Java ile Azure Service Bus kuyruklarını kullanma (Azure-mesajlaşma-ServiceBus)
description: Bu öğreticide, Java kullanarak Azure Service Bus kuyruğundan ileti gönderme ve iletileri alma hakkında bilgi edineceksiniz. Yeni Azure-mesajlaşma-ServiceBus paketini kullanın.
ms.devlang: Java
ms.topic: quickstart
ms.date: 11/09/2020
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019, devx-track-java
ms.openlocfilehash: a91ed2a358a9595a4d22dd629b8d470423b786d6
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95909535"
---
# <a name="send-messages-to-and-receive-messages-from-azure-service-bus-queues-java"></a>Azure Service Bus kuyruklarından ileti gönderme ve iletileri alma (Java)
Bu hızlı başlangıçta, bir Azure Service Bus sırasından ileti göndermek ve ileti almak için bir Java uygulaması oluşturacaksınız. 

> [!IMPORTANT]
> Bu hızlı başlangıç, **önizlemede** olan yeni Azure-Messaging-ServiceBus paketini kullanır. Geçerli genel kullanıma açık (GA) Azure-ServiceBus paketini kullanan bir hızlı başlangıç için bkz. [Azure-ServiceBus kullanarak Ileti gönderme ve alma](service-bus-java-how-to-use-queues-legacy.md).

## <a name="prerequisites"></a>Önkoşullar
- Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)için kaydolabilirsiniz.
- Birlikte çalışmak için bir kuyruğunuz yoksa, bir kuyruk oluşturmak için [Service Bus kuyruğu oluşturmak üzere Azure Portal kullanma](service-bus-quickstart-portal.md) adımlarını izleyin. Service Bus ad alanınız ve oluşturduğunuz **kuyruğun** adı için **bağlantı dizesini** aklınızda edin.
- [Java Için Azure SDK][Azure SDK for Java]'yı yükler. Çakışan Küreler kullanıyorsanız, Java için Azure SDK 'sını içeren [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse] yükleyebilirsiniz. Ardından, projenize **Java için Microsoft Azure kitaplıklarını** ekleyebilirsiniz. IntelliJ kullanıyorsanız, bkz. [Azure Toolkit for IntelliJ yüklemesi](/azure/developer/java/toolkit-for-intellij/installation). 


## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Bu bölümde, bir Java konsol projesi oluşturacak ve daha önce oluşturduğunuz kuyruğa ileti göndermek için kod ekleyeceksiniz. 

### <a name="create-a-java-console-project"></a>Java konsol projesi oluşturma
Çakışan Küreler veya tercih ettiğiniz bir aracı kullanarak bir Java projesi oluşturun. 

### <a name="configure-your-application-to-use-service-bus"></a>Uygulamanızı kullanmak için yapılandırma Service Bus
Azure Service Bus kitaplığına bir başvuru ekleyin. Service Bus için Java istemci kitaplığı, [Maven merkezi deposunda](https://search.maven.org/search?q=a:azure-messaging-servicebus)bulunabilir. Bu kitaplığa, Maven proje dosyanızda aşağıdaki bağımlılık bildirimini kullanarak başvurabilirsiniz:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-messaging-servicebus</artifactId>
    <version>7.0.0-beta.7</version>
</dependency>
```

### <a name="add-code-to-send-messages-to-the-queue"></a>Kuyruğa ileti göndermek için kod ekleme
1. `import`Java dosyasının konusunun aşağıdaki deyimlerini ekleyin. 

    ```java
    import com.azure.messaging.servicebus.*;
    import com.azure.messaging.servicebus.models.*;
    import java.util.concurrent.TimeUnit;
    import java.util.function.Consumer;
    import java.util.Arrays;
    import java.util.List;
    ```    
5. Sınıfında aşağıda gösterildiği gibi bağlantı dizesini ve sıra adını tutacak değişkenleri tanımlayın: 

    ```java
    static String connectionString = "<NAMESPACE CONNECTION STRING>";
    static String queueName = "<QUEUE NAME>";    
    ```

    `<NAMESPACE CONNECTION STRING>`Service Bus ad alanınız için bağlantı dizesiyle değiştirin. Ve öğesini `<QUEUE NAME>` kuyruğun adıyla değiştirin.
3. `sendMessage`Kuyruğa bir ileti göndermek için sınıfında adlı bir yöntem ekleyin. 

    ```java
    static void sendMessage()
    {
        // create a Service Bus Sender client for the queue 
        ServiceBusSenderClient senderClient = new ServiceBusClientBuilder()
                .connectionString(connectionString)
                .sender()
                .queueName(queueName)
                .buildClient();
        
        // send one message to the queue
        senderClient.sendMessage(new ServiceBusMessage("Hello, World!"));
        System.out.println("Sent a single message to the queue: " + queueName);        
    }
    ```
1. `createMessages`İleti listesi oluşturmak için sınıfında adlı bir yöntem ekleyin. Genellikle, bu iletileri uygulamanızın farklı bölümlerinden alırsınız. Burada, örnek mesajların bir listesini oluşturacağız.

    ```java
    static List<ServiceBusMessage> createMessages()
    {
        // create a list of messages and return it to the caller
        ServiceBusMessage[] messages = {
                new ServiceBusMessage("First message"),
                new ServiceBusMessage("Second message"),
                new ServiceBusMessage("Third message")
        };
        return Arrays.asList(messages);
    }
    ```
1. `sendMessageBatch`Oluşturduğunuz sıraya ileti göndermek için yöntemi adlı bir yöntem ekleyin. Bu yöntem `ServiceBusSenderClient` kuyruk için bir oluşturur, `createMessages` ileti listesini almak için yöntemini çağırır, bir veya daha fazla toplu işi hazırlar ve toplu işleri kuyruğa gönderir. 

```java
    static void sendMessageBatch()
    {
        // create a Service Bus Sender client for the queue 
        ServiceBusSenderClient senderClient = new ServiceBusClientBuilder()
                .connectionString(connectionString)
                .sender()
                .queueName(queueName)
                .buildClient();

        // Creates an ServiceBusMessageBatch where the ServiceBus.
        ServiceBusMessageBatch messageBatch = senderClient.createMessageBatch();        
        
        // create a list of messages
        List<ServiceBusMessage> listOfMessages = createMessages();
        
        // We try to add as many messages as a batch can fit based on the maximum size and send to Service Bus when
        // the batch can hold no more messages. Create a new batch for next set of messages and repeat until all
        // messages are sent.        
        for (ServiceBusMessage message : listOfMessages) {
            if (messageBatch.tryAddMessage(message)) {
                continue;
            }

            // The batch is full, so we create a new batch and send the batch.
            senderClient.sendMessages(messageBatch);
            System.out.println("Sent a batch of messages to the queue: " + queueName);
            
            // create a new batch
            messageBatch = senderClient.createMessageBatch();

            // Add that message that we couldn't before.
            if (!messageBatch.tryAddMessage(message)) {
                System.err.printf("Message is too large for an empty batch. Skipping. Max size: %s.", messageBatch.getMaxSizeInBytes());
            }
        }
        
        if (messageBatch.getCount() > 0) {
            senderClient.sendMessages(messageBatch);
            System.out.println("Sent a batch of messages to the queue: " + queueName);
        }

        //close the client
        senderClient.close();
    }
```

## <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma
Bu bölümde, kuyruktan ileti almak için kod ekleyeceksiniz. 

1. `receiveMessages`Sıradan ileti almak için adlı bir yöntem ekleyin. Bu yöntem `ServiceBusProcessorClient` , iletileri işlemek için bir işleyici ve hataları işlemek için başka bir tane belirterek kuyruk için bir oluşturur. Ardından, işlemciyi başlatır, birkaç saniye bekler, alınan iletileri yazdırır ve sonra işlemciyi durdurup kapatır.

    ```java
    // handles received messages
    static void receiveMessages() throws InterruptedException
    {
        // consumer that processes a single message received from Service Bus
        Consumer<ServiceBusReceivedMessageContext> messageProcessor = context -> {
            ServiceBusReceivedMessage message = context.getMessage();
            System.out.println("Received message: " + message.getBody().toString());
        };

        // handles any errors that occur when receiving messages
        Consumer<Throwable> errorHandler = throwable -> {
            System.out.println("Error when receiving messages: " + throwable.getMessage());
            if (throwable instanceof ServiceBusReceiverException) {
                ServiceBusReceiverException serviceBusReceiverException = (ServiceBusReceiverException) throwable;
                System.out.println("Error source: " + serviceBusReceiverException.getErrorSource());
            }
        };

        // create an instance of the processor through the ServiceBusClientBuilder
        ServiceBusProcessorClient processorClient = new ServiceBusClientBuilder()
            .connectionString(connectionString)
            .processor()
            .queueName(queueName)
            .processMessage(messageProcessor)
            .processError(errorHandler)
            .buildProcessorClient();

        System.out.println("Starting the processor");
        processorClient.start();

        TimeUnit.SECONDS.sleep(10);
        System.out.println("Stopping and closing the processor");
        processorClient.close();        
    }    
    ```
2. `main` `sendMessage` , `sendMessageBatch` , Ve `receiveMessages` yöntemlerini çağırmak ve throw metodunu güncelleştirin `InterruptedException` .     

    ```java
    public static void main(String[] args) throws InterruptedException {        
        sendMessage();
        sendMessageBatch();
        receiveMessages();
    }   
    ```

## <a name="run-the-app"></a>Uygulamayı çalıştırma
Uygulamayı çalıştırdığınızda, konsol penceresinde aşağıdaki iletileri görürsünüz. 

```console
Sent a single message to the queue: myqueue
Sent a batch of messages to the queue: myqueue
Starting the processor
Received message: Hello, World!
Received message: First message in the batch
Received message: Second message in the batch
Received message: Three message in the batch
Stopping and closing the processor
```

Azure portal Service Bus ad alanının **genel bakış** sayfasında **gelen** ve **giden** ileti sayısını görebilirsiniz. Bir dakika beklemeniz gerekebilir ve en son değerleri görmek için sayfayı yenilemeniz gerekebilir. 

:::image type="content" source="./media/service-bus-java-how-to-use-queues/overview-incoming-outgoing-messages.png" alt-text="Gelen ve giden ileti sayısı" lightbox="./media/service-bus-java-how-to-use-queues/overview-incoming-outgoing-messages.png":::

**Service Bus kuyruğu** sayfasına gitmek Için bu **genel bakış** sayfasında kuyruğu seçin. Bu sayfada **gelen** ve **giden** ileti sayısını da görürsünüz. Ayrıca, sıranın **geçerli boyutu** , **en büyük boyut**, **etkin ileti sayısı** vb. gibi diğer bilgileri de görürsünüz. 

:::image type="content" source="./media/service-bus-java-how-to-use-queues/queue-details.png" alt-text="Kuyruk ayrıntıları" lightbox="./media/service-bus-java-how-to-use-queues/queue-details.png":::



## <a name="next-steps"></a>Sonraki Adımlar
Aşağıdaki belgelere ve örneklere bakın:

- [Java için Azure Service Bus istemci kitaplığı-Benioku](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/servicebus/azure-messaging-servicebus/README.md)
- [GitHub’daki örnekler](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/servicebus/azure-messaging-servicebus/src/samples)
- [Java API'si başvurusu](https://docs.microsoft.com/java/api/overview/azure/servicebus?view=azure-java-preview&preserve-view=true)

[GitHub 'da daha fazla örnek](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/servicebus/azure-messaging-servicebus)görüntüleyin. 

[Azure SDK for Java]: /azure/developer/java/sdk/java-sdk-azure-get-started
[Azure Toolkit for Eclipse]: /azure/developer/java/toolkit-for-eclipse/installation
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
