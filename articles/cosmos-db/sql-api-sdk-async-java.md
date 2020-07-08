---
title: "Azure Cosmos DB: SQL zaman uyumsuz Java API 'SI, SDK & kaynakları"
description: Sürüm tarihleri, kullanımdan kaldırma tarihleri ve SQL zaman uyumsuz Java SDK Azure Cosmos DB her sürümü arasında yapılan değişiklikler dahil olmak üzere SQL zaman uyumsuz Java API 'SI ve SDK hakkında bilgi edinin.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 05/11/2020
ms.author: anfeldma
ms.openlocfilehash: 4bdfe1609d80a0e3198a9e428a148be2f28c72d3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85392123"
---
# <a name="azure-cosmos-db-async-java-sdk-for-sql-api-release-notes-and-resources"></a>SQL API için zaman uyumsuz Java SDK Azure Cosmos DB: sürüm notları ve kaynakları
> [!div class="op_single_selector"]
> * [.NET SDK v3](sql-api-sdk-dotnet-standard.md)
> * [.NET SDK v2](sql-api-sdk-dotnet.md)
> * [.NET Core SDK v2](sql-api-sdk-dotnet-core.md)
> * [.NET değişiklik akışı SDK v2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java SDK’sı v4](sql-api-sdk-java-v4.md)
> * [Zaman uyumsuz Java SDK v2](sql-api-sdk-async-java.md)
> * [Zaman uyumlu Java SDK v2](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * KALANı (/REST/API
> * [REST Kaynak Sağlayıcısı](/rest/apier/)
> * [SQL](sql-api-query-reference.md)
> * [Toplu yürütücü-.NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu yürütücü-Java](sql-api-sdk-bulk-executor-java.md)

SQL API zaman uyumsuz Java SDK 'Sı, [Netty kitaplığı](https://netty.io/)desteğiyle zaman uyumsuz Işlemler sağlayarak SQL API Java SDK 'sinden farklıdır. Önceden var olan [SQL API Java SDK 'sı](sql-api-sdk-java.md) , zaman uyumsuz işlemleri desteklemez. 

> [!IMPORTANT]  
> Bu, Azure Cosmos DB için en son Java SDK 'Sı *değildir* ! Projeniz için [Azure Cosmos DB Java SDK v4](sql-api-sdk-java-v4.md) kullanmayı düşünün. Yükseltmek için [Azure Cosmos DB Java SDK 'sı v4](migrate-java-v4-sdk.md) Kılavuzu ve [reaktör vs rxjava](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/reactor-rxjava-guide.md) Kılavuzu 'ndaki yönergeleri izleyin. 
>

| |  |
|---|---|
| **SDK Indirmesi** | [Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) |
|**API belgeleri** |[Java API başvuru belgeleri](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient?view=azure-java-stable) | 
|**SDK 'ya katkıda bulunma** | [GitHub](https://github.com/Azure/azure-cosmosdb-java) | 
|**Kullanmaya başlama** | [Zaman uyumsuz Java SDK 'sını kullanmaya başlama](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started) | 
|**Kod örneği** | [GitHub](https://github.com/Azure/azure-cosmosdb-java#usage-code-sample)| 
| **Performans ipuçları**| [GitHub Benioku dosyası](https://github.com/Azure/azure-cosmosdb-java#guide-for-prod)| 
| **Desteklenen en düşük çalışma zamanı**|[JDK 8](/java/azure/jdk/?view=azure-java-stable) | 

[!INCLUDE [Release notes](~/azure-cosmosdb-java-v2/changelog/README.md)]
## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi için bkz. [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası.

