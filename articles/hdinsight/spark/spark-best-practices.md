---
title: Azure HDInsight'ta Apache Spark en iyi uygulamaları
description: Azure HDInsight'ta Apache Spark'ı kullanmak için en iyi uygulamaları öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 09/18/2019
ms.openlocfilehash: 20033e52e862f086d1491c06d38cdf4f2c57ba8d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71106131"
---
# <a name="apache-spark-best-practices"></a>Apache Spark en iyi uygulamalar

Bu makale, Azure HDInsight'ta Apache Spark'ı kullanmak için çeşitli en iyi uygulamalar sağlar.

## <a name="how-do-i-run-or-submit-spark-jobs"></a>Spark işlerini nasıl çalıştırıyorum veya gönderirim?

| Seçenek | Belgeler |
|---|---|
| VSCode | [Visual Studio Code için Kıvılcım & Kovan Araçlarını Kullanma](../hdinsight-for-vscode.md) |
| Jupyter Notebooks | [Öğretici: Azure HDInsight içindeki bir Apache Spark kümesinde veri yükleme ve sorgular çalıştırma](./apache-spark-load-data-run-query.md) |
| IntelliJ | [Öğretici: Bir HDInsight kümesi için Apache Spark uygulamaları oluşturmak için IntelliJ için Azure Araç Kiti'ni kullanın](./apache-spark-intellij-tool-plugin.md) |
| IntelliJ | [Öğretici: IntelliJ kullanarak HDInsight Apache Spark için bir Scala Maven uygulaması oluşturun](./apache-spark-create-standalone-application.md) |
| Zeppelin not defterleri | [Azure HDInsight'ta Apache Spark kümesiyle Apache Zeppelin not defterlerini kullanma](./apache-spark-zeppelin-notebook.md) |
| Livy ile uzak iş teslimi | [HDInsight Spark kümesine uzak işleri göndermek için Apache Spark REST API’sini kullanma](./apache-spark-livy-rest-interface.md) |

## <a name="how-do-i-monitor-and-debug-spark-jobs"></a>Kıvılcım işlerini nasıl izleyebilir ve hata ayıklama yaparım?

| Seçenek | Belgeler |
|---|---|
| Azure Toolkit for IntelliJ | [IntelliJ için Azure Araç Seti ile hata ayıklama hatası (önizleme)](apache-spark-intellij-tool-failure-debug.md) |
| SSH ile IntelliJ için Azure Araç Seti | [SSH aracılığıyla Azure Toolkit for IntelliJ ile HDInsight kümesinde yerel olarak veya uzaktan Apache Spark uygulamalarının hatasını ayıklama](apache-spark-intellij-tool-debug-remotely-through-ssh.md) |
| VPN ile IntelliJ için Azure Araç Seti | [VPN üzerinden HDInsight'ta Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ için Azure Araç Kiti'ni kullanın](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md) |
| Apache Spark History Server'da iş grafiği | [Apache Spark uygulamalarının hatasını ayıklamak ve bu uygulamaları tanılamak için genişletilmiş Apache Spark Geçmiş Sunucusunu kullanma](./apache-azure-spark-history-server.md) |

## <a name="how-do-i-make-my-spark-jobs-run-more-efficiently"></a>Kıvılcım işlerimin daha verimli çalışmasını nasıl sağlarım?

| Seçenek | Belgeler |
|---|---|
| IO Önbellek | [Azure HDInsight IO Önbelleği 'ni kullanarak Apache Spark iş yüklerinin performansını artırın (Önizleme)](./apache-spark-improve-performance-iocache.md) |
| Yapılandırma seçenekleri | [Apache Spark işlerini iyileştirme](./apache-spark-perf.md) |

## <a name="how-do-i-connect-to-other-azure-services"></a>Diğer Azure Hizmetlerine nasıl bağlanırım?

| Seçenek | Belgeler |
|---|---|
| HdInsight üzerinde Apache Hive | [Apache Spark ve Apache Hive'ı Hive Depo Konektörüyle bütünleştirin](../interactive-query/apache-hive-warehouse-connector.md) |
| HDInsight üzerinde Apache HBase | [Apache HBase verilerini okuyup yazmak için Apache Spark kullanma](../hdinsight-using-spark-query-hbase.md) |
| Apache Kafka HDInsight'ta | [Öğretici: HDInsight üzerinde Apache Spark Yapılandırılmış Akışını Apache Kafka ile kullanma](../hdinsight-apache-kafka-spark-structured-streaming.md) |
| Azure Cosmos DB | [Azure Cosmos DB: Azure platformunda bir lambda mimarisi uygulayın](../../cosmos-db/lambda-architecture.md) |

## <a name="what-are-my-storage-options"></a>Depolama seçeneklerim nelerdir?

| Seçenek | Belgeler |
|---|---|
| Data Lake Storage Gen2 | [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanma](../hdinsight-hadoop-use-data-lake-storage-gen2.md) |
| Data Lake Storage Gen1 | [Azure HDInsight kümeleriyle Veri Gölü Depolama Gen1'i kullanma](../hdinsight-hadoop-use-data-lake-store.md) |
| Azure Blob Depolama | [Azure HDInsight kümeleri ile Azure Depolama'yı kullanma](../hdinsight-hadoop-use-blob-storage.md) |

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Spark ayarlarını yapılandırma](apache-spark-settings.md)
* [HDInsight'ta Apache Spark işlerini optimize edin](apache-spark-perf.md)
