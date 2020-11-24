---
title: Java ile Azure Service Bus konuları ve abonelikleri kullanma (Azure-mesajlaşma-ServiceBus)
description: Bu hızlı başlangıçta, bir Azure Service Bus konuya ileti göndermek ve sonra bu konuya yönelik aboneliklerden ileti almak için Azure-Messaging-ServiceBus paketini kullanarak Java kodu yazın.
ms.devlang: Java
ms.topic: quickstart
ms.date: 11/09/2020
ms.openlocfilehash: 2e32213b5d2e405a48df4b3c89115fbd0bb90c43
ms.sourcegitcommit: 6a770fc07237f02bea8cc463f3d8cc5c246d7c65
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95805842"
---
# <a name="send-messages-to-an-azure-service-bus-topic-and-receive-messages-from-subscriptions-to-the-topic-java"></a>Azure Service Bus konuya ileti gönderme ve aboneliklerden konuya ileti alma (Java)
Bu hızlı başlangıçta, bir Azure Service Bus konuya ileti göndermek ve sonra bu konuya yönelik aboneliklerden ileti almak için Azure-Messaging-ServiceBus paketini kullanarak Java kodu yazın.

> [!IMPORTANT]
> Bu hızlı başlangıç, yeni Azure-mesajlaşma-ServiceBus paketini kullanır. Eski Azure-ServiceBus paketini kullanan bir hızlı başlangıç için bkz. [Azure-ServiceBus kullanarak Ileti gönderme ve alma](service-bus-java-how-to-use-topics-subscriptions-legacy.md).

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [Visual Studio veya MSDN abonesi avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A85619ABF) etkinleştirebilir veya [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)için kaydolabilirsiniz.
- Hızlı başlangıçtaki adımları izleyin [: konuya bir Service Bus konu ve abonelik oluşturmak için Azure Portal kullanın](service-bus-quickstart-topics-subscriptions-portal.md). Bağlantı dizesi, konu adı ve abonelik adı ' nı aklınızda edin. Bu hızlı başlangıç için yalnızca bir abonelik kullanacaksınız. 
- [Java Için Azure SDK][Azure SDK for Java]'yı yükler. Çakışan Küreler kullanıyorsanız, Java için Azure SDK 'sını içeren [Azure Toolkit for Eclipse][Azure Toolkit for Eclipse] yükleyebilirsiniz. Ardından, projenize **Java için Microsoft Azure kitaplıklarını** ekleyebilirsiniz. IntelliJ kullanıyorsanız, bkz. [Azure Toolkit for IntelliJ yüklemesi](/azure/developer/java/toolkit-for-intellij/installation). 


## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme
Bu bölümde, bir Java konsol projesi oluşturacak ve oluşturduğunuz konuya ileti göndermek için kod ekleyeceksiniz. 

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

### <a name="add-code-to-send-messages-to-the-topic"></a>Konuya ileti göndermek için kod ekleme
1. `import`Java dosyasının konusunun aşağıdaki deyimlerini ekleyin. 

    ```java
    import com.azure.messaging.servicebus.*;
    import com.azure.messaging.servicebus.models.*;
    import java.util.concurrent.TimeUnit;
    import java.util.function.Consumer;
    import java.util.Arrays;
    import java.util.List;
    ```    
5. Sınıfında, bağlantı dizesini ve konu adını aşağıda gösterildiği gibi tutacak değişkenleri tanımlayın: 

    ```java
    static String connectionString = "<NAMESPACE CONNECTION STRING>";
    static String topicName = "<TOPIC NAME>";    
    static String subName = "<SUBSCRIPTION NAME>";
    ```

    `<NAMESPACE CONNECTION STRING>`Service Bus ad alanınız için bağlantı dizesiyle değiştirin. Ve, `<TOPIC NAME>` konusunun adıyla değiştirin.
3. `sendMessage`Konuya bir ileti göndermek için sınıfında adlı bir yöntem ekleyin. 

    ```java
        static void sendMessage()
    {
        // create a Service Bus Sender client for the queue 
        ServiceBusSenderClient senderClient = new ServiceBusClientBuilder()
                .connectionString(connectionString)
                .sender()
                .topicName(topicName)
                .buildClient();
        
        // send one message to the topic
        senderClient.sendMessage(new ServiceBusMessage("Hello, World!"));
        System.out.println("Sent a single message to the topic: " + topicName);        
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
1. `sendMessageBatch`Oluşturduğunuz konuya ileti göndermek için yöntemi adlı bir yöntem ekleyin. Bu yöntem, `ServiceBusSenderClient` konusu için bir oluşturur, `createMessages` ileti listesini almak için yöntemini çağırır, bir veya daha fazla toplu işi hazırlar ve toplu işleri konuya gönderir. 

```java
    static void sendMessageBatch()
    {
        // create a Service Bus Sender client for the topic 
        ServiceBusSenderClient senderClient = new ServiceBusClientBuilder()
                .connectionString(connectionString)
                .sender()
                .topicName(topicName)
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
            System.out.println("Sent a batch of messages to the topic: " + topicName);
            
            // create a new batch
            messageBatch = senderClient.createMessageBatch();

            // Add that message that we couldn't before.
            if (!messageBatch.tryAddMessage(message)) {
                System.err.printf("Message is too large for an empty batch. Skipping. Max size: %s.", messageBatch.getMaxSizeInBytes());
            }
        }

        if (messageBatch.getCount() > 0) {
            senderClient.sendMessages(messageBatch);
            System.out.println("Sent a batch of messages to the topic: " + topicName);
        }

        //close the client
        senderClient.close();
    }
