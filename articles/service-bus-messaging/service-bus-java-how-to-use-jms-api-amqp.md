---
title: Java Ileti hizmeti API 'SI ve Azure Service Bus AMQP kullanın
description: Azure Service Bus ve Gelişmiş İleti Sıraya Alma Protokolü (AMQP) 1,0 ile Java Ileti hizmeti 'ni (JMS) kullanın.
ms.topic: article
ms.date: 06/23/2020
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019, devx-track-java
ms.openlocfilehash: 97031abaedaa3e5595e290fa0292646feb744d47
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90086700"
---
# <a name="use-the-java-message-service-with-azure-service-bus-and-amqp-10"></a>Azure Service Bus ve AMQP 1,0 ile Java Ileti hizmetini kullanma

> [!WARNING]
> Bu makale, Java Ileti hizmeti (JMS) 1,1 API 'SI için *sınırlı desteğe* ve yalnızca Azure Service Bus standart katmanı için mevcuttur.
>
> Java Ileti hizmeti 2,0 API 'SI için tam destek, yalnızca önizleme aşamasında olan [Azure Service Bus Premium katmanında](how-to-use-java-message-service-20.md)kullanılabilir. Bu katmanı kullanmanızı öneririz.
>

Bu makalede, popüler JMS API standardı kullanılarak Java uygulamalarından Service Bus mesajlaşma özelliklerinin nasıl kullanılacağı açıklanmaktadır. Bu mesajlaşma özellikleri kuyrukları ve yayımlama veya konulara abone olmayı içerir. Bir [yardımcı makalede](service-bus-amqp-dotnet.md) , Azure Service Bus .NET API 'si kullanılarak nasıl yapılacağı açıklanır. Gelişmiş İleti Sıraya Alma Protokolü (AMQP) 1,0 kullanarak platformlar arası mesajlaşma hakkında bilgi edinmek için bu iki makaleyi birlikte kullanabilirsiniz.

AMQP 1,0, güçlü, platformlar arası mesajlaşma uygulamaları oluşturmak için kullanabileceğiniz verimli, güvenilir, hat düzeyinde bir mesajlaşma protokolüdür.

Service Bus ' de AMQP 1,0 desteği, verimli bir ikili protokol kullanarak, bir dizi platformda sıraya almayı ve bunları yayınlamayı veya abone olmayı kullanabileceğiniz anlamına gelir. Ayrıca, bir dil, çerçeve ve işletim sistemi karışımı kullanılarak oluşturulan bileşenlerden oluşan uygulamalar oluşturabilirsiniz.

## <a name="get-started-with-service-bus"></a>Service Bus’ı kullanmaya başlama

