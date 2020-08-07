---
title: SQL API sürüm notları ve kaynakları için Azure Cosmos DB Apache Spark Bağlayıcısı
description: Sürüm tarihleri, emeklilik tarihleri ve Azure Cosmos DB SQL zaman uyumsuz Java SDK 'sının her sürümü arasında yapılan değişiklikler dahil olmak üzere SQL API için Azure Cosmos DB Apache Spark Bağlayıcısı hakkında bilgi edinin.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/05/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: 72b3b190492be5cec9986729875c5b09e2559ae5
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87854322"
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
> * [Spring Data](sql-api-sdk-java-spring.md)
> * [Spark Bağlayıcısı](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Toplu yürütücü-.NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu yürütücü-Java](sql-api-sdk-bulk-executor-java.md)

Çekirdek (SQL) için Azure Cosmos DB Apache Spark bağlayıcısını kullanarak büyük veri analizlerini hızlandırın. Spark Bağlayıcısı, Azure Cosmos DB depolanan veriler üzerinde [Spark](https://spark.apache.org/) işleri çalıştırmanızı sağlar. Toplu işlem ve akış işleme desteklenir.

Bağlayıcıyı Azure 'da yönetilen Spark kümeleri sağlayan [Azure Databricks](https://azure.microsoft.com/services/databricks) veya [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/)ile kullanabilirsiniz. Aşağıdaki tabloda desteklenen Spark sürümleri gösterilmektedir.

| Bileşen | Sürüm |
|---------|-------|
| Apache Spark | 2.4. x, 2.3. x, 2.2. x ve 2.1. x |
| Scala | 2,11 |
| Azure Databricks çalışma zamanı sürümü | > 3,4 |

> [!WARNING]
> Bu bağlayıcı Azure Cosmos DB Çekirdek (SQL) API 'sini destekler.
> MongoDB API 'SI için Cosmos DB için [MongoDB Spark bağlayıcısını](https://docs.mongodb.com/spark-connector/master/)kullanın.
> Cosmos DB Cassandra API için [Cassandra Spark bağlayıcısını](https://github.com/datastax/spark-cassandra-connector)kullanın.
>

## <a name="helpful-content"></a>Yardımcı içerik

| İçerik | Bağlantı |
|---|---|
| **SDK indirmesi** | [Apache Spark 'ten indir](https://aka.ms/CosmosDB_OLTP_Spark_2.4_LKG) |
|**API belgeleri** | [Spark bağlayıcı başvurusu]() |
|**SDK 'ya katkıda bulunma** | [GitHub 'da Apache Spark için Azure Cosmos DB Bağlayıcısı](https://github.com/Azure/azure-cosmosdb-spark) | 
|**Kullanmaya başlama** | [Apache Spark Azure Cosmos DB bağlayıcısını kullanarak büyük veri analizlerini hızlandırma](https://docs.microsoft.com/azure/cosmos-db/spark-connector#bk_working_with_connector) <br> [Apache Kafka ve Azure Cosmos DB ile yapılandırılmış Apache Spark akışı kullanın](https://docs.microsoft.com/azure/hdinsight/apache-kafka-spark-structured-streaming-cosmosdb?toc=/azure/cosmos-db/toc.json&bc=/azure/cosmos-db/breadcrumb/toc.json) | 

## <a name="release-history"></a>Yayın geçmişi

### <a name="311"></a>3.1.1
#### <a name="new-features"></a>Yeni özellikler
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* "Kimlik" içinde "ChangeFeedMaxPagesPerBatch" yapılandırması uygulanmış "|" karakteri içeren bir akış kontrol noktası uç durumunu düzeltir

### <a name="310"></a>3.1.0
#### <a name="new-features"></a>Yeni özellikler
* İç içe geçmiş bölüm anahtarlarını kullanırken toplu güncelleştirmeler için destek ekler
* Cosmos DB yazma sırasında Decimal ve float veri türleri için destek ekler.
* Değer olarak uzun (Unix dönemi) kullanılırken zaman damgası türleri için destek ekler
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri

### <a name="308"></a>3.0.8
#### <a name="new-features"></a>Yeni özellikler
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* "Writethroughputbütçe" yapılandırması kullanılırken tür dönüştürme özel durumunu düzeltir.

### <a name="307"></a>3.0.7
#### <a name="new-features"></a>Yeni özellikler
* Toplu hatalar için özel durum ve günlüğe hata bilgilerini ekler.
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri

### <a name="306"></a>3.0.6
#### <a name="new-features"></a>Yeni özellikler
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Akış denetim noktası sorunlarını düzeltir.

### <a name="305"></a>3.0.5
#### <a name="new-features"></a>Yeni özellikler
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Paraziti azaltmak için düzey hata ile istem dışı olarak kalan bir iletinin günlük düzeyini düzeltir

### <a name="304"></a>3.0.4
#### <a name="new-features"></a>Yeni özellikler
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Bölüm bölme işlemleri sırasında yapılandırılmış akıştaki bir hatayı düzeltir; büyük olasılıkla bazı değişiklik akışı kayıtlarını eksik veya denetim noktası yazmaları için null özel durumları görmeye neden oldu

### <a name="303"></a>3.0.3
#### <a name="new-features"></a>Yeni özellikler
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* ReadStream için belirtilen özel şemanın yok sayıldığı bir hatayı düzeltir

### <a name="302"></a>3.0.2
#### <a name="new-features"></a>Yeni özellikler
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Düzeltmeler (gölgesiz JAR), derleme süresini %50 oranında arttığı tüm gölgeli bağımlılıkları içerir

### <a name="301"></a>3.0.1
#### <a name="new-features"></a>Yeni özellikler
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* TCP üzerinden doğrudan taşımanın RequestTimeoutException ile başarısız olmasına neden olan bir bağımlılık sorununu düzeltir

### <a name="300"></a>3.0.0
#### <a name="new-features"></a>Yeni özellikler
* , Meta veri çağrılarının sayısını azaltmak için bağlantı yönetimini ve bağlantı havuzunu geliştirir
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB hakkında daha fazla bilgi için bkz. [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası.

Apache Spark hakkında daha fazla bilgi için bkz. [giriş sayfası](https://spark.apache.org/).