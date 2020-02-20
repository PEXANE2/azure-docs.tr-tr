---
title: Azure Izleme günlükleri Apache Kafka-Azure HDInsight
description: Azure Izleyici günlüklerini kullanarak Azure HDInsight 'ta Apache Kafka kümeden günlükleri analiz etme hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/17/2020
ms.openlocfilehash: 3f8ff3cbc24f6e3a7e0eccf1b18e01941c9584b9
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77471189"
---
# <a name="analyze-logs-for-apache-kafka-on-hdinsight"></a>HDInsight üzerinde Apache Kafka için günlükleri analiz etme

HDInsight üzerinde Apache Kafka tarafından oluşturulan günlükleri çözümlemek için Azure Izleyici günlüklerini nasıl kullanacağınızı öğrenin.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="logs-location"></a>Günlük konumu

Kümedeki Apache Kafka Günlükler `/var/log/kafka`konumunda bulunur. Yönetilen diskler kullanılıyorsa, Kafka günlükleri küme yaşam döngüleri arasında kaydedilmez veya kalıcı olmaz. Aşağıdaki tabloda kullanılabilir Günlükler gösterilmektedir.

|Günlük |Açıklama |
|---|---|
|Kafka. Out|Kafka işleminin stdout ve stderr 'i. Bu dosyada Kafka başlangıç ve kapalı günlükleri bulacaksınız.|
|Server. log|Ana Kafka sunucu günlüğü. Tüm Kafka Aracısı günlükleri burada sona erdir.|
|Controller. log|Aracı denetleyici olarak hareket ettiğini denetleyici günlükleri.|
|StateChange. log|Aracıların tüm durum değişikliği olayları bu dosyada günlüğe kaydedilir.|
|Kafka-GC. log|Kafka çöp toplama istatistikleri.|

## <a name="enable-azure-monitor-logs-for-apache-kafka"></a>Apache Kafka için Azure Izleyici günlüklerini etkinleştirme

HDInsight için Azure Izleyici günlüklerini etkinleştirme adımları tüm HDInsight kümeleri için aynıdır. Gerekli hizmetleri nasıl oluşturup yapılandıracağınızı anlamak için aşağıdaki bağlantıları kullanın:

1. Log Analytics çalışma alanı oluşturun. Daha fazla bilgi için bkz. [Azure izleyici belgesinde Günlükler](../../azure-monitor/platform/data-platform-logs.md) .

2. HDInsight kümesi üzerinde bir Kafka oluşturun. Daha fazla bilgi için bkz. [HDInsight 'ta Apache Kafka kullanmaya başlama](apache-kafka-get-started.md) belgesi.

3. Kafka kümesini Azure Izleyici günlüklerini kullanacak şekilde yapılandırın. Daha fazla bilgi için bkz. [HDInsight 'ı izlemek Için Azure izleyici günlüklerini kullanma](../hdinsight-hadoop-oms-log-analytics-tutorial.md) .

> [!IMPORTANT]  
> Verilerin Azure Izleyici günlükleri için kullanılabilmesi 20 dakika sürebilir.

## <a name="query-logs"></a>Sorgu günlükleri

1. [Azure Portal](https://portal.azure.com), Log Analytics çalışma alanınızı seçin.

2. Sol taraftaki menüden, **genel**altında **Günlükler**' i seçin. Buradan, Kafka adresinden toplanan verileri arayabilirsiniz. Sorgu penceresinde bir sorgu girin ve sonra **Çalıştır**' ı seçin. Aşağıda bazı örnek aramalar verilmiştir:

* Disk kullanımı:

    ```kusto
    Perf
    | where ObjectName == "Logical Disk" and CounterName == "Free Megabytes" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) 
    | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)
    ```

* CPU kullanımı:

    ```kusto
    Perf 
    | where CounterName == "% Processor Time" and InstanceName == "_Total" and ((Computer startswith_cs "hn" and Computer contains_cs "-") or (Computer startswith_cs "wn" and Computer contains_cs "-")) 
    | summarize AggregatedValue = avg(CounterValue) by Computer, bin(TimeGenerated, 1h)
    ```

* Saniye başına gelen ileti: (`your_kafka_cluster_name`, küme adınızla değiştirin.)

    ```kusto
    metrics_kafka_CL 
    | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-MessagesInPerSec-Count" 
    | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_MessagesInPerSec_Count_value_d) by HostName_s, bin(TimeGenerated, 1h)
    ```

* Saniye başına gelen bayt: (`wn0-kafka` bir çalışan düğümü ana bilgisayar adıyla değiştirin.)

    ```kusto
    metrics_kafka_CL 
    | where HostName_s == "wn0-kafka" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesInPerSec-Count" 
    | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesInPerSec_Count_value_d) by bin(TimeGenerated, 1h)
    ```

* Saniye başına giden bayt: (`your_kafka_cluster_name`, küme adınızla değiştirin.)

    ```kusto
    metrics_kafka_CL 
    | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesOutPerSec-Count" 
    | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesOutPerSec_Count_value_d) by bin(TimeGenerated, 1h)
    ```

    Günlüğe kaydedilen tüm türleri aramak için `*` de girebilirsiniz. Şu anda sorgularda aşağıdaki Günlükler mevcuttur:

    | Günlük türü | Açıklama |
    | ---- | ---- |
    | günlük\_kafkaserver\_CL | Kafka Broker Server. log |
    | günlük\_kafkacontroller\_CL | Kafka Broker Controller. log |
    | ölçümler\_Kafka\_CL | Kafka JMX ölçümleri |

    ![Apache Kafka Log Analytics CPU kullanımı](./media/apache-kafka-log-analytics-operations-management/apache-kafka-cpu-usage.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Izleyici hakkında daha fazla bilgi için bkz. [Azure izleyici 'ye genel bakış](../../log-analytics/log-analytics-get-started.md)ve [HDInsight kümelerini Izlemek için Azure izleyici günlüklerini sorgulama](../hdinsight-hadoop-oms-log-analytics-use-queries.md).

Apache Kafka ile çalışma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [HDInsight kümeleri arasında yansıtma Apache Kafka](apache-kafka-mirroring.md)
* [HDInsight üzerinde Apache Kafka ölçeğini artırma](apache-kafka-scalability.md)
* [Apache Kafka ile Apache Spark akışı (DStreams) kullanma](../hdinsight-apache-spark-with-kafka.md)
* [Apache Kafka ile yapılandırılmış akış Apache Spark kullanma](../hdinsight-apache-kafka-spark-structured-streaming.md)
