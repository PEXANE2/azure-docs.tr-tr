---
title: Değişiklik akışı tahmini Azure Cosmos DB kullanın
description: Değişiklik akışı işlemcinizin ilerlemesini çözümlemek için değişiklik akışı tahmin aracı 'ı nasıl kullanacağınızı öğrenin
author: ealsur
ms.service: cosmos-db
ms.topic: sample
ms.date: 08/15/2019
ms.author: maquaran
ms.openlocfilehash: 582b0a586f830659bbea4dd665be938d2895de47
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69544834"
---
# <a name="use-the-change-feed-estimator"></a>Değişiklik akışı tahmin aracı 'ı kullanın

Bu makalede değişiklik akışını okurken [değişiklik akışı işlemci](./change-feed-processor.md) örneklerinizin ilerlemesini nasıl izleyebileceğinizi açıklar.

## <a name="why-is-monitoring-progress-important"></a>İzleme ilerleme durumu neden önemlidir?

Değişiklik akışı işlemcisi, [değişiklik akışınızda](./change-feed.md) ileri doğru hareket eden bir işaretçi görevi görür ve değişiklikleri bir temsilci uygulamasına gönderir. 

Değişiklik akışı işlemcisi dağıtımı, CPU, bellek, ağ gibi kullanılabilir kaynaklara bağlı olarak belirli bir hızda değişiklik işleyebilir.

Bu oran, değişikliklerinizin Azure Cosmos DB kapsayıcıda gerçekleştiği orandan daha yavaşsa, işlemciniz geri gecikme için başlar.

Bu senaryonun belirlenmesi, değişiklik akışı işlemci dağıtımınızı ölçeklendirmemiz gerektiğini anlamanıza yardımcı olur.

## <a name="implement-the-change-feed-estimator"></a>Değişiklik akışı tahmin aracı 'ı uygulama

[Değişiklik akışı işlemcisi](./change-feed-processor.md)gibi değişiklik akışı tahmini, bir gönderme modeli olarak da kullanılır. Tahmin Aracı, son işlenen öğe (kiralamalar kapsayıcısının durumu tarafından tanımlanır) ve kapsayıcıda en son değişiklik arasındaki farkı ölçecek ve bu değeri bir temsilciye göndermeyecektir. Ölçümün alındığı zaman aralığı, varsayılan değer olan 5 saniyeye de özelleştirilebilir.

Örnek olarak, değişiklik akışı işlemciniz aşağıdaki gibi tanımlanır:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=StartProcessorEstimator)]

İşlemcinin şu şekilde kullanılması `GetChangeFeedEstimatorBuilder` için bir tahmin aracı başlatmanın doğru yolu:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=StartEstimator)]

Hem işlemcinin hem de tahmin aracı aynı adı `leaseContainer` ve aynı adı paylaşır.

Diğer iki parametre, işlemci tarafından **okunmayı bekleyen kaç değişiklik** olduğunu ve bu ölçümün alınmasını istediğiniz zaman aralığını temsil eden bir sayı alacak olan temsilcidir.

Tahmini alan bir temsilci örneği şunlardır:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=EstimationDelegate)]

Bu tahmini, izleme çözümünüze gönderebilir ve zaman içinde ilerleme durumunun nasıl davrandığınızı anlamak için kullanabilirsiniz.

> [!NOTE]
> Değişiklik akışı tahmin aracı 'ın, değişiklik akışı işlemcinizin bir parçası olarak dağıtılması veya aynı projenin bir parçası olması gerekmez. Bağımsız olabilir ve tamamen farklı bir örnekte çalıştırılabilir. Yalnızca aynı adı ve kira yapılandırmasını kullanması gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)
* [GitHub 'da kullanım örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/usage/changefeed)
* [GitHub 'da ek örnekler](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler de akış değiştirme hakkında daha fazla bilgi edinmek için şimdi geçebilirsiniz:

* [Değişiklik akışına genel bakış](change-feed.md)
* [Değişiklik akışını okuma yolları](read-change-feed.md)
* [Değişiklik akışı işlemcisini kullanma](change-feed-processor.md)
