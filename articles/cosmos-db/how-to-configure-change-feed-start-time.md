---
title: Değişiklik akışı işlemcisi başlangıç zamanı-Azure Cosmos DB nasıl yapılandırılır
description: Değişiklik akışı işlemcisinin belirli bir tarih ve saatten okumaya başlaması için nasıl yapılandırılacağını öğrenin
author: ealsur
ms.service: cosmos-db
ms.topic: how-to
ms.date: 08/13/2019
ms.author: maquaran
ms.openlocfilehash: d3d9af1b8cbda3f0fa2ff4650fef4602f7812f79
ms.sourcegitcommit: 635114a0f07a2de310b34720856dd074aaf4f9cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85261724"
---
# <a name="how-to-configure-the-change-feed-processor-start-time"></a>Değişiklik akışı işlemcisi başlangıç saatini yapılandırma

Bu makalede, [değişiklik akışı işlemcisini](./change-feed-processor.md) belirli bir tarih ve saatten okumayı başlatacak şekilde nasıl yapılandırabileceğiniz açıklanmaktadır.

## <a name="default-behavior"></a>Varsayılan davranış

Bir değişiklik akışı işlemcisi ilk kez başladığında, kira kapsayıcısını başlatır ve [işlem yaşam döngüsünü](./change-feed-processor.md#processing-life-cycle)başlatır. Değişiklik akışı işlemcisi ilk kez başlatılmadan önce kapsayıcıda gerçekleşen tüm değişiklikler algılanmaz.

## <a name="reading-from-a-previous-date-and-time"></a>Önceki bir tarih ve saatten okuma

Değişiklik akışı işlemcisini, **belirli bir tarih ve saatte**başlayan değişiklikleri okumak için, bir a örneğini `DateTime` Oluşturucu uzantısına geçirerek başlatmak mümkündür `WithStartTime` :

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=TimeInitialization)]

Değişiklik akışı işlemcisi, belirli bir tarih ve saat için başlatılır ve sonrasında gerçekleşen değişiklikleri okumaya başlar.

## <a name="reading-from-the-beginning"></a>Baştan itibaren okunuyor

Veri geçişleri veya bir kapsayıcının tüm geçmişinin çözümlenmesi gibi diğer senaryolarda, **Bu kapsayıcının yaşam süresinden itibaren**değişiklik akışını okuduk. Bunu yapmak için, Oluşturucu uzantısı üzerinde kullanabiliriz, ancak bu, `WithStartTime` `DateTime.MinValue.ToUniversalTime()` En düşük değerin UTC gösterimini oluşturan, `DateTime` şöyle olabilir:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=StartFromBeginningInitialization)]

Değişiklik akışı işlemcisi başlatılır ve kapsayıcının yaşam süresinden itibaren yapılan değişiklikleri okumaya başlar.

> [!NOTE]
> Bu özelleştirme seçenekleri yalnızca değişiklik akışı işlemcisinin başlangıç noktasını ayarlamaya çalışır. Kiralamalar kapsayıcısı ilk kez başlatıldıktan sonra, bunların değiştirilmesinin etkisi olmaz.

> [!NOTE]
> Bir zaman noktası belirtirken, yalnızca kapsayıcıda bulunan öğeler için değişiklikler okunacaktır. Bir öğe silinirse, değişiklik akışındaki geçmişi de kaldırılır.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)
* [GitHub 'da kullanım örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [GitHub 'da ek örnekler](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki makalelerde akış işlemcisini Değiştir hakkında daha fazla bilgi edinebilirsiniz:

* [Değişiklik akışı işlemcisine genel bakış](change-feed-processor.md)
* [Değişiklik akışı tahmin aracını kullanma](how-to-use-change-feed-estimator.md)
