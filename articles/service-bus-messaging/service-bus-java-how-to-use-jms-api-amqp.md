---
title: Azure Hizmet Veri Veri Tos&Java İleti Hizmeti API ile AMQP'yi kullanma
description: Azure Hizmet Veri Servisi ve Gelişmiş İleti Kuyruk Protokolü (AMQP) 1.0 ile Java İleti Hizmeti (JMS) nasıl kullanılır?
services: service-bus-messaging
documentationcenter: java
author: axisc
editor: spelluru
ms.assetid: be766f42-6fd1-410c-b275-8c400c811519
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 10/22/2019
ms.author: aschhab
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: cd06838abbb69af5684fdea18c42f6a8f95ffe2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77371258"
---
# <a name="use-the-java-message-service-jms-with-azure-service-bus-and-amqp-10"></a>Azure Hizmet Veri Gönderisi ve AMQP 1.0 ile Java İleti Hizmetini (JMS) kullanma
Bu makalede, popüler Java İleti Hizmeti (JMS) API standardını kullanarak Java uygulamalarından Azure Hizmet Veri Servisi mesajlaşma özelliklerinin (kuyruklar ve yayımlama/abone olunma konuları) nasıl kullanılacağı açıklanmaktadır. Azure Hizmet Veri Servisi .NET API'sini kullanarak aynı şeyi nasıl yapacağız açıklayan bir [yardımcı makale](service-bus-amqp-dotnet.md) vardır. Bu iki kılavuzu, AMQP 1.0'ı kullanarak platformlar arası mesajlaşma hakkında bilgi edinmek için birlikte kullanabilirsiniz.

Gelişmiş İleti Sıraya Protokolü (AMQP) 1.0, sağlam, platformlar arası mesajlaşma uygulamaları oluşturmak için kullanabileceğiniz verimli, güvenilir, kablo düzeyinde bir ileti protokolüdür.

Azure Hizmet Veri Yolu'nda AMQP 1.0 desteği, etkin bir ikili protokol kullanarak çeşitli platformlardan sıraya alma ve aracılı mesajlaşma özelliklerini yayımlama/abone olta özelliğini kullanabileceğiniz anlamına gelir. Ayrıca, dillerin, çerçevelerin ve işletim sistemlerinin bir karışımını kullanarak oluşturulmuş bileşenlerden oluşan uygulamalar oluşturabilirsiniz.