```

## <a name="receive-messages-from-a-subscription"></a>Bir abonelikten ileti alma
Bu bölümde, bir aboneliğden konuya ileti almak için kod ekleyeceksiniz. 

1. `receiveMessages`Abonelikten ileti almak için adlı bir yöntem ekleyin. Bu yöntem `ServiceBusProcessorClient` , iletileri işlemek için bir işleyici ve hataları işlemek için başka bir tane belirterek abonelik için bir oluşturur. Ardından, işlemciyi başlatır, birkaç saniye bekler, alınan iletileri yazdırır ve sonra işlemciyi durdurup kapatır.

    ```java
    // handles received messages
    static void receiveMessages() throws InterruptedException
    {
        // Consumer that processes a single message received from Service Bus
        Consumer<ServiceBusReceivedMessageContext> messageProcessor = context -> {
            ServiceBusReceivedMessage message = context.getMessage();
            System.out.println("Received message: " + message.getBody().toString() + " from the subscription: " + subName);
        };

        // Consumer that handles any errors that occur when receiving messages
        Consumer<Throwable> errorHandler = throwable -> {
            System.out.println("Error when receiving messages: " + throwable.getMessage());
            if (throwable instanceof ServiceBusReceiverException) {
                ServiceBusReceiverException serviceBusReceiverException = (ServiceBusReceiverException) throwable;
                System.out.println("Error source: " + serviceBusReceiverException.getErrorSource());
            }
        };

        // Create an instance of the processor through the ServiceBusClientBuilder
        ServiceBusProcessorClient processorClient = new ServiceBusClientBuilder()
            .connectionString(connectionString)
            .processor()
            .topicName(topicName)
            .subscriptionName(subName)
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
Aşağıdaki çıktıya benzer çıktıyı görmek için programı çalıştırın:

```console
Sent a batch of messages to the topic: mytopic
Starting the processor
Received message: First message from the subscription: mysub
Received message: Second message from the subscription: mysub
Received message: Third message from the subscription: mysub
Stopping and closing the processor
```

Azure portal Service Bus ad alanının **genel bakış** sayfasında **gelen** ve **giden** ileti sayısını görebilirsiniz. Bir dakika beklemeniz gerekebilir ve en son değerleri görmek için sayfayı yenilemeniz gerekebilir. 

:::image type="content" source="./media/service-bus-java-how-to-use-queues/overview-incoming-outgoing-messages.png" alt-text="Gelen ve giden ileti sayısı" lightbox="./media/service-bus-java-how-to-use-queues/overview-incoming-outgoing-messages.png":::

Orta alt bölmedeki **konular** sekmesine geçin ve konunun **Service Bus konu** sayfasını görmek için konuyu seçin. Bu sayfada, **iletiler** grafiğinde dört gelen ve dört giden ileti görmeniz gerekir. 

:::image type="content" source="./media/service-bus-java-how-to-use-topics-subscriptions/topic-page-portal.png" alt-text="Gelen ve giden iletiler" lightbox="./media/service-bus-java-how-to-use-topics-subscriptions/topic-page-portal.png":::

`receiveMessages` `main` Yöntemi çağırın ve uygulamayı yeniden çalıştırırsanız, **Service Bus konu** sayfasında, 8 gelen ileti (4 yeni), ancak dört giden ileti görürsünüz. 

:::image type="content" source="./media/service-bus-java-how-to-use-topics-subscriptions/updated-topic-page.png" alt-text="Konu başlığı sayfası güncelleştirildi" lightbox="./media/service-bus-java-how-to-use-topics-subscriptions/updated-topic-page.png":::

Bu sayfada, bir abonelik seçerseniz **Service Bus abonelik** sayfasına ulaşabilirsiniz. Etkin ileti sayısını, atılacak ileti sayısını ve daha fazlasını bu sayfada görebilirsiniz. Bu örnekte, henüz bir alıcı tarafından alınmayan dört etkin ileti vardır. 

:::image type="content" source="./media/service-bus-java-how-to-use-topics-subscriptions/active-message-count.png" alt-text="Etkin ileti sayısı" lightbox="./media/service-bus-java-how-to-use-topics-subscriptions/active-message-count.png":::

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki belgelere ve örneklere bakın:

- [Java için Azure Service Bus istemci kitaplığı-Benioku](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/servicebus/azure-messaging-servicebus/README.md)
- [GitHub’daki örnekler](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/servicebus/azure-messaging-servicebus/src/samples)
- [Java API'si başvurusu](https://docs.microsoft.com/java/api/overview/azure/servicebus?view=azure-java-preview&preserve-view=true)


[Azure SDK for Java]: /java/api/overview/azure/
[Azure Toolkit for Eclipse]: /azure/developer/java/toolkit-for-eclipse/installation
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter]: /dotnet/api/microsoft.azure.servicebus.sqlfilter
[SqlFilter.SqlExpression]: /dotnet/api/microsoft.azure.servicebus.sqlfilter.sqlexpression
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage

