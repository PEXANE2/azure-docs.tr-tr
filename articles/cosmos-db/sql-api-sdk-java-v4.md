---
title: SQL API sürüm notları ve kaynakları için Azure Cosmos DB Java SDK v4
description: SQL API 'SI ve SDK için Azure Cosmos DB Java SDK v4, sürüm tarihleri, kullanımdan kaldırma tarihleri ve Azure Cosmos DB SQL zaman uyumsuz Java SDK 'sının her sürümü arasında yapılan değişiklikler dahil olmak üzere.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 05/11/2020
ms.author: anfeldma
ms.openlocfilehash: cdbb6d0f26b6a999d289e7e0ddbd8cb45dac8e47
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83665204"
---
# <a name="azure-cosmos-db-java-sdk-v4-for-core-sql-api-release-notes-and-resources"></a>Core (SQL) API 'SI için Java SDK v4 Azure Cosmos DB: sürüm notları ve kaynakları
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

Core (SQL) için Java SDK 'Sı v4 Azure Cosmos DB, zaman uyumsuz bir API ve eşitleme API 'sini tek bir Maven yapıtı halinde birleştirir. V4 SDK 'Sı, proje reaktör ve [Netty kitaplığı](https://netty.io/)temelinde gelişmiş performans, yeni API özellikleri ve zaman uyumsuz destek sunar. Kullanıcılar, Azure Cosmos DB Java SDK v4 ile [Azure Cosmos DB zaman uyumsuz Java SDK v2](sql-api-sdk-async-java.md) ve [Azure Cosmos DB Sync Java SDK v2](sql-api-sdk-java.md)ile daha iyi performans bekleyebilir.

> [!IMPORTANT]  
> Bu sürüm notları yalnızca Azure Cosmos DB Java SDK v4 içindir. Şu anda v4 'den daha eski bir sürüm kullanıyorsanız, v4 'ye yükseltme konusunda yardım için [Azure Cosmos DB Java SDK 'sı v4](migrate-java-v4-sdk.md) Kılavuzu ' na bakın.
>
> İşte hızlı bir şekilde çalışmaya yönelik üç adım!
> 1. SDK 'yı kullanabilmek için [desteklenen en düşük Java çalışma zamanı 'nı (JDK 8](/java/azure/jdk/?view=azure-java-stable) ) yükleyebilirsiniz.
> 2. Maven yapıtına erişmenizi sağlayan [Azure Cosmos DB Java SDK v4 Için hızlı başlangıç kılavuzu](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java) aracılığıyla çalışın ve temel Azure Cosmos DB isteklerine kılavuzluk eder.
> 3. Uygulamanızın SDK 'sını iyileştirmek için Azure Cosmos DB Java SDK v4 [Performans ipuçları](performance-tips-java-sdk-v4-sql.md) ve [sorun giderme](troubleshoot-java-sdk-v4-sql.md) kılavuzlarını okuyun.
>
> [Azure Cosmos DB atölyeler ve Labs](https://aka.ms/cosmosworkshop) , Java SDK 'sı v4 Azure Cosmos DB kullanmayı öğrenmek için başka harika bir kaynaktır!
>

| |  |
|---|---|
| **SDK Indirmesi** | [Maven](https://mvnrepository.com/artifact/com.azure/azure-cosmos) |
|**API belgeleri** |Java API başvuru belgeleri|
|**SDK 'ya katkıda bulunma** | [GitHub 'da Java merkezi deposu için Azure SDK](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos) | 
|**Kullanmaya başlayın** | [Hızlı başlangıç: Azure Cosmos db SQL API verilerini yönetmek için bir Java uygulaması oluşturma](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java) · [Hızlı başlangıç kodu Ile GitHub deposu](https://github.com/Azure-Samples/azure-cosmos-java-getting-started) | 
|**Temel kod örnekleri** | Azure Cosmos DB: SQL API 'SI için Java örnekleri · [Örnek kodla GitHub deposu](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples)|
|**Değişiklik akışı olan konsol uygulaması**| [Değişiklik akışı-Java SDK v4 örneği](create-sql-api-java-changefeed.md) · [Örnek kodla GitHub deposu](https://github.com/Azure-Samples/azure-cosmos-java-sql-app-example)| 
|**Web uygulaması örneği**| [Java SDK 'sı v4 · bir Web uygulaması oluşturma](sql-api-java-application.md) [Örnek kodla GitHub deposu](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-todo-app)|
| **Performans ipuçları**| [Java SDK v4 için performans ipuçları](performance-tips-java-sdk-v4-sql.md)| 
| **Sorun giderme** | [Java SDK v4 sorunlarını giderme](troubleshoot-java-sdk-v4-sql.md) |
| **Daha eski bir SDK 'dan v4 'ye geçiş** | [Java V4 SDK’ya geçirme](migrate-java-v4-sdk.md) |
| **Desteklenen en düşük çalışma zamanı**|[JDK 8](/java/azure/jdk/?view=azure-java-stable) | 
| **Azure Cosmos DB atölyeler ve Labs** |[Cosmos DB atölyeler giriş sayfası](https://aka.ms/cosmosworkshop)

