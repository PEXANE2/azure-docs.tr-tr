---
title: Apache Spark ile tümleştirme - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bu makalede, Olay Hub'ları ile Yapılandırılmış Akış etkinleştirmek için Apache Spark ile nasıl tümleştirilen gösterir.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 4c4fd74e9123e1310be297a15090433d365d24cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76311691"
---
# <a name="integrating-apache-spark-with-azure-event-hubs"></a>Apache Spark'ı Azure Etkinlik Hub'larıyla tümleştirme

Azure Etkinlik Hub'ları, dağıtılmış akış uygulamaları oluşturmayı etkinleştirmek için [Apache Spark](https://spark.apache.org/) ile sorunsuz bir şekilde tümleşir. Bu tümleştirme [Spark Core, Spark](https://spark.apache.org/docs/latest/rdd-programming-guide.html) [Streaming](https://spark.apache.org/docs/latest/streaming-programming-guide.html)ve [Yapılandırılmış Akış](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html)destekler. Apache Spark için Olay Hub'ları bağlayıcısı [GitHub'da](https://github.com/Azure/azure-event-hubs-spark)kullanılabilir. Bu kütüphane, [Maven Merkez Deposu'ndan](https://search.maven.org/#artifactdetails%7Ccom.microsoft.azure%7Cazure-eventhubs-spark_2.11%7C2.1.6%7C)Maven projelerinde de kullanılabilir.

Bu makalede, [Azure Veri Tuğlaları'nda](https://azure.microsoft.com/services/databricks/)sürekli bir uygulamanın nasıl oluşturulacağı anlatılıyor. Bu makalede Azure Databricks kullanılırken, Spark kümeleri [HDInsight](../hdinsight/spark/apache-spark-overview.md)ile de kullanılabilir.

Bu makaledeki örnekte iki Scala dizüstü bilgisayar kullanır: biri olay hub'ından olayları akışa aktarmak, diğeri de olayları ona geri göndermek için.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Hesabınız yoksa, ücretsiz [bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* Olay Hub'ları örneği. Eğer yoksa, bir [oluşturun.](event-hubs-create.md)
* [Azure Databricks](https://azure.microsoft.com/services/databricks/) örneği. Eğer yoksa, bir [oluşturun.](../azure-databricks/quickstart-create-databricks-workspace-portal.md)
* [Maven koordinatlarını kullanarak bir kütüphane oluşturun:](https://docs.databricks.com/user-guide/libraries.html#upload-a-maven-package-or-spark-package) `com.microsoft.azure:azure‐eventhubs‐spark_2.11:2.3.1`.

Aşağıdaki kodu kullanarak olay merkezinizden olayları akışı:

```scala
import org.apache.spark.eventhubs._

// To connect to an event hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build 
val ehConf = EventHubsConf(connectionString)
  .setStartingPosition(EventPosition.fromEndOfStream)

// Create a stream that reads data from the specified Event Hub.
val reader = spark.readStream
  .format("eventhubs")
  .options(ehConf.toMap)
  .load()
val eventhubs = reader.select($"body" cast "string")

eventhubs.writeStream
  .format("console")
  .outputMode("append")
  .start()
  .awaitTermination()
```
Aşağıdaki kod, Olayları Spark toplu API'leri ile olay merkezinize gönderir. Olayları olay merkezine göndermek için bir akış sorgusu da yazabilirsiniz:

```scala
import org.apache.spark.eventhubs._
import org.apache.spark.sql.functions._

// To connect to an event hub, EntityPath is required as part of the connection string.
// Here, we assume that the connection string from the Azure portal does not have the EntityPath part.
val connectionString = ConnectionStringBuilder("{EVENT HUB CONNECTION STRING FROM AZURE PORTAL}")
  .setEventHubName("{EVENT HUB NAME}")
  .build
val ehConf = EventHubsConf(connectionString)

// Create random strings as the body of the message.
val bodyColumn = concat(lit("random nunmber: "), rand()).as("body")

// Write 200 rows to the specified event hub.
val df = spark.range(200).select(bodyColumn)
df.write
  .format("eventhubs")
  .options(ehConf.toMap)
  .save() 
```

## <a name="next-steps"></a>Sonraki adımlar

Artık Apache Spark için Olay Hub'ları Bağlayıcısı'nı kullanarak ölçeklenebilir, hataya dayanıklı bir akış nasıl ayarlayabileceğinizi biliyorsunuz. Bu bağlantıları izleyerek Yapılandırılmış Akış ve Kıvılcım Akışı ile Etkinlik Hub'larını kullanma hakkında daha fazla bilgi edinin:

* [Yapılandırılmış Akış + Azure Olay Hub'ları Tümleştirme Kılavuzu](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/structured-streaming-eventhubs-integration.md)
* [Kıvılcım Akışı + Olay Hub'ları Tümleştirme Kılavuzu](https://github.com/Azure/azure-event-hubs-spark/blob/master/docs/spark-streaming-eventhubs-integration.md)
