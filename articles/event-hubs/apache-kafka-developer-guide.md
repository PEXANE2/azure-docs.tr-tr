---
title: Etkinlik Hub'ları için Apache Kafka geliştirici kılavuzu
description: Bu makalede, Kafka uygulamalarınızı Azure Etkinlik Hub'larıyla nasıl tümleştirişize açık makalelere bağlantılar sağlanmaktadır.
services: event-hubs
author: spelluru
manager: ''
ms.author: spelluru
ms.date: 03/31/2020
ms.topic: article
ms.service: event-hubs
ms.openlocfilehash: d90bf8efab28624672dcedb5bf53d45052dd9123
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81605117"
---
# <a name="apache-kafka-developer-guide-for-azure-event-hubs"></a>Azure Etkinlik Hub'ları için Apache Kafka geliştirici kılavuzu
Bu makalede, Apache Kafka uygulamalarınızı Azure Etkinlik Hub'larıyla nasıl tümleştireceğinizi açıklayan makalelere bağlantılar sağlanmaktadır. 

## <a name="overview"></a>Genel Bakış
Olay Hub'ları, mevcut Kafka tabanlı uygulamalarınız tarafından kendi Kafka kümenizi çalıştırmaya alternatif olarak kullanılabilecek bir Kafka uç noktası sağlar. Event Hubs, Apache Kafka protokolü 1.0 ve sonrası protokolünü destekler ve MirrorMaker da dahil olmak üzere mevcut Kafka uygulamalarınız ile çalışır. Daha fazla bilgi için [Apache Kafka için Etkinlik Hub'larına](event-hubs-for-kafka-ecosystem-overview.md) bakın

## <a name="quickstarts"></a>Hızlı Başlangıçlar
Hızlı başlangıçları GitHub'da ve Kafka için Etkinlik Hub'larına hızla yükseltmenize yardımcı olan bu içerik setinde bulabilirsiniz.

### <a name="quickstarts-in-github"></a>GitHub'da Hızlı Başlangıçlar
**Azure-event-hubs-for-kafka** repo'sunda aşağıdaki hızlı başlangıçlara bakın: 

