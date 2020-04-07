---
title: Varlıklar
description: Azure Uzaktan İşleme API'si kapsamındaki varlıkların tanımı
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: conceptual
ms.openlocfilehash: d7b9ecd048b080ae0ec9fd3fb7a4fb35009551b8
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681954"
---
# <a name="entities"></a>Varlıklar

*Varlık,* uzayda taşınabilir bir nesneyi temsil eder ve uzaktan işlenen içeriğin temel yapı taşıdır.

## <a name="entity-properties"></a>Varlık özellikleri

Varlıklar bir konum, döndürme ve ölçek tarafından tanımlanan bir dönüştürme var. Tek başlarına varlıkların herhangi bir gözlemlenebilir işlevselliği yoktur. Bunun yerine, davranış varlıklara bağlı bileşenler aracılığıyla eklenir. Örneğin, [Bir CutPlaneBileşeni](../overview/features/cut-planes.md) takmak varlığın konumunda bir kesme düzlemoluşturur.

Varlığın kendisinin en önemli yönü hiyerarşi ve ortaya çıkan hiyerarşik dönüşümdür. Örneğin, paylaşılan bir ana varlığa birden çok varlık alt öğe olarak eklendiğinde, bu varlıkların tümü üst varlığın dönüşümünün değiştirilmesiyle birlikte taşınabilir, döndürülebilir ve ölçeklenebilir.

Bir varlık benzersiz bir üst tarafından sahip olunan, `Entity.Destroy()`yani üst ile yok olduğunda , böylece çocukları ve tüm bağlı [bileşenleri](components.md)vardır. Böylece, sahneden bir model kaldırma bir `Destroy` modelin kök düğümü çağırarak gerçekleştirilir, tarafından `AzureSession.Actions.LoadModelAsync()` döndürülen veya SAS varyantı `AzureSession.Actions.LoadModelFromSASAsync()`.

Varlıklar, sunucu içeriği yüklendiğinde veya kullanıcı sahneye bir nesne eklemek istediğinde oluşturulur. Örneğin, bir kullanıcı bir kafesin içini görselleştirmek için kesik bir düzlem eklemek isterse, kullanıcı düzlemin olması gereken bir varlık oluşturabilir ve ardından kesme düzlembileşenini buna ekleyebilir.

## <a name="query-functions"></a>Sorgu işlevleri

Varlıklar üzerinde iki tür sorgu işlevi vardır: senkron ve eşzamanlı çağrılar. Senkron sorgular yalnızca istemcide bulunan ve çok fazla hesaplama içermeyen veriler için kullanılabilir. Örnekler bileşenleri, göreli nesne dönüşümleri veya üst/alt ilişkileri için sorgu vardır. Eşkenar dört bir hata, yalnızca sunucuda bulunan veya istemcide çalıştırılamayacak kadar pahalı olacak ekstra hesaplama içeren veriler için kullanılır. Örnekler uzamsal sınır sorguları veya meta veri sorgularıdır.

### <a name="querying-components"></a>Bileşenleri sorgulama

Belirli bir türdeki bir bileşeni `FindComponentOfType`bulmak için şunları kullanın:

```cs
CutPlaneComponent cutplane = (CutPlaneComponent)entity.FindComponentOfType(ObjectType.CutPlaneComponent);

// or alternatively:
CutPlaneComponent cutplane = entity.FindComponentOfType<CutPlaneComponent>();
```

### <a name="querying-transforms"></a>Dönüşümleri sorgulama

Dönüştürme sorguları nesneüzerinde eşzamanlı çağrılardır. API'de sorgulandırılabilen dönüşümlerin nesnenin üst öğesine göre yerel alan dönüşümleri olduğunu belirtmek önemlidir. Özel durumlar, yerel alan ve dünya alanının aynı olduğu kök nesnelerdir.

> [!NOTE]
> Rasgele nesnelerin dünya alanı dönüşümsorgulamak için özel bir API yoktur.

```cs
// local space transform of the entity
Double3 translation = entity.Position;
Quaternion rotation = entity.Rotation;
```

### <a name="querying-spatial-bounds"></a>Uzamsal sınırları sorgulama

Bounds sorguları, bir varlığı kök olarak kullanarak tam nesne hiyerarşisi üzerinde çalışan eşzamanlı çağrılardır. Nesne sınırları yla ilgili özel [bölüme](object-bounds.md)bakın.

### <a name="querying-metadata"></a>Meta verileri sorgulama

Meta veriler, sunucu tarafından yoksayılabilen nesnelerüzerinde depolanan ek verilerdir. Nesne meta verileri aslında _değer_ sayısal, boolean veya dize türü olabilir (ad, değer) çiftleri kümesidir. Meta veriler modelle dışa aktarılabilir.

Meta veri sorguları belirli bir varlık üzerinde eşzamanlı çağrılar vardır. Sorgu, bir alt grafiğin birleştirilmiş bilgilerini değil, yalnızca tek bir varlığın meta verilerini döndürür.

```cs
MetadataQueryAsync metaDataQuery = entity.QueryMetaDataAsync();
metaDataQuery.Completed += (MetadataQueryAsync query) =>
{
    if (query.IsRanToCompletion)
    {
        ObjectMetaData metaData = query.Result;
        ObjectMetaDataEntry entry = metaData.GetMetadataByName("MyInt64Value");
        System.Int64 intValue = entry.AsInt64;

        // ...
    }
};
```

Nesne herhangi bir meta veri tutmasa bile sorgu başarılı olur.

## <a name="next-steps"></a>Sonraki adımlar

* [Bileşenler](components.md)
* [Nesne sınırları](object-bounds.md)
