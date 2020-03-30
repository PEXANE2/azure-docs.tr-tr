---
title: Azure Cosmos DB dizin oluşturma ilkeleri
description: Azure Cosmos DB'de otomatik dizin oluşturma ve daha fazla performans için varsayılan dizin oluşturma ilkesini nasıl yapılandırıp değiştireceğinizi öğrenin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/26/2020
ms.author: tisande
ms.openlocfilehash: 930f156ebec76be860e7af02d41540ce67982f92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80292048"
---
# <a name="indexing-policies-in-azure-cosmos-db"></a>Azure Cosmos DB'de dizin oluşturma ilkeleri

Azure Cosmos DB'de, her kapsayıcının, kapsayıcının öğelerinin nasıl dizine eklenmesini belirleyen bir dizin oluşturma ilkesi vardır. Yeni oluşturulan kapsayıcılar için varsayılan dizin oluşturma ilkesi, her öğenin her özelliğini dizine dizine, herhangi bir dize veya sayı için aralık dizinlerini ve Point türündeki herhangi bir GeoJSON nesnesi için uzamsal dizinleri zorlar. Bu, dizin oluşturma ve dizin yönetimi hakkında önceden düşünmek zorunda kalmadan yüksek sorgu performansı elde etmenizi sağlar.

Bazı durumlarda bu otomatik davranışı kendi gereksinimlerinize daha iyi uyacak şekilde geçersiz kılmak isteyebilirsiniz. *Dizin oluşturma modunu*ayarlayarak bir kapsayıcının dizin oluşturma ilkesini özelleştirebilir ve *özellik yollarını*ekleyebilir veya hariç tutabilirsiniz.

> [!NOTE]
> Bu makalede açıklanan dizin oluşturma ilkelerini güncelleştirme yöntemi yalnızca Azure Cosmos DB'nin SQL (Core) API'si için geçerlidir.

## <a name="indexing-mode"></a>Dizin oluşturma modu

Azure Cosmos DB iki dizin oluşturma modunu destekler:

