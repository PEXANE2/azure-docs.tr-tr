---
title: Java ile Azure Servis Veri Servisi kuyruklarını kullanma
description: Bu eğitimde, bir Azure Hizmet Veri Yolu kuyruğuna ileti göndermek ve ileti almak için Java uygulamaları oluşturmayı öğrenirsiniz.
services: service-bus-messaging
documentationcenter: java
author: axisc
manager: timlt
editor: spelluru
ms.assetid: f701439c-553e-402c-94a7-64400f997d59
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aschhab
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: ac6bc8f78bd3d526e68dba3e81825a28a9ac47f7
ms.sourcegitcommit: fab450a18a600d72b583ecfbe6c5e53afd43408c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80294137"
---
# <a name="quickstart-use-azure-service-bus-queues-with-java-to-send-and-receive-messages"></a>Hızlı başlangıç: İleti göndermek ve almak için Java ile Azure Servis Veri Servisi kuyruklarını kullanın

[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]
Bu eğitimde, bir Azure Hizmet Veri Yolu kuyruğuna ileti göndermek ve ileti almak için Java uygulamaları oluşturmayı öğrenirsiniz. 

> [!NOTE]
> GitHub'da Java örneklerini [azure servis veri](https://github.com/Azure/azure-service-bus/tree/master/samples/Java)yolunda bulabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
1. Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. [MSDN abone avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) etkinleştirebilir veya ücretsiz bir [hesaba](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF)kaydolabilirsiniz.
2. Üzerinde çalışmanız gereken bir sıra yoksa, kuyruk oluşturmak için Hizmet Veri Hizmetleri sırası makalesi [oluşturmak için Azure kullan portalındaki](service-bus-quickstart-portal.md) adımları izleyin.
    1. Servis Veri Servisi **kuyruklarına**hızlı **bir genel bakış** okuyun. 
    2. Hizmet Veri Günü **ad alanı**oluşturun. 
    3. Bağlantı **dizesini**alın.
    4. Servis Veri Servisi **kuyruğu**oluşturun.
