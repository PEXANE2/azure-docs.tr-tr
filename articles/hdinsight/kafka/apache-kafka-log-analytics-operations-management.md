---
title: Apache Kafka için Azure Monitor günlükleri - Azure HDInsight
description: Azure HDInsight'ta Apache Kafka kümesindeki günlükleri analiz etmek için Azure Monitor günlüklerini nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/17/2020
ms.openlocfilehash: 3f8ff3cbc24f6e3a7e0eccf1b18e01941c9584b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471189"
---
# <a name="analyze-logs-for-apache-kafka-on-hdinsight"></a>HDInsight üzerinde Apache Kafka için günlükleri çözümleme

Apache Kafka tarafından HDInsight'ta oluşturulan günlükleri analiz etmek için Azure Monitor günlüklerini nasıl kullanacağınızı öğrenin.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="logs-location"></a>Günlükler konumu

Kümedeki Apaçi Kafka günlükleri `/var/log/kafka`. Yönetilen diskler kullanılsa da Kafka günlükleri küme yaşam döngüleri boyunca kaydedilmez veya kalıcı değildir. Aşağıdaki tablo, kullanılabilir günlükleri gösterir.

|Günlük |Açıklama |
|---|---|
|kafka.out|Kafka sürecinin stdout ve stderr. Kafka başlangıç ve kapatma günlüklerini bu dosyada bulabilirsiniz.|
|server.log|Ana Kafka sunucu günlüğü. Tüm Kafka broker kayıtları buraya geliyor.|
|controller.log|Broker denetleyici olarak hareket ediyorsa denetleyici günlükleri.|
|statechange.log|Aracılara tüm durum değişikliği olayları bu dosyada günlüğe kaydedilir.|
|kafka-gc.log|Kafka Çöp Toplama istatistikleri.|

## <a name="enable-azure-monitor-logs-for-apache-kafka"></a>Apache Kafka için Azure Monitör günlüklerini etkinleştirme

HDInsight için Azure Monitor günlüklerini etkinleştirme adımları tüm HDInsight kümeleri için aynıdır. Gerekli hizmetlerin nasıl oluşturulup yapılandırılmak için aşağıdaki bağlantıları kullanın:

1. Günlük Analizi çalışma alanı oluşturun. Daha fazla bilgi için [Azure Monitor belgesinde Günlükler'e](../../azure-monitor/platform/data-platform-logs.md) bakın.

2. HDInsight kümesinde bir Kafka oluşturun. Daha fazla bilgi için [HDInsight belgesinde Apache Kafka ile Başlat'a](apache-kafka-get-started.md) bakın.

3. Kafka kümesini Azure Monitor günlüklerini kullanacak şekilde yapılandırın. Daha fazla bilgi için HDInsight belgesini [izlemek için Azure Monitörünü Kullan günlüklerine](../hdinsight-hadoop-oms-log-analytics-tutorial.md) bakın.

> [!IMPORTANT]  
> Azure Monitor günlükleri için verilerin kullanılabilir olması yaklaşık 20 dakika sürebilir.

## <a name="query-logs"></a>Sorgu günlükleri

1. Azure [portalından](https://portal.azure.com)Günlük Analizi çalışma alanınızı seçin.

2. Sol menüden, **Genel**altında , **Günlükleri**seçin. Buradan Kafka'dan toplanan verileri arayabilirsiniz. Sorgu penceresine bir sorgu girin ve ardından **Çalıştır'ı**seçin. Aşağıda bazı örnek aramalar verilmiştir:

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

* Saniyede gelen iletiler: (Küme adınız ile değiştirin.) `your_kafka_cluster_name`

    ```kusto
    metrics_kafka_CL 
    | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-MessagesInPerSec-Count" 
    | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_MessagesInPerSec_Count_value_d) by HostName_s, bin(TimeGenerated, 1h)
    ```

* Saniyede gelen baytlar: (Bir alt düğüm ana bilgisayarı adı ile değiştirin.) `wn0-kafka`

    ```kusto
    metrics_kafka_CL 
    | where HostName_s == "wn0-kafka" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesInPerSec-Count" 
    | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesInPerSec_Count_value_d) by bin(TimeGenerated, 1h)
    ```

* Saniyede giden baytlar: (Küme adınız ile değiştirin.) `your_kafka_cluster_name`

    ```kusto
    metrics_kafka_CL 
    | where ClusterName_s == "your_kafka_cluster_name" and InstanceName_s == "kafka-BrokerTopicMetrics-BytesOutPerSec-Count" 
    | summarize AggregatedValue = avg(kafka_BrokerTopicMetrics_BytesOutPerSec_Count_value_d) by bin(TimeGenerated, 1h)
    ```

    Ayrıca günlüğe `*` kaydedilmiş tüm türleri aramak için girebilirsiniz. Şu anda aşağıdaki günlükleri sorguları için kullanılabilir:

    | Günlük türü | Açıklama |
    | ---- | ---- |
    | log\_kafkaserver\_CL | Kafka broker server.log |
    | log\_kafkacontroller\_CL | Kafka broker controller.log |
    | ölçümler\_kafka\_CL | Kafka JMX ölçümleri |

    ![Apache kafka log analytics işlemci kullanımı](./media/apache-kafka-log-analytics-operations-management/apache-kafka-cpu-usage.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Monitor hakkında daha fazla bilgi için, HDInsight kümelerini izlemek için [Azure Monitor'a genel bakış](../../log-analytics/log-analytics-get-started.md)ve Azure [Monitor'u sorgula günlüklerine](../hdinsight-hadoop-oms-log-analytics-use-queries.md)bakın.

Apache Kafka ile çalışma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [HDInsight kümeleri arasında Ayna Apache Kafka](apache-kafka-mirroring.md)
* [HDInsight'ta Apache Kafka ölçeğini artırın](apache-kafka-scalability.md)
* [Apache Kafka ile Apache Spark akışını (DStreams) kullanma](../hdinsight-apache-spark-with-kafka.md)
* [Apache Kafka ile Apache Spark yapılandırılmış akış kullanın](../hdinsight-apache-kafka-spark-structured-streaming.md)