- **Tutarlı**: Öğeleri oluştururken, güncellerken veya silerken dizin eşzamanlı olarak güncelleştirilir. Bu, okundu sorgularınızın tutarlılığının [hesap için yapılandırılan tutarlılık](consistency-levels.md)olacağı anlamına gelir.
- **Yok**: Dizin oluşturma kapsayıcıda devre dışı bırakılır. Bu genellikle bir kapsayıcı ikincil dizinler gerek kalmadan saf bir anahtar değeri deposu olarak kullanıldığında kullanılır. Toplu işlemlerin performansını artırmak için de kullanılabilir. Toplu işlemler tamamlandıktan sonra, dizin modu Tutarlı olarak ayarlanabilir ve tamamlanana kadar [IndexTransformationProgress](how-to-manage-indexing-policy.md#use-the-net-sdk-v2) kullanılarak izlenebilir.

> [!NOTE]
> Azure Cosmos DB, Tembel dizin oluşturma modunu da destekler. Tembel dizin oluşturma, dizin güncelleştirmelerini, altyapı başka bir iş yapmadığında çok daha düşük öncelik düzeyinde gerçekleştirir. Bu tutarsız **veya eksik** sorgu sonuçlarına neden olabilir. Bir Cosmos kapsayıcısını sorgulamayı planlıyorsanız, tembel dizin oluşturmayı seçmemelisiniz.

Varsayılan olarak, dizin oluşturma `automatic`ilkesi . Dizin oluşturma ilkesindeki `automatic` özelliği `true`. Bu özelliği, `true` Azure CosmosDB'nin belgeleri yazıldığı gibi otomatik olarak dizine ekecek şekilde ayarlama.

## <a name="including-and-excluding-property-paths"></a><a id="include-exclude-paths"></a>Özellik yolları dahil ve hariç

Özel dizin oluşturma ilkesi, açıkça dahil edilen veya dizin oluşturma dışında olan özellik yollarını belirtebilir. Dizine eklenmiş yol sayısını en iyi duruma düşürerek, kapsayıcınız tarafından kullanılan depolama miktarını düşürebilir ve yazma işlemlerinin gecikmesini artırabilirsiniz. Bu yollar, [dizin oluşturma genel bakış bölümünde açıklanan yöntem](index-overview.md#from-trees-to-property-paths) ve aşağıdaki eklemeler aşağıdaki şekilde tanımlanır:

- skaler değere (dize veya sayı) giden bir yol,`/?`
- bir diziden öğeler `/[]` gösterim yoluyla birlikte ele `/0`alınır `/1` (yerine, vb.)
- `/*` joker düğüm altındaki tüm öğeleri eşleştirmek için kullanılabilir

Yine aynı örneği alarak:

```
    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 }
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }
```

- 'ın `headquarters` `employees` yolu`/headquarters/employees/?`

- ' `locations` `country` yolu`/locations/[]/country/?`

- altında `headquarters` bir şey için yol`/headquarters/*`

Örneğin, `/headquarters/employees/?` yolu ekleyebiliriz. Bu yol, çalışanların özelliğini dizine dizine dizine sahip olmamızı sağlar, ancak bu özellik içinde ek iç içe json dizine olmaz.

## <a name="includeexclude-strategy"></a>Stratejiekleme/hariç tutma

Herhangi bir dizin oluşturma ilkesi, kök yolu `/*` dahil edilmiş veya dışlanmış bir yol olarak içermelidir.

- Dizine alınması gerekmeyen yolları seçiçi olarak dışlamak için kök yolu ekleyin. Azure Cosmos DB'nin modelinize eklenebilecek yeni mülkleri proaktif olarak dizine eklemesine olanak tanıyan önerilen yaklaşım budur.
- Dizine alınması gereken yolları seçikar bir şekilde eklemek için kök yolu hariç tut.

- Alfasayısal karakterler ve _ (alt çizgi) içeren normal karakterli yollar için çift tırnak (örneğin, "/path/?") etrafındaki yol dizelerinden kaçmak zorunda değilsiniz. Diğer özel karakterlerle olan yollar için çift tırnak (örneğin, "/\"path-abc\"/?") etrafında yol dizesini kaçmış olmanız gerekir. Eğer yolunuza özel karakterler bekliyorsanız, güvenlik için her yoldan kaçabilirsiniz. Fonksiyonel olarak her yoldan kaçarsanız bir fark yaratmaz Vs sadece özel karakterleri olanlar.

- Etag `_etag` dizin oluşturma için dahil edilen yola eklenmedikçe, sistem özelliği varsayılan olarak dizin oluşturma nın dışında tutulur.

- Dizin oluşturma modu **tutarlı**olarak ayarlanmışsa, sistem özellikleri `id` ve `_ts` otomatik olarak dizine eklenir.

Yolları dahil ederken ve hariç tbelirtirken, aşağıdaki özniteliklerle karşılaşabilirsiniz:

- `kind`ya da `range` `hash`olabilir . Aralık dizini işlevselliği karma dizinin tüm işlevselliğini sağlar, bu nedenle bir aralık dizini kullanmanızı öneririz.

- `precision`dahil edilen yollar için dizin düzeyinde tanımlanan bir sayıdır. Değeri maksimum `-1` hassasiyeti gösterir. Bu değeri her zaman `-1`'ye ayarlamanızı öneririz.

- `dataType`ya da `String` `Number`olabilir . Bu, dizine ekilecek JSON özelliklerinin türlerini gösterir.

Belirtilmediğinde, bu özellikler aşağıdaki varsayılan değerlere sahip olacaktır:

| **Özellik Adı**     | **Varsayılan Değer** |
| ----------------------- | -------------------------------- |
| `kind`   | `range` |
| `precision`   | `-1`  |
| `dataType`    | `String` ve `Number` |

Yolları dahil etmek ve hariç tetmek için ilke örneklerini dizine ekin ler için [bu bölüme](how-to-manage-indexing-policy.md#indexing-policy-examples) bakın.

## <a name="spatial-indexes"></a>Uzamsal dizinler

Dizin oluşturma ilkesinde uzamsal bir yol tanımladığınızda, bu yola hangi dizin ```type``` uygulanması gerektiğini tanımlamanız gerekir. Uzamsal dizinler için olası türleri şunlardır:

* Nokta

* Çokgen

* Multipolygon

* Linestring

Azure Cosmos DB, varsayılan olarak, herhangi bir uzamsal dizin oluşturmaz. Uzamsal SQL yerleşik işlevlerini kullanmak istiyorsanız, gerekli özellikleri bir uzamsal dizin oluşturmanız gerekir. Uzamsal dizinler eklemek için ilke örneklerini dizine eklemek için [bu bölüme](geospatial.md) bakın.

## <a name="composite-indexes"></a>Bileşik dizinler

İki veya daha `ORDER BY` fazla özelliği olan bir yan tümcesi olan sorgular bileşik dizin gerektirir. Ayrıca, birçok eşitlik ve aralık sorgusunun performansını artırmak için bileşik dizin tanımlayabilirsiniz. Varsayılan olarak, hiçbir bileşik dizinler tanımlanır, bu nedenle gerektiğinde [bileşik dizinler eklemeniz](how-to-manage-indexing-policy.md#composite-indexing-policy-examples) gerekir.

Bileşik dizini tanımlarken şunları belirtirsiniz:

- İki veya daha fazla özellik yolu. Özellik yollarının tanımlandığı sıra önemlidir.

- Sıra (artan veya azalan).

> [!NOTE]
> Bileşik dizin eklediğinizde, sorgu, yeni bileşik dizin ekini tamamlanana kadar varolan aralık dizinlerini kullanır. Bu nedenle, bileşik dizini eklediğinizde, performans iyileştirmelerini hemen gözlemlemeyebilirsiniz. [SDK'lardan birini kullanarak](how-to-manage-indexing-policy.md)dizin dönüşümündeki ilerlemeyi izlemek mümkündür.

### <a name="order-by-queries-on-multiple-properties"></a>Birden çok özellik üzerinde sorguları BY ORDER:

İki veya daha fazla özele sahip bir `ORDER BY` yan tümcesi olan sorgular için bileşik dizinler kullanılırken aşağıdaki hususlar kullanılır:

- Bileşik dizin yolları `ORDER BY` yan tümcedeki özelliklerin sırasıyla eşleşmiyorsa, bileşik dizin sorguyu destekleyemez.

- Bileşik dizin yollarının sırası (artan veya azalan) `order` da `ORDER BY` yan tümceyle eşleşmelidir.

- Bileşik dizini de `ORDER BY` tüm yollarda ters sırayla bir yan tümceyi destekler.

Özellikler adı, yaş ve _ts bileşik dizinin tanımlandığı aşağıdaki örneği göz önünde bulundurun:

| **Bileşik Dizin**     | **Örnek `ORDER BY` Sorgu**      | **Bileşik Endeksi tarafından desteklenir?** |
| ----------------------- | -------------------------------- | -------------- |
| ```(name ASC, age ASC)```   | ```SELECT * FROM c ORDER BY c.name ASC, c.age asc``` | ```Yes```            |
| ```(name ASC, age ASC)```   | ```SELECT * FROM c ORDER BY c.age ASC, c.name asc```   | ```No```             |
| ```(name ASC, age ASC)```    | ```SELECT * FROM c ORDER BY c.name DESC, c.age DESC``` | ```Yes```            |
| ```(name ASC, age ASC)```     | ```SELECT * FROM c ORDER BY c.name ASC, c.age DESC``` | ```No```             |
| ```(name ASC, age ASC, timestamp ASC)``` | ```SELECT * FROM c ORDER BY c.name ASC, c.age ASC, timestamp ASC``` | ```Yes```            |
| ```(name ASC, age ASC, timestamp ASC)``` | ```SELECT * FROM c ORDER BY c.name ASC, c.age ASC``` | ```No```            |

Gerekli `ORDER BY` tüm sorguları hizmet böylece dizin oluşturma ilkesini özelleştirmeniz gerekir.

### <a name="queries-with-filters-on-multiple-properties"></a>Birden fazla özelliğinde filtreler olan sorgular

Bir sorguda iki veya daha fazla özellik üzerinde filtreler varsa, bu özellikler için bileşik dizin oluşturmak yararlı olabilir.

Örneğin, iki özellik üzerinde eşitlik filtresi olan aşağıdaki sorguyu göz önünde bulundurun:

```sql
SELECT * FROM c WHERE c.name = "John" AND c.age = 18
```

Bu sorgu daha verimli olacak, daha az zaman alıyor ve daha az RU tüketen, bir bileşik dizini kaldıraç yapabiliyorsa (asc adı, yaş ASC).

Aralık filtrelerine sahip sorgular bileşik dizinle de en iyi duruma getirilebilir. Ancak, sorguyalnızca tek bir aralık filtresi olabilir. Aralık filtreleri `>` `<`, `<=` `>=`, `!=`, , ve . Aralık filtresi bileşik dizinde son olarak tanımlanmalıdır.

Hem eşitlik hem de aralık filtreleriyle aşağıdaki sorguyu düşünün:

```sql
SELECT * FROM c WHERE c.name = "John" AND c.age > 18
```

Bu sorgu üzerinde bileşik dizin (asc adı, yaş ASC) ile daha verimli olacaktır. Ancak, eşitlik filtreleri bileşik dizinde ilk olarak tanımlanması gerektiğinden, sorguda (yaş ASC, ad ASC) bileşik dizini kullanılmaz.

Birden çok özelliği olan sorgular için bileşik dizinler oluşturulurken aşağıdaki hususlar kullanılır

- Sorgunun filtresindeki özellikler bileşik dizindekilerle eşleşmelidir. Bir özellik bileşik dizinde yse ancak sorguya filtre olarak dahil edilmezse, sorgu bileşik dizini kullanmaz.
- Bir sorgunun filtrede bileşik dizinde tanımlanmamış ek özellikleri varsa, sorguyu değerlendirmek için bileşik ve aralık dizinleri birleşimi kullanılır. Bu sadece aralık dizinleri kullanarak daha az RU gerektirir.
- Bir özelliğin aralık filtresi`>` `<`varsa `<=` `>=`(, `!=`, , , , veya ), bu özellik bileşik dizinde son olarak tanımlanmalıdır. Bir sorguda birden fazla aralık filtresi varsa, bileşik dizini kullanmaz.
- Birden çok filtreiçeren sorguları en iyi duruma `ORDER` getirmek için bileşik dizin oluştururken, bileşik dizinin sonuçları üzerinde hiçbir etkisi olmaz. Bu özellik isteğe bağlıdır.
- Birden çok özellik üzerinde filtreleri olan bir sorgu için bileşik dizin tanımlamazsanız, sorgu yine de başarılı olur. Ancak, sorgunun RU maliyeti bileşik dizin ile azaltılabilir.

Özellikler adı, yaş ve zaman damgası üzerinde bileşik dizinin tanımlandığı aşağıdaki örnekleri göz önünde bulundurun:

| **Bileşik Dizin**     | **Örnek Sorgu**      | **Bileşik Endeksi tarafından desteklenir?** |
| ----------------------- | -------------------------------- | -------------- |
| ```(name ASC, age ASC)```   | ```SELECT * FROM c WHERE c.name = "John" AND c.age = 18``` | ```Yes```            |
| ```(name ASC, age ASC)```   | ```SELECT * FROM c WHERE c.name = "John" AND c.age > 18```   | ```Yes```             |
| ```(name DESC, age ASC)```    | ```SELECT * FROM c WHERE c.name = "John" AND c.age > 18``` | ```Yes```            |
| ```(name ASC, age ASC)```     | ```SELECT * FROM c WHERE c.name != "John" AND c.age > 18``` | ```No```             |
| ```(name ASC, age ASC, timestamp ASC)``` | ```SELECT * FROM c WHERE c.name = "John" AND c.age = 18 AND c.timestamp > 123049923``` | ```Yes```            |
| ```(name ASC, age ASC, timestamp ASC)``` | ```SELECT * FROM c WHERE c.name = "John" AND c.age < 18 AND c.timestamp = 123049923``` | ```No```            |

### <a name="queries-with-a-filter-as-well-as-an-order-by-clause"></a>Filtresi ve ORDER BY yan tümcesi olan sorgular

Bir sorgu bir veya daha fazla özelliği filtreler ve ORDER BY yan tümcesinde farklı özelliklere sahipse, filtredeki özellikleri `ORDER BY` yan tümceye eklemek yararlı olabilir.

Örneğin, filtredeki özellikleri ORDER BY yan tümcesine ekleyerek, bileşik dizinden yararlanmak için aşağıdaki sorgu yeniden yazılabilir:

Aralık dizini kullanarak sorgu:

```sql
SELECT * FROM c WHERE c.name = "John" ORDER BY c.timestamp
```

Bileşik dizin kullanarak sorgu:

```sql
SELECT * FROM c WHERE c.name = "John" ORDER BY c.name, c.timestamp
```

Aynı desen ve sorgu optimizasyonları birden çok eşitlik filtresi olan sorgular için genelleştirilmiş olabilir:

Aralık dizini kullanarak sorgu:

```sql
SELECT * FROM c WHERE c.name = "John", c.age = 18 ORDER BY c.timestamp
```

Bileşik dizin kullanarak sorgu:

```sql
SELECT * FROM c WHERE c.name = "John", c.age = 18 ORDER BY c.name, c.age, c.timestamp
```

Bir filtre ve `ORDER BY` yan tümceile bir sorgu en iyi duruma getirmek için bileşik dizinler oluştururken aşağıdaki hususlar kullanılır:

* Sorgu özellikleri filtreler, bu `ORDER BY` ilk yan tümcesi dahil edilmelidir.
* Bir özellik ve farklı bir özellik kullanarak ayrı `ORDER BY` bir yan tümcesi olan bir sorguda bileşik dizin tanımlamazsanız, sorgu yine de başarılı olur. Ancak, sorgunun RU maliyeti bileşik bir dizin le, özellikle `ORDER BY` de yan tümcedeki özelliğin yüksek bir kardinallik varsa azaltılabilir.
* Birden çok özelliği olan sorgular için bileşik dizinler ve birden çok özellik üzerinde filtreleri olan sorgular için bileşik dizinler oluşturmak için `ORDER BY` tüm hususlar hala geçerlidir.


| **Bileşik Dizin**                      | **Örnek `ORDER BY` Sorgu**                                  | **Bileşik Endeksi tarafından desteklenir?** |
| ---------------------------------------- | ------------------------------------------------------------ | --------------------------------- |
| ```(name ASC, timestamp ASC)```          | ```SELECT * FROM c WHERE c.name = "John" ORDER BY c.name ASC, c.timestamp ASC``` | `Yes` |
| ```(name ASC, timestamp ASC)```          | ```SELECT * FROM c WHERE c.name = "John" ORDER BY c.timestamp ASC, c.name ASC``` | `No`  |
| ```(name ASC, timestamp ASC)```          | ```SELECT * FROM c WHERE c.name = "John" ORDER BY c.timestamp ASC``` | ```No```   |
| ```(age ASC, name ASC, timestamp ASC)``` | ```SELECT * FROM c WHERE c.age = 18 and c.name = "John" ORDER BY c.age ASC, c.name ASC,c.timestamp ASC``` | `Yes` |
| ```(age ASC, name ASC, timestamp ASC)``` | ```SELECT * FROM c WHERE c.age = 18 and c.name = "John" ORDER BY c.timestamp ASC``` | `No` |

## <a name="modifying-the-indexing-policy"></a>Dizin oluşturma ilkesini değiştirme

Bir kapsayıcının dizin oluşturma [ilkesi, Azure portalı veya desteklenen SDK'lardan birini kullanarak](how-to-manage-indexing-policy.md)herhangi bir zamanda güncelleştirilebilir. Dizin oluşturma ilkesine yapılan bir güncelleştirme, eski dizinden çevrimiçi ve yerinde gerçekleştirilen yeni dizine dönüşümü tetikler (böylece işlem sırasında ek depolama alanı tüketilmez). Eski ilke dizini, yazma kullanılabilirliğini veya kapsayıcıda sağlanan iş başına verilen iş buzunu etkilemeden verimli bir şekilde yeni ilkeye dönüştürülür. Dizin dönüştürme bir eşzamanlı işlemdir ve tamamlanması gereken süre, sağlanan iş başına, madde sayısına ve boyutlarına bağlıdır.

> [!NOTE]
> Bir aralık veya uzamsal dizin eklerken, sorgular eşleşen tüm sonuçları döndürmeyebilir ve bunu herhangi bir hata döndürmeden yapar. Bu, dizin dönüşümü tamamlanana kadar sorgu sonuçlarının tutarlı olmadığı anlamına gelir. [SDK'lardan birini kullanarak](how-to-manage-indexing-policy.md)dizin dönüşümündeki ilerlemeyi izlemek mümkündür.

Yeni dizin oluşturma ilkesi modu Tutarlı olarak ayarlanmışsa, dizin dönüşümü devam ederken başka bir dizin oluşturma ilkesi değişikliği uygulanamaz. Çalışan dizin dönüşümü, dizin oluşturma ilkesinin modunu Yok olarak ayarlayarak iptal edilebilir (bu da dizini hemen düşürür).

## <a name="indexing-policies-and-ttl"></a>Dizin oluşturma ilkeleri ve TTL

[Live'a Zaman (TTL) özelliği,](time-to-live.md) açık olduğu kapsayıcıda dizini etkin olmasını gerektirir. Bu şu anlama gelir:

- dizin oluşturma modunun Yok olarak ayarlandığı bir kapsayıcıda TTL'yi etkinleştirmek mümkün değildir,
- Dizin oluşturma modunu TTL'nin etkinleştirildiği bir kapsayıcıda Yok olarak ayarlamak mümkün değildir.

Özellik yolunun dizine eklenmemesi gereken, ancak TTL gerektiren senaryolarda, dizin oluşturma ilkesini şu şekilde kullanabilirsiniz:

- tutarlı olarak ayarlanmış bir dizin oluşturma modu ve
- hiçbir yol dahil ve
- `/*`tek dışlanan yol olarak.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde dizin oluşturma hakkında daha fazla bilgi edinin:

- [Dizin oluşturma genel bakış](index-overview.md)
- [Dizin oluşturma ilkesi nasıl yönetilir?](how-to-manage-indexing-policy.md)
