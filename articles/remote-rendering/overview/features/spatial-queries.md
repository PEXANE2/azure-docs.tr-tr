---
title: Uzamsal sorgular
description: Sahnede uzamsal sorgular nasıl yapılır?
author: jakrams
ms.author: jakras
ms.date: 02/07/2020
ms.topic: article
ms.openlocfilehash: 9a981aeb08ec46900994fd599b592b9f16034f34
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80680537"
---
# <a name="spatial-queries"></a>Uzamsal sorgular

Uzamsal sorgular, bir alanda hangi nesnelerin bulunduğu uzaktan işleme hizmetine sorabileceğiniz işlemlerdir. Uzamsal sorgular, kullanıcının işaret ettiği nesneyi bulmak gibi etkileşimleri uygulamak için sıklıkla kullanılır.

Tüm uzamsal sorgular sunucuda değerlendirilir. Sonuç olarak bunlar eşzamanlı işlemlerdir ve sonuçlar ağınızın gecikme durumuna bağlı olarak bir gecikmeyle gelecektir. Her uzamsal sorgu ağ trafiği oluşturduğundan, aynı anda çok fazla yapmamaya dikkat edin.

## <a name="collision-meshes"></a>Çarpışma meshes

Uzamsal sorgular [Havok Fizik](https://www.havok.com/products/havok-physics) motoru tarafından desteklenmektedir ve mevcut olması için özel bir çarpışma örgü gerektirir. Varsayılan olarak, [model dönüştürme](../../how-tos/conversion/model-conversion.md) çakışma meshes oluşturur. Karmaşık bir modelde uzamsal sorgular gerektirmezseniz, birden çok yönden etkisi [olduğundan, dönüşüm seçeneklerinde](../../how-tos/conversion/configure-model-conversion.md)çakışma kafesi oluşturmayı devre dışı bırakmayı düşünün:

* [Model dönüştürme](../../how-tos/conversion/model-conversion.md) çok daha uzun sürer.
* Dönüştürülmüş model dosya boyutları, indirme hızını etkileyen belirgin şekilde daha büyüktür.
* Çalışma zamanı yükleme süreleri daha uzundur.
* Çalışma zamanı CPU bellek tüketimi daha yüksektir.
* Her model örneği için hafif bir çalışma zamanı performansı yükü vardır.

## <a name="ray-casts"></a>Işın dökümleri

*Ray dökümü,* çalışma zamanının hangi nesnelerin bir ışınla kesiştiğini, belirli bir konumdan başlayıp belirli bir yöne işaret ettiğini denetlediği uzamsal bir sorgudur. Bir optimizasyon olarak, çok uzaktaki nesneleri aramamak için maksimum ışın mesafesi de verilir.

````c#
async void CastRay(AzureSession session)
{
    // trace a line from the origin into the +z direction, over 10 units of distance.
    RayCast rayCast = new RayCast(new Double3(0, 0, 0), new Double3(0, 0, 1), 10);

    // only return the closest hit
    rayCast.HitCollection = HitCollectionPolicy.ClosestHit;

    RayCastHit[] hits = await session.Actions.RayCastQueryAsync(rayCast).AsTask();

    if (hits.Length > 0)
    {
        var hitObject = hits[0].HitEntity;
        var hitPosition = hits[0].HitPosition;
        var hitNormal = hits[0].HitNormal;

        // do something with the hit information
    }
}
````

Üç isabet toplama modu vardır:

* **En yakın:** Bu modda, yalnızca en yakın isabet bildirilir.
* **Herhangi bir:** Tüm bilmek istediğiniz bir ışın bir şey vurmak *olup olmadığını* bu modu tercih, ama tam olarak vuruldu ne umurumda değil. Bu sorguyu değerlendirmek çok daha ucuz olabilir, ancak yalnızca birkaç uygulaması vardır.
* **Tümü:** Bu modda, ışın boyunca tüm isabetler bildirilir, mesafeye göre sıralanır. Gerçekten ilk hit daha fazla ihtiyacınız olmadıkça bu modu kullanmayın. Bildirilen isabet sayısını seçenekle `MaxHits` sınırlayın.

Nesneleri ışık dökümleri için seçikal olarak dikkate alınmaması için [Hiyerarşik StateOverrideComponent](override-hierarchical-state.md) bileşeni kullanılabilir.

<!--
The CollisionMask allows the quey to consider or ignore some objects based on their collision layer. If an object has layer L, it will be hit only if the mask has  bit L set.
It is useful in case you want to ignore objects, for instance when setting an object transparent, and trying to select another object behind it.
TODO : Add an API to make that possible.
-->

### <a name="hit-result"></a>Hit sonuç

Bir ışın döküm sorgusunun sonucu bir dizi isabetdir. Nesne isabet edilemeseydi dizi boştur.

A Hit aşağıdaki özelliklere sahiptir:

* **HitEntity:** Hangi [varlık](../../concepts/entities.md) vuruldu.
* **Subpartid:** Hangi *alt kafes* bir [MeshComponent](../../concepts/meshes.md)vuruldu. Bu noktada `MeshComponent.UsedMaterials` [malzemenin](../../concepts/materials.md) içine indekslemek ve bakmak için kullanılabilir.
* **HitPosition:** Işın nesneyle kesiştiği dünya uzay pozisyonu.
* **HitNormal:** Kesişme konumundaki kafesin dünya uzay yüzeyi normaldir.
* **Distancetohit:** Ray başlangıç pozisyonundan vuruşa olan uzaklığı.

## <a name="next-steps"></a>Sonraki adımlar

* [Nesne sınırları](../../concepts/object-bounds.md)
* [Hiyerarşik durumları geçersiz kılma](override-hierarchical-state.md)
