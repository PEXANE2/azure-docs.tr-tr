---
title: Nesne ve kaynak ömrü
description: Farklı türler için yaşam boyu yönetimini açıklar
author: jakrams
ms.author: jakras
ms.date: 02/06/2020
ms.topic: conceptual
ms.openlocfilehash: d031ff4a6ee86da2843f0f18ac428c50f7cfc121
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681876"
---
# <a name="object-and-resource-lifetime"></a>Nesne ve kaynak ömrü

Azure Uzaktan İşleme iki tür arasında ayrım sağlar: **nesneler** ve **kaynaklar.**

## <a name="object-lifetime"></a>Nesne ömrü

*Nesneler,* kullanıcının kendi takdirine bağlı olarak oluşturabileceği, değiştirebileceği ve yok edebileceği şeyler olarak kabul edilir. Nesneler serbestçe çoğaltılabilir ve her örnek zaman içinde mutasyona uğrayabilir. Sonuç olarak [varlıklar](entities.md) ve [bileşenler](components.md) nesnelerdir.

Nesnelerin ömrü tamamen kullanıcı denetimi altındadır. Ancak, istemci tarafı temsil ömrü ile ilgili değildir. Uzak `Entity` işleme `Component` ana `Destroy` bilgisayarındaki nesneyi bulmak için çağrılması gereken bir işlev gibi ve sahip olunması gereken sınıflar. Ayrıca, `Entity.Destroy()` varlığı, onun çocuklarını ve bu hiyerarşideki tüm bileşenleri yok edecektir.

## <a name="resource-lifetime"></a>Kaynak ömrü

*Kaynaklar,* ömrü tamamen uzaktan görüntü ana bilgisayarı tarafından yönetilen şeylerdir. Kaynaklar dahili olarak sayılır. Artık kimse onlara atıfta bulunmadığında görevden alınmıyor.

Çoğu kaynak yalnızca dolaylı olarak, genellikle bir dosyadan yükleyerek oluşturulabilir. Aynı dosya birden çok kez yüklendiğinde, Azure Uzaktan İşleme aynı başvuruyu döndürecek ve verileri yeniden yüklemez.

Birçok kaynak değişmez, örneğin [meshes](meshes.md) ve [dokular.](textures.md) Bazı kaynaklar, örneğin [malzemeler](materials.md)için olsa da, mutable vardır. Kaynaklar sık sık paylaşıldığından, kaynağın değiştirilmesi birden çok nesneyi etkileyebilir. Örneğin, bir malzemenin rengini değiştirmek, meshes kullanan tüm nesnelerin rengini değiştirir ve bu da bu malzemeye başvurur.

### <a name="built-in-resources"></a>Yerleşik kaynaklar

Azure Uzaktan İşleme, `builtin://` `AzureSession.Actions.LoadXYZAsync()`''ye çağrı sırasında ilgili tanımlayıcıları önceden taranarak yüklenebilen bazı yerleşik kaynaklar içerir. Kullanılabilir yerleşik kaynaklar, ilgili her özellik için belgelerde listelenir. Örneğin, [gökyüzü bölümü](../overview/features/sky.md) yerleşik gökyüzü dokularını listeler.

## <a name="general-lifetime"></a>Genel yaşam süresi

Tüm nesnelerin ve kaynakların ömrü bağlantıya bağlıdır. Bağlantıyı kesmek üzerine her şey atılır. Aynı oturuma yeniden bağlanırken, sahne grafiği boş olur ve tüm kaynaklar temizlenir.

Uygulamada, aynı kaynağı bir oturuma yüklemek, bir bağlantı kesildikten sonra, genellikle ilk seferden daha hızlıdır. Bu durum, çoğu kaynağın ilk kez Azure Depolama'dan indirilmesi gerektiğinden ve ikinci kez gerekli olmadığı için önemli ölçüde zaman tasarrufu sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Varlıklar](entities.md)
* [Bileşenler](components.md)
* [Modeller](models.md)
