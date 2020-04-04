---
title: Apache Spark uygulamanızla bağlantı kurun - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bu makalede, Kafka için Azure Etkinlik Hub'ları ile Apache Spark'ın nasıl kullanılacağı hakkında bilgi verilmektedir.
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: how-to
ms.date: 04/02/2020
ms.author: shvija
ms.openlocfilehash: 1b292f9b894dac7df6b9634ce1bad627c8d24fe6
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632758"
---
# <a name="connect-your-apache-spark-application-with-azure-event-hubs"></a>Apache Spark uygulamanızı Azure Etkinlik Hub'larına bağlayın
Bu öğretici, Spark uygulamanızı gerçek zamanlı akış için Olay Hub'larına bağlamanız için size yol eder. Bu tümleştirme, protokol istemcilerinizi değiştirmek ya da kendi Kafka veya Zookeeper kümelerinizi çalıştırmak zorunda kalmadan akış yapmanıza olanak tanır. Bu öğretici için Apache Spark v2.4+ ve Apache Kafka v2.0+ gerekir.

> [!NOTE]
> Bu örnek [GitHub'da](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/spark/) mevcuttur

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projeyi kopyalama
> * Spark'ı çalıştırma
> * Kafka için Event Hubs'dan okuma
> * Kafka için Event Hubs'a yazma

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce şunlara sahip olduğunuzdan emin olun:
-   Azure aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
-   [Apache Spark v2.4](https://spark.apache.org/downloads.html)
-   [Apache Kafka v2.0]( https://kafka.apache.org/20/documentation.html)
-   [Git](https://www.git-scm.com/downloads)

> [!NOTE]
> Spark-Kafka bağdaştırıcısı Spark v2.4'ten itibaren Kafka v2.0'ı destekleyecek şekilde güncelleştirildi. Spark'ın önceki sürümlerinde, bağdaştırıcı Kafka v0.10 ve üstünü destekliyordu ama özel olarak Kafka v0.10 API'lerine dayanıyordu. Kafka için Event Hubs Kafka v0.10'u desteklemediğinden, Spark'ın v2.4'ten önceki sürümlerinden Spark-Kafka bağdaştırıcıları Kafka için Event Hubs Ekosistemlerinde desteklenmez.


## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma
Herhangi bir Event Hubs hizmetinden göndermek ve almak için Event Hubs ad alanı gereklidir. Bkz. Ad alanı ve olay hub'ı oluşturmak için yönergeler için bir [olay hub'ı oluşturma.](event-hubs-create.md) Daha sonra kullanmak üzere Event Hubs bağlantı dizesini ve tam etki alanı adını (FQDN) alın. Yönergeler için bkz. [Event Hubs bağlantı dizesi alma](event-hubs-get-connection-string.md). 

## <a name="clone-the-example-project"></a>Örnek projeyi kopyalama
Azure Event Hubs deposunu kopyalayın ve `tutorials/spark` alt klasörüne gidin:

```bash
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/spark
```

## <a name="read-from-event-hubs-for-kafka"></a>Kafka için Event Hubs'dan okuma
Birkaç yapılandırma değişikliğiyle, Kafka için Event Hubs'dan okumaya başlayabilirsiniz. **BOOTSTRAP_SERVERS** ve **EH_SASL** öğelerini ad alanınızdan gelen ayrıntılarla güncelleştirin. Bundan sonra aynı Kafka'yla yaptığınız gibi Event Hubs ile akışı başlatabilirsiniz. Örnek kodun tamamı için GitHub'da sparkConsumer.scala dosyasına bakın. 

```scala
//Read from your Event Hub!
val df = spark.readStream
    .format("kafka")
    .option("subscribe", TOPIC)
    .option("kafka.bootstrap.servers", BOOTSTRAP_SERVERS)
    .option("kafka.sasl.mechanism", "PLAIN")
    .option("kafka.security.protocol", "SASL_SSL")
    .option("kafka.sasl.jaas.config", EH_SASL)
    .option("kafka.request.timeout.ms", "60000")
    .option("kafka.session.timeout.ms", "30000")
    .option("kafka.group.id", GROUP_ID)
    .option("failOnDataLoss", "false")
    .load()

//Use dataframe like normal (in this example, write to console)
val df_write = df.writeStream
    .outputMode("append")
    .format("console")
    .start()
```

## <a name="write-to-event-hubs-for-kafka"></a>Kafka için Event Hubs'a yazma
Kafka'ya yazdığınız gibi Event Hub'larına da yazabilirsiniz. Yapılandırmanızı güncelleştirip **BOOTSTRAP_SERVERS** ve **EH_SASL** öğelerini Event Hubs ad alanınızdan gelen bilgilerle değiştirmeyi unutmayın.  Örnek kodun tamamı için GitHub'da sparkProducer.scala dosyasına bakın. 

```scala
df = /**Dataframe**/

//Write to your Event Hub!
df.writeStream
    .format("kafka")
    .option("topic", TOPIC)
    .option("kafka.bootstrap.servers", BOOTSTRAP_SERVERS)
    .option("kafka.sasl.mechanism", "PLAIN")
    .option("kafka.security.protocol", "SASL_SSL")
    .option("kafka.sasl.jaas.config", EH_SASL)
    .option("checkpointLocation", "./checkpoint")
    .start()
```



## <a name="next-steps"></a>Sonraki adımlar
Kafka için Etkinlik Hub'ları ve Etkinlik Hub'ları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:  

- [Bir olay hub'ında Kafka aracısı yansıtma](event-hubs-kafka-mirror-maker-tutorial.md)
- [Apache Flink'i bir olay hub'ına bağlama](event-hubs-kafka-flink-tutorial.md)
- [Kafka Connect'i bir etkinlik merkeziyle bütünleştirin](event-hubs-kafka-connect-tutorial.md)
- [GitHub'ımızdaki örnekleri inceleme](https://github.com/Azure/azure-event-hubs-for-kafka)
- [Akka Akışlarını bir etkinlik merkezine bağlayın](event-hubs-kafka-akka-streams-tutorial.md)
- [Azure Etkinlik Hub'ları için Apache Kafka geliştirici kılavuzu](apache-kafka-developer-guide.md)

