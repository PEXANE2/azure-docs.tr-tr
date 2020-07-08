---
title: Apache Kafka için Apache flink kullanın-Azure Event Hubs | Microsoft Docs
description: Bu makalede Apache flink 'i bir Azure Olay Hub 'ına bağlama hakkında bilgi sağlanır
ms.topic: how-to
ms.date: 06/23/2020
ms.openlocfilehash: 43cbf99a6ba2c0384ceffc10b01916f6ad22b26a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85320147"
---
# <a name="use-apache-flink-with-azure-event-hubs-for-apache-kafka"></a>Apache Kafka için Azure Event Hubs ile Apache Flink'i kullanma
Bu öğreticide, protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan Apache flink 'i bir olay hub 'ına nasıl bağlayabileceğiniz gösterilmektedir. Azure Event Hubs [Apache Kafka 1,0 sürümünü destekliyor.](https://kafka.apache.org/10/documentation.html).

Apache Kafka kullanmanın önemli avantajlarından biri, bağlanamaların ekosistemidir. Event Hubs, Azure ekosisteminin ölçeklenebilirlik, tutarlılık ve desteğiyle Kafka esnekliğini birleştirir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projeyi kopyalama
> * Flink üreticisi Çalıştır 
> * Flink tüketicisi Çalıştır

> [!NOTE]
> Bu örnek [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/flink) 'da kullanılabilir

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlayabilmeniz için aşağıdaki önkoşullara sahip olduğunuzdan emin olun:

* [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md) makalesini okuyun. 
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.7+](https://aka.ms/azure-jdks)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* Maven ikili Arşivi [indirme](https://maven.apache.org/download.cgi) ve [yükleme](https://maven.apache.org/install.html)
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/downloads)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Herhangi bir Event Hubs hizmetinden göndermek veya almak için bir Event Hubs ad alanı gerekir. Ad alanı ve Olay Hub 'ı oluşturma yönergeleri için bkz. bir [Olay Hub 'ı oluşturma](event-hubs-create.md) . Event Hubs bağlantı dizesini daha sonra kullanmak üzere kopyalamadığınızdan emin olun.

## <a name="clone-the-example-project"></a>Örnek projeyi kopyalama

Artık Event Hubs bağlantı dizesine sahip olduğunuza göre, Azure Event Hubs for Kafka Repository 'yi klonlayın ve `flink` alt klasöre gidin:

```shell
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/flink
```

## <a name="run-flink-producer"></a>Flink üreticisi Çalıştır

Belirtilen flink üreticisi örneğini kullanarak, Event Hubs hizmetine ileti gönderin.

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Event Hubs Kafka uç noktası sağlama

#### <a name="producerconfig"></a>producer.config

`bootstrap.servers` `sasl.jaas.config` Üretici ve değerlerini, `producer/src/main/resources/producer.config` doğru kimlik doğrulamasıyla, üreticiyi Event Hubs Kafka uç noktasına yönlendirmek için güncelleştirin.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=FlinkExampleProducer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-producer-from-the-command-line"></a>Üretici 'yi komut satırından Çalıştır

Producer 'ı komut satırından çalıştırmak için, JAR 'yi oluşturun ve Maven içinden çalıştırın (veya Maven kullanarak JAR 'yi oluşturun ve ardından, gerekli Kafka JAR 'leri, Sınıfyoluna ekleyerek Java 'da çalıştırın):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestProducer"
```

Artık üretici, konu başlığı altında Olay Hub 'ına olay göndermeye `test` ve olayları stdout 'a yazdırmaya başlayacak.

## <a name="run-flink-consumer"></a>Flink tüketicisi Çalıştır

Belirtilen tüketici örneğini kullanarak Olay Hub 'ından ileti alın. 

### <a name="provide-an-event-hubs-kafka-endpoint"></a>Event Hubs Kafka uç noktası sağlama

#### <a name="consumerconfig"></a>consumer.config

`bootstrap.servers`' İ ve `sasl.jaas.config` değerlerini, `consumer/src/main/resources/consumer.config` doğru kimlik doğrulamasıyla tüketicisini Event Hubs Kafka uç noktasına yönlendirmek için güncelleştirin.

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
group.id=FlinkExampleConsumer
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
   username="$ConnectionString" \
   password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

### <a name="run-consumer-from-the-command-line"></a>Komut satırından tüketici çalıştırma

Tüketici 'yi komut satırından çalıştırmak için, JAR 'yi oluşturun ve Maven içinden çalıştırın (veya Maven kullanarak JAR 'yi oluşturun ve ardından, gerekli Kafka JAR 'leri, Sınıfyoluna ekleyerek Java 'da çalıştırın):

```shell
mvn clean package
mvn exec:java -Dexec.mainClass="FlinkTestConsumer"
```

Olay Hub 'ının olayları varsa (örneğin, üreticisi de çalışıyorsa), tüketici artık konudan olay almaya başlar `test` .

Flink 'in Kafka 'e bağlanması hakkında daha ayrıntılı bilgi için [flink 'In Kafka bağlayıcı kılavuzuna](https://ci.apache.org/projects/flink/flink-docs-stable/dev/connectors/kafka.html) göz atın.

## <a name="next-steps"></a>Sonraki adımlar
Kafka için Event Hubs hakkında daha fazla bilgi için aşağıdaki makalelere bakın:  

- [Bir olay hub'ında Kafka aracısı yansıtma](event-hubs-kafka-mirror-maker-tutorial.md)
- [Apache Spark'ı bir olay hub'ına bağlama](event-hubs-kafka-spark-tutorial.md)
- [Kafka Connect 'i bir olay hub 'ı ile tümleştirme](event-hubs-kafka-connect-tutorial.md)
- [GitHub'ımızdaki örnekleri inceleme](https://github.com/Azure/azure-event-hubs-for-kafka)
- [Akka akışlarını bir olay hub 'ına bağlama](event-hubs-kafka-akka-streams-tutorial.md)
- [Azure Event Hubs için Apache Kafka Geliştirici Kılavuzu](apache-kafka-developer-guide.md)