---
title: SQL API sürüm notları ve kaynakları için Cosmos DB Apache Spark Bağlayıcısı
description: Sürüm tarihleri, kullanımdan kaldırma tarihleri ve Azure Cosmos DB SQL zaman uyumsuz Java SDK 'sının her sürümü arasında yapılan değişiklikler dahil olmak üzere SQL API için Azure Cosmos DB Apache Spark Bağlayıcısı hakkında bilgi edinin.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/12/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: 05f81e4d93244db854bf8d0ec254ee647f81d9cc
ms.sourcegitcommit: 1fe5127fb5c3f43761f479078251242ae5688386
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90069178"
---
# <a name="azure-cosmos-db-apache-spark-connector-for-core-sql-api-release-notes-and-resources"></a>Çekirdek (SQL) API 'SI için Azure Cosmos DB Apache Spark Bağlayıcısı: sürüm notları ve kaynakları
> [!div class="op_single_selector"]
> * [.NET SDK v3](sql-api-sdk-dotnet-standard.md)
> * [.NET SDK v2](sql-api-sdk-dotnet.md)
> * [.NET Core SDK v2](sql-api-sdk-dotnet-core.md)
> * [.NET değişiklik akışı SDK v2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java SDK’sı v4](sql-api-sdk-java-v4.md)
> * [Zaman uyumsuz Java SDK v2](sql-api-sdk-async-java.md)
> * [Zaman uyumlu Java SDK v2](sql-api-sdk-java.md)
> * [Spring Data v2](sql-api-sdk-java-spring-v2.md)
> * [Spring Data v3](sql-api-sdk-java-spring-v3.md)
> * [Spark Bağlayıcısı](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Toplu yürütücü-.NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu yürütücü-Java](sql-api-sdk-bulk-executor-java.md)

Çekirdek (SQL) için Azure Cosmos DB Apache Spark bağlayıcısını kullanarak büyük veri analizlerini hızlandırabilirsiniz. Spark Bağlayıcısı, Azure Cosmos DB depolanan veriler üzerinde [Spark](https://spark.apache.org/) işleri çalıştırmanızı sağlar. Toplu işlem ve akış işleme desteklenir.

Bağlayıcıyı Azure 'da yönetilen Spark kümeleri sağlayan [Azure Databricks](https://azure.microsoft.com/services/databricks) veya [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/)ile kullanabilirsiniz. Aşağıdaki tabloda desteklenen sürümler gösterilmektedir:

| Bileşen | Sürüm |
|---------|-------|
| Apache Spark | 2,4.*x*, 2,3. *x*, 2,2. *x*ve 2,1. *x* |
| Scala | 2.11 |
| Azure Databricks (çalışma zamanı sürümü) | 3,4 'den sonraki |

> [!WARNING]
> Bu bağlayıcı Azure Cosmos DB Çekirdek (SQL) API 'sini destekler.
> MongoDB için Cosmos DB API 'SI için, [Spark Için MongoDB bağlayıcısını](https://docs.mongodb.com/spark-connector/master/)kullanın.
> Cosmos DB Cassandra API için [Cassandra Spark bağlayıcısını](https://github.com/datastax/spark-cassandra-connector)kullanın.
>

## <a name="resources"></a>Kaynaklar

| Kaynak | Bağlantı |
|---|---|
| **SDK indirmesi** | [Apache Spark 'ten indir](https://aka.ms/CosmosDB_OLTP_Spark_2.4_LKG) |
|**API belgeleri** | [Spark bağlayıcı başvurusu]() |
|**SDK 'ya katkıda bulunma** | [GitHub 'da Apache Spark için Azure Cosmos DB Bağlayıcısı](https://github.com/Azure/azure-cosmosdb-spark) | 
|**Kullanmaya başlama** | [Apache Spark Azure Cosmos DB bağlayıcısını kullanarak büyük veri analizlerini hızlandırma](https://docs.microsoft.com/azure/cosmos-db/spark-connector#bk_working_with_connector) <br> [Apache Kafka ve Azure Cosmos DB ile yapılandırılmış Apache Spark akışı kullanın](https://docs.microsoft.com/azure/hdinsight/apache-kafka-spark-structured-streaming-cosmosdb?toc=/azure/cosmos-db/toc.json&bc=/azure/cosmos-db/breadcrumb/toc.json) | 

## <a name="release-history"></a>Yayın geçmişi

### <a name="311"></a>3.1.1
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* , `ID` Yapılandırma uygulanmış kanal karakterini (|) içeren bir akış kontrol noktası kenar durumunu düzeltir `ChangeFeedMaxPagesPerBatch` .

### <a name="310"></a>3.1.0
#### <a name="new-features"></a>Yeni özellikler
* İç içe bölüm anahtarları kullanıldığında toplu güncelleştirmeler için destek ekler.
* Azure Cosmos DB yazma sırasında Decimal ve float veri türleri için destek ekler.
* Değer olarak uzun (UNIX dönemi) kullanılırken zaman damgası türleri için destek ekler.

### <a name="308"></a>3.0.8
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Yapılandırma kullanıldığında oluşan typecast özel durumunu düzeltir `WriteThroughputBudget` .

### <a name="307"></a>3.0.7
#### <a name="new-features"></a>Yeni özellikler
* Toplu hatalar için özel durum ve günlüğe hata bilgilerini ekler.

### <a name="306"></a>3.0.6
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Akış denetim noktası sorunlarını düzeltir.

### <a name="305"></a>3.0.5
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Paraziti azaltmak için, düzey hata ile istem dışı olarak kalan bir iletinin günlük düzeyini düzeltir.

### <a name="304"></a>3.0.4
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Bölüm bölmelerini sırasında yapısal akışta bir hatayı düzeltir. Hata, bazı eksik değişiklik akışı kayıtlarına veya denetim noktası yazmaları için null özel durumlara neden olabilir.

### <a name="303"></a>3.0.3
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* ReadStream için belirtilen özel şemanın yoksayılmasına neden olan bir hatayı düzeltir.

### <a name="302"></a>3.0.2
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Bir gerileme (gölgesiz JAR), derleme süresini yüzde 50 oranında artıran tüm gölgeli bağımlılıkları içerir.

### <a name="301"></a>3.0.1
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* TCP üzerinden doğrudan aktarıma RequestTimeoutException ile başarısız olmasına neden olan bir bağımlılık sorununu düzeltir.

### <a name="300"></a>3.0.0
#### <a name="new-features"></a>Yeni özellikler
* , Meta veri çağrılarının sayısını azaltmak için bağlantı yönetimini ve bağlantı havuzunu geliştirir.

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="next-steps"></a>Sonraki adımlar

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)hakkında daha fazla bilgi edinin.

[Apache Spark](https://spark.apache.org/) hakkında daha fazla bilgi edinin.