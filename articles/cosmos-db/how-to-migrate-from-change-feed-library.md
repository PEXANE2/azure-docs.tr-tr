---
title: Değişiklik akışı işlemcisi kitaplığından .NET v3 SDK Azure Cosmos DB geçirin
description: Değişiklik akışı işlemcisi kitaplığını kullanarak uygulamanızı Azure Cosmos DB SDK V3 'e geçirmeyi öğrenin
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/17/2019
ms.author: maquaran
ms.openlocfilehash: f651beb181430f65d0b4c86f285e74958f8366eb
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77588892"
---
# <a name="migrate-from-the-change-feed-processor-library-to-the-azure-cosmos-db-net-v3-sdk"></a>Değişiklik akışı işlemcisi kitaplığından .NET v3 SDK Azure Cosmos DB geçirin

Bu makalede, [değişiklik akışı işlemci kitaplığını](https://github.com/Azure/azure-documentdb-changefeedprocessor-dotnet) kullanan mevcut bir uygulamanın kodunu, .NET SDK 'sının en son sürümündeki (.net v3 SDK olarak da bilinir), [akışı Değiştir](change-feed.md) özelliğine geçirmek için gereken adımlar açıklanmaktadır.

## <a name="required-code-changes"></a>Gerekli kod değişiklikleri

.NET v3 SDK 'sının birkaç önemli değişikliği vardır ve uygulamanızı geçirmeye yönelik temel adımlar aşağıda verilmiştir:

1. `DocumentCollectionInfo` örnekleri izlenen ve kira kapsayıcıları için `Container` başvurularına dönüştürür.
1. `WithProcessorOptions` kullanan özelleştirmeler, `WithLeaseConfiguration` ve `WithPollInterval` aralıklar için, [Başlangıç saati](how-to-configure-change-feed-start-time.md)`WithStartTime` `WithMaxItems` ve en fazla öğe sayısını tanımlamak için güncelleştirilmeleri gerekir.
1. `GetChangeFeedProcessorBuilder` `processorName` `ChangeFeedProcessorOptions.LeasePrefix`yapılandırılan değerle eşleşecek şekilde ayarlayın veya aksi halde `string.Empty` kullanın.
1. Değişiklikler artık `IReadOnlyList<Document>`olarak teslim değildir, bunun yerine, `T` tanımlamanız gereken bir tür olduğu `IReadOnlyCollection<T>`, artık temel öğe sınıfı olmadığından, bu bir öğedir.
1. Değişiklikleri işlemek için artık bir uygulamaya ihtiyacınız yoktur, bunun yerine [bir temsilci tanımlamanız](change-feed-processor.md#implementing-the-change-feed-processor)gerekir. Temsilci bir statik Işlev olabilir veya yürütmeler genelinde durum korumanız gerekiyorsa, kendi sınıfınızı oluşturabilir ve temsilci olarak bir örnek yöntemi geçirebilirsiniz.

Örneğin, değişiklik akışı işlemcisini derlemek için özgün kod aşağıdaki gibi görünür:

:::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs" id="ChangeFeedProcessorLibrary":::

Geçirilen kod şöyle görünür:

:::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs" id="ChangeFeedProcessorMigrated":::

Ve temsilci bir statik yöntem olabilir:

:::code language="csharp" source="~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs" id="Delegate":::

## <a name="state-and-lease-container"></a>Eyalet ve Kiralama kapsayıcısı

Değişiklik akışı işlemci kitaplığına benzer şekilde, .NET v3 SDK 'daki değişiklik akışı özelliği, durumu depolamak için bir [Kiralama kapsayıcısı](change-feed-processor.md#components-of-the-change-feed-processor) kullanır. Ancak, şemalar farklıdır.

SDK V3 değişiklik akışı işlemcisi, eski kitaplık durumunu algılayacak ve geçirilen uygulama kodunun ilk yürütmesi üzerine otomatik olarak yeni şemaya geçirecektir. 

Eski kodu kullanarak uygulamayı güvenle durdurabilir, kodu yeni sürüme geçirebilir, geçirilen uygulamayı başlatabilir ve uygulama durdurulduğunda gerçekleşen tüm değişiklikler yeni sürüm tarafından oluşturulur ve işlenir.

> [!NOTE]
> Kitaplığı kullanan uygulamalardan .NET v3 SDK 'sına geçiş tek bir yoldur çünkü durum (kiralamalar) yeni şemaya geçirilecektir. Geçiş geriye dönük olarak uyumlu değildir.


## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)
* [GitHub 'da kullanım örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [GitHub 'da ek örnekler](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki makalelerde akış işlemcisini Değiştir hakkında daha fazla bilgi edinebilirsiniz:

* [Değişiklik akışı işlemcisine genel bakış](change-feed-processor.md)
* [Değişiklik akışı tahmin aracı 'ı kullanma](how-to-use-change-feed-estimator.md)
* [Akış işlemcisi başlangıç saatini değiştir](how-to-configure-change-feed-start-time.md)
