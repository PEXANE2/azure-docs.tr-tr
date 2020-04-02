---
title: Apache Kafka için Akka Akışlarını Kullanma - Azure Etkinlik Hub'ları| Microsoft Dokümanlar
description: Bu makalede, Akka Akışları'nın bir Azure etkinlik merkezine nasıl bağlanılacak larına ilişkin bilgiler verilmektedir.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: c53fcc7b4b99e77237ba7ea9219aef4182f2af61
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521833"
---
# <a name="using-akka-streams-with-event-hubs-for-apache-kafka"></a>Apache Kafka için Event Hubs ile Akka Streams’i kullanma
Bu öğretici, akka akışlarını protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan bir etkinlik merkezine nasıl bağlayabileceğinizi gösterir. Kafka için Azure Etkinlik Hub'ları [Apache Kafka sürüm 1.0'ı destekler.](https://kafka.apache.org/10/documentation.html)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projeyi kopyalama
> * Akka Akarsu ları yapımcısı çalıştırın 
> * Akka Streams tüketici çalıştırın

> [!NOTE]
> Bu örnek [GitHub'da](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/akka/java) mevcuttur

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşullara sahip olduğunuzdan emin olun:

* [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md) makalesini okuyun. 
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.8+](https://aka.ms/azure-jdks)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* Maven ikili [arşivini indirin](https://maven.apache.org/download.cgi) ve [kurun](https://maven.apache.org/install.html)
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/downloads)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Herhangi bir Olay Hub'ı hizmetinden göndermek veya almak için olay hub'ları ad alanı gereklidir. Bkz. Ayrıntılı bilgi için [bir etkinlik merkezi oluşturun.](event-hubs-create.md) Daha sonra kullanmak üzere Olay Hub'ları bağlantı dizesini kopyaladığından emin olun.

## <a name="clone-the-example-project"></a>Örnek projeyi kopyalama

Artık bir Olay Hub'ları bağlantı dizeniz olduğuna göre, Kafka deposu için Azure `akka` Etkinlik Hub'larını klonla ve alt klasöre gidin:

```shell
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/akka/java
```

## <a name="run-akka-streams-producer"></a>Akka Akarsu ları yapımcısı çalıştırın

Sağlanan Akka Akışları üretici örneğini kullanarak, Olay Hub'ları hizmetine ileti gönderin.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Etkinlik Hub'ları Kafka bitiş noktası sağlayın

#### <a name="producer-applicationconf"></a>Üretici uygulaması.conf

Doğru `bootstrap.servers` kimlik `sasl.jaas.config` doğrulaması ile üreticiyi Event Hubs Kafka bitiş noktasına yönlendirmek `producer/src/main/resources/application.conf` için bu değerleri ve değerleri güncelleştirin.

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

### <a name="run-producer-from-the-command-line"></a>Üreticiyi komut satırından çalıştırma

Üreticiyi komut satırından çalıştırmak için JAR'ı oluşturun ve Maven'in içinden çalıştırın (veya Maven kullanarak JAR'ı oluşturun, ardından gerekli Kafka JAR(lar) sınıf yoluna ekleyerek Java'da çalıştırın):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestProducer"
```

Üretici, olay hub'ına konu `test`yla ilgili olarak etkinlik göndermeye başlar ve olayları stdout'a yazdırır.

## <a name="run-akka-streams-consumer"></a>Akka Streams tüketici çalıştırın

Sağlanan tüketici örneğini kullanarak, olay merkezinden iletiler alın.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Etkinlik Hub'ları Kafka bitiş noktası sağlayın

#### <a name="consumer-applicationconf"></a>Tüketici uygulaması.conf

Doğru `bootstrap.servers` kimlik `sasl.jaas.config` doğrulaması ile tüketiciyi Olay Hub'ları Kafka bitiş noktasına yönlendirmek `consumer/src/main/resources/application.conf` için değerleri ve değerleri güncelleştirin.

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

### <a name="run-consumer-from-the-command-line"></a>Tüketiciyi komut satırından çalıştırma

Tüketiciyi komut satırından çalıştırmak için JAR'ı oluşturun ve Maven'in içinden çalıştırın (veya Maven kullanarak JAR'ı oluşturun, ardından gerekli Kafka JAR(lar) sınıf yoluna ekleyerek Java'da çalıştırın):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="AkkaTestConsumer"
```

Olay hub'ında olaylar varsa (örneğin, üreticiniz de çalışıyorsa), tüketici `test`konudan olay almaya başlar. 

Akka Akımları hakkında daha detaylı bilgi için [Akka Akımları Kafka Rehberi'ne](https://doc.akka.io/docs/akka-stream-kafka/current/home.html) göz atın.

## <a name="next-steps"></a>Sonraki adımlar
Bu eğitimde, akka akışlarını iletişim istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan olay merkezine nasıl bağlayabileceğinizi öğrendiniz. Kafka için Azure Etkinlik Hub'ları [Apache Kafka sürüm 1.0'ı destekler.](https://kafka.apache.org/10/documentation.html) Bu öğreticinin bir parçası olarak aşağıdaki eylemleri yaptınız: 

> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projeyi kopyalama
> * Akka Akarsu ları yapımcısı çalıştırın 
> * Akka Streams tüketici çalıştırın

Event Hubs ve Kafka için Event Hubs hakkında daha fazla bilgi edinmek için aşağıdaki konuya bakın:  

- [Event Hubs hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)
- [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md)
- [Olay hub'ı oluşturma](event-hubs-create.md)
- [Kafka uygulamalarınızdan Event Hubs'a akış yapma](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [Bir olay hub'ında Kafka aracısı yansıtma](event-hubs-kafka-mirror-maker-tutorial.md)
- [Apache Spark'ı bir olay hub'ına bağlama](event-hubs-kafka-spark-tutorial.md)
- [Apache Flink'i bir olay hub'ına bağlama](event-hubs-kafka-flink-tutorial.md)
- [Kafka Connect'i bir etkinlik merkeziyle bütünleştirin](event-hubs-kafka-connect-tutorial.md)
- [GitHub'ımızdaki örnekleri inceleme](https://github.com/Azure/azure-event-hubs-for-kafka)