3. [Java için Azure SDK'yı yükleyin.][Azure SDK for Java] 


## <a name="configure-your-application-to-use-service-bus"></a>Uygulamanızı Servis Veri Aracı'nı kullanacak şekilde yapılandırın
Bu örneği oluşturmadan önce [Azure SDK'yı Java için][Azure SDK for Java] yüklediğinizden emin olun. 

Eclipse kullanıyorsanız, Java için [Azure][Azure Toolkit for Eclipse] SDK'yı içeren Azure Araç Seti'ni Eclipse için yükleyebilirsiniz. Ardından, projenize **Java için Microsoft Azure Kitaplıkları** ekleyebilirsiniz. IntelliJ kullanıyorsanız, Bkz. [IntelliJ için Azure Araç Kitini Yükle.](/azure/java/intellij/azure-toolkit-for-intellij-installation) 

![Eclipse projenize Java için Microsoft Azure Kitaplıkları ekleyin](./media/service-bus-java-how-to-use-queues/eclipse-azure-libraries-java.png)


Java dosyasının üst kısmında aşağıdaki `import` ifadeleri ekleyin:

```java
// Include the following imports to use Service Bus APIs
import com.google.gson.reflect.TypeToken;
import com.microsoft.azure.servicebus.*;
import com.microsoft.azure.servicebus.primitives.ConnectionStringBuilder;
import com.google.gson.Gson;

import static java.nio.charset.StandardCharsets.*;

import java.time.Duration;
import java.util.*;
import java.util.concurrent.*;

import org.apache.commons.cli.*;

```

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Hizmet Veri Servisi Kuyruğu'na ileti göndermek için, uygulamanız bir **QueueClient** nesnesini anında alır ve iletileri eşitbir şekilde gönderir. Aşağıdaki kod, portal üzerinden oluşturulan bir Kuyruk için iletinin nasıl gönderilen şekli gösterir.

```java
public void run() throws Exception {
    // Create a QueueClient instance and then asynchronously send messages.
    // Close the sender once the send operation is complete.
    QueueClient sendClient = new QueueClient(new ConnectionStringBuilder(ConnectionString, QueueName), ReceiveMode.PEEKLOCK);
    this.sendMessageAsync(sendClient).thenRunAsync(() -> sendClient.closeAsync());

    sendClient.close();
}

    CompletableFuture<Void> sendMessagesAsync(QueueClient sendClient) {
        List<HashMap<String, String>> data =
                GSON.fromJson(
                        "[" +
                                "{'name' = 'Einstein', 'firstName' = 'Albert'}," +
                                "{'name' = 'Heisenberg', 'firstName' = 'Werner'}," +
                                "{'name' = 'Curie', 'firstName' = 'Marie'}," +
                                "{'name' = 'Hawking', 'firstName' = 'Steven'}," +
                                "{'name' = 'Newton', 'firstName' = 'Isaac'}," +
                                "{'name' = 'Bohr', 'firstName' = 'Niels'}," +
                                "{'name' = 'Faraday', 'firstName' = 'Michael'}," +
                                "{'name' = 'Galilei', 'firstName' = 'Galileo'}," +
                                "{'name' = 'Kepler', 'firstName' = 'Johannes'}," +
                                "{'name' = 'Kopernikus', 'firstName' = 'Nikolaus'}" +
                                "]",
                        new TypeToken<List<HashMap<String, String>>>() {}.getType());

        List<CompletableFuture> tasks = new ArrayList<>();
        for (int i = 0; i < data.size(); i++) {
            final String messageId = Integer.toString(i);
            Message message = new Message(GSON.toJson(data.get(i), Map.class).getBytes(UTF_8));
            message.setContentType("application/json");
            message.setLabel("Scientist");
            message.setMessageId(messageId);
            message.setTimeToLive(Duration.ofMinutes(2));
            System.out.printf("\nMessage sending: Id = %s", message.getMessageId());
            tasks.add(
                    sendClient.sendAsync(message).thenRunAsync(() -> {
                        System.out.printf("\n\tMessage acknowledged: Id = %s", message.getMessageId());
                    }));
        }
        return CompletableFuture.allOf(tasks.toArray(new CompletableFuture<?>[tasks.size()]));
    }

```

Hizmet Veri Servisi kuyruklarına gönderilen ve alınan iletiler [İleti](/java/api/com.microsoft.azure.servicebus.message?view=azure-java-stable) sınıfının örnekleridir. İleti nesnelerinin standart özellikleri (Label ve TimeToLive gibi), özel uygulamaya özgü özellikleri tutmak için kullanılan bir sözlük ve rasgele uygulama verileri gövdesi vardır. Bir uygulama, iletinin herhangi bir serileştirilebilir nesnesini İleti'nin oluşturucuya geçirerek ayarlayabilir ve uygun serileştirici daha sonra nesneyi serihale getirmek için kullanılır. Alternatif olarak, bir **java sağlayabilir. ıo. InputStream** nesnesi.


Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Standart ve özel uygulama özelliklerini içeren üst bilginin maksimum dosya boyutu 64 KB olabilir. Kuyrukta tutulan ileti sayısına ilişkin bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu için uç sınır vardır. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir.

## <a name="receive-messages-from-a-queue"></a>Kuyruktan ileti alma
Bir kuyruktan ileti almanın birincil yolu **ServiceBusContract** nesnesi kullanmaktır. Alınan iletiler iki farklı modda çalışabilir: **ReceiveAndDelete** ve **PeekLock**.

**ReceiveAndDelete** modunu kullanırken, receive tek çekimlik bir işlemdir - yani Servis Veri Servisi kuyruktaki bir ileti için okuma isteği aldığında, iletinin tüketildiği şeklinde işaretler ve uygulamaya geri verir. **ReceiveAndDelete** modu (varsayılan moddur) en basit modeldir ve bir uygulamanın bir hata durumunda bir iletiyi işlememeye tahammül edebileceği senaryolar için en iyi şekilde çalışır. Bu durumu daha iyi anlamak için müşterinin bir alma isteği bildirdiğini ve bu isteğin işlenmeden çöktüğünü varsayın.
Servis Veri Kurumu iletinin tüketildiği olarak işaretlendiğinden, uygulama yeniden başlatıldığında ve iletileri yeniden almaya başladığında, kilitlenmeden önce tüketilen iletiyi kaçırmıştır.

**PeekLock** modunda receive, eksik iletileri tolere edemeyen uygulamaları desteklemeyi mümkün kılan iki aşamalı bir işlem haline gelir. Service Bus bir istek aldığında bir sonraki kullanılacak iletiyi bulur, diğer tüketicilerin bu iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi bitirdikten sonra (veya gelecekteki işlemler için güvenilir bir şekilde depoladıktan) alınan iletide **tam()** çağırarak alma işleminin ikinci aşamasını tamamlar. Servis Veri Servisi aramanın **tamamını** gördüğünde, iletinin tüketildiğini işaretler ve sıradan kaldırır. 

Aşağıdaki örnek, iletilerin **PeekLock** modu (varsayılan mod değil) kullanılarak nasıl alınabileceğini ve işlenebileceğini gösterir. Aşağıdaki örnekte kayıtlı bir ileti işleyicisi olan geri arama modeli `TestQueue`kullanır ve iletileri bizim . Bu mod, geri arama normal olarak geri döndüğünden otomatik olarak **tam()** çağırır ve geri arama bir özel durum oluşturursa **terk()** çağırır. 

```java
    public void run() throws Exception {
        // Create a QueueClient instance for receiving using the connection string builder
        // We set the receive mode to "PeekLock", meaning the message is delivered
        // under a lock and must be acknowledged ("completed") to be removed from the queue
        QueueClient receiveClient = new QueueClient(new ConnectionStringBuilder(ConnectionString, QueueName), ReceiveMode.PEEKLOCK);
        this.registerReceiver(receiveClient);

        // shut down receiver to close the receive loop
        receiveClient.close();
    }
    void registerReceiver(QueueClient queueClient) throws Exception {
        // register the RegisterMessageHandler callback
        queueClient.registerMessageHandler(new IMessageHandler() {
        // callback invoked when the message handler loop has obtained a message
            public CompletableFuture<Void> onMessageAsync(IMessage message) {
            // receives message is passed to callback
                if (message.getLabel() != null &&
                    message.getContentType() != null &&
                    message.getLabel().contentEquals("Scientist") &&
                    message.getContentType().contentEquals("application/json")) {

                        byte[] body = message.getBody();
                        Map scientist = GSON.fromJson(new String(body, UTF_8), Map.class);

                        System.out.printf(
                            "\n\t\t\t\tMessage received: \n\t\t\t\t\t\tMessageId = %s, \n\t\t\t\t\t\tSequenceNumber = %s, \n\t\t\t\t\t\tEnqueuedTimeUtc = %s," +
                            "\n\t\t\t\t\t\tExpiresAtUtc = %s, \n\t\t\t\t\t\tContentType = \"%s\",  \n\t\t\t\t\t\tContent: [ firstName = %s, name = %s ]\n",
                            message.getMessageId(),
                            message.getSequenceNumber(),
                            message.getEnqueuedTimeUtc(),
                            message.getExpiresAtUtc(),
                            message.getContentType(),
                            scientist != null ? scientist.get("firstName") : "",
                            scientist != null ? scientist.get("name") : "");
                    }
                    return CompletableFuture.completedFuture(null);
                }

                // callback invoked when the message handler has an exception to report
                public void notifyException(Throwable throwable, ExceptionPhase exceptionPhase) {
                    System.out.printf(exceptionPhase + "-" + throwable.getMessage());
                }
        },
        // 1 concurrent call, messages are auto-completed, auto-renew duration
        new MessageHandlerOptions(1, true, Duration.ofMinutes(1)));
    }

```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Uygulama çökmelerini ve okunmayan iletileri giderme
Service Bus, uygulamanızda gerçekleşen hataları veya ileti işlenirken oluşan zorlukları rahat bir şekilde ortadan kaldırmanıza yardımcı olmak için işlevsellik sağlar. Bir alıcı uygulaması iletiyi nedense işleyemiyorsa, **getLockToken()** üzerinden alınan iletinin kilit belirteci ile istemci nesnesi üzerinde **terk etme()** yöntemini arayabilir. Bu işlem Service Bus hizmetinin kuyruktaki iletinin kilidini açmasına ve iletiyi aynı veya başka bir kullanıcı uygulama tarafından tekrar alınabilir hale getirmesine neden olur.

Sıra içinde kilitli bir iletiyle ilişkili bir zaman aşımı da vardır ve uygulama kilit süresi sona ermeden önce iletiyi işlemezse (örneğin, uygulama çöküyorsa), Servis Veri Servisi iletiyi otomatik olarak açar ve tekrar alınabilir.

Uygulamanın iletiyi işledikten sonra ancak **tam()** isteği verilmeden önce çökmesi durumunda, ileti yeniden başlatıldığında uygulamaya yeniden teslim edilir. Bu durum *En Az Bir Kez İşleme* olarak adlandırılır. Her ileti en az bir kez işlenir ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Senaryo yinelenen işlemeyi kabul etmiyorsa yinelenen ileti teslimine izin vermek için uygulama geliştiricilerin uygulamaya ilave bir mantık eklemesi gerekir. Bu genellikle iletinin **getMessageId** yöntemi kullanılarak elde edilir ve bu yöntem, teslim girişimleri arasında sabit kalır.

> [!NOTE]
> [Servis](https://github.com/paolosalvatori/ServiceBusExplorer/)Veri Servisi Explorer ile Servis Veri Servisi kaynaklarını yönetebilirsiniz. Service Bus Explorer, kullanıcıların bir Service Bus ad alanına bağlanmasına ve ileti varlıklarını kolay bir şekilde yönetmesine olanak tanır. Araç, alma/dışa aktarma işlevselliği veya konuyu, kuyrukları, abonelikleri, geçiş hizmetlerini, bildirim hub'larını ve olay hub'larını test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki Adımlar
Artık Servis Veri Servisi kuyruklarının temellerini öğrendiğiniz için, daha fazla bilgi için [Kuyruklar, konular ve aboneliklere][Queues, topics, and subscriptions] bakın.

Daha fazla bilgi için bkz. [Java Geliştirici Merkezi](https://azure.microsoft.com/develop/java/).

[Azure SDK for Java]: /azure/java/java-sdk-azure-get-started
[Azure Toolkit for Eclipse]: https://docs.microsoft.com/java/azure/eclipse/azure-toolkit-for-eclipse
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