| İstemci dili/çerçevesi | Açıklama | 
| ------------------------- | ----------- | 
| [.NET](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/dotnet) | <p>Bu hızlı başlangıç, .NET Core 2.0 kullanarak C# ile yazılmış örnek bir üretici ve tüketiciyi kullanarak Bir Event Hubs Kafka bitiş noktasının nasıl oluşturulacağını ve bağlanacağını gösterir.</p><p>Bu örnek, Kafka için Event Hub'ları ile kullanılmak üzere değiştirilen [Confluent'in Apache Kafka .NET istemcisi](https://github.com/confluentinc/confluent-kafka-dotnet)temel alınarak hazırdır.</p> | 
| [Java](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/java) | Bu hızlı başlangıç, Java'da yazılmış örnek bir üretici ve tüketiciyi kullanarak Bir Event Hubs Kafka bitiş noktasının nasıl oluşturulacağını ve bağlanacağını gösterir. |
| [Node.js](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/node) | <p>Bu hızlı başlangıç, Düğüm'de yazılmış örnek bir üretici ve tüketiciyi kullanarak Bir Event Hubs Kafka bitiş noktasının nasıl oluşturulacağını ve bağlanacağını gösterir.</p><p>Bu [örnekdüğüm-rdkafka](https://github.com/Blizzard/node-rdkafka) kitaplığını kullanır. </p>| 
| [Python](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/python) | <p>Bu hızlı başlangıç, python'da yazılmış örnek bir üretici ve tüketiciyi kullanarak bir Event Hubs Kafka bitiş noktasının nasıl oluşturulacağını ve bağlanacağını gösterir.</p><p>Bu örnek, Kafka için Olay Hub'ları ile kullanılmak üzere modifiye edilmiş [Confluent'in Apache Kafka Python istemcisi](https://github.com/confluentinc/confluent-kafka-python)temel alınarak hazırdır.</p>|
| [Git](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/go) | <p>Bu hızlı başlangıç, Go'da yazılmış örnek bir üretici ve tüketiciyi kullanarak bir Event Hubs Kafka bitiş noktasının nasıl oluşturulacağını ve bağlanacağını gösterir.</p><p>Bu örnek, Kafka için Event Hub'ları ile kullanılmak üzere modifiye edilmiş [Confluent'in Apache Kafka Golang istemcisi](https://github.com/confluentinc/confluent-kafka-go)temel alınarak hazırdır.</p>| 
| [Sarama kafka Git](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/go-sarama-client) | Bu hızlı başlangıç, [Sarama Kafka istemci](https://github.com/Shopify/sarama) kitaplığını kullanarak Go'da yazılmış örnek bir üretici ve tüketiciyi kullanarak Bir Event Hubs Kafka bitiş noktasının nasıl oluşturulacağını ve bağlanacağını gösterecektir. |
| [Kafka](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/kafka-cli) | Bu hızlı başlangıç, Apache Kafka dağılımıyla birlikte gelen CLI'yi kullanarak bir Event Hub'ı Kafka bitiş noktasının nasıl oluşturulacağını ve bağlanacağını gösterecektir.| 
| [Kafkacat](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/kafkacat) | kafkacat, hızı ve küçük ayak izi nedeniyle popüler olan librdkafka'ya dayalı JVM olmayan bir komuta hattı tüketicisi ve üreticisidir. Bu hızlı başlatma, örnek bir yapılandırma ve birkaç basit örnek kafkacat komutu içerir. | 
 
### <a name="quickstarts-in-docs"></a>DOCS'da Hızlı Başlangıçlar
Hızlı başlatmaya bakın: Olay Hub'larına nasıl akta aktarılacağa ilişkin adım adım talimatlar sağlayan bu içerik kümesinde [Kafka protokolünü kullanarak Olay Hub'ları ile veri akışı.](event-hubs-quickstart-kafka-enabled-event-hubs.md) Uygulamalarınızda sadece bir yapılandırma değişikliğiyle Etkinlik Hub'larıyla konuşmak için üreticilerinizi ve tüketicilerinizi nasıl kullanacağınızı öğrenirsiniz. 


## <a name="tutorials"></a>Öğreticiler 

### <a name="tutorials-in-github"></a>GitHub'daki Eğitimler
GitHub'da aşağıdaki öğreticilere bakın:

| Öğretici | Açıklama | 
| ------------------------- | ----------- | 
| [Akka](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/akka/java) | Bu öğretici, akka akışlarını protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan Kafka özellikli Etkinlik Hub'larına nasıl bağlayabilirsiniz' ı gösterir. **Java** ve **Scala** programlama dillerini kullanarak iki ayrı öğretici vardır. | 
| [Bağlan](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/connect) | Bu belge, Kafka Connect'i Azure Etkinlik Hub'larıyla tümleştirme ve temel FileStreamSource ve FileStreamSink bağlayıcılarını dağıtma konusunda size yol gösterecektir. Bu bağlayıcılar üretim kullanımı için kullanılmasa da, Azure Etkinlik Hub'larının Kafka aracısı kılığına düştüğü uçlardan uca Kafka Connect Senaryosu'yu gösterirler.| 
| [Filebeat](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/filebeat) | Bu belge, Filebeat'in Kafka çıktısı aracılığıyla Filebeat ve Event Hub'larını entegre etmede size yol gösterecektir. | 
| [Flink](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/flink) | Bu öğretici, protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan Apache Flink'i Kafka özellikli Etkinlik Hub'larına nasıl bağlayacağınızı gösterecektir. | 
| [Akıcı](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/fluentd) | Bu belge, Fluentd için çıktı eklentisini `out_kafka` kullanarak Akıcı ve Etkinlik Hub'larını entegre etmede size yol gösterecektir. |
| [Interop](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/interop) | Bu öğretici, farklı protokoller kullanarak tüketiciler ve üreticiler arasında nasıl olay alışverişi yapılacağını gösterir. |
| [Logstash](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/logstash) | Bu öğretici, Logstash Kafka giriş/çıkış eklentilerini kullanarak Logstash'ı Kafka özellikli Etkinlik Hub'larıyla bütünleştirmede size yol göreceksiniz. | 
| [Aynacı](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/mirror-maker) | Bu öğretici, bir etkinlik merkezinin ve Kafka MirrorMaker'ın, Etkinlik Hub'ları hizmetindeki Kafka giriş akışını yansıtarak mevcut kafka ardışık hattını Azure'a nasıl entegre edebileceğini gösterir. |
| [NiFi](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/nifi) | Bu öğretici, Apache NiFi'yi Olay Hub'ları ad alanına nasıl bağlayacağımı gösterir. | 
| [Oauth](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth) | Hızlı başlangıçlar, Go ve Java programlama dillerinde yazılmış örnek bir üretici ve tüketiciyi kullanarak bir Event Hubs Kafka bitiş noktasını nasıl oluşturup bağlanabileceğinizi gösterir. |
| [Confluent's Schema Kayıt Defteri](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/schema-registry) | Bu öğretici Kafka için Şema Kayıt ve Etkinlik Hub'ları entegre size yol verecektir. | 
| [Spark](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/spark) | Bu öğretici, protokol istemcilerinizi değiştirmeden veya kendi Kafka kümelerinizi çalıştırmadan Spark uygulamanızı bir etkinlik merkezine nasıl bağlayacağınızı gösterecektir. | 

### <a name="tutorials-in-docs"></a>DOCS'deki Eğitimler
Ayrıca, öğreticiye bakın: Bu içerik kümesinde [Akış analitiğini kullanarak Etkinlik Hub'ları etkinlikleri için Apache Kafka'yı işleyip,](event-hubs-kafka-stream-analytics.md) bu içerik kümeye nasıl veri aktarılacağa ve Azure Akış Analitiği ile nasıl işlenirse gösterin.

## <a name="how-to-guides"></a>Nasıl yapılır kılavuzları
Belgelerimizde aşağıdaki Nasıl YapIlen Ler kılavuzuna bakın:

| Makale | Açıklama | 
| ------- | ----------- | 
| [Bir olay hub'ında Kafka aracısı yansıtma](event-hubs-kafka-mirror-maker-tutorial.md) | Kafka MirrorMaker kullanarak bir etkinlik merkezinde bir Kafka broker yansıtmak için nasıl gösterir. |
| [Apache Spark'ı bir olay hub'ına bağlama](event-hubs-kafka-spark-tutorial.md) | Spark uygulamanızı gerçek zamanlı akış için Event Hub'larına bağlamanızda size yol sağlar. |
| [Apache Flink'i bir olay hub'ına bağlama](event-hubs-kafka-flink-tutorial.md) | Apache Flink'i protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan bir etkinlik hub'ına nasıl bağlayabileceğinizi gösterir. |
| [Apache Kafka Connect'i etkinlik merkeziyle tümleştir (Önizleme)](event-hubs-kafka-connect-tutorial.md) | Kafka Connect'i bir etkinlik merkeziyle bütünleştirme ve temel FileStreamSource ve FileStreamSink konektörlerini dağıtma konusunda size yol açar. |
| [Akka Streams’i bir olay hub’ına bağlama](event-hubs-kafka-akka-streams-tutorial.md) | Akka Akışları'nı protokol istemcilerinizi değiştirmeden veya kendi kümelerinizi çalıştırmadan bir etkinlik merkezine nasıl bağlayabileceğinizi gösterir. |
| [Azure Etkinlik Hub'ları ile Apache Kafka için Bahar Önyükleme Başlatıcısını Kullanma](/azure/java/spring-framework/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub) | Apache Kafka'yı Azure Etkinlik Hub'larıyla kullanmak için Bahar Önyükleme Başlatıcısı ile oluşturulan Java tabanlı Bir Bahar Bulut Akışı Bağlayıcısı'nın nasıl yapılandırılabildiğini gösterir. |

## <a name="next-steps"></a>Sonraki adımlar
Hızlı başlatma ve öğreticiler klasörleri altında GitHub repo [azure-event-hubs-for-kafka](https://github.com/Azure/azure-event-hubs-for-kafka) örnekleri gözden geçirin.

Ayrıca, aşağıdaki makalelere bakın:

- [Etkinlik Hub'ları için Apache Kafka sorun giderme kılavuzu](apache-kafka-troubleshooting-guide.md)
- [Sık sorulan sorular - Apache Kafka için Etkinlik Hub'ları](apache-kafka-frequently-asked-questions.md)
- [Etkinlik Hub'ları için Apache Kafka geçiş kılavuzu](apache-kafka-migration-guide.md)



