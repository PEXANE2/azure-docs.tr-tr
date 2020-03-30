---
title: Apache Kafka için Apache Flink'i kullanma - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bu makalede, Apache Flink'in Apache Kafka etkin leştirilmiş bir Azure etkinlik hub'ına nasıl bağlanılabildiği hakkında bilgi
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: db877279bcfa7e132841e342cfc25b66bb3ec384
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80283608"
---
# <a name="use-apache-flink-with-azure-event-hubs-for-apache-kafka"></a>Apache Kafka için Azure Event Hubs ile Apache Flink'i kullanma
Bu öğretici, protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan Apache Flink'i bir etkinlik merkezine nasıl bağlayabileceğinizi gösterir. Azure Etkinlik Hub'ları [Apache Kafka sürüm 1.0'ı destekler.](https://kafka.apache.org/10/documentation.html)

Apache Kafka'yı kullanmanın en önemli avantajlarından biri de bağlanabileceği çerçeveekosistemidir. Etkinlik Hub'ları Kafka'nın esnekliğini Azure ekosisteminin ölçeklenebilirliği, tutarlılığı ve desteğiyle birleştirir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projeyi kopyalama
> * Flink yapımcısıçalıştırın 
> * Flink tüketici çalıştırın

> [!NOTE]
> Bu örnek [GitHub'da](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/flink) mevcuttur

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşullara sahip olduğunuzdan emin olun:

* [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md) makalesini okuyun. 
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.7+](https://aka.ms/azure-jdks)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* Maven ikili [arşivini indirin](https://maven.apache.org/download.cgi) ve [kurun](https://maven.apache.org/install.html)
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/downloads)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Herhangi bir Olay Hub'ı hizmetinden göndermek veya almak için olay hub'ları ad alanı gereklidir. Etkinlik Hub'ları Kafka bitiş noktası alma hakkında bilgi almak için [Kafka Etkin Etkinlik Hub'ları Oluştur'a](event-hubs-create.md) bakın. Daha sonra kullanmak üzere Olay Hub'ları bağlantı dizesini kopyaladığından emin olun.

## <a name="clone-the-example-project"></a>Örnek projeyi kopyalama

Artık Olay Hub'ları bağlantı dizesine sahip olduğunuza göre, Kafka deposu için `flink` Azure Etkinlik Hub'larını klonla ve alt klasöre gidin:

```shell
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/flink
```

## <a name="run-flink-producer"></a>Flink yapımcısıçalıştırın

Sağlanan Flink üretici örneğini kullanarak, Olay Hub'ları hizmetine ileti gönderin.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Etkinlik Hub'ları Kafka bitiş noktası sağlayın

#### <a name="producerconfig"></a>producer.config

Doğru `bootstrap.servers` kimlik `sasl.jaas.config` doğrulaması ile üreticiyi Event Hubs Kafka bitiş noktasına yönlendirmek `producer/src/main/resources/producer.config` için bu değerleri ve değerleri güncelleştirin.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=FlinkExampleProducer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-producer-from-the-command-line"></a>Üreticiyi komut satırından çalıştırma

Üreticiyi komut satırından çalıştırmak için JAR'ı oluşturun ve Maven'in içinden çalıştırın (veya Maven kullanarak JAR'ı oluşturun, ardından gerekli Kafka JAR(lar) sınıf yoluna ekleyerek Java'da çalıştırın):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestProducer"
```

Yapımcı şimdi kafka etkin Event Hub konu `test` ve stdout için olayları yazdırmaya olaylar göndermeye başlayacaktır.

## <a name="run-flink-consumer"></a>Flink tüketici çalıştırın

Sağlanan tüketici örneğini kullanarak Kafka etkin Etkinlik Hub'larından iletiler alın.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Etkinlik Hub'ları Kafka bitiş noktası sağlayın

#### <a name="consumerconfig"></a>consumer.config

Doğru `bootstrap.servers` kimlik `sasl.jaas.config` doğrulaması ile tüketiciyi Olay Hub'ları Kafka bitiş noktasına yönlendirmek `consumer/src/main/resources/consumer.config` için değerleri ve değerleri güncelleştirin.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
group.id=FlinkExampleConsumer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-consumer-from-the-command-line"></a>Tüketiciyi komut satırından çalıştırma

Tüketiciyi komut satırından çalıştırmak için JAR'ı oluşturun ve Maven'in içinden çalıştırın (veya Maven kullanarak JAR'ı oluşturun, ardından gerekli Kafka JAR(lar) sınıf yoluna ekleyerek Java'da çalıştırın):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestConsumer"
```

Olay hub'ında olaylar varsa (örneğin, üreticiniz de çalışıyorsa), tüketici artık `test`konudan olaylar almaya başlar.

Flink'i Kafka'ya bağlama hakkında daha ayrıntılı bilgi için [Flink'in Kafka Konektör Kılavuzu'na](https://ci.apache.org/projects/flink/flink-docs-stable/dev/connectors/kafka.html) göz atın.

## <a name="next-steps"></a>Sonraki adımlar
Bu eğitimde, protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan Apache Flink'i Olay Hub'larına bağlamayı öğrendiniz. Bu öğreticinin bir parçası olarak aşağıdaki adımları gerçekleştirdiniz: 

> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projeyi kopyalama
> * Flink yapımcısıçalıştırın 
> * Flink tüketici çalıştırın

Event Hubs ve Kafka için Event Hubs hakkında daha fazla bilgi edinmek için aşağıdaki konuya bakın:  

- [Event Hubs hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)
- [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md)
- [Kafka özellikli Event Hubs oluşturma](event-hubs-create.md)
- [Kafka uygulamalarınızdan Event Hubs'a akış yapma](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [Bir olay hub'ında Kafka aracısı yansıtma](event-hubs-kafka-mirror-maker-tutorial.md)
- [Apache Spark'ı bir olay hub'ına bağlama](event-hubs-kafka-spark-tutorial.md)
- [Kafka Connect'i bir etkinlik merkeziyle bütünleştirin](event-hubs-kafka-connect-tutorial.md)
- [Akka Akışlarını bir etkinlik merkezine bağlayın](event-hubs-kafka-akka-streams-tutorial.md)
- [GitHub'ımızdaki örnekleri inceleme](https://github.com/Azure/azure-event-hubs-for-kafka)
