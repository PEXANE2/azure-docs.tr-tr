---
title: Java Message Service API & AMQP kullanın Azure Service Bus
description: Java Ileti hizmeti 'ni (JMS) Azure Service Bus ve Gelişmiş İleti Sıraya Alma Protokolü (AMQP) 1,0 ile kullanma.
ms.topic: article
ms.date: 06/23/2020
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 04d2595951640b7fe878decfeb862863f06c17a2
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86119182"
---
# <a name="use-the-java-message-service-jms-with-azure-service-bus-and-amqp-10"></a>Azure Service Bus ve AMQP 1,0 ile Java Ileti hizmeti 'ni (JMS) kullanma

Azure Service Bus **Gelişmiş ileti sıraya alma Protokolü (AMQP) 1,0** protokolü için destek, verimli bir ikili protokol kullanarak, bir dizi platformda sıraya alma ve yayımlama/Aracılı mesajlaşma özelliklerini de kullanabileceğiniz anlamına gelir. Ayrıca, bir dil, çerçeve ve işletim sistemi karışımı kullanılarak oluşturulan bileşenlerden oluşan uygulamalar oluşturabilirsiniz.

Bu makalede, AMQP protokolü üzerinden popüler **Java Ileti hizmeti (JMS)** API 'Sini kullanarak Java uygulamalarından Azure Service Bus mesajlaşma özelliklerinin (kuyruklar ve yayımlama/abone olma konuları) nasıl kullanılacağı açıklanmaktadır.

