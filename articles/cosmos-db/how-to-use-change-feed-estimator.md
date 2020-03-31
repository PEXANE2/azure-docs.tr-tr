---
title: Değişiklik akışı tahmincisi kullanın - Azure Cosmos DB
description: Değişim akışı işlemcinizin ilerlemesini analiz etmek için değişim akışı tahmincisinin nasıl kullanılacağını öğrenin
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/15/2019
ms.author: maquaran
ms.openlocfilehash: 0023f68400b36b9abd3b9d4a789895e79f67aa03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77585297"
---
# <a name="use-the-change-feed-estimator"></a>Değişiklik akışı tahmincisi kullanma

Bu makalede, değişiklik akışı akışı okurken [değişiklik akışı işlemci](./change-feed-processor.md) örneklerinin ilerlemesini nasıl izleyebileceğinizi açıklayınız.

## <a name="why-is-monitoring-progress-important"></a>İlerlemenin izlenmesi neden önemlidir?

Değişiklik akışı işlemcisi, [değişiklik akışınızda](./change-feed.md) ilerleyen ve değişiklikleri bir temsilci uygulamasına teslim eden bir işaretçi gibi davranır. 

Değişiklik akışı işlemci dağıtımınız, CPU, bellek, ağ gibi kullanılabilir kaynaklarına bağlı olarak değişiklikleri belirli bir hızda işleyebilir.

Bu oran Azure Cosmos kapsayıcınızda değişikliklerinizin gerçekleşme hızından daha yavaşsa, işlemciniz geride kalmaya başlar.

Bu senaryoyu tanımlamak, akış işlemcisi dağıtımımızı ölçeklendirmemiz gerekip gerekmeden anlamaya yardımcı olur.

## <a name="implement-the-change-feed-estimator"></a>Değişiklik akışı tahmincisi uygulama

Değişiklik [beslemeişlemcisi](./change-feed-processor.md)gibi, değişiklik besleme tahmincisi de itme modeli olarak çalışır. Tahminci, son işlenen madde (kira kapsayıcısının durumuyla tanımlanan) ile kapsayıcıdaki en son değişiklik arasındaki farkı ölçer ve bu değeri bir temsilciye iter. Ölçümün alındığı aralık, varsayılan değeri 5 saniye olan özelleştirilebilir.

Örnek olarak, değişiklik beslemeişlemciniz şu şekilde tanımlanırsa:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=StartProcessorEstimator)]

İşlemcinin bu şekilde kullandığını `GetChangeFeedEstimatorBuilder` ölçmek için bir tahmincinin başlatılmasının doğru yolu:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=StartEstimator)]

Hem işlemcinin hem de tahmincinin aynı `leaseContainer` ve aynı adı paylaştığı yer.

Diğer iki parametre, işlemci tarafından **okunmayı bekleyen kaç değişiklik** olduğunu ve bu ölçümün alınmasını istediğiniz zaman aralığını temsil eden bir sayı alan temsilcidir.

Tahmini alan bir temsilciörneği:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=EstimationDelegate)]

Bu tahmini izleme çözümünüze gönderebilir ve ilerlemenizin zaman içinde nasıl davrandığını anlamak için kullanabilirsiniz.

> [!NOTE]
> Değişiklik akışı tahmincisinin, değişiklik akışı işlemcinizin bir parçası olarak dağıtılması veya aynı projenin bir parçası olması gerekmez. Bağımsız olabilir ve tamamen farklı bir durumda çalıştırAbilirsiniz. Sadece aynı adı ve kiralama yapılandırması kullanmanız gerekir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)
* [GitHub'da kullanım örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [GitHub'da ek örnekler](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki makalelerde besleme işlemcisi değiştir hakkında daha fazla bilgi edinmek için devam edebilirsiniz:

* [Değişiklik besleme işlemcisine genel bakış](change-feed-processor.md)
* [Değişiklik akışı işlemcisi başlangıç zamanı](how-to-configure-change-feed-start-time.md)
