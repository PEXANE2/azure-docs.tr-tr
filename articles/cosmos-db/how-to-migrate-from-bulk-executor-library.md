---
title: Toplu yürütücü kitaplığından Azure Cosmos DB .NET v3 SDK 'daki toplu desteğe geçiş yapın
description: Toplu yürütücü kitaplığı 'nı kullanarak uygulamanızı Azure Cosmos DB SDK V3 'deki toplu desteğe geçirmeyi öğrenin
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/24/2020
ms.author: maquaran
ms.openlocfilehash: d63b34c118cd719f73abbd6711dcb3ef02a6fb28
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82146301"
---
# <a name="migrate-from-the-bulk-executor-library-to-the-bulk-support-in-azure-cosmos-db-net-v3-sdk"></a>Toplu yürütücü kitaplığından Azure Cosmos DB .NET v3 SDK 'daki toplu desteğe geçiş yapın

Bu makalede, .net [toplu yürütücü kitaplığı](bulk-executor-dot-net.md) 'nı kullanan mevcut bir uygulamanın kodunu .NET SDK 'sının en son sürümündeki [toplu destek](tutorial-sql-api-dotnet-bulk-import.md) özelliğine geçirmek için gereken adımlar açıklanmaktadır.

## <a name="enable-bulk-support"></a>Toplu desteği etkinleştir

[Allowbulkexecution](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.allowbulkexecution) yapılandırması aracılığıyla `CosmosClient` örnekte toplu desteği etkinleştirin:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="Initialization":::

## <a name="create-tasks-for-each-operation"></a>Her işlem için görev oluşturma

.NET SDK 'da toplu destek, [görev paralel kitaplığı](https://docs.microsoft.com/dotnet/standard/parallel-programming/task-parallel-library-tpl) ve aynı anda oluşan gruplandırma işlemleri üzerinden çalışarak işe yarar. 

SDK 'da belge veya işlem listenizi giriş parametresi olarak alacak tek bir yöntem yoktur, ancak bunun yerine, toplu olarak yürütmek istediğiniz her işlem için bir görev oluşturmanız ve ardından bunların tamamlanmasını beklemeniz yeterlidir.

Örneğin, ilk giriş, her öğenin aşağıdaki şemaya sahip olduğu öğelerin bir listesidir:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="Model":::

Toplu içeri aktarma yapmak istiyorsanız (Bulkyürütücü. Bulkımportasync kullanmaya benzer), için `CreateItemAsync`eşzamanlı olarak çağrılarına sahip olmanız gerekir. Örneğin:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="BulkImport":::

Toplu *güncelleştirme* yapmak Istiyorsanız ( [Bulkyürütücü. bulkupdateasync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkexecutor.bulkupdateasync)kullanmaya benzer), öğe değerini güncelleştirdikten sonra metoda eşzamanlı olarak `ReplaceItemAsync` çağrı yapmanız gerekir. Örneğin:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="BulkUpdate":::

Toplu *silme* yapmak Istiyorsanız ( [Bulkyürütücü. BulkDeleteAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkexecutor.bulkdeleteasync)ile benzer), her bir öğenin `DeleteItemAsync` `id` ve bölüm anahtarıyla, için eşzamanlı olarak çağrılarına sahip olmanız gerekir. Örneğin:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="BulkDelete":::

## <a name="capture-task-result-state"></a>Görev sonucu durumunu yakala

Önceki kod örneklerinde, eş zamanlı bir görev listesi oluşturdunuz ve bu görevlerin her birinde `CaptureOperationResponse` yöntemi dentik. Bu yöntem, tüm hataları yakalayıp [istek birimlerinin kullanımını](request-units.md)izleyerek *benzer bir yanıt şemasını* bulkyürütücü olarak korumamıza imkan tanıyan bir uzantıdır.

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="CaptureOperationResult":::

`OperationResponse` Şöyle bildirildiği konum:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="OperationResult":::

## <a name="execute-operations-concurrently"></a>İşlemleri eşzamanlı olarak Yürüt

Tüm görev listesinin kapsamını izlemek için bu yardımcı sınıfı kullanıyoruz:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="BulkOperationsHelper":::

`ExecuteAsync` Yöntemi tüm işlemler tamamlanana kadar bekler ve bunu şöyle kullanabilirsiniz:

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="WhenAll":::

## <a name="capture-statistics"></a>İstatistikleri yakala

Önceki kod, tüm işlemler tamamlanana kadar bekler ve gerekli istatistikleri hesaplar. Bu istatistikler, toplu yürütücü kitaplığı 'nın [Bulkımportresponse](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkimport.bulkimportresponse)ile benzerdir.

   :::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration/Program.cs" ID="ResponseType":::

`BulkOperationResponse` Şunu içerir:

1. Toplu destek aracılığıyla işlem listesini işlemek için harcanan toplam süre.
1. Başarılı işlem sayısı.
1. Tüketilen istek birimlerinin toplamı.
1. Hata varsa, günlüğe kaydetme ve tanımlama amacıyla özel durumu ve ilişkili öğeyi içeren başlıkların bir listesini görüntüler.

## <a name="retry-configuration"></a>Yapılandırmayı yeniden dene

Toplu yürütücü kitaplığı, [guidance](bulk-executor-dot-net.md#bulk-import-data-to-an-azure-cosmos-account) `MaxRetryWaitTimeInSeconds` denetimin kitaplığa atamasını sağlamak için `MaxRetryAttemptsOnThrottledRequests` ve [RetryOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.retryoptions) olarak `0` ayarlanması için bahsedilen kılavuza sahipti.

.NET SDK 'da toplu destek için gizli bir davranış yoktur. Yeniden deneme seçeneklerini [Cosmosclientoptions. MaxRetryAttemptsOnRateLimitedRequests](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.maxretryattemptsonratelimitedrequests) ve [Cosmosclientoptions. MaxRetryWaitTimeOnRateLimitedRequests](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.maxretrywaittimeonratelimitedrequests)aracılığıyla doğrudan yapılandırabilirsiniz.

> [!NOTE]
> Sağlanan istek birimlerinin, veri miktarına göre beklenenden çok daha düşük olduğu durumlarda, bunları yüksek değerlere ayarlamayı düşünmek isteyebilirsiniz. Toplu işlem daha uzun sürer, ancak daha yüksek yeniden denemeler nedeniyle tamamen başarılı olma şansı daha yüksektir.

## <a name="performance-improvements"></a>Performans iyileştirmeleri

.NET SDK ile diğer işlemlerde olduğu gibi, Stream API 'Leri kullanılması daha iyi performansa neden olur ve gereksiz Serileştirmeyi önler. 

Stream API 'Lerinin kullanılması yalnızca kullandığınız verilerin doğası bir bayt akışı (örneğin, dosya akışları) ile eşleşiyorsa mümkündür. Bu gibi `CreateItemStreamAsync`durumlarda,, `ReplaceItemStreamAsync`, veya `DeleteItemStreamAsync` yöntemlerini kullanarak `ResponseMessage` (yerine `ItemResponse`), elde edilen aktarım hızını artırır.

## <a name="next-steps"></a>Sonraki adımlar

* .NET SDK sürümleri hakkında daha fazla bilgi edinmek için [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md) makalesine bakın.
* GitHub 'dan tüm [geçiş kaynak kodunu](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/BulkExecutorMigration) alın.
* [GitHub 'da ek toplu örnekler](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/BulkSupport)
