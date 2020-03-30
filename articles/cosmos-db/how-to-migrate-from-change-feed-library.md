---
title: Değişiklik akışı işlemci kitaplığından Azure Cosmos DB .NET V3 SDK'ya geçiş
description: Değişiklik akışı işlemci kitaplığını kullanarak uygulamanızı Azure Cosmos DB SDK V3'e nasıl geçireceğinizi öğrenin
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/17/2019
ms.author: maquaran
ms.openlocfilehash: 9570a8512e3437b12ecce2ef0c708a74a8806482
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588892"
---
# <a name="migrate-from-the-change-feed-processor-library-to-the-azure-cosmos-db-net-v3-sdk"></a>Değişiklik akışı işlemci kitaplığından Azure Cosmos DB .NET V3 SDK'ya geçiş

Bu makalede, .NET SDK'nın (.NET V3 SDK olarak da adlandırılır) en son sürümündeki [değişiklik akışı](change-feed.md) özelliğine besleme [işlemci kitaplığını](https://github.com/Azure/azure-documentdb-changefeedprocessor-dotnet) kullanan varolan bir uygulamakodunu geçirmek için gerekli adımlar açıklanmaktadır.

## <a name="required-code-changes"></a>Gerekli kod değişiklikleri

.NET V3 SDK'da birkaç kesme değişikliği vardır, uygulamanızı geçirmek için önemli adımlar şunlardır:

1. `DocumentCollectionInfo` Örnekleri izlenen ve `Container` kiralanan kapsayıcılar için başvurulara dönüştürün.
1. Kullanılan `WithProcessorOptions` özelleştirmeler, başlangıç saati `WithLeaseConfiguration` `WithMaxItems` `WithPollInterval` ve maksimum `WithStartTime` madde sayısını tanımlamak [için,](how-to-configure-change-feed-start-time.md)kullanım için ve aralıklarla güncelleştirilmelidir.
1. Üzerinde yapılandırılan `ChangeFeedProcessorOptions.LeasePrefix`veya başka bir şekilde `string.Empty` kullanılacak değeri eşleşecek şekilde ayarlayın. `processorName` `GetChangeFeedProcessorBuilder`
1. Değişiklikler artık bir `IReadOnlyList<Document>`, bunun yerine, tanımlamanız `IReadOnlyCollection<T>` `T` gereken bir tür olarak teslim edilir, artık hiçbir temel madde sınıfı yoktur.
1. Değişiklikleri işlemek için artık bir uygulamaya ihtiyacınız yok, bunun yerine [bir temsilci tanımlamanız](change-feed-processor.md#implementing-the-change-feed-processor)gerekir. Temsilci statik bir Işlev olabilir veya yürütmeler arasında durumu korumanız gerekiyorsa, kendi sınıfınızı oluşturabilir ve bir örnek yöntemini temsilci olarak geçirebilirsiniz.

Örneğin, değişiklik akışı işlemcisini oluşturmak için orijinal kod aşağıdaki gibi görünüyorsa:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=ChangeFeedProcessorLibrary)]

Geçirilen kod aşağıdaki gibi görünecektir:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=ChangeFeedProcessorMigrated)]

Ve temsilci, statik bir yöntem olabilir:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=Delegate)]

## <a name="state-and-lease-container"></a>Devlet ve kira konteyneri

Akış işlemcisini değiştir kitaplığına benzer şekilde, .NET V3 SDK'daki değişiklik akışı özelliği, durumu depolamak için bir [kira kapsayıcısı](change-feed-processor.md#components-of-the-change-feed-processor) kullanır. Ancak, şemafarklıdır.

SDK V3 değiştirme besleme işlemcisi herhangi bir eski kitaplık durumunu algılar ve geçirilen uygulama kodunun ilk yürütülmesinden sonra otomatik olarak yeni şemaya geçirilir. 

Eski kodu kullanarak uygulamayı güvenli bir şekilde durdurabilir, kodu yeni sürüme geçirebilirsiniz, geçirilen uygulamayı başlatabilirsiniz ve uygulama durdurulurken gerçekleşen tüm değişiklikler yeni sürüm tarafından alınır ve işlenir.

> [!NOTE]
> Kitaplığı kullanan uygulamalardan .NET V3 SDK'ya geçişler tek yönlüolduğundan, durum (kiralamalar) yeni şemaya geçirilecektir. Geçiş geriye dönük uyumlu değil.


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)
* [GitHub'da kullanım örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [GitHub'da ek örnekler](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki makalelerde besleme işlemcisi değiştir hakkında daha fazla bilgi edinmek için devam edebilirsiniz:

* [Değişiklik besleme işlemcisine genel bakış](change-feed-processor.md)
* [Değişiklik akışı tahmin aracını kullanma](how-to-use-change-feed-estimator.md)
* [Değişiklik akışı işlemcisi başlangıç zamanı](how-to-configure-change-feed-start-time.md)