## <a name="get-started-with-service-bus"></a>Service Bus’ı kullanmaya başlama
Bu kılavuz, zaten bir Hizmet Veri Servisi ad alanı `basicqueue`adlı bir sıra içeren varsayar. Bunu yapmazsanız, [Azure portalını](https://portal.azure.com)kullanarak [ad alanı ve sıra oluşturabilirsiniz.](service-bus-create-namespace-portal.md) Hizmet Veri Servisi ad alanları ve kuyrukları nasıl oluşturulacak hakkında daha fazla bilgi için [bkz.](service-bus-dotnet-get-started-with-queues.md)

> [!NOTE]
> Bölümlenmiş kuyruklar ve konular da AMQP'yi destekler. Daha fazla bilgi için, Hizmet Veri Kurumu bölümlenmiş kuyruklar ve konular için [Bölümlenmiş ileti varlıkları](service-bus-partitioning.md) [ve AMQP 1.0 desteğine](service-bus-partitioned-queues-and-topics-amqp-overview.md)bakın.
> 
> 

## <a name="downloading-the-amqp-10-jms-client-library"></a>AMQP 1.0 JMS istemci kitaplığını indirme
Apache Qpid JMS AMQP 1.0 istemci kitaplığı en son sürümünü [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html)indirmek için nerede hakkında bilgi için, ziyaret edin.

Hizmet Veri Yolu ile JMS uygulamaları kurarken ve çalıştırırken Apache Qpid JMS AMQP 1.0 dağıtım arşivinden java CLASSPATH'e aşağıdaki dört JAR dosyasını eklemeniz gerekir:

* geronimo-jms\_1.1\_spec-1.0.jar
* qpid-jms-istemci-[sürüm].jar

> [!NOTE]
> JMS JAR adları ve sürümleri değişmiş olabilir. Ayrıntılar için [Qpid JMS - AMQP 1.0'a](https://qpid.apache.org/maven.html#qpid-jms-amqp-10)bakın.

## <a name="coding-java-applications"></a>Java uygulamalarını kodlama
### <a name="java-naming-and-directory-interface-jndi"></a>Java Adlandırma ve Dizin Arabirimi (JNDI)
JMS, mantıksal adlar ve fiziksel adlar arasında bir ayrım oluşturmak için Java Adlandırma ve Dizin Arabirimi'ni (JNDI) kullanır. JNDI: ConnectionFactory ve Destination kullanılarak iki tür JMS nesnesi çözülür. JNDI, ad çözümleme görevlerini işlemek için farklı dizin hizmetlerini takabileceğiniz bir sağlayıcı modeli kullanır. Apache Qpid JMS AMQP 1.0 kitaplığı, aşağıdaki biçimdeki özellikler dosyası kullanılarak yapılandırılan basit bir özellik dosya tabanlı JNDI Sağlayıcısı ile birlikte gelir:

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

#### <a name="setup-jndi-context-and-configure-the-connectionfactory"></a>Kurulum JNDI bağlam ve BağlantıFabrika yapılandırma

Birincil Bağlantı Dizesi altında [Azure portalında](https://portal.azure.com) bulunan 'Paylaşılan Erişim İlkeleri'nde bulunan **ConnectionString'te** başvurulan **Bağlantı String**
```java
// The connection string builder is the only part of the azure-servicebus SDK library
// we use in this JMS sample and for the purpose of robustly parsing the Service Bus 
// connection string. 
ConnectionStringBuilder csb = new ConnectionStringBuilder(connectionString);
        
// set up JNDI context
Hashtable<String, String> hashtable = new Hashtable<>();
hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + "?amqp.idleTimeout=120000&amqp.traceFrames=true");
hashtable.put("queue.QUEUE", "BasicQueue");
hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
Context context = new InitialContext(hashtable);

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");

// Look up queue
Destination queue = (Destination) context.lookup("QUEUE");
```

#### <a name="configure-producer-and-consumer-destination-queues"></a>Üretici ve Tüketici Hedef Kuyruklarını Yapılandırma
Qpid özellikleri dosyasıjndi sağlayıcısında bir hedefi tanımlamak için kullanılan giriş aşağıdaki biçimdedir:

Üretici için hedef sıra oluşturmak için - 
```java
String queueName = "queueName";
Destination queue = (Destination) queueName;

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
Connection connection - cf.createConnection(csb.getSasKeyName(), csb.getSasKey());

Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);

// Create Producer
MessageProducer producer = session.createProducer(queue);
```

Tüketici için bir hedef sırası oluşturmak için - 
```java
String queueName = "queueName";
Destination queue = (Destination) queueName;

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
Connection connection - cf.createConnection(csb.getSasKeyName(), csb.getSasKey());

Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);

// Create Consumer
MessageConsumer consumer = session.createConsumer(queue);
```

### <a name="write-the-jms-application"></a>JMS uygulamasını yazın
Servis Veri Yol'u ile JMS kullanırken özel API'ler veya seçenekler gerekmez. Ancak, daha sonra karşılanacak birkaç kısıtlama vardır. Herhangi bir JMS uygulamasında olduğu gibi, gerekli olan ilk şey, Bir **ConnectionFactory** ve hedefleri çözmek edebilmek için, JNDI ortamının yapılandırmasıdır.

#### <a name="configure-the-jndi-initialcontext"></a>JNDI İlk Bağlamını Yapılandır
JNDI ortamı, javax.naming.InitialContext sınıfının oluşturucusu içine yapılandırma bilgilerinin bir karma geçirerek yapılandırılır. Karma tabloda gerekli iki öğe, Başlangıç Bağlam Fabrikası'nın sınıf adı ve Sağlayıcı URL'sidir. Aşağıdaki kod, **Servicebus.properties**adlı bir özellik dosyası ile Qpid özellikleri dosya tabanlı JNDI Sağlayıcı kullanmak için JNDI ortamıyapılandırmak için nasıl gösterir.

```java
// set up JNDI context
Hashtable<String, String> hashtable = new Hashtable<>();
hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + \
"?amqp.idleTimeout=120000&amqp.traceFrames=true");
hashtable.put("queue.QUEUE", "BasicQueue");
hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
Context context = new InitialContext(hashtable);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Servis Veri Servisi sıranı kullanan basit bir JMS uygulaması
Aşağıdaki örnek program, JMS TextMessages'ı Sıra'nın JNDI mantıksal adı olan bir Hizmet Veri Servisi kuyruğuna gönderir ve iletileri geri alır.

Azure [Hizmet Veri Yolundan Örnekler JMS Queue hızlı başlat'tan](https://github.com/Azure/azure-service-bus/tree/master/samples/Java/qpid-jms-client/JmsQueueQuickstart) tüm kaynak kodu ve yapılandırma bilgilerine erişebilirsiniz

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
 * This sample demonstrates how to send messages from a JMS Queue producer into
 * an Azure Service Bus Queue, and receive them with a JMS message consumer.
 * JMS Queue. 
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
        
        // set up JNDI context
        Hashtable<String, String> hashtable = new Hashtable<>();
        hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + "?amqp.idleTimeout=120000&amqp.traceFrames=true");
        hashtable.put("queue.QUEUE", "BasicQueue");
        hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
        Context context = new InitialContext(hashtable);
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        
        // Look up queue
        Destination queue = (Destination) context.lookup("QUEUE");

        // we create a scope here so we can use the same set of local variables cleanly 
        // again to show the receive side separately with minimal clutter
        {
            // Create Connection
            Connection connection = cf.createConnection(csb.getSasKeyName(), csb.getSasKey());
            // Create Session, no transaction, client ack
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
            // Create Connection
            Connection connection = cf.createConnection(csb.getSasKeyName(), csb.getSasKey());
            connection.start();
            // Create Session, no transaction, client ack
            Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            // Create consumer
            MessageConsumer consumer = session.createConsumer(queue);
            // create a listener callback to receive the messages
            consumer.setMessageListener(message -> {
                try {
                    // receives message is passed to callback
                    System.out.printf("Received message %d with sq#: %s\n",
                            totalReceived.incrementAndGet(), // increments the tracking counter
                            message.getJMSMessageID());
                    message.acknowledge();
                } catch (Exception e) {
                    logger.error(e);
                }
            });

            // wait on the main thread until all sent messages have been received
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

            // parse connection string from command line
            Options options = new Options();
            options.addOption(new Option("c", true, "Connection string"));
            CommandLineParser clp = new DefaultParser();
            CommandLine cl = clp.parse(options, args);
            if (cl.getOptionValue("c") != null) {
                connectionString = cl.getOptionValue("c");
            }

            // get overrides from the environment
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
Uygulamayı çalıştırmak için Paylaşılan Erişim İlkeleri'nden **Bağlantı Dizesini** geçirin.
Aşağıda Uygulama çalıştırarak formun çıktısı:

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

## <a name="amqp-disposition-and-service-bus-operation-mapping"></a>AMQP disposition ve Servis Veri Servisi operasyon haritalama
Bir AMQP eğiliminin Servis Veri Servisi işlemine nasıl çevrildiği aşağıda açıklanmıştır:

```Output
ACCEPTED = 1; -> Complete()
REJECTED = 2; -> DeadLetter()
RELEASED = 3; (just unlock the message in service bus, will then get redelivered)
MODIFIED_FAILED = 4; -> Abandon() which increases delivery count
MODIFIED_FAILED_UNDELIVERABLE = 5; -> Defer()
```

## <a name="jms-topics-vs-service-bus-topics"></a>JMS Konular vs Hizmet Veri Günü Konular
Java İleti Hizmeti (JMS) API'si aracılığıyla Azure Hizmet Veri Servisi konularını ve aboneliklerini kullanmak temel gönderme ve alma özellikleri sağlar. Hizmet Veri Mes'i konuları JMS Konuları'ndan farklı olsa ve birkaç ayarlama gerekse de, diğer ileti aracılarından jms uyumlu API'lerle uygulamaları taşımada kullanışlı bir seçimdir. 

Azure Hizmet Veri Yolu konuları, iletileri Azure Kaynak Yönetimi arabirimi, Azure komut satırı araçları veya Azure portalı aracılığıyla yönetilen adlandırılmış, paylaşılan ve dayanıklı aboneliklere yönlendirir. Her abonelik, her biri bir filtre koşulu ve SQL filtreleri için de bir meta veri dönüştürme eylemi olan en fazla 2000 seçim kuralına izin verir. Her filtre koşul eşleşmesi, aboneliğe kopyalanacak giriş iletisini seçer.  

Aboneliklerden ileti almak, kuyruklardan ileti almakla aynıdır. Her aboneliğin ilişkili bir ölü harf sırası ve iletileri otomatik olarak başka bir kuyruğa veya konulara iletme yeteneği vardır. 

JMS Konuları, istemcilerin ileti seçicilerle iletileri filtrelemelerine isteğe bağlı olarak izin veren dinamik olarak dayanıklı ve dayanıklı olmayan aboneler oluşturmasına olanak sağlar. Bu paylaşılmayan varlıklar Servis Veri Servisi tarafından desteklenmez. Ancak, Hizmet Veri Kurumu için SQL filtre kuralı sözdizimi JMS tarafından desteklenen ileti seçici sözdizimine benzer. 

JMS Topic yayıncı tarafı, bu örnekte gösterildiği gibi Servis Veri Servisi ile uyumludur, ancak dinamik aboneler değildir. Aşağıdaki topoloji ile ilgili JMS API'leri Servis Veri Servisi ile desteklenmez. 

## <a name="unsupported-features-and-restrictions"></a>Desteklenmeyen özellikler ve kısıtlamalar
Hizmet Veri Servisi ile AMQP 1.0 üzerinden JMS kullanırken aşağıdaki kısıtlamalar vardır:

* **Oturum**başına yalnızca bir **MesajÜretici** veya **MessageConsumer'a** izin verilir. Bir uygulamada birden çok **MessageProducers** veya **MessageConsumers** oluşturmanız gerekiyorsa, her biri için özel bir **Oturum** oluşturun.
* Geçici konu abonelikleri şu anda desteklenmez.
* **MessageSelectors** şu anda desteklenmez.
* Dağıtılmış hareketler desteklenmez (ancak işlem yapılan oturumlar desteklenir).

Ayrıca, Azure Servis Veri Servisi denetim düzlemini veri düzleminden böler ve bu nedenle JMS'nin dinamik topoloji işlevlerinden birkaçını desteklemez:

| Desteklenmeyen yöntem          | Şununla değiştir                                                                             |
|-----------------------------|------------------------------------------------------------------------------------------|
| createDurableSubscriber     | ileti seçiciyi taşımacı bir Konu aboneliği oluşturma                                 |
| createDurableConsumer       | ileti seçiciyi taşımacı bir Konu aboneliği oluşturma                                 |
| createSharedConsumer        | Servis Veri Servisi konuları her zaman paylaşılabilir, yukarıya bakın                                       |
| createSharedDurableConsumer | Servis Veri Servisi konuları her zaman paylaşılabilir, yukarıya bakın                                       |
| createTemporaryTopic        | *autoDeleteOnIdle* bir sona erme süresi ayarlanmış yönetim API/araçları/ portalı üzerinden bir konu oluşturmak |
| createTopic                 | yönetim API/araçları/portalı aracılığıyla bir konu oluşturma                                           |
| Iptal                 | konu yönetimi API'sini/araçlarını/portalını silme                                             |
| createBrowser               | Desteklenme -yen. Hizmet Veri Servisi API'sinin Peek() işlevini kullanma                         |
| createQueue                 | yönetim API/araçları/portalı aracılığıyla kuyruk oluşturma                                           | 
| geçici sıra oluşturma        | *AutoDeleteOnIdle* bir sona erme süresi ne ayarlanmış yönetim API/araçları/portalı üzerinden bir kuyruk oluşturma |
| receiveNoWait               | Service Bus SDK tarafından sağlanan receive() yöntemini kullanın ve çok düşük veya sıfır zaman aşımları belirtin |

## <a name="summary"></a>Özet
Bu nasıl yapılır kılavuzu, popüler JMS API ve AMQP 1.0'ı kullanarak Java'dan Service Bus aracılı mesajlaşma özelliklerinin (kuyruklar ve yayımlama/abone konuları) nasıl kullanılacağını gösterdi.

.NET, C, Python ve PHP gibi diğer dillerdeki Service Bus AMQP 1.0'ı da kullanabilirsiniz. Bu farklı diller kullanılarak oluşturulmuş bileşenler, Hizmet Veri Servisi'ndeki AMQP 1.0 desteğini kullanarak iletileri güvenilir ve tam doğrulukla değiştirebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Hizmet Veri Servisi'nde AMQP 1.0 desteği](service-bus-amqp-overview.md)
* [Hizmet Veri Mestonu .NET API ile AMQP 1.0 nasıl kullanılır?](service-bus-dotnet-advanced-message-queuing.md)
* [Servis Veri Servisi AMQP 1.0 Geliştirici Kılavuzu](service-bus-amqp-dotnet.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Java Geliştirici Merkezi](https://azure.microsoft.com/develop/java/)

