---
title: "Azure Cosmos DB: toplu yürütücü Java API 'SI, SDK & kaynakları"
description: Toplu yürütücü Java API 'SI ve SDK ve sürüm tarihleri, emeklilik tarihleri ve Azure Cosmos DB toplu yürütücü Java SDK 'sının her sürümü arasında yapılan değişiklikler dahil olmak üzere tüm bilgileri öğrenin.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 05/11/2020
ms.author: anfeldma
ms.openlocfilehash: c826b9f813c30a50d8b88b2cca1b188c328465b0
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84171659"
---
# <a name="java-bulk-executor-library-download-information"></a>Java toplu yürütücü kitaplığı: Indirme bilgileri

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET değişiklik akışı](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java SDK v4](sql-api-sdk-java-v4.md)
> * [Zaman uyumsuz Java SDK v2](sql-api-sdk-async-java.md)
> * [Zaman uyumlu Java SDK v2](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Toplu yürütücü-.NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu yürütücü-Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
|**Açıklama**|Toplu yürütücü kitaplığı, istemci uygulamalarının Azure Cosmos DB hesaplarında toplu işlemler gerçekleştirmesine olanak sağlar. toplu yürütücü kitaplığı, BulkImport ve BulkUpdate ad alanları sağlar. BulkImport modülü, bir koleksiyon için sağlanan aktarım hızı en büyük ölçüde tüketilebilmesi için belgeleri en iyi duruma getirilmiş bir şekilde toplu Içe alabilir. BulkUpdate modülü Azure Cosmos kapsayıcılarındaki mevcut verileri düzeltme eki olarak toplu güncelleştirebilir.|
|**SDK indirmesi**|[Maven](https://search.maven.org/#search%7Cga%7C1%7Cdocumentdb-bulkexecutor)|
|**GitHub 'da toplu yürütücü kitaplığı**|[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-java-getting-started)|
| **API belgeleri**| [Java API başvuru belgeleri](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor)|
|**Kullanmaya başlayın**|[Toplu yürütücü kitaplığı Java SDK 'sını kullanmaya başlama](bulk-executor-java.md)|
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