Bu makalede adlı bir sırayı içeren bir Service Bus ad alanınız zaten var `basicqueue` . Bunu yapmazsanız, [Azure Portal](https://portal.azure.com)kullanarak [ad alanını ve kuyruğu oluşturabilirsiniz](service-bus-create-namespace-portal.md) . Service Bus ad alanları ve kuyrukları oluşturma hakkında daha fazla bilgi için, bkz. [Service Bus kuyrukları kullanmaya başlama](service-bus-dotnet-get-started-with-queues.md).

> [!NOTE]
> Bölümlenmiş kuyruklar ve konular AMQP 'yi de destekler. Daha fazla bilgi için bkz. bölümlenmiş [mesajlaşma varlıkları](service-bus-partitioning.md) ve [Service Bus bölümlenmiş kuyruklar ve konular için AMQP 1,0 desteği](./service-bus-amqp-protocol-guide.md).
> 
> 

## <a name="download-the-amqp-10-jms-client-library"></a>AMQP 1,0 JMS istemci kitaplığını indirin

Apache Qpid JMS AMQP 1,0 istemci kitaplığı 'nın en son sürümünü indirme yeri hakkında daha fazla bilgi için bkz. [Apache qpid indirme sitesi](https://qpid.apache.org/download.html).

Service Bus ile JMS uygulamaları oluşturup çalıştırdığınızda, Apache Qpid JMS AMQP 1,0 dağıtım Arşivi 'nden aşağıdaki JAR dosyalarını Java SıNıFYOLU ortam değişkenine eklemeniz gerekir:

* GERONIMO-JMS \_ 1,1 \_ spec-1.0. jar
* qpid-JMS-Client-[sürüm]. jar

> [!NOTE]
> JMS JAR adları ve sürümleri değişmiş olabilir. Daha fazla bilgi için bkz. [Qpid JMS AMQP 1,0](https://qpid.apache.org/maven.html#qpid-jms-amqp-10).

## <a name="code-java-applications"></a>Kod Java uygulamaları

### <a name="java-naming-and-directory-interface"></a>Java adlandırma ve Dizin arabirimi

JMS, mantıksal adlar ve fiziksel adlar arasında bir ayrım oluşturmak için Java adlandırma ve Dizin arabirimini (JNDı) kullanır. İki tür JMS nesnesi, JNDı: **Connectionfactory** ve **Destination**kullanılarak çözümlenir. JNDı, ad çözümlemesi görevlerini işlemek üzere farklı Dizin Hizmetleri ekleyebileceğiniz bir sağlayıcı modeli kullanır. Apache Qpid JMS AMQP 1,0 kitaplığı, aşağıdaki biçimdeki bir özellikler dosyası kullanılarak yapılandırılmış basit özellik dosya tabanlı JNDı sağlayıcısı ile birlikte gelir:

```TEXT
# servicebus.properties - sample JNDI configuration

# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net

# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="set-up-jndi-context-and-configure-the-connectionfactory-object"></a>JNDı bağlamını ayarlama ve ConnectionFactory nesnesini yapılandırma

Başvurulan bağlantı dizesi, **birincil bağlantı dizesi**altındaki [Azure Portal](https://portal.azure.com) paylaşılan erişim ilkelerinde kullanılabilir bir dizedir.

```java
// The connection string builder is the only part of the azure-servicebus SDK library
// we use in this JMS sample and for the purpose of robustly parsing the Service Bus 
// connection string. 
ConnectionStringBuilder csb = new ConnectionStringBuilder(connectionString);
        
// Set up JNDI context
Hashtable<String, String> hashtable = new Hashtable<>();
hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + "?amqp.idleTimeout=120000&amqp.traceFrames=true");
hashtable.put("queue.QUEUE", "BasicQueue");
hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
Context context = new InitialContext(hashtable);

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");

// Look up queue
Destination queue = (Destination) context.lookup("QUEUE");
```

#### <a name="configure-producer-and-consumer-destination-queues"></a>Üretici ve tüketici hedef sıralarını yapılandırma

JNDı sağlayıcısında Qpid Özellikler dosyasındaki bir hedefi tanımlamak için kullanılan giriş aşağıdaki biçimdedir.

Üretici için bir hedef kuyruğu oluşturmak için:
```java
String queueName = "queueName";
Destination queue = (Destination) queueName;

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
Connection connection - cf.createConnection(csb.getSasKeyName(), csb.getSasKey());

Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);

// Create producer
MessageProducer producer = session.createProducer(queue);
```

Tüketiciye yönelik bir hedef sıra oluşturmak için:
```java
String queueName = "queueName";
Destination queue = (Destination) queueName;

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
Connection connection - cf.createConnection(csb.getSasKeyName(), csb.getSasKey());

Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);

// Create consumer
MessageConsumer consumer = session.createConsumer(queue);
```

### <a name="write-the-jms-application"></a>JMS uygulamasını yazma

JMS 'yi Service Bus kullandığınızda özel API 'Ler veya seçenekler gerekli değildir. Daha sonra ele alınacaktır bazı kısıtlamalar vardır. Her JMS uygulamasında olduğu gibi, gereken ilk şey, bir **Connectionfactory** nesnesini ve hedeflerini çözebilmek için JNDI ortamının yapılandırılması olur.

#### <a name="configure-the-jndi-initialcontext-object"></a>JNDı InitialContext nesnesini yapılandırma

JNDı ortamı, yapılandırma bilgilerinin karma tablosu javax.naming.InitialContext sınıfının oluşturucusuna geçirilerek yapılandırılır. Karma tablodaki iki gerekli öğe, Ilk bağlam fabrikasının sınıf adı ve sağlayıcı URL 'sidir. Aşağıdaki kod, JNDı ortamının, **ServiceBus. Properties**adlı bir özellikler dosyası Ile qpid özellikleri dosya tabanlı JNDI sağlayıcısını kullanmak için nasıl yapılandırılacağını gösterir.

```java
// Set up JNDI context
Hashtable<String, String> hashtable = new Hashtable<>();
hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + \
"?amqp.idleTimeout=120000&amqp.traceFrames=true");
hashtable.put("queue.QUEUE", "BasicQueue");
hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
Context context = new InitialContext(hashtable);
``` 

### <a name="a-simple-jms-application-that-uses-a-service-bus-queue"></a>Service Bus kuyruğu kullanan basit bir JMS uygulaması

Aşağıdaki örnek program, JNDı mantıksal adına sahip bir Service Bus kuyruğuna JMS metin iletileri gönderir ve iletileri geri alır.

Tüm kaynak kodu ve yapılandırma bilgilerine, [JMS kuyruğu hızlı başlangıcı Azure Service Bus örneklerinden](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/qpid-jms-client/JmsQueueQuickstart)erişebilirsiniz.

```java
// Copyright (c) Microsoft. All rights reserved.
// Licensed under the MIT license. See LICENSE file in the project root for full license information.

