---
title: "Azure Cosmos DB: toplu yürütücü Java API 'SI, SDK & kaynakları"
description: Toplu yürütücü Java API 'SI ve SDK ve sürüm tarihleri, emeklilik tarihleri ve Azure Cosmos DB toplu yürütücü Java SDK 'sının her sürümü arasında yapılan değişiklikler dahil olmak üzere tüm bilgileri öğrenin.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/05/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: 1e389450562fe731ac750c779da236f379429423
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87852659"
---
# <a name="java-bulk-executor-library-download-information"></a>Java toplu yürütücü kitaplığı: Indirme bilgileri

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

| |  |
|---|---|
|**Açıklama**|Toplu yürütücü kitaplığı, istemci uygulamalarının Azure Cosmos DB hesaplarında toplu işlemler gerçekleştirmesine olanak sağlar. toplu yürütücü kitaplığı, BulkImport ve BulkUpdate ad alanları sağlar. BulkImport modülü, bir koleksiyon için sağlanan aktarım hızı en büyük ölçüde tüketilebilmesi için belgeleri en iyi duruma getirilmiş bir şekilde toplu Içe alabilir. BulkUpdate modülü Azure Cosmos kapsayıcılarındaki mevcut verileri düzeltme eki olarak toplu güncelleştirebilir.|
|**SDK indirmesi**|[Maven](https://search.maven.org/#search%7Cga%7C1%7Cdocumentdb-bulkexecutor)|
|**GitHub 'da toplu yürütücü kitaplığı**|[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-java-getting-started)|
| **API belgeleri**| [Java API başvuru belgeleri](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor)|
|**Kullanmaya başlama**|[Toplu yürütücü kitaplığı Java SDK 'sını kullanmaya başlama](bulk-executor-java.md)|
|**Desteklenen en düşük çalışma zamanı**|[Java Development Kit (JDK) 7 +](/java/azure/jdk/?view=azure-java-stable)|

## <a name="release-notes"></a>Sürüm notları

### <a name="2100"></a><a name="2.10.0"></a>2.10.0

* JSON 'dan iç içe bölüm anahtar değerlerini doğru bir şekilde ayıklamak için DocumentAnalyzer. Java için çözüm.

### <a name="294"></a><a name="2.9.4"></a>2.9.4

* Belirli hatalarda yeniden denemek için BulkDelete işlemlerine işlevsellik ekleyin ve ayrıca yeniden denenebilecek kullanıcıya hataların bir listesini geri döndürün.

### <a name="293"></a><a name="2.9.3"></a>2.9.3

* Cosmos SDK sürümü 2.4.7 için güncelleştirme.

### <a name="292"></a><a name="2.9.2"></a>2.9.2

* ' İd ' ve bölüm anahtarı değerinden sonra gelen düzeltme eki uygulanmış belge özellikleri güncelleştirilmiş öğe listesine eklendikten sonra, ' mergeAll ' için ' id ' ve bölüm anahtarı değeri ile devam etmek için düzeltme.

### <a name="291"></a><a name="2.9.1"></a>2.9.1

* Eşzamanlılık başlangıç derecesini 1 ' e güncelleştirin ve mini Batch için hata ayıklama günlüklerini eklendi.


