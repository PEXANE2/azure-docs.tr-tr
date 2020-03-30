---
title: Örnek Apache Storm, Azure HDInsight'ta topolojiler
description: Temel C# ve Java topolojileri de dahil olmak üzere HDInsight'ta Apache Storm ile oluşturulan ve test edilen örnek Storm topolojilerinin ve Olay Hub'ları ile çalışmanın bir listesi.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/27/2019
ms.openlocfilehash: 302ba583f11b15be98832316b1ea05c7f9be931f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75530672"
---
# <a name="example-apache-storm-topologies-and-components-for-apache-storm-on-hdinsight"></a>ÖRNEK Apache Storm topolojileri ve HDInsight'ta Apache Storm bileşenleri

Aşağıda, Microsoft tarafından HDInsight'ta [Apache Storm](https://storm.apache.org/) ile kullanılmak üzere oluşturulan ve tutulan örneklerin bir listesi verilmiştir. Bu örnekler, temel C# ve Java topolojileri oluşturmaktan Event Hubs, Cosmos DB, SQL Database, [Apache HBase](https://hbase.apache.org/) on HDInsight ve Azure Depolama gibi Azure hizmetleriyle çalışmaya kadar çeşitli konuları kapsar. Bazı örnekler, SignalR ve Socket.IO gibi Azure'a uygun olmayan ve hatta Microsoft'a ait olmayan teknolojilerle nasıl çalışılabildiğini de gösterir.

| Açıklama | Gösteriler | Dil / Çerçeve |
|:--- |:--- |:--- |
| [Apache Storm'dan Azure Veri Gölü Depolamasına Yazma](apache-storm-write-data-lake-store.md) |Azure Veri Gölü Depolamasına Yazma |Java |
| [Olay Hub Spout ve Cıvata kaynağı](https://github.com/apache/storm/tree/master/external/storm-eventhubs) |Olay Hub Spout ve Bolt için kaynak |Java |
| [HDInsight'ta Apache Storm için Java tabanlı topolojiler geliştirin][5797064f] |Maven |Java |
| [Visual Studio'yu kullanarak HDInsight'ta Apache Storm için C# topolojileri geliştirin][16fce2d1] |Visual Studio için HDInsight Araçları |C#, Java |
| [HDInsight'ta Apache Storm ile Azure Etkinlik Hub'larından etkinlikleri işleme (C#)][844d1d81] |Event Hubs |C# ve Java |
| [HDInsight üzerinde Storm ile Azure Event Hubs’tan olay işleme (Java)](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) |Event Hubs |Java |
| [HDInsight'ta Apache Storm kullanarak Olay Hub'larından gelen araç sensör verilerini işleme][246ee964] |Etkinlik Hub'ları, Cosmos DB, Azure Depolama Blob (WASB) |C#, Java |
| [HDInsight'ta Apache Storm kullanarak Azure Etkinlik Hub'larından Apache HBase'e Çıkarma, Dönüştürme ve Yükleme (ETL)][b4b68194] |Olay Hub'ları, HBase |C# |
| [HDInsight'ta Apache Storm'un Azure hizmetleriyle çalışmak için C# Storm topolojisi şablonu][ce0c02a2] |Olay Hub'ları, Cosmos DB, SQL Veritabanı, HBase, SignalR |C#, Java |
| [HDInsight'ta Apache Storm kullanan Azure Etkinlik Hub'larından okuma için ölçeklenebilirlik ölçütleri][d6c540e3] |İleti girişi, Olay Hub'ları, SQL Veritabanı |C#, Java |
| [Apache Kafka'yı Apache Storm ile HDInsight'ta kullanın](../hdinsight-apache-storm-with-kafka.md) | Apaçi Fırtınası Apaçi Kafka'ya okuma ve yazma | Java |

> [!WARNING]  
> Bu listedeki C# örnekleri başlangıçta oluşturulmuş ve Windows tabanlı HDInsight ile test edilmiştir ve Linux tabanlı HDInsight kümeleriyle doğru çalışmayabilir. Linux tabanlı kümeler .NET kodunu çalıştırmak için Mono'yu kullanır ve örnekte kullanılan çerçeveler ve paketlerle uyumluluk sorunları olabilir.
>
> Linux, HDInsight sürüm 3.4 veya sonraki sürümlerinde kullanılan tek işletim sistemidir.

## <a name="python-only"></a>Yalnızca Python

Flux topolojisi olan Python bileşenlerinin bir örneği için [HDInsight'ta Apache Storm ile Python'u kullanın.](apache-storm-develop-python-topology.md)

## <a name="next-steps"></a>Sonraki Adımlar

* [Azure HDInsight'ta Bir Apache Storm topolojisi oluşturun ve izleyin](./apache-storm-quickstart.md)
* [HDInsight'ta Apache Storm ile Apache Storm topolojilerini nasıl dağıtacağınız ve yöneteceğinizi öğrenin][6eb0d3b8]

[6eb0d3b8]:apache-storm-deploy-monitor-topology-linux.md "Web tabanlı Apache Storm Dashboard ve Storm UI veya Visual Studio için HDInsight Tools'u kullanarak topolojileri nasıl dağıtacağınız ve yöneteceğinizi öğrenin."
[16fce2d1]:apache-storm-develop-csharp-visual-studio-topology.md "Visual Studio için HDInsight Araçlarını kullanarak C# Storm topolojilerini nasıl oluşturabilirsiniz öğrenin."
[5797064f]:apache-storm-develop-java-topology.md "Temel bir kelime sayısı topolojisi oluşturarak Maven'i kullanarak Java'da Fırtına topolojilerini nasıl oluşturabilirsiniz öğrenin."
[844d1d81]:apache-storm-develop-csharp-event-hub-topology.md "HDInsight'ta Storm ile Azure Etkinlik Hub'larından veri okuma ve yazma öğrenin."
[246ee964]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md "Azure Etkinlik Hub'larından gelen iletileri okumak, veri yönlendirmesi için Azure Cosmos DB'den gelen belgeleri okumak ve verileri Azure Depolama'ya kaydetmek için Fırtına topolojisini nasıl kullanacağınızı öğrenin."
[d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "HdInsight'ta Apache Storm kullanarak Azure Etkinlik Hub'larından okuma ve SQL Veritabanı'na depolama yaparken iş gücünü göstermek için çeşitli topolojiler."
[b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Azure Etkinlik Hub'larından gelen verileri nasıl okuyup dönüştürecek & dönüştürmeyi ve ardından HDInsight'ta HBase'e nasıl depolayabilirsiniz öğrenin."
[ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Bu proje, Olay Hub'ları, Cosmos DB ve SQL Veritabanı gibi çeşitli Azure hizmetleriyle etkileşim kurmak için dışa doğru, cıvatalara ve topolojilere yönelik şablonlar içerir."
