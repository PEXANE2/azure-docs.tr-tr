---
title: Java Ileti hizmeti (JMS) uygulamalarını ActiveMQ 'dan Azure Service Bus 'e geçirme | Microsoft Docs
description: Bu makalede, Azure Service Bus etkileşimde bulunmak için etkin MQ ile etkileşime geçen mevcut JMS uygulamalarının nasıl geçirileceği açıklanır.
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2020
ms.author: aschhab
ms.openlocfilehash: 3da4f693f4cfec47c5456a0c5998f58f5fe02949
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86122411"
---
# <a name="migrate-existing-java-message-service-jms-20-applications-from-active-mq-to-azure-service-bus"></a>Mevcut Java Ileti hizmeti (JMS) 2,0 uygulamalarını Active MQ 'dan Azure Service Bus 'e geçirin

Azure Service Bus, gelişmiş Ileti sıraya alma Protokolü (AMQP) protokolü üzerinden Java Ileti hizmeti (JMS) 2,0 API kullanan Java/J2EE ve yay iş yüklerini destekler.

Bu kılavuzda, Azure Service Bus etkileşimde bulunmak için bir JMS aracısıyla (özellikle Apache ActiveMQ veya Amazon MQ) etkileşim kuran mevcut bir Java Ileti hizmeti (JMS) 2,0 uygulamasını değiştirmek istediğinizde dikkat etmeniz gerekenler açıklanmaktadır.

## <a name="before-you-start"></a>Başlamadan önce

### <a name="differences-between-azure-service-bus-and-apache-activemq"></a>Azure Service Bus ile Apache ActiveMQ arasındaki farklar

Azure Service Bus ve Apache ActiveMQ, istemci uygulamalarının ileti gönderebilmesi ve iletileri alabilmesi için JMS sağlayıcıları olarak çalışan ileti aracılarıdır. Bunlar her ikisi de **kuyruklarla** ve yayımlama-abonelik semantiğinin **konular** ve **abonelikler**ile noktadan noktaya semantiğini etkinleştirir. 

Bu nedenle, iki durumda da bazı farklılıklar vardır.

| Kategori | Etkin MQ | Azure Service Bus |
| --- | --- | --- |
| Uygulama katmanlama | Kümelenmiş mimariden | İki katmanlı <br> (Ağ geçidi + arka uç) |
| Protokol desteği | <ul> <li>AMQP</li> <li> RASTLAYABILIR </li> <li> Opentel </li> </ul> | AMQP |
| Sağlama modu | <ul> <li> IaaS (Şirket içi) </li> <li> Amazon MQ (yönetilen PaaS) </li> | Yönetilen PaaS |
| İleti boyutu | Müşteri yapılandırılabilir | 1 MB (Premium katman) |
| Yüksek Kullanılabilirlik | Müşteri tarafından yönetilen | Platform yönetiliyor |
| Olağanüstü Durum Kurtarma | Müşteri tarafından yönetilen | Platform yönetiliyor | 

### <a name="current-supported-and-unsupported-features"></a>Desteklenen ve desteklenmeyen geçerli özellikler

[!INCLUDE [service-bus-jms-features-list](../../includes/service-bus-jms-feature-list.md)]

### <a name="caveats"></a>Uyarılar

Azure Service Bus iki katmanlı doğası, çeşitli iş sürekliliği özelliklerine sahiptir (yüksek kullanılabilirlik ve olağanüstü durum kurtarma). Ancak, JMS özelliklerinden yararlanarak bazı önemli noktalar vardır.

#### <a name="service-upgrades"></a>Hizmet yükseltmeleri

Service Bus yükseltmeleri ve yeniden başlatmalar söz konusu olduğunda geçici kuyruklar veya konular silinir.

Uygulama geçici kuyruklarda veya konularda veri kaybına karşı duyarlıysa geçici kuyrukları ve konuları kullanmaz ve bunun yerine dayanıklı kuyruklar, konular ve **abonelikler kullanılması önerilir** .

#### <a name="data-migration"></a>Veri geçişi

İstemci uygulamalarınızı Azure Service Bus etkileşimde bulunmak üzere geçirme/değiştirme işleminin bir parçası olarak, ActiveMQ ' de tutulan veriler Service Bus geçirilecek.

ActiveMQ kuyruklarını, konuları ve abonelikleri boşaltmak ve iletileri Service Bus ' kuyruklar, konular ve abonelikler için yeniden oynatmak için özel bir uygulama gerekebilir.

#### <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme

Azure ActiveDirectory tarafından desteklenen rol tabanlı Access Control (RBAC), Azure Service Bus için tercih edilen kimlik doğrulama mekanizmasıdır.

Ancak, Apache QPID JMS tarafından talep tabanlı kimlik doğrulama desteğinin olmamasından dolayı RBAC Şu anda desteklenmediğinden desteklenmemektedir.

Şimdilik kimlik doğrulaması yalnızca SAS anahtarlarıyla desteklenir.

## <a name="pre-migration"></a>Geçiş öncesi

### <a name="version-check"></a>Sürüm denetimi

Aşağıda, JMS uygulamaları ve desteklenen belirli sürümler yazılırken kullanılan bileşenler verilmiştir. 

| Bileşenler | Sürüm |
|---|---|
| Java Ileti hizmeti (JMS) API | 1,1 veya üzeri |
| AMQP Protokolü | 1.0 |

### <a name="ensure-that-amqp-ports-are-open"></a>AMQP bağlantı noktalarının açık olduğundan emin olun

Azure Service Bus AMQP protokolü üzerinden iletişimi destekler. Bu amaçla 5671 (AMQP) ve 443 (TCP) bağlantı noktaları üzerinden iletişimin etkinleştirilmesi gerekir. İstemci uygulamalarının nerede barındırıldığına bağlı olarak, bu bağlantı noktaları üzerinden iletişime izin vermek için bir destek bileti gerekebilir.

> [!IMPORTANT]
> Azure Service Bus **yalnızca** amqp 1,0 protokolünü destekler.

### <a name="set-up-enterprise-configurations-vnet-firewall-private-endpoint-etc"></a>Kurumsal yapılandırmaların (VNET, güvenlik duvarı, Özel uç nokta vb.) kurulumunu yapın

Azure Service Bus, çeşitli kurumsal güvenlik ve yüksek kullanılabilirlik özelliklerine izin verebilir. Bunlarla ilgili daha fazla bilgi edinmek için aşağıdaki belge bağlantılarını izleyin.

  * [Sanal ağ hizmeti uç noktaları](service-bus-service-endpoints.md)
  * [Güvenlik Duvarı](service-bus-ip-filtering.md)
  * [Müşterinin yönettiği anahtarla hizmet tarafı şifreleme (BYOK)](configure-customer-managed-key.md)
  * [Özel uç noktalar](private-link-service.md)
  * [Kimlik doğrulama ve yetkilendirme](service-bus-authentication-and-authorization.md)

### <a name="monitoring-alerts-and-tracing"></a>İzleme, uyarılar ve izleme

Ölçümler her bir Service Bus ad alanı için Azure Izleyici üzerinde yayımlanır ve bu ad alanına ayrılan kaynakların uyarı ve dinamik ölçeklendirilmesi için yararlanılabilir olabilir.

Farklı ölçümler hakkında daha fazla bilgi edinin ve [Azure izleyici 'de Service Bus ölçümler ' de](service-bus-metrics-azure-monitor.md)bunların üzerinde Uyarılar ayarlama.

Ayrıca, [veri işlemleri için istemci tarafı izleme](service-bus-end-to-end-tracing.md) ve [yönetim işlemleri için işletimsel/tanılama günlüğü](service-bus-diagnostic-logs.md) hakkında daha fazla bilgi edinin

### <a name="metrics---newrelic"></a>Ölçümler-Newrelik