package com.microsoft.azure.servicebus.samples.jmsqueuequickstart;

import com.microsoft.azure.servicebus.primitives.ConnectionStringBuilder;
import org.apache.commons.cli.*;
import org.apache.log4j.*;

import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.util.Hashtable;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.function.Function;

/**
 * This sample demonstrates how to send messages from a JMS queue producer into
 * an Azure Service Bus queue and receive them with a JMS message consumer.
 * JMS queue. 
 */
public class JmsQueueQuickstart {

    // Number of messages to send
    private static int totalSend = 10;
    //Tracking counter for how many messages have been received; used as termination condition
    private static AtomicInteger totalReceived = new AtomicInteger(0);
    // log4j logger 
    private static Logger logger = Logger.getRootLogger();

    public void run(String connectionString) throws Exception {

        // The connection string builder is the only part of the azure-servicebus SDK library
        // we use in this JMS sample and for the purpose of robustly parsing the Service Bus 
        // connection string. 
        ConnectionStringBuilder csb = new ConnectionStringBuilder(connectionString);
        
        // Set up JNDI context
        Hashtable<String, String> hashtable = new Hashtable<>();
        hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + "?amqp.idleTimeout=120000&amqp.traceFrames=true");
        hashtable.put("queue.QUEUE", "BasicQueue");
        hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
        Context context = new InitialContext(hashtable);
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        
        // Look up queue
        Destination queue = (Destination) context.lookup("QUEUE");

        // We create a scope here so we can use the same set of local variables cleanly 
        // again to show the receive side separately with minimal clutter.
        {
            // Create connection
            Connection connection = cf.createConnection(csb.getSasKeyName(), csb.getSasKey());
            // Create session, no transaction, client ack
            Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);

            // Create producer
            MessageProducer producer = session.createProducer(queue);

            // Send messages
            for (int i = 0; i < totalSend; i++) {
                BytesMessage message = session.createBytesMessage();
                message.writeBytes(String.valueOf(i).getBytes());
                producer.send(message);
                System.out.printf("Sent message %d.\n", i + 1);
            }