## <a name="get-started-with-service-bus"></a>Service Bus’ı kullanmaya başlama
Bu kılavuzda zaten adlı bir kuyruğu içeren bir Service Bus ad alanınız olduğunu varsayılır `basicqueue` . Bunu yapmazsanız, [Azure Portal](https://portal.azure.com)kullanarak [ad alanını ve kuyruğu oluşturabilirsiniz](service-bus-create-namespace-portal.md) . Service Bus ad alanları ve kuyrukları oluşturma hakkında daha fazla bilgi için, bkz. [Service Bus kuyrukları kullanmaya başlama](service-bus-dotnet-get-started-with-queues.md).

> [!NOTE]
> Bölümlenmiş kuyruklar ve konular AMQP 'yi de destekler. Daha fazla bilgi için bkz. bölümlenmiş [mesajlaşma varlıkları](service-bus-partitioning.md) ve [Service Bus bölümlenmiş kuyruklar ve konular için AMQP 1,0 desteği](service-bus-partitioned-queues-and-topics-amqp-overview.md).
> 
>

## <a name="what-jms-features-are-supported"></a>Hangi JMS özellikleri destekleniyor?

Azure Service Bus desteklenen JMS özellikleri aşağıda verilmiştir.

| Özellikler | Azure Service Bus Standart katman-JMS 1,1 | Azure Service Bus Premium katmanı-JMS 2,0 (Önizleme) |
|---|---|---|
| AMQP üzerinden varlıkların otomatik olarak oluşturulması | Desteklenmiyor | **Destek** |
| Kuyruklar | **Destek** | **Destek** |
| Konu başlıkları | **Destek** | **Destek** |
| Geçici kuyruklar | Desteklenmiyor <br/> (Bunun yerine, *oto Deleteonıdle* kümesiyle düzenli bir sıra oluşturun) | **Destek** |
| Geçici konular | Desteklenmiyor | **Destek** |
| İleti seçicileri | Desteklenmiyor | **Destek** |
| Tarayıcıları kuyruğa al | Desteklenmiyor <br/> (Service Bus API 'sinin *göz atma* işlevini kullanın) | **Destek** |
| Paylaşılan dayanıklı abonelikler | **Destek** | **Destek**|
| Paylaşılmayan dayanıklı abonelikler | Desteklenmiyor | **Destek** |
| Paylaşılan, dayanıklı olmayan abonelikler | Desteklenmiyor | **Destek** |
| Paylaşılmayan kalıcı olmayan abonelikler | Desteklenmiyor | **Destek** |
| Dayanıklı abonelikler için abonelik kaldırma | Desteklenmiyor | **Destek** |
| ReceiveNoWait | Desteklenmiyor | **Destek** |
| Dağıtılmış İşlemler | Desteklenmiyor | Desteklenmiyor |
| Dayanıklı Sonlandırus | Desteklenmiyor | Desteklenmiyor |

### <a name="additional-caveats-for-service-bus-standard-tier"></a>Service Bus standart katmanı için ek uyarılar
**Oturum**başına yalnızca bir **Messageproducer** veya **messageconsumer** 'a izin verilir. Bir uygulamada birden çok **MessageProducers** veya **messagetüketiciler** oluşturmanız gerekiyorsa, bunların her biri için özel bir **oturum** oluşturun.

## <a name="downloading-the-java-message-service-jms-client-library"></a>Java Ileti hizmeti (JMS) istemci kitaplığı indiriliyor

Azure Service Bus ile bağlanmak ve AMQP üzerinden Java Ileti hizmeti (JMS) API 'sinden yararlanmak için aşağıdaki kitaplıkların yararlanılabilir olması gerekir. Bunlar, projeniz için tercih edilen bağımlılık Yönetim Aracı kullanılarak derleme yoluna eklenmelidir.

Gerekli olan istemci kitaplığı, hangi fiyatlandırma katmanının kullanıldığına bağlıdır.

### <a name="premium-tier---jms-20-over-amqp-preview"></a>Premium katman-AMQP üzerinden JMS 2,0 (Önizleme)

Azure Service Bus Premium katmanında kullanılabilen tüm Önizleme özelliklerini kullanmak için [Azure-ServiceBus-JMS](https://search.maven.org/artifact/com.microsoft.azure/azure-servicebus-jms) Kitaplığı ' nı kullanın.

### <a name="standard-tier---jms-11-over-amqp"></a>Standart katman-AMQP üzerinden JMS 1,1

Service Bus standart katmanı tarafından desteklenen JMS özelliklerini kullanmak için (bkz. [JMS özellikleri destekleniyor?](service-bus-java-how-to-use-jms-api-amqp.md#what-jms-features-are-supported)) aşağıdaki kitaplıkları kullanın-

* [GERONIMO JMS 1,1 spec](https://search.maven.org/artifact/org.apache.geronimo.specs/geronimo-jms_1.1_spec)
* [Qpid JMS Istemcisi](https://search.maven.org/artifact/org.apache.qpid/qpid-jms-client)

> [!NOTE]
> JMS JAR adları ve sürümleri değişmiş olabilir. Ayrıntılar için bkz. [Qpid JMS-AMQP 1,0](https://qpid.apache.org/maven.html#qpid-jms-amqp-10).
>

## <a name="coding-java-applications"></a>Java uygulamalarını kodlama

Bağımlılıklar içeri aktarıldıktan sonra, Java uygulamaları bir JMS sağlayıcısı tarafından belirsiz şekilde yazılabilir.

Azure Service Bus standart ve Premium, bağımlılıklarda ve destekledikleri JMS özelliklerinin sayısıyla farklı olduğundan, programlama modeli ikisi için biraz farklıdır.

> [!IMPORTANT]
> Aşağıdaki kılavuzda, basit bir uygulama verilen Azure Service Bus bağlanma çalışmaları gösterilmektedir.
>
> Çoğu kurumsal uygulama mimarilerinin bağımlılıkları ve konfigürasyonları yönetmek için özel bir yolu olabileceğinden, nelerin gerekli olduğunu anlamak ve uygulamanıza uygun şekilde uyum sağlamak için aşağıdaki kılavuzu kullanın.
>

### <a name="connecting-to-azure-service-bus-using-jms"></a>JMS kullanarak Azure Service Bus bağlanma

JMS istemcilerini kullanarak Azure Service Bus ile bağlantı kurmak için, **birincil bağlantı dizesi**altındaki [Azure Portal](https://portal.azure.com) ' paylaşılan erişim Ilkeleri ' içinde bulunan **ConnectionString** öğesine ihtiyacınız vardır.


#### <a name="connecting-to-azure-service-bus-premium-over-jms-20-preview"></a>JMS 2,0 (Önizleme) üzerinden Azure Service Bus Premium 'a bağlanma

1. Şunu oluşturun`ServiceBusJmsConnectionFactorySettings`
    ```java
    ServiceBusJmsConnectionFactorySettings connFactorySettings = new ServiceBusJmsConnectionFactorySettings();

    connFactorySettings.setConnectionIdleTimeoutMS(20000);
    ```
2. `ServiceBusJmsConnectionFactory`Uygun şekilde örneğini oluşturun `ServiceBusConnectionString` .
    ```java
    String ServiceBusConnectionString = "<SERVICE_BUS_CONNECTION_STRING_WITH_MANAGE_PERMISSIONS>";
    ConnectionFactory factory = new ServiceBusJmsConnectionFactory(ServiceBusConnectionString, connFactorySettings);
    ```

3. Şunu `ConnectionFactory` oluşturmak için `Connection` öğesini kullanın ve ardından bir`Session` 
    ```java
    Connection connection = factory.createConnection();
    Session session = connection.createSession();
    ```
    veya a `JMSContext` (JMS 2,0 istemcileri için)

    ```java
    JMSContext jmsContext = factory.createContext();
    ```

#### <a name="connecting-to-azure-service-bus-standard-over-jms-11"></a>JMS 1,1 üzerinden Azure Service Bus Standard 'a bağlanma

1. **ServiceBus. Properties**adlı JNDI özellikleri dosyasına Azure Service Bus yapılandırması ekleyin.
    ```properties
    # servicebus.properties - sample JNDI configuration
    
    # Register a ConnectionFactory in JNDI using the form:
    # connectionfactory.[jndi_name] = [ConnectionURL]
    connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
    ```

2. JNDı bağlamını kurma ve ConnectionFactory 'yi yapılandırma
    ```java
    ConnectionStringBuilder csb = new ConnectionStringBuilder(connectionString);

    // set up JNDI context
    Hashtable<String, String> hashtable = new Hashtable<>();
    hashtable.put("connectionfactory.SBCF", "amqps://" + csb.getEndpoint().getHost() + "?amqp.idleTimeout=120000&amqp.traceFrames=true");
    hashtable.put("queue.QUEUE", "BasicQueue");
    hashtable.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.jms.jndi.JmsInitialContextFactory");
    Context context = new InitialContext(hashtable);
    
    ConnectionFactory factory = (ConnectionFactory) context.lookup("SBCF");
    ```
3. `ConnectionFactory`Oluşturmak için `Connection` öğesini kullanın ve ardından bir `Session` .
    ```java
    Connection connection - factory.createConnection(csb.getSasKeyName(), csb.getSasKey());
    Session session = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
    ```

### <a name="write-the-jms-application"></a>JMS uygulamasını yazma

`Session`Veya örneği oluşturulduktan sonra `JMSContext` uygulamanız, hem yönetim hem de veri işlemlerini gerçekleştirmek için tanıdık JMS API 'lerinden yararlanabilir.

Her katmanda hangi API 'Lerin desteklenecek olduğunu görmek için lütfen standart ve Premium katman için [desteklenen JMS özellikleri](service-bus-java-how-to-use-jms-api-amqp.md#what-jms-features-are-supported) listesine bakın.

## <a name="summary"></a>Özet
Bu kılavuz, AMQP 1,0 üzerinden Java Ileti hizmeti (JMS) kullanan Java istemci uygulamalarının Azure Service Bus nasıl etkileşime gireceğini gösterir.

.NET, C, Python ve PHP gibi diğer dillerden AMQP 1,0 Service Bus de kullanabilirsiniz. Bu farklı diller kullanılarak oluşturulan bileşenler, Service Bus içinde AMQP 1,0 desteği kullanılarak güvenilir bir şekilde ve tam uygunlukta iletiler değiş tokuş edebilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Service Bus hakkında daha fazla bilgi ve Java Ileti hizmeti (JMS) varlıkları hakkında daha fazla bilgi için aşağıdaki bağlantıları inceleyin- 
* [Service Bus-kuyruklar, konular ve abonelikler](service-bus-queues-topics-subscriptions.md)
* [Service Bus-Java Ileti hizmeti varlıkları](service-bus-queues-topics-subscriptions.md#java-message-service-jms-20-entities-preview)
* [Azure Service Bus 'da AMQP 1,0 desteği](service-bus-amqp-overview.md)
* [Service Bus AMQP 1,0 Geliştirici Kılavuzu](service-bus-amqp-dotnet.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)