Aşağıda, ActiveMQ ' daki hangi ölçümlerin Azure Service Bus ' deki ölçülerle eşlendiğini belirten kullanışlı bir kılavuz yer alır. Aşağıda Newrelik 'ten başvurulur.

  * [ActiveMQ/Amazon MQ Newrelik ölçümleri](https://docs.newrelic.com/docs/integrations/amazon-integrations/aws-integrations-list/aws-mq-integration)
  * [Azure Service Bus Newrelik ölçümleri](https://docs.newrelic.com/docs/integrations/microsoft-azure-integrations/azure-integrations-list/azure-service-bus-monitoring-integration)

> [!NOTE]
> Şu anda Newrelik, ActiveMQ ile sorunsuz bir tümleştirme sahibi değildir, ancak Amazon MQ için kullanılabilir ölçümleri vardır.
> Amazon MQ, ActiveMQ ' den türetildiğinden, aşağıdaki kılavuz Newrelik ölçümlerini AmazonMQ ' dan Azure Service Bus eşler.
>

|Ölçüm gruplandırması| AmazonMQ/etkin MQ ölçümü | Azure Service Bus ölçümü |
|------------|---------------------------|--------------------------|
|'Ndan|`CpuUtilization`|`CPUXNS`|
|'Ndan|`MemoryUsage`|`WSXNS`|
|'Ndan|`CurrentConnectionsCount`|`activeConnections`|
|'Ndan|`EstablishedConnectionsCount`|`activeConnections` + `connectionsClosed`|
|'Ndan|`InactiveDurableTopicSubscribersCount`|Abonelik ölçümlerinden yararlanın|
|'Ndan|`TotalMessageCount`|Kuyruk/konu/abonelik düzeyinden yararlanın`activeMessages`|
|Kuyruk/konu|`EnqueueCount`|`incomingMessages`|
|Kuyruk/konu|`DequeueCount`|`outgoingMessages`|
|Kuyruk|`QueueSize`|`sizeBytes`|



## <a name="migration"></a>Geçiş

Mevcut JMS 2,0 uygulamanızı Azure Service Bus etkileşimde bulunmak üzere geçirmek için aşağıdaki adımların gerçekleştirilmesi gerekir.

### <a name="export-topology-from-activemq-and-create-the-entities-in-azure-service-bus-optional"></a>ActiveMQ 'dan topolojiyi dışarı aktarın ve Azure Service Bus varlıkları oluşturun (isteğe bağlı)

İstemci uygulamalarının Azure Service Bus sorunsuzca bağlanabildiğinden emin olmak için, kuyrukları, konuları ve abonelikleri içeren topolojinin, **Apache ActiveMQ** ' dan **Azure Service Bus**geçirilmesi gerekir.

> [!NOTE]
> Java Ileti hizmeti (JMS) uygulamaları için kuyruklar, konu başlıkları ve aboneliklerin oluşturulması bir çalışma zamanı işlemidir. Çoğu Java Ileti hizmeti (JMS) sağlayıcısı (ileti aracıları) çalışma zamanında *Kuyruklar*, *Konu başlıkları* ve *abonelikler* oluşturma işlevselliği sunar.
>
> Bu nedenle yukarıdaki adım isteğe bağlıdır.
>
> Uygulamanızın çalışma zamanında topoloji oluşturma izinlerine sahip olduğundan emin olmak için lütfen ***SAS ' Yönet '*** izinlerine sahip bağlantı dizesinin kullanıldığından emin olun.

Bunu yapmak için 
  * Topolojiyi dışarı aktarmak için [ActiveMQ komut satırı araçlarından](https://activemq.apache.org/activemq-command-line-tools-reference) yararlanın
  * [Azure Resource Manager şablonu](../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md) kullanarak aynı topolojiyi yeniden oluşturma
  * Azure Resource Manager şablonunu yürütün.


### <a name="import-the-maven-dependency-for-service-bus-jms-implementation"></a>JMS uygulamasının Service Bus Maven bağımlılığını içeri aktarma

Azure Service Bus ile sorunsuz bağlantı sağlamak için ***Azure-ServiceBus-JMS*** paketinin `pom.xml` aşağıda gösterildiği gibi Maven dosyasına bir bağımlılık olarak eklenmesi gerekir.

```xml
<dependencies>
...
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus-jms</artifactId>
    </dependency>
...
</dependencies>
```

### <a name="application-server-configuration-changes"></a>Uygulama sunucusu yapılandırma değişiklikleri

Bu bölüm, etkin MQ ' a bağlanan istemci uygulamalarınızı barındıran uygulama sunucusu için özel bir uygulamadır.

#### <a name="tomcat"></a>Tomcat

Burada, dosyasında gösterildiği gibi etkin MQ 'e özgü yapılandırma ile başlayacağız `/META-INF/context.xml` .

```XML
<Context antiJARLocking="true">
    <Resource
        name="jms/ConnectionFactory"
        auth="Container"
        type="org.apache.activemq.ActiveMQConnectionFactory"
        description="JMS Connection Factory"
        factory="org.apache.activemq.jndi.JNDIReferenceFactory"
        brokerURL="tcp://localhost:61616"
        brokerName="LocalActiveMQBroker"
        useEmbeddedBroker="false"/>

    <Resource name="jms/topic/MyTopic"
        auth="Container"
        type="org.apache.activemq.command.ActiveMQTopic"
        factory="org.apache.activemq.jndi.JNDIReferenceFactory"
        physicalName="MY.TEST.FOO"/>
    <Resource name="jms/queue/MyQueue"
        auth="Container"
        type="org.apache.activemq.command.ActiveMQQueue"
        factory="org.apache.activemq.jndi.JNDIReferenceFactory"
        physicalName="MY.TEST.FOO.QUEUE"/>
</Context>
```

Azure Service Bus işaret etmek için aşağıdaki gibi uyarlanabilen

```xml
<Context antiJARLocking="true">
    <Resource
        name="jms/ConnectionFactory"
        auth="Container"
        type="com.microsoft.azure.servicebus.jms.ServiceBusJmsConnectionFactory"
        description="JMS Connection Factory"
        factory="org.apache.qpid.jms.jndi.JNDIReferenceFactory"
        connectionString="<INSERT YOUR SERVICE BUS CONNECTION STRING HERE>"/>

    <Resource name="jms/topic/MyTopic"
        auth="Container"
        type="org.apache.qpid.jms.JmsTopic"
        factory="org.apache.qpid.jms.jndi.JNDIReferenceFactory"
        physicalName="MY.TEST.FOO"/>
    <Resource name="jms/queue/MyQueue"
        auth="Container"
        type="org.apache.qpid.jms.JmsQueue"
        factory="org.apache.qpid.jms.jndi.JNDIReferenceFactory"
        physicalName="MY.TEST.FOO.QUEUE"/>
</Context>
```

#### <a name="spring-applications"></a>Yay uygulamaları

##### <a name="update-applicationproperties-file"></a>Güncelleştirme `application.properties` dosyası

ActiveMQ 'e bağlanmak için bir Spring Boot uygulaması kullanılıyorsa.

Burada, hedef, ActiveMQ 'e özgü özellikleri dosyadan ***kaldırmaktır*** `application.properties` .

```properties
spring.activemq.broker-url=<ACTIVEMQ BROKER URL>
spring.activemq.user=<ACTIVEMQ USERNAME>
spring.activemq.password=<ACTIVEMQ PASSWORD>
```

Sonra Service Bus belirli özellikleri dosyaya ***ekleyin*** `application.properties` .

```properties
azure.servicebus.connection-string=Endpoint=myEndpoint;SharedAccessKeyName=mySharedAccessKeyName;SharedAccessKey=mySharedAccessKey
```

##### <a name="replace-the-activemqconnectionfactory-with-servicebusjmsconnectionfactory"></a>ActiveMQConnectionFactory 'yi ServiceBusJmsConnectionFactory ile değiştirme

Sonraki adım, ActiveMQConnectionFactory örneğinin ServiceBusJmsConnectionFactory ile değiştirilmesini sağlar.

> [!NOTE] 
> Gerçek kod değişiklikleri uygulamaya özeldir ve bağımlılıkların yönetilmesi, ancak aşağıdaki örnek, ***nelerin*** değiştirilmesi gerektiğine ilişkin rehberlik sağlar.
>

Daha önce ActiveMQConnectionFactory 'nin bir nesnesini aşağıda gösterildiği gibi örneklenebilir.

```java

String BROKER_URL = "<URL of the hosted ActiveMQ broker>";
ConnectionFactory factory = new ActiveMQConnectionFactory(BROKER_URL);

Connection connection = factory.createConnection();
connection.start();

```

Bu, ServiceBusJmsConnectionFactory 'nin bir nesnesinin örneğini oluşturmak için değiştirilir

```java

ServiceBusJmsConnectionFactorySettings settings = new ServiceBusJmsConnectionFactorySettings();
String SERVICE_BUS_CONNECTION_STRING = "<Service Bus Connection string>";

ConnectionFactory factory = new ServiceBusJmsConnectionFactory(SERVICE_BUS_CONNECTION_STRING, settings);

Connection connection = factory.createConnection();
connection.start();

```

## <a name="post-migration"></a>Geçiş sonrası

Artık uygulamayı Azure Service Bus iletileri göndermeye ve almaya başlamak üzere değiştirdiğine göre, bunun beklendiği gibi çalıştığını doğrulamanız gerekir. Bu işlem yapıldıktan sonra, uygulama yığınınızı daha da belirginleştirmek ve modernleştirin için devam edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure Service Bus ile sorunsuz tümleştirme için [Azure Service Bus JMS Için Spring Boot Starter](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-boot-starter-java-app-with-azure-service-bus) 'ten yararlanın.

Service Bus mesajlaşma ve Java Ileti hizmeti (JMS) hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [JMS Service Bus](service-bus-java-how-to-use-jms-api-amqp.md)
* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