            producer.close();
            session.close();
            connection.stop();
            connection.close();
        }

        {
            // Create connection
            Connection connection = cf.createConnection(csb.getSasKeyName(), csb.getSasKey());
            connection.start();
            // Create session, no transaction, client ack
            Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            // Create consumer
            MessageConsumer consumer = session.createConsumer(queue);
            // Create a listener callback to receive the messages
            consumer.setMessageListener(message -> {
                try {
                    // Received message is passed to callback
                    System.out.printf("Received message %d with sq#: %s\n",
                            totalReceived.incrementAndGet(), // increments the tracking counter
                            message.getJMSMessageID());
                    message.acknowledge();
                } catch (Exception e) {
                    logger.error(e);
                }
            });

            // Wait on the main thread until all sent messages have been received
            while (totalReceived.get() < totalSend) {
                Thread.sleep(1000);
            }
            consumer.close();
            session.close();
            connection.stop();
            connection.close();
        }

        System.out.printf("Received all messages, exiting the sample.\n");
        System.out.printf("Closing queue client.\n");
    }

    public static void main(String[] args) {

        System.exit(runApp(args, (connectionString) -> {
            JmsQueueQuickstart app = new JmsQueueQuickstart();
            try {
                app.run(connectionString);
                return 0;
            } catch (Exception e) {
                System.out.printf("%s", e.toString());
                return 1;
            }
        }));
    }

    static final String SB_SAMPLES_CONNECTIONSTRING = "SB_SAMPLES_CONNECTIONSTRING";

    public static int runApp(String[] args, Function<String, Integer> run) {
        try {

            String connectionString = null;

            // Parse connection string from command line
            Options options = new Options();
            options.addOption(new Option("c", true, "Connection string"));
            CommandLineParser clp = new DefaultParser();
            CommandLine cl = clp.parse(options, args);
            if (cl.getOptionValue("c") != null) {
                connectionString = cl.getOptionValue("c");
            }

            // Get overrides from the environment
            String env = System.getenv(SB_SAMPLES_CONNECTIONSTRING);
            if (env != null) {
                connectionString = env;
            }

            if (connectionString == null) {
                HelpFormatter formatter = new HelpFormatter();
                formatter.printHelp("run jar with", "", options, "", true);
                return 2;
            }
            return run.apply(connectionString);
        } catch (Exception e) {
            System.out.printf("%s", e.toString());
            return 3;
        }
    }
}
```

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırmak için, paylaşılan erişim Ilkelerinden **bağlantı dizesini** geçirin.
Aşağıdaki çıktı, uygulamayı çalıştıran formundadır:

```Output
> mvn clean package
>java -jar ./target/jmsqueuequickstart-1.0.0-jar-with-dependencies.jar -c "<CONNECTION_STRING>"

Sent message 1.
Sent message 2.
Sent message 3.
Sent message 4.
Sent message 5.
Sent message 6.
Sent message 7.
Sent message 8.
Sent message 9.
Sent message 10.
Received message 1 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-1
Received message 2 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-2
Received message 3 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-3
Received message 4 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-4
Received message 5 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-5
Received message 6 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-6
Received message 7 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-7
Received message 8 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-8
Received message 9 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-9
Received message 10 with sq#: ID:7f6a7659-bcdf-4af6-afc1-4011e2ddcb3c:1:1:1-10
Received all messages, exiting the sample.
Closing queue client.

