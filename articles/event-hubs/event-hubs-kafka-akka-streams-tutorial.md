---
title: Apache Kafka için Akka akışlarını kullanma-Azure Event Hubs | Microsoft Docs
description: Bu makalede, Azure Olay Hub 'ına Akka akışlarını bağlama hakkında bilgi sağlanır.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: how-to
ms.date: 04/02/2020
ms.author: shvija
ms.openlocfilehash: 0b96f1448fd223aae2dde77c5c05a8c9bd74ee9b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80632853"
---
# <a name="using-akka-streams-with-event-hubs-for-apache-kafka"></a>Apache Kafka için Event Hubs ile Akka Streams’i kullanma
Bu öğreticide, protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan Akka akışlarını bir olay hub 'ına nasıl bağlayabilmeniz gösterilmektedir. Kafka için Azure Event Hubs [Apache Kafka 1,0 sürümünü destekler.](https://kafka.apache.org/10/documentation.html)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projeyi kopyalama
> * Akka Streams üreticisi çalıştırma 
> * Akka akışları tüketicisini Çalıştır

> [!NOTE]
> Bu örnek [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/akka/java) 'da kullanılabilir

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlayabilmeniz için aşağıdaki önkoşullara sahip olduğunuzdan emin olun:

* [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md) makalesini okuyun. 
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.8+](https://aka.ms/azure-jdks)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* Maven ikili Arşivi [indirme](https://maven.apache.org/download.cgi) ve [yükleme](https://maven.apache.org/install.html)
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/downloads)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Herhangi bir Event Hubs hizmetinden göndermek veya almak için bir Event Hubs ad alanı gerekir. Ayrıntılı bilgi için bkz. [Olay Hub 'ı oluşturma](event-hubs-create.md) . Event Hubs bağlantı dizesini daha sonra kullanmak üzere kopyalamadığınızdan emin olun.

## <a name="clone-the-example-project"></a>Örnek projeyi kopyalama

Artık Event Hubs bir bağlantı dizesine sahip olduğunuza göre, Azure Event Hubs for Kafka Repository 'yi klonlayın ve `akka` alt klasöre gidin:

```shell
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/akka/java
```

## <a name="run-akka-streams-producer"></a>Akka Streams üreticisi çalıştırma

Sunulan Akka Streams üreticisi örneğini kullanarak, Event Hubs hizmetine ileti gönderin.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Event Hubs Kafka uç noktası sağlama

#### <a name="producer-applicationconf"></a>Producer Application. conf

Üretici ve `bootstrap.servers` `sasl.jaas.config` değerlerini, doğru `producer/src/main/resources/application.conf` kimlik doğrulamasıyla, üreticiyi Event Hubs Kafka uç noktasına yönlendirmek için güncelleştirin.

```xml
akka.kafka.producer {
    #Akka Kafka producer properties can be defined here


    # Properties defined by org.apache.kafka.clients.producer.ProducerConfig
    # can be defined in this configuration section.
    kafka-clients {
        bootstrap.servers="{YOUR.EVENTHUBS.FQDN}:9093"
        sasl.mechanism=PLAIN
        security.protocol=SASL_SSL
        sasl.jaas.config="org.apache.kafka.common.security.plain.PlainLoginModule required username=\"$ConnectionString\" password=\"{YOUR.EVENTHUBS.CONNECTION.STRING}\";"
    }
}
```

### <a name="run-producer-from-the-command-line"></a>Üretici 'yi komut satırından Çalıştır

Producer 'ı komut satırından çalıştırmak için, JAR 'yi oluşturun ve Maven içinden çalıştırın (veya Maven kullanarak JAR 'yi oluşturun ve ardından, gerekli Kafka JAR 'leri, Sınıfyoluna ekleyerek Java 'da çalıştırın):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestProducer"
```

Üretici, konuya `test`Olay Hub 'ına olay göndermeye başlar ve olayları stdout 'a yazdırır.

## <a name="run-akka-streams-consumer"></a>Akka akışları tüketicisini Çalıştır

Belirtilen tüketici örneğini kullanarak Olay Hub 'ından ileti alın.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Event Hubs Kafka uç noktası sağlama

#### <a name="consumer-applicationconf"></a>Tüketici uygulaması. conf

' `consumer/src/main/resources/application.conf` İ `bootstrap.servers` ve `sasl.jaas.config` değerlerini, doğru kimlik doğrulamasıyla tüketicisini Event Hubs Kafka uç noktasına yönlendirmek için güncelleştirin.

```xml
akka.kafka.consumer {
    #Akka Kafka consumer properties defined here
    wakeup-timeout=60s

    # Properties defined by org.apache.kafka.clients.consumer.ConsumerConfig
    # defined in this configuration section.
    kafka-clients {
       request.timeout.ms=60000
       group.id=akka-example-consumer

       bootstrap.servers="{YOUR.EVENTHUBS.FQDN}:9093"
       sasl.mechanism=PLAIN
       security.protocol=SASL_SSL
       sasl.jaas.config="org.apache.kafka.common.security.plain.PlainLoginModule required username=\"$ConnectionString\" password=\"{YOUR.EVENTHUBS.CONNECTION.STRING}\";"
    }
}
```

### <a name="run-consumer-from-the-command-line"></a>Komut satırından tüketici çalıştırma

Tüketici 'yi komut satırından çalıştırmak için, JAR 'yi oluşturun ve Maven içinden çalıştırın (veya Maven kullanarak JAR 'yi oluşturun ve ardından, gerekli Kafka JAR 'leri, Sınıfyoluna ekleyerek Java 'da çalıştırın):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestConsumer"
```

Olay Hub 'ının olayları varsa (örneğin, üreticisi de çalışıyorsa), tüketici konusunun `test`olayları almaya başlar. 

Akka akışları hakkında daha ayrıntılı bilgi için [Akka Streams Kafka kılavuzuna](https://doc.akka.io/docs/akka-stream-kafka/current/home.html) göz atın.

## <a name="next-steps"></a>Sonraki adımlar
Kafka için Event Hubs hakkında daha fazla bilgi için aşağıdaki makalelere bakın:  

- [Bir olay hub'ında Kafka aracısı yansıtma](event-hubs-kafka-mirror-maker-tutorial.md)
- [Apache Spark'ı bir olay hub'ına bağlama](event-hubs-kafka-spark-tutorial.md)
- [Apache Flink'i bir olay hub'ına bağlama](event-hubs-kafka-flink-tutorial.md)
- [Kafka Connect 'i bir olay hub 'ı ile tümleştirme](event-hubs-kafka-connect-tutorial.md)
- [GitHub'ımızdaki örnekleri inceleme](https://github.com/Azure/azure-event-hubs-for-kafka)
- [Azure Event Hubs için Apache Kafka Geliştirici Kılavuzu](apache-kafka-developer-guide.md)
