---
title: "Quickstart: Kafka protokolünü kullanarak Azure Etkinlik Hub'ları ile veri akışı"
description: "Hızlı başlangıç: Bu makalede, Kafka protokolü ve API'leri kullanarak Azure Etkinlik Hub'larına nasıl akış sağlanır hakkında bilgi verilmektedir."
services: event-hubs
author: ShubhaVijayasarathy
ms.author: shvija
ms.service: event-hubs
ms.topic: quickstart
ms.custom: seodec18
ms.date: 02/12/2020
ms.openlocfilehash: 89d2c16e3f54d3170b4f0d23590294be13dd8caa
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632743"
---
# <a name="quickstart-data-streaming-with-event-hubs-using-the-kafka-protocol"></a>Quickstart: Kafka protokolünü kullanarak Olay Hub'ları ile veri akışı
Bu hızlı başlangıç, protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan Olay Hub'larına nasıl akış yapılacağını gösterir. Uygulamalarınızda sadece bir yapılandırma değişikliğiyle Etkinlik Hub'larıyla konuşmak için üreticilerinizi ve tüketicilerinizi nasıl kullanacağınızı öğrenirsiniz. Azure Event Hubs [Apache Kafka sürüm 1.0](https://kafka.apache.org/10/documentation.html)’ı destekler.

> [!NOTE]
> Bu örnek [GitHub'da](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/java) mevcuttur

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdaki önkoşulların karşılandığından emin olun:

* [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md) makalesini okuyun.
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Geliştirme Kiti (JDK) 1.7+](https://aka.ms/azure-jdks).
* Bir Maven ikili arşivini [indirin](https://maven.apache.org/download.cgi) ve [yükleyin](https://maven.apache.org/install.html).
* [Git](https://www.git-scm.com/)


## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma
**Standart** bir katman Olay Hub'ları ad alanı oluşturduğunuzda, ad alanı için Kafka bitiş noktası otomatik olarak etkinleştirilir. Kafka protokolünü kullanan uygulamalarınızdan olayları standart katman Etkinlik Hub'larına aktarabilirsiniz. **Standart** bir katman Olay Hub'ları ad alanı oluşturmak için [Azure portalını kullanarak etkinlik hub'ı oluştur'da](event-hubs-create.md) adım adım yönergeleri izleyin. 

> [!NOTE]
> Kafka için Etkinlik Hub'ları yalnızca **standart** ve **özel** katmanlarda kullanılabilir. **Temel** katman Etkinlik Hub'larında Kafka'yı desteklemez.

## <a name="send-and-receive-messages-with-kafka-in-event-hubs"></a>Event Hubs’da Kafka ile ileti gönderme ve alma

1. [Kafka için Azure Event Hubs deposunu](https://github.com/Azure/azure-event-hubs-for-kafka) kopyalayın.

2. `azure-event-hubs-for-kafka/quickstart/java/producer` sayfasına gidin.

3. `src/main/resources/producer.config` konumundaki üreticinin yapılandırma ayrıntılarını aşağıdaki şekilde güncelleştirin:

    **Ssl:**

    ```xml
    bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```
    **Oauth:**

    ```xml
    bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=OAUTHBEARER
    sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required;
    sasl.login.callback.handler.class=CustomAuthenticateCallbackHandler;
    ```    

    Burada [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth/java/appsecret/producer/src/main/java)örnek işleyici sınıf CustomAuthenticateCallbackHandler için kaynak kodunu bulabilirsiniz.
4. Üretici kodunu çalıştırın ve olayları Olay Hub'larına aktarın:
   
    ```shell
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestProducer"                                    
    ```
    
5. `azure-event-hubs-for-kafka/quickstart/java/consumer` sayfasına gidin.

6. `src/main/resources/consumer.config` konumundaki tüketicinin yapılandırma ayrıntılarını aşağıdaki şekilde güncelleştirin:
   
    **Ssl:**

    ```xml
    bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=PLAIN
    sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
    ```

    **Oauth:**

    ```xml
    bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
    security.protocol=SASL_SSL
    sasl.mechanism=OAUTHBEARER
    sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required;
    sasl.login.callback.handler.class=CustomAuthenticateCallbackHandler;
    ``` 

    Burada [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth/java/appsecret/consumer/src/main/java)örnek işleyici sınıf CustomAuthenticateCallbackHandler için kaynak kodunu bulabilirsiniz.

    Kafka için Etkinlik Merkezleri için tüm OAuth örneklerini [burada](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth)bulabilirsiniz.
7. Kafka müşterilerinizi kullanarak etkinlik merkezinden tüketici kodu ve işlem olaylarını çalıştırın:

    ```java
    mvn clean package
    mvn exec:java -Dexec.mainClass="TestConsumer"                                    
    ```

Event Hubs Kafka kümenizin olayları varsa, bu olayları artık tüketiciden almaya başlarsınız.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan Olay Hub'larına nasıl akış yapılacağını öğrendiniz. Daha fazla bilgi için [Azure Etkinlik Hub'ları için Apache Kafka geliştirici kılavuzuna](apache-kafka-developer-guide.md)bakın. 
