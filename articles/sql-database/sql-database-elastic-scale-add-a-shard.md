---
title: Elastik veritabanı araçlarını kullanarak parça ekleme
description: Bir parça kümesine yeni kırıklar eklemek için Elastik Ölçek API'leri nasıl kullanılır?
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 01/03/2019
ms.openlocfilehash: 4043fd374a314735173a1f07f46c8394592b81e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73823705"
---
# <a name="adding-a-shard-using-elastic-database-tools"></a>Elastik Veritabanı araçlarını kullanarak parça ekleme

## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Yeni bir aralık veya anahtar için parça eklemek için

Uygulamalar genellikle zaten var olan bir parça harita için, yeni anahtarlar veya anahtar aralıkları beklenen verileri işlemek için yeni kırıklar eklemek gerekir. Örneğin, Kiracı Kimliği tarafından bünyesine katfedilen bir uygulamanın yeni bir kiracı için yeni bir parça sağlaması gerekebilir veya aylık olarak biçilmiş veriler için her yeni ayın başlangıcından önce yeni bir parça sağlanması gerekebilir.

Yeni anahtar değerleri aralığı zaten varolan bir eşlemenin bir parçası değilse, yeni parçayı eklemek ve yeni anahtarı veya aralığı bu parçayla ilişkilendirmek kolaydır.

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Örnek: varolan bir parça haritaya bir parça ve aralığı ekleme

Bu örnek TryGetShard[(Java](/java/api/com.microsoft.azure.elasticdb.shard.map.shardmap.trygetshard), [.NET](https://docs.microsoft.com/previous-versions/azure/dn823929(v=azure.100))) CreateShard[(Java](/java/api/com.microsoft.azure.elasticdb.shard.map.shardmap.createshard), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard)), CreateRangeMapping ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap.createrangemapping), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1) yöntemlerini kullanır ve ShardLocation[(Java](/java/api/com.microsoft.azure.elasticdb.shard.base.shardlocation), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation)) sınıfının bir örneğini oluşturur. Aşağıdaki örnekte, **sample_shard_2** adlı bir veritabanı ve içindeki tüm gerekli şema nesneleri aralık tutmak için oluşturulmuştur [300, 400).  

```csharp
// sm is a RangeShardMap object.
// Add a new shard to hold the range being added.
Shard shard2 = null;

if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2))
{
    shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
}

// Create the mapping and associate it with the new shard
sm.CreateRangeMapping(new RangeMappingCreationInfo<long>
                            (new Range<long>(300, 400), shard2, MappingStatus.Online));
```

.NET sürümü için powershell'i yeni bir Shard Harita Yöneticisi oluşturmak için alternatif olarak da kullanabilirsiniz. Burada bir örnek [mevcuttur.](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)

## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Varolan aralığın boş bir parçası için parça eklemek için

Bazı durumlarda, bir aralığı zaten bir parçaya eşlemiş ve kısmen verilerle doldurmuş olabilirsiniz, ancak şimdi yaklaşan verilerin farklı bir parçaya yönlendirilmesini istiyorsunuz. Örneğin, gün aralığına göre parçalanırsınız ve zaten bir parçaya 50 gün ayırmışsınızdır, ancak 24. Elastik veritabanı [bölme birleştirme aracı](sql-database-elastic-scale-overview-split-and-merge.md) bu işlemi gerçekleştirebilir, ancak veri hareketi gerekli değilse (örneğin, gün aralığı için veri [25, 50), yani, gün 25 dahil 50 özel, henüz yok) tamamen Shard Harita Yönetimi API'leri kullanarak doğrudan gerçekleştirebilirsiniz.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Örnek: bir aralığı bölme ve boş kısmı yeni eklenen bir parçaya atama

"sample_shard_2" adlı bir veritabanı ve içindeki tüm gerekli şema nesneleri oluşturuldu.  

```csharp
// sm is a RangeShardMap object.
// Add a new shard to hold the range we will move
Shard shard2 = null;

if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2))
{
    shard2 = sm.CreateShard(new ShardLocation(shardServer,"sample_shard_2"));  
}

// Split the Range holding Key 25
sm.SplitMapping(sm.GetMappingForKey(25), 25);

// Map new range holding [25-50) to different shard:
// first take existing mapping offline
sm.MarkMappingOffline(sm.GetMappingForKey(25));

// now map while offline to a different shard and take online
RangeMappingUpdate upd = new RangeMappingUpdate();
upd.Shard = shard2;
sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd));
```

**Önemli**: Bu tekniği yalnızca güncelleştirilmiş eşleme aralığının boş olduğundan eminseniz kullanın.  Önceki yöntemler taşınan aralık için verileri denetlemez, bu nedenle kodunuza denetimler eklemek en iyisidir.  Taşınan aralıkta satırlar varsa, gerçek veri dağıtımı güncelleştirilen parça eşle eşleşmez. Bu gibi durumlarda işlemi gerçekleştirmek için [bölme birleştirme aracını](sql-database-elastic-scale-overview-split-and-merge.md) kullanın.  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
