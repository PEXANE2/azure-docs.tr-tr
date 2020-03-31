---
title: Değişiklik akışı işlemcisi başlangıç saati nasıl yapılandırılabilen - Azure Cosmos DB
description: Belirli bir tarih ve tarihten okumaya başlamak için değişiklik akışı işlemcisini nasıl yapılandırıştırmayı öğrenin
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: maquaran
ms.openlocfilehash: 600556a06d3f58c4d2ec79a49fdee5e8e04d4036
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586283"
---
# <a name="how-to-configure-the-change-feed-processor-start-time"></a>Değişim beslemesi işlemcisinin başlangıç saatini yapılandırma

Bu makalede, değişiklik akışı [işlemcisini](./change-feed-processor.md) belirli bir tarih ve tarihten okumaya başlamak üzere nasıl yapılandırabileceğiniz açıklanmaktadır.

## <a name="default-behavior"></a>Varsayılan davranış

Bir değişiklik beslemesi işlemcisi ilk kez başlatıldığında, kira kapsayıcısını başlatacak ve [işleme yaşam döngüsünü](./change-feed-processor.md#processing-life-cycle)başlatacaktır. Değişiklik akışı işlemcisi ilk kez başlatılmasından önce kapsayıcıda gerçekleşen değişiklikler algılanmıyor.

## <a name="reading-from-a-previous-date-and-time"></a>Önceki tarih ve saate göre okuma

Belirli bir **tarih ve saatte**başlayan değişiklikleri okumak için değişiklik beslemesi işlemcisini `DateTime` `WithStartTime` başlatarak, oluşturucu uzantısına bir örnek geçirerek:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=TimeInitialization)]

Değişiklik akışı işlemcisi, belirli bir tarih ve saat için başlatılacaktır ve sonrasında gerçekleşen değişiklikleri okumaya başlar.

## <a name="reading-from-the-beginning"></a>Baştan okuma

Veri geçişleri veya bir kapsayıcının tüm geçmişini çözümleme gibi diğer senaryolarda, **bu kapsayıcının yaşam süresinin başından**itibaren değişiklik akışını okumamız gerekir. Bunu yapmak için, `WithStartTime` oluşturucu uzantısı kullanabilirsiniz, ancak geçen `DateTime.MinValue.ToUniversalTime()`, minimum `DateTime` değerUTC temsili oluşturacak, gibi:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=StartFromBeginningInitialization)]

Değişiklik beslemesi işlemcisi başlatılacaktır ve kapsayıcının kullanım ömrü başından itibaren değişiklikleri okumaya başlar.

> [!NOTE]
> Bu özelleştirme seçenekleri yalnızca değişiklik akışı işlemcisinin zamanındaki başlangıç noktasını ayarlamak için çalışır. Kiralama kapsayıcısı ilk kez başharfe bulaştığında, bunları değiştirmenin bir etkisi olmaz.

> [!NOTE]
> Zaman içinde bir nokta belirtirken, yalnızca kapsayıcıda şu anda bulunan öğeler için değişiklikler okunur. Bir öğe silinmişse, Değişiklik Yayını'ndaki geçmişi de kaldırılır.

## <a name="additional-resources"></a>Ek kaynaklar

* [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)
* [GitHub'da kullanım örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [GitHub'da ek örnekler](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki makalelerde besleme işlemcisi değiştir hakkında daha fazla bilgi edinmek için devam edebilirsiniz:

* [Değişiklik besleme işlemcisine genel bakış](change-feed-processor.md)
* [Değişiklik akışı tahmin aracını kullanma](how-to-use-change-feed-estimator.md)
