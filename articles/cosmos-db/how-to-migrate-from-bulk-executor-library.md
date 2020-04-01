---
title: Azure Cosmos DB .NET V3 SDK'daki toplu yürütme kitaplığından toplu desteğe geçiş
description: Azure Cosmos DB SDK V3'teki toplu yürütme kitaplığını kullanarak uygulamanızı nasıl geçirebilirsiniz öğrenin
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: maquaran
ms.openlocfilehash: e1a2a5d849d3c94d62b8645c41f288ba130aa6a4
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80479337"
---
# <a name="migrate-from-the-bulk-executor-library-to-the-bulk-support-in-azure-cosmos-db-net-v3-sdk"></a>Azure Cosmos DB .NET V3 SDK'daki toplu yürütme kitaplığından toplu desteğe geçiş

Bu makalede, .NET toplu [yürütme kitaplığını](bulk-executor-dot-net.md) kullanan varolan bir uygulama kodunu .NET SDK'nın en son sürümünde [toplu destek](tutorial-sql-api-dotnet-bulk-import.md) özelliğine geçirmek için gerekli adımlar açıklanmaktadır.

## <a name="enable-bulk-support"></a>Toplu destek etkinleştirme

`CosmosClient` [AllowBulkExecution](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.allowbulkexecution) yapılandırması aracılığıyla örnekte toplu destek etkinleştirin:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="Initialization":::

## <a name="create-tasks-for-each-operation"></a>Her işlem için Görevler Oluşturma

.NET SDK'daki toplu destek, [Görev Paralel Kitaplığı'ndan](https://docs.microsoft.com/dotnet/standard/parallel-programming/task-parallel-library-tpl) yararlanarak ve eşzamanlı olarak gerçekleşen gruplandırma işlemlerini yürütmektedir. 

Belge veya işlem listenizi giriş parametresi olarak alacak tek bir yöntem yoktur, ancak toplu olarak yürütmek istediğiniz her işlem için bir Görev oluşturmanız gerekir.

Örneğin, ilk girdiniz her öğenin aşağıdaki şema sahip olduğu öğelerin listesiyse:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="Model":::

Toplu alma işlemi yapmak istiyorsanız (BulkExecutor.BulkImportAsync kullanmaya benzer), her madde `CreateItemAsync` değeriyle eşzamanlı çağrılar yapmanız gerekir. Örnek:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="BulkImport":::

Toplu *güncelleştirme* yapmak istiyorsanız [(BulkExecutor.BulkUpdateAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkexecutor.bulkupdateasync)kullanmaya benzer), madde değerini güncelledikten sonra yönteme `ReplaceItemAsync` eşzamanlı çağrılar yapmanız gerekir. Örnek:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="BulkUpdate":::

Ve toplu *silme* yapmak istiyorsanız [(BulkExecutor.BulkDeleteAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkexecutor.bulkdeleteasync)kullanarak benzer), her öğenin `DeleteItemAsync` `id` ve bölüm anahtarı ile eşzamanlı aramalar olması gerekir. Örnek:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="BulkDelete":::

## <a name="capture-task-result-state"></a>Görev sonuç durumunu yakalama

Önceki kod örneklerinde, eşzamanlı bir görev listesi oluşturdunuz `CaptureOperationResponse` ve bu görevlerin her birinde yöntemi çağırdınız. Bu yöntem, herhangi bir hata yakalayarak ve [istek birimleri kullanımını](request-units.md)izleyerek, BulkExecutor gibi benzer bir yanıt *şema* korumak sağlayan bir uzantısıdır.

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="CaptureOperationResult":::

Nerede `OperationResponse` olarak ilan edilir:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="OperationResult":::

## <a name="execute-operations-concurrently"></a>İşlemleri aynı anda yürütme

Görev listesi tanımlandıktan sonra, hepsi tamamlanana kadar bekleyin. Toplu işleminizin kapsamını aşağıdaki kod snippet'inde gösterildiği gibi tanımlayarak görevlerin tamamlanmasını izleyebilirsiniz:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="WhenAll":::

## <a name="capture-statistics"></a>İstatistikleri yakalama

Önceki kod tüm işlemler tamamlanana kadar bekler ve gerekli istatistikleri hesaplar. Bu istatistikler toplu uygulayıcı kitaplığın [BulkImportResponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkimport.bulkimportresponse)benzer.

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="ResponseType":::

İçeriği: `BulkOperationResponse`

1. Toplu destek yoluyla operasyon listesini işlemek için gereken toplam süre.
1. Başarılı operasyonların sayısı.
1. Tüketilen istek birimlerinin toplamı.
1. Hata lar varsa, günlüğe kaydetme ve tanımlama amacıyla özel durum ve ilişkili öğeyi içeren bir tuples listesini görüntüler.

## <a name="performance-improvements"></a>Performans iyileştirmeleri

.NET SDK'daki diğer işlemlerde olduğu gibi, akış API'lerini kullanmak daha iyi performans sağlar ve gereksiz serileştirmeyi önler. 

Akış API'lerinin kullanılması, yalnızca kullandığınız verilerin yapısı bir bayt akışıyla (örneğin, dosya akışları) eşleşirse mümkündür. Bu gibi durumlarda, `CreateItemStreamAsync` `ReplaceItemStreamAsync`, `DeleteItemStreamAsync` veya yöntemleri `ResponseMessage` kullanarak ve `ItemResponse`(yerine) ile çalışma elde edilebilir iş kısmını artırır.

## <a name="next-steps"></a>Sonraki adımlar

* .NET SDK sürümleri hakkında daha fazla bilgi edinmek için [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md) makalesine bakın.
* GitHub'dan tüm [geçiş kaynak kodunu](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration) alın.
* [GitHub'da ek toplu örnekler](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/BulkSupport)
