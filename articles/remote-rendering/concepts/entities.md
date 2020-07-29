---
title: Varlıklar
description: Azure uzaktan Işleme API 'sinin kapsamındaki varlıkların tanımı
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: conceptual
ms.openlocfilehash: 5f6f7fc52a186117afcb92f6a2f80bf068e50ab9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84509211"
---
# <a name="entities"></a>Varlıklar

Bir *varlık* , boşluk alanındaki taşınabilir bir nesneyi temsil eder ve uzaktan işlenmiş içeriğin temel yapı taşıdır.

## <a name="entity-properties"></a>Varlık özellikleri

Varlıkların bir konum, döndürme ve ölçeğe göre tanımlanan bir dönüşümü vardır. Kendilerine ait varlıkların herhangi bir observable işlevselliği yoktur. Bunun yerine, davranış, varlıklara eklenen bileşenler aracılığıyla eklenir. Örneğin, bir [CutPlaneComponent](../overview/features/cut-planes.md) iliştirmek varlığın konumunda kesilmiş bir düzlem oluşturacaktır.

Varlığın kendisinin en önemli yönü hiyerarşinin ve sonuçta elde edilen sıradüzenli dönüşümüdür. Örneğin, birden çok varlık paylaşılan bir üst varlığa alt öğe olarak eklendiğinde, bu varlıkların tümü uyum içinde taşınabilir, döndürülebilir ve üst varlığın dönüşümü değiştirilerek ölçekleniyor.

Bir varlık kendi üst öğesine sahip olduğu anlamına gelir, yani üst öğe ile yok edildiğinde `Entity.Destroy()` , alt öğeleri ve tüm bağlı [Bileşenler](components.md). Bu nedenle, bir modelin sahne alanından kaldırılması `Destroy` , bir modelin kök düğümüne çağırarak `AzureSession.Actions.LoadModelAsync()` veya bunun SAS varyantı tarafından döndürülen bir şekilde gerçekleştirilir `AzureSession.Actions.LoadModelFromSASAsync()` .

Varlıklar, sunucu içerik yüklediğinde veya Kullanıcı sahneye nesne eklemek istediğinde oluşturulur. Örneğin, bir Kullanıcı bir kafesin iç kısmını görselleştirmek için kesilmiş bir düzlem eklemek isterse, Kullanıcı düzlemin mevcut olması gereken bir varlık oluşturabilir ve ardından bu nesneye kesilen düzlemi bileşenini ekler.

## <a name="create-an-entity"></a>Varlık oluşturma

Sahneye yeni bir varlık eklemek için örneğin, modeli yüklemek veya ona bileşenleri eklemek üzere bir kök nesne olarak geçirmek için, aşağıdaki kodu kullanın:

```cs
Entity CreateNewEntity(AzureSession session)
{
    Entity entity = session.Actions.CreateEntity();
    entity.Position = new LocalPosition(1, 2, 3);
    return entity;
}
```

```cpp
ApiHandle<Entity> CreateNewEntity(ApiHandle<AzureSession> session)
{
    ApiHandle<Entity> entity(nullptr);
    if (auto entityRes = session->Actions()->CreateEntity())
    {
        entity = entityRes.value();
        entity->Position(Double3{ 1, 2, 3 });
        return entity;
    }
    return entity;
}
```

## <a name="query-functions"></a>Sorgu işlevleri

Varlıklarda iki tür sorgu işlevi vardır: zaman uyumlu ve zaman uyumsuz çağrılar. Zaman uyumlu sorgular yalnızca istemcide bulunan ve çok fazla hesaplama içermeyen veriler için kullanılabilir. Örnekler, bileşenler, göreli nesne dönüştürmeleri veya üst/alt ilişkileri sorgular. Zaman uyumsuz sorgular yalnızca sunucuda bulunan veya istemcide çok pahalı olan ek hesaplamayı içeren veriler için kullanılır. Örnekler, uzamsal sınır sorguları veya meta veri sorgulardır.

### <a name="querying-components"></a>Bileşenler sorgulanıyor

Belirli bir türün bileşenini bulmak için şunu kullanın `FindComponentOfType` :

```cs
CutPlaneComponent cutplane = (CutPlaneComponent)entity.FindComponentOfType(ObjectType.CutPlaneComponent);

// or alternatively:
CutPlaneComponent cutplane = entity.FindComponentOfType<CutPlaneComponent>();
```

```cpp
ApiHandle<CutPlaneComponent> cutplane = entity->FindComponentOfType(ObjectType::CutPlaneComponent)->as<CutPlaneComponent>();

// or alternatively:
ApiHandle<CutPlaneComponent> cutplane = *entity->FindComponentOfType<CutPlaneComponent>();
```

### <a name="querying-transforms"></a>Dönüşümler sorgulanıyor

Dönüştürme sorguları nesnedeki zaman uyumlu çağrılardır. API aracılığıyla sorgulanan dönüşümlerinin, nesnenin üst öğesiyle ilişkili olan yerel alan dönüştürmelerini aklınızda olması önemlidir. Özel durumlar, yerel alan ve dünya alanının aynı olduğu kök nesnelerdir.

> [!NOTE]
> Rastgele nesnelerin dünya alanı dönüşümünü sorgulamak için adanmış bir API yok.

```cs
// local space transform of the entity
Double3 translation = entity.Position;
Quaternion rotation = entity.Rotation;
```

```cpp
// local space transform of the entity
Double3 translation = *entity->Position();
Quaternion rotation = *entity->Rotation();
```


### <a name="querying-spatial-bounds"></a>Uzamsal sınırları sorgulama

Sınır sorguları, bir varlığı kök olarak kullanan tam bir nesne hiyerarşisinde çalışan zaman uyumsuz çağrılardır. [Nesne sınırları](object-bounds.md)hakkında adanmış bölüme bakın.

### <a name="querying-metadata"></a>Meta veriler sorgulanıyor

Meta veriler, nesneler üzerinde depolanan ve sunucu tarafından yoksayılan ek verilere sahiptir. Nesne meta verileri aslında _değer_ sayısal, Boolean veya dize türünde olabilecek bir dizi (ad, değer) çiftleridir. Meta veriler modelle birlikte verilebilirler.

Meta veri sorguları belirli bir varlıkta zaman uyumsuz çağrılardır. Sorgu, bir alt grafiğin birleştirilmiş bilgisini değil, yalnızca tek bir varlığın meta verilerini döndürür.

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

```cpp
ApiHandle<MetadataQueryAsync> metaDataQuery = *entity->QueryMetaDataAsync();
metaDataQuery->Completed([](const ApiHandle<MetadataQueryAsync>& query)
    {
        if (query->IsRanToCompletion())
        {
            ApiHandle<ObjectMetaData> metaData = *query->Result();
            ApiHandle<ObjectMetaDataEntry> entry = *metaData->GetMetadataByName("MyInt64Value");
            int64_t intValue = *entry->AsInt64();

            // ...
        }
    });
```

Nesne herhangi bir meta veri tutmasa bile sorgu başarılı olur.

## <a name="next-steps"></a>Sonraki adımlar

* [Bileşenler](components.md)
* [Nesne sınırları](object-bounds.md)