```

## <a name="amqp-disposition-and-service-bus-operation-mapping"></a>AMQP değerlendirmesi ve Service Bus işlem eşleme

Bir AMQP değerlendirmesi Service Bus bir işleme çevrilir:

```Output
ACCEPTED = 1; -> Complete()
REJECTED = 2; -> DeadLetter()
RELEASED = 3; (just unlock the message in service bus, will then get redelivered)
MODIFIED_FAILED = 4; -> Abandon() which increases delivery count
MODIFIED_FAILED_UNDELIVERABLE = 5; -> Defer()
```

## <a name="jms-topics-vs-service-bus-topics"></a>JMS konuları ile Service Bus konuları

JMS API aracılığıyla Service Bus konuları ve abonelikleri kullanmak, temel gönderme ve alma özellikleri sağlar. JMS uyumlu API 'lerle diğer ileti aracılarından uygulamalar için bağlantı noktası kullanıyorsanız, Service Bus konular JMS konularından farklı ve birkaç ayarlama gerektirse de kullanışlı bir seçenektir.

Service Bus konular, Azure Kaynak yönetimi arabirimi, Azure komut satırı araçları veya Azure portal aracılığıyla yönetilen adlandırılmış, paylaşılan ve dayanıklı aboneliklerdeki iletileri yönlendirir. Her abonelik, her biri bir filtre koşuluna sahip olabilecek ve SQL filtreleri için bir meta veri dönüştürme eylemi olan 2.000 adede kadar seçim kuralına izin verir. Her filtre koşulu eşleşmesi, aboneliğe kopyalanacak giriş iletisini seçer.  

Aboneliklerden ileti alma, kuyruklardan ileti almaya benzer. Her aboneliğin ilişkili bir atılacak ileti sırası ve iletileri otomatik olarak başka bir kuyruğa veya konuya iletme yeteneği vardır.

JMS konuları, istemcilerin isteğe bağlı olarak ileti seçiciyle ileti filtrelemesine izin veren dayanıklı ve dayanıklı aboneler dinamik olarak oluşturmalarına olanak tanır. Bu paylaşılmayan varlıklar Service Bus tarafından desteklenmez. Service Bus için SQL filtre kuralı sözdizimi, JMS tarafından desteklenen ileti seçici sözdizimine benzerdir.

JMS konu yayımcı tarafı, bu örnekte gösterildiği gibi Service Bus ile uyumludur, ancak dinamik aboneler değildir. Topoloji ile ilgili aşağıdaki JMS API 'Leri Service Bus desteklenmez.

## <a name="unsupported-features-and-restrictions"></a>Desteklenmeyen özellikler ve kısıtlamalar

Aşağıdaki kısıtlamalar, Service Bus ile AMQP 1,0 üzerinden JMS kullandığınızda mevcuttur, yani:

* Oturum başına yalnızca bir **Messageproducer** veya **messageconsumer** nesnesine izin verilir. Bir uygulamada birden çok **Messageproducer** veya **messageconsumer** nesnesi oluşturmanız gerekiyorsa, bunların her biri için özel bir oturum oluşturun.
* Geçici konu abonelikleri şu anda desteklenmiyor.
* **Messageselector** nesneleri şu anda desteklenmiyor.
* Dağıtılmış işlemler desteklenmez, ancak işlenen oturumlar desteklenir.

Service Bus, denetim düzlemi 'ni veri düzleminden ayırır, bu nedenle JMS 'nin dinamik topoloji işlevlerinin birkaçını desteklemez.

| Desteklenmeyen Yöntem          | Şununla değiştir                                                                             |
|-----------------------------|------------------------------------------------------------------------------------------|
| Createdurableabonesi     | İleti seçicisine bağlantı noktası veren bir konu aboneliği oluşturun.                                |
| createDurableConsumer       | İleti seçicisine bağlantı noktası veren bir konu aboneliği oluşturun.                                |
| createSharedConsumer        | Service Bus konular her zaman paylaşılabilir. "JMS konuları vs. Service Bus konuları" bölümüne bakın.                                    |
| createSharedDurableConsumer | Service Bus konular her zaman paylaşılabilir. "JMS konuları vs. Service Bus konuları" bölümüne bakın.                                      |
| createTemporaryTopic        | Yönetim API 'SI, araçları veya Portal aracılığıyla, *oto Deleteonıdle* bir süre sonu süresine ayarlanmış bir konu oluşturun. |
| createTopic                 | Yönetim API 'SI, araçları veya portalı aracılığıyla bir konu oluşturun.                                         |
| kaldırmak                 | Konu yönetimi API 'sini, araçları veya portalı 'nı silin.                                            |
| createBrowser               | Desteklenmez. Service Bus API 'sinin Peek () işlevini kullanın.                         |
| createQueue                 | Yönetim API 'SI, araçları veya portalı aracılığıyla bir kuyruk oluşturun.                                           | 
| createTemporaryQueue        | Yönetim API 'SI, araçlar veya Portal aracılığıyla, bir sona erme süresine ayarlanmış olan, *oto Deleteonıdle* içeren bir kuyruk oluşturun. |
| receiveNoWait               | Service Bus SDK tarafından sunulan Receive () yöntemini kullanın ve çok düşük veya sıfır zaman aşımı belirtin. |

## <a name="summary"></a>Özet

Bu makalede, popüler JMS API ve AMQP 1,0 kullanarak Java 'dan kuyruklar ve yayımlama ya da abone olma gibi Service Bus Aracılı mesajlaşma özelliklerinin nasıl kullanılacağı gösterilmektedir.

.NET, C, Python ve PHP gibi diğer dillerden AMQP 1,0 Service Bus de kullanabilirsiniz. Bu farklı diller kullanılarak oluşturulan bileşenler, Service Bus içinde AMQP 1,0 desteğini kullanarak güvenilir bir şekilde ve tam aslına uygunluk iletileri gönderip alabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Service Bus 'da AMQP 1,0 desteği](service-bus-amqp-overview.md)
* [Service Bus .NET API ile AMQP 1,0 kullanma](./service-bus-amqp-dotnet.md)
* [Service Bus AMQP 1,0 Geliştirici Kılavuzu](service-bus-amqp-dotnet.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Java Geliştirici Merkezi](https://azure.microsoft.com/develop/java/)