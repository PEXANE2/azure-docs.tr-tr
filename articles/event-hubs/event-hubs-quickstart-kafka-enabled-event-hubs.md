---
title: 'Hızlı başlangıç: Kafka protokolünü kullanarak Azure Event Hubs ile veri akışı'
description: "Hızlı başlangıç: Bu makalede, Kafka Protokolü ve API 'Leri kullanılarak Azure Event Hubs 'da nasıl akış yapılacağı hakkında bilgiler sağlanmaktadır."
ms.topic: quickstart
ms.date: 06/23/2020
ms.openlocfilehash: 612a7515c14b77569b34c91d9b43413353b4d0e8
ms.sourcegitcommit: 51df05f27adb8f3ce67ad11d75cb0ee0b016dc5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90061556"
---
# <a name="quickstart-data-streaming-with-event-hubs-using-the-kafka-protocol"></a>Hızlı başlangıç: Kafka protokolünü kullanarak Event Hubs veri akışı
Bu hızlı başlangıçta, protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan Event Hubs nasıl akışının yapılacağı gösterilmektedir. Uygulamalarınızda yalnızca bir yapılandırma değişikliği ile Event Hubs konuşabilmek için üreticileri ve tüketicilerinizi nasıl kullanacağınızı öğrenirsiniz. 

> [!NOTE]
> Bu örnek [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/java) 'da kullanılabilir

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

* [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md) makalesini okuyun.
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.7 +](https://aka.ms/azure-jdks).
* Bir Maven ikili arşivini [indirin](https://maven.apache.org/download.cgi) ve [yükleyin](https://maven.apache.org/install.html).
* [Git](https://www.git-scm.com/)


## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma
Bir **Standart** katman Event Hubs ad alanı oluşturduğunuzda, ad alanı için Kafka uç noktası otomatik olarak etkinleştirilir. Kafka protokolünü kullanan uygulamalarınızdan olayları Standart katman Event Hubs akışı yapabilirsiniz. **Standart** katman Event Hubs ad alanı oluşturmak için [Azure Portal kullanarak Olay Hub 'ı oluşturma](event-hubs-create.md) bölümünde adım adım yönergeleri izleyin. 

> [!NOTE]
> Kafka için Event Hubs yalnızca **Standart** ve **adanmış** katmanlarda kullanılabilir. **Temel** katman Event Hubs Kafka desteklemez.

## <a name="send-and-receive-messages-with-kafka-in-event-hubs"></a>Event Hubs’da Kafka ile ileti gönderme ve alma

1. [Kafka için Azure Event Hubs deposunu](https://github.com/Azure/azure-event-hubs-for-kafka) kopyalayın.

2. `azure-event-hubs-for-kafka/quickstart/java/producer` sayfasına gidin.

3. `src/main/resources/producer.config` konumundaki üreticinin yapılandırma ayrıntılarını aşağıdaki şekilde güncelleştirin:

    **TLS/SSL:**

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
4. Üretici kodu ve akış olaylarını Event Hubs içine çalıştırın:
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
    
5. `azure-event-hubs-for-kafka/quickstart/java/consumer` sayfasına gidin.

6. `src/main/resources/consumer.config` konumundaki tüketicinin yapılandırma ayrıntılarını aşağıdaki şekilde güncelleştirin:
   
    **TLS/SSL:**

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
7. Kafka istemcilerinizi kullanarak Olay Hub 'ından tüketici kodunu çalıştırın ve olayları işleyin:

    ```java
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestConsumer"                                    
    ```

Event Hubs Kafka kümenizin olayları varsa, bu olayları artık tüketiciden almaya başlarsınız.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan Event Hubs nasıl akışa alınacağını öğrendiniz. Daha fazla bilgi için bkz. [Azure Event Hubs için Apache Kafka Geliştirici Kılavuzu](apache-kafka-developer-guide.md). 
