---
title: 'Hızlı başlangıç: Kafka protokolünü kullanarak Azure Event Hubs ile veri akışı'
description: "Hızlı başlangıç: Bu makalede, Kafka Protokolü ve API 'Leri kullanılarak Azure Event Hubs 'da nasıl akış yapılacağı hakkında bilgiler sağlanmaktadır."
services: event-hubs
author: ShubhaVijayasarathy
ms.author: shvija
ms.service: event-hubs
ms.topic: quickstart
ms.custom: seodec18
ms.date: 02/12/2020
ms.openlocfilehash: 18976a29a716a0e5a627747d98edc0d3e1bf71e9
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77587150"
---
# <a name="quickstart-data-streaming-with-event-hubs-using-the-kafka-protocol"></a>Hızlı başlangıç: Kafka protokolünü kullanarak Event Hubs veri akışı
Bu hızlı başlangıçta, protokol istemcilerinizi değiştirmenize veya kendi kümelerinizi çalıştırmanıza gerek kalmadan Kafka etkin Event Hubs’a nasıl akış oluşturulacağı gösterilir. Yalnızca uygulamalarınızdaki bir yapılandırma değişikliğiyle Kafka etkin Event Hubs ile konuşmak için üreticilerinizi ve tüketicilerinizi nasıl kullanacağınızı öğrenirsiniz. Azure Event Hubs [Apache Kafka sürüm 1.0](https://kafka.apache.org/10/documentation.html)’ı destekler.

> [!NOTE]
> Bu örnek [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/java)'da sağlanır

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

* [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md) makalesini okuyun.
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.7+](https://aka.ms/azure-jdks).
* Bir Maven ikili arşivini [indirin](https://maven.apache.org/download.cgi) ve [yükleyin](https://maven.apache.org/install.html).
* [Git](https://www.git-scm.com/)


## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Kafka etkin Event Hubs ad alanı oluşturma
Bir **Standart** katman Event Hubs ad alanı oluşturduğunuzda, ad alanı için Kafka uç noktası otomatik olarak etkinleştirilir. Kafka protokolünü kullanan uygulamalarınızdan olayları Standart katman Event Hubs akışı yapabilirsiniz. **Standart** katman Event Hubs ad alanı oluşturmak için [Azure Portal kullanarak Olay Hub 'ı oluşturma](event-hubs-create.md) bölümünde adım adım yönergeleri izleyin. 

> [!NOTE]
> Kafka için Event Hubs yalnızca **Standart** ve **adanmış** katmanlarda kullanılabilir. **Temel** katman Event Hubs Kafka desteklemez.

## <a name="send-and-receive-messages-with-kafka-in-event-hubs"></a>Event Hubs’da Kafka ile ileti gönderme ve alma

1. [Kafka için Azure Event Hubs deposunu](https://github.com/Azure/azure-event-hubs-for-kafka) kopyalayın.

2. `azure-event-hubs-for-kafka/quickstart/java/producer` sayfasına gidin.

3. `src/main/resources/producer.config` konumundaki üreticinin yapılandırma ayrıntılarını aşağıdaki şekilde güncelleştirin:

    **SSL**

    ```xml
    bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```
    **OAuth**

    ```xml
    bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=OAUTHBEARER
    sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required;
    sasl.login.callback.handler.class=CustomAuthenticateCallbackHandler;
    ```    

    GitHub üzerinde bir örnek işleyici sınıfı Customabuliscallbackhandler kaynak kodunu [buradan](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth/java/appsecret/producer/src/main/java)bulabilirsiniz.
4. Üretici kodu ve akış olaylarını Kafka etkin Event Hubs olarak çalıştırın:
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
    
5. `azure-event-hubs-for-kafka/quickstart/java/consumer` sayfasına gidin.

6. `src/main/resources/consumer.config` konumundaki tüketicinin yapılandırma ayrıntılarını aşağıdaki şekilde güncelleştirin:
   
    **SSL**

    ```xml
    bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```

    **OAuth**

    ```xml
    bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=OAUTHBEARER
    sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required;
    sasl.login.callback.handler.class=CustomAuthenticateCallbackHandler;
    ``` 

    GitHub üzerinde bir örnek işleyici sınıfı Customabuliscallbackhandler kaynak kodunu [buradan](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth/java/appsecret/consumer/src/main/java)bulabilirsiniz.

    Kafka for Event Hubs için tüm OAuth örneklerini [buradan](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth)bulabilirsiniz.
7. Tüketici kodunu çalıştırın ve Kafka istemcilerinizi kullanarak Kafka etkin Event Hubs’tan işleyin:

    ```java
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestConsumer"                                    
    ```

Event Hubs Kafka kümenizin olayları varsa, bu olayları artık tüketiciden almaya başlarsınız.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede protokol istemcilerinizi değiştirmenize veya kendi kümelerinizi çalıştırmanıza gerek kalmadan Kafka etkin Event Hubs’a nasıl akış oluşturacağınızı öğrendiniz. Daha fazla bilgi edinmek için aşağıdaki makalelere ve örneklere bakın:

- [Kafka için Event Hubs hakkında bilgi edinin](event-hubs-for-kafka-ecosystem-overview.md)
- [GitHub üzerinde Kafka için hızlı başlangıç Event Hubs](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart)
- [GitHub üzerinde Kafka için Event Hubs öğreticileri](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials)
- [Kafka 'ten Şirket içindeki olayları, bulutta Kafka etkin Event Hubs akışa](event-hubs-kafka-mirror-maker-tutorial.md) almak Için [mirrormaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) 'ı kullanın.
- [Apache Flink](event-hubs-kafka-flink-tutorial.md) ya da [Akka Streams](event-hubs-kafka-akka-streams-tutorial.md) kullanarak Kafka etkin Event Hubs’a nasıl akış oluşturacağınızı öğrenin.
