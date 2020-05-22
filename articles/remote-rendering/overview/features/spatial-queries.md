---
title: Uzamsal sorgular
description: Sahnede uzamsal sorgular yapma
author: jakrams
ms.author: jakras
ms.date: 02/07/2020
ms.topic: article
ms.openlocfilehash: 8f64c4a9a438b07fef428a5ed044985736055525
ms.sourcegitcommit: 0690ef3bee0b97d4e2d6f237833e6373127707a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83758852"
---
# <a name="spatial-queries"></a>Uzamsal sorgular

Uzamsal sorgular, uzaktan işleme hizmetine hangi nesnelerin bir alanda konumlandırılabilir olduğunu sorabileceğiniz işlemlerdir. Uzamsal sorgular sıklıkla bir kullanıcının işaret ettiği nesneyi bulmak gibi etkileşimleri uygulamak için kullanılır.

Tüm uzamsal sorgular sunucuda değerlendirilir. Sonuç olarak, zaman uyumsuz işlemlerdir ve sonuçlar ağ gecikme süresine bağlı bir gecikmeyle gönderilir. Her uzamsal sorgu ağ trafiği oluşturduğundan, aynı anda çok fazla yapamamaya dikkat edin.

## <a name="collision-meshes"></a>Çakışma kafesleri

Uzamsal sorgular [Havok fizik](https://www.havok.com/products/havok-physics) altyapısı tarafından desteklenir ve özel bir çakışma ağı bulunmasını gerektirir. Varsayılan olarak, [model dönüştürme](../../how-tos/conversion/model-conversion.md) çakışma kafesleri oluşturur. Karmaşık bir modelde uzamsal sorgular gerekmiyorsa, [dönüştürme seçeneklerinde](../../how-tos/conversion/configure-model-conversion.md)çakışma kafesi oluşturmayı devre dışı bırakmayı göz önünde bulundurun, çünkü bu birden çok şekilde etkiler:

* [Model dönüştürme](../../how-tos/conversion/model-conversion.md) oldukça uzun sürer.
* Dönüştürülen model dosya boyutları, önemli ölçüde daha büyük olduğundan indirme hızını etkiler.
* Çalışma zamanı yükleme süreleri daha uzun.
* Çalışma zamanı CPU bellek tüketimi daha yüksektir.
* Her model örneği için hafif bir çalışma zamanı performans yükü vardır.

## <a name="ray-casts"></a>Işın yayınları

Bir *Ray cast* , çalışma zamanının, belirli bir konumdan başlayıp belirli bir yöne işaret eden bir ışın tarafından hangi nesnelerin kesişen olduğunu denetlediğinde uzamsal bir sorgudur. Bir iyileştirme olarak, çok uzakta olan nesneleri aramak için bir en yüksek ışın uzaklığı de verilir.

```cs
async void CastRay(AzureSession session)
{
    // trace a line from the origin into the +z direction, over 10 units of distance.
    RayCast rayCast = new RayCast(new Double3(0, 0, 0), new Double3(0, 0, 1), 10);

    // only return the closest hit
    rayCast.HitCollection = HitCollectionPolicy.ClosestHit;

    RayCastHit[] hits = await session.Actions.RayCastQueryAsync(rayCast).AsTask();

    if (hits.Length > 0)
    {
        var hitObject = hits[0].HitObject;
        var hitPosition = hits[0].HitPosition;
        var hitNormal = hits[0].HitNormal;

        // do something with the hit information
    }
}
```

```cpp
void CastRay(ApiHandle<AzureSession> session)
{
    // trace a line from the origin into the +z direction, over 10 units of distance.
    RayCast rayCast;
    rayCast.StartPos = { 0, 0, 0 };
    rayCast.EndPos = { 0, 0, 1 };
    rayCast.MaxHits = 10;

    // only return the closest hit
    rayCast.HitCollection = HitCollectionPolicy::ClosestHit;

    ApiHandle<RaycastQueryAsync> castQuery = *session->Actions()->RayCastQueryAsync(rayCast);

    castQuery->Completed([](const ApiHandle<RaycastQueryAsync>& async)
    {
        std::vector<RayCastHit> hits = *async->Result();

        if (hits.size() > 0)
        {
            auto hitObject = hits[0].HitObject;
            auto hitPosition = hits[0].HitPosition;
            auto hitNormal = hits[0].HitNormal;

            // do something with the hit information
        }
    });

}
```


Üç isabet toplama modu vardır:

* **En yakın:** Bu modda, yalnızca en yakın isabet raporlanır.
* **Herhangi biri:** Tüm bilmeniz için bu modu tercih etmek istediğiniz zaman, bir ışın her şeyi vurmasına, ancak tam olarak isabet *duymadığını dikkate almamasını* sağlar. Bu sorgu, değerlendirmek için önemli ölçüde olabilir, ancak aynı zamanda yalnızca birkaç uygulama içerir.
* **Tümü:** Bu modda, ışın üzerinde tüm isabetler raporlanır ve uzaklığa göre sıralanır. Gerçekten ilk isabetden daha fazlasına ihtiyaç duymadığınız takdirde bu modu kullanmayın. Bildirilen isabetlerin sayısını `MaxHits` seçeneğiyle sınırlayın.

Nesneleri seçmeli bir şekilde kabul edilmeden dışlamak için, [HierarchicalStateOverrideComponent](override-hierarchical-state.md) bileşeni kullanılabilir.

<!--
The CollisionMask allows the query to consider or ignore some objects based on their collision layer. If an object has layer L, it will be hit only if the mask has bit L set.
It is useful in case you want to ignore objects, for instance when setting an object transparent, and trying to select another object behind it.
TODO : Add an API to make that possible.
-->

### <a name="hit-result"></a>İsabet sonucu

Bir Ray cast sorgusunun sonucu bir isabet dizisidir. Bir nesne isabet ettirilse dizi boştur.

Bir Isabet aşağıdaki özelliklere sahiptir:

* **Hitentity:** Hangi [varlığa](../../concepts/entities.md) ulaşıldı.
* **Alt partid:** Bir [Meshcomponent](../../concepts/meshes.md)'ta hangi *alt kafesde* ulaşıldı. , İçinde dizin kurmak `MeshComponent.UsedMaterials` ve bu noktada [malzemenin](../../concepts/materials.md) aramak için kullanılabilir.
* **Hitposition:** Ray 'un nesnenin kesişen bulunduğu dünya alanı konumu.
* **Hitnormal:** Dünyanın her yerindeki Dünya alanı, kesişme konumunun normal yüzeyi.
* **DistanceToHit:** Işın başlangıç konumundan isabetden uzaklığı.

## <a name="next-steps"></a>Sonraki adımlar

* [Nesne sınırları](../../concepts/object-bounds.md)
* [Hiyerarşik durumları geçersiz kılma](override-hierarchical-state.md)
