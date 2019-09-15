---
title: Azure Cosmos DB Dizin oluşturma ilkeleri
description: Azure Cosmos DB ' de otomatik dizin oluşturma ve daha fazla performans için varsayılan dizin oluşturma ilkesini yapılandırma ve değiştirme hakkında bilgi edinin.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/10/2019
ms.author: thweiss
ms.openlocfilehash: 60b323c12e5c548c974a7d660d08861637ac2381
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70996677"
---
# <a name="indexing-policies-in-azure-cosmos-db"></a>Azure Cosmos DB 'de dizin oluşturma ilkeleri

Azure Cosmos DB, her kapsayıcının kapsayıcının öğelerinin nasıl dizine alınacağını belirleyen bir dizin oluşturma ilkesi vardır. Yeni oluşturulan kapsayıcılar için varsayılan dizin oluşturma ilkesi her öğenin her bir özelliğini dizinleyen, herhangi bir dize veya sayı için Aralık dizini uygulayan ve tür noktası olan herhangi bir GeoJSON nesnesi için uzamsal dizinler. Bu, dizin oluşturma ve dizin yönetimi ile ilgili düşünmek zorunda kalmadan yüksek sorgu performansı almanızı sağlar.

Bazı durumlarda, gereksinimlerinize daha iyi uyacak şekilde bu otomatik davranışı geçersiz kılmak isteyebilirsiniz. *Dizin oluşturma modunu*ayarlayarak bir kapsayıcının dizin oluşturma ilkesini özelleştirebilir ve *özellik yollarını*dahil edebilir veya dışlayabilirsiniz.

> [!NOTE]
> Bu makalede açıklanan dizin oluşturma ilkelerini güncelleştirme yöntemi yalnızca Azure Cosmos DB SQL (Core) API 'SI için geçerlidir.

## <a name="indexing-mode"></a>Dizin oluşturma modu

Azure Cosmos DB iki dizin oluşturma modunu destekler:

- **Tutarlı**: Kapsayıcının dizin oluşturma ilkesi tutarlı olarak ayarlandıysa, öğe oluşturma, güncelleştirme veya silme işlemi sırasında dizin zaman uyumlu olarak güncelleştirilir. Bu, okuma sorgularınızın tutarlılığı, [hesap için yapılandırılmış tutarlılığa](consistency-levels.md)sahip olacağı anlamına gelir.
- **Hiçbiri**: Kapsayıcının dizin oluşturma ilkesi hiçbiri olarak ayarlandıysa, dizin oluşturma Bu kapsayıcıda etkin bir şekilde devre dışı bırakılır. Bu genellikle bir kapsayıcı, ikincil dizinlere gerek olmadan saf anahtar-değer deposu olarak kullanıldığında kullanılır. Toplu ekleme işlemlerinin hızlandırde sağlanmasına yardımcı olabilir.

Varsayılan olarak, dizin oluşturma ilkesi olarak `automatic`ayarlanır. Dizin oluşturma ilkesindeki `automatic` özelliği olarak `true`ayarlanarak elde edilir. Bu özelliği ayarlamak için `true` Azure cosmosdb 'nin belgeleri yazıldığı gibi otomatik olarak dizin oluşturulmasına izin verir.

## <a name="including-and-excluding-property-paths"></a>Özellik yollarını dahil etme ve hariç tutma

Özel bir dizin oluşturma ilkesi, dizin oluşturma işleminden açıkça dahil edilen veya dışlanan Özellik yollarını belirtebilir. Dizini oluşturulmuş yolların sayısını en iyi duruma getirerek, Kapsayıcınız tarafından kullanılan depolama miktarını düşürebilirsiniz ve yazma işlemlerinin gecikme süresini artırabilirsiniz. Bu yollar, [Dizin oluşturma genel bakış bölümünde açıklanan yöntemi](index-overview.md#from-trees-to-property-paths) aşağıdaki eklemelerle izleyerek tanımlanmıştır:

- skaler bir değere (dize veya sayı) öndeki bir yol, şununla biter`/?`
- bir dizideki öğeler `/[]` `/0`, Gösterim (yerine, `/1` vb.) ile birlikte karşılanır
- `/*` joker karakter, düğümün altındaki herhangi bir öğeyi eşleştirmek için kullanılabilir

Aynı örneği yeniden almak:

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

- `headquarters`yolu `employees``/headquarters/employees/?`

- `locations`' yol`country``/locations/[]/country/?`

- altında `headquarters` herhangi bir şeyin yolu`/headquarters/*`

Örneğin, `/headquarters/employees/?` yolunu dahil eteceğiz. Bu yol çalışanlar özelliğini dizinliyoruz, ancak bu özellik içinde iç içe geçmiş JSON dizinini dizinliyoruz.

## <a name="includeexclude-strategy"></a>Dahil etme/hariç tutma stratejisi

Herhangi bir dizin oluşturma ilkesinin kök yolu `/*` dahil edilen ya da hariç tutulmuş bir yol olarak içermesi gerekmez.

- Dizin oluşturma gerektirmeyen yolları seçmeli olarak hariç tutmak için kök yolu ekleyin. Bu, modelinize eklenebilen yeni bir özelliğin Azure Cosmos DB proaktif olarak dizinlemenizi sağlayan bu, önerilen yaklaşımdır.
- Dizine eklenmesi gereken yolları seçmeli olarak dahil etmek için kök yolu hariç tutun.

- : Alfasayısal karakterler ve _ (alt çizgi) içeren normal karakter içeren yollar için, yol dizesinin çift tırnak etrafında (örneğin, "/path/?") kaçış olması gerekmez. Diğer özel karakterlere sahip yollar için, yol dizesini çift tırnak etrafında (örneğin, "/\"Path-ABC\"/?") kaçış yapmanız gerekir. Yolunuzda özel karakterler bekleliyorsanız, güvenlik için her yolu da kaçış yapabilirsiniz. İşlevsel olarak, tüm yolları yalnızca özel karakterlere sahip olanlara karşı atladıysanız herhangi bir farklılık yapmaz.

- "ETag" sistem özelliği dizin oluşturma için eklenen yola eklenemediği takdirde varsayılan olarak dizin oluşturma işleminden çıkarılır.

Yolları dahil etme ve hariç tutma sırasında aşağıdaki özniteliklerle karşılaşabilirsiniz:

- `kind``range` ya`hash`da olabilir. Aralık dizini işlevselliği bir karma dizinin tüm işlevlerini sağlar, bu nedenle bir Aralık dizini kullanmanızı öneririz.

- `precision`, eklenen yollar için dizin düzeyinde tanımlanmış bir sayıdır. Değeri en fazla `-1` duyarlığı gösterir. Bu değeri her zaman olarak `-1`ayarlamayı öneririz.

- `dataType``String` ya`Number`da olabilir. Bu, dizine eklenecek JSON özelliklerinin türlerini gösterir.

Belirtilmediğinde, bu özellikler aşağıdaki varsayılan değerlere sahip olur:

| **Özellik adı**     | **Varsayılan değer** |
| ----------------------- | -------------------------------- |
| `kind`   | `range` |
| `precision`   | `-1`  |
| `dataType`    | `String` ve `Number` |

Yolların dahil edilmesi ve dışlanması için ilke örneklerinin dizinini oluşturma [bölümüne](how-to-manage-indexing-policy.md#indexing-policy-examples) bakın.

## <a name="spatial-indexes"></a>Uzamsal dizinler

Dizin oluşturma ilkesinde bir uzamsal yol tanımladığınızda, bu yola hangi dizinin ```type``` uygulanacağını tanımlamanız gerekir. Uzamsal dizinler için olası türler şunlardır:

* Seçeneğinin

* Gen

* MultiPolygon

* LineString

Azure Cosmos DB, varsayılan olarak hiçbir uzamsal dizin oluşturmaz. Uzamsal SQL yerleşik işlevlerini kullanmak isterseniz, gereken özelliklerde bir uzamsal dizin oluşturmanız gerekir. Uzamsal dizinler eklemeye yönelik dizin oluşturma örnekleri için [Bu bölüme](geospatial.md) bakın.

## <a name="composite-indexes"></a>Bileşik dizinler

İki veya daha fazla `ORDER BY` özelliği olan bir yan tümcesine sahip sorgular bileşik bir dizin gerektirir. Ayrıca, birçok eşitlik ve Aralık sorgusunun performansını artırmak için bir bileşik dizin tanımlayabilirsiniz. Varsayılan olarak, bir bileşik dizin tanımlanmadığında, gereken şekilde [Bileşik dizinler eklemelisiniz](how-to-manage-indexing-policy.md#composite-indexing-policy-examples) .

Bileşik dizin tanımlarken şunu belirtirsiniz:

- İki veya daha fazla özellik yolu. Özellik yollarının önemli olarak tanımlandığı sıra.

- Sıra (artan veya azalan).

> [!NOTE]
> Diğer dizin türlerinde olduğu gibi bileşik bir dizin eklerken, Dizin güncelleştirildiğinden sorgular tutarsız sonuçlar döndürebilir.

### <a name="order-by-queries-on-multiple-properties"></a>Birden çok özelliklerde sorguya göre sırala:

İki veya daha fazla özelliği olan bir `ORDER BY` yan tümcesine sahip sorgular için Bileşik dizinler kullanılırken aşağıdaki noktalar kullanılır:

- Bileşik dizin yolları `ORDER BY` yan tümcesindeki özelliklerin dizisiyle eşleşmezse, bileşik dizin sorguyu desteklemiyor.

- Bileşik dizin yollarının sırası (artan veya azalan), `order` `ORDER BY` yan tümcesindeki ile de eşleşmelidir.

- Bileşik dizin aynı zamanda tüm yollarda `ORDER BY` ters sırada olan bir yan tümceyi destekler.

Bir bileşik dizinin özellikler adı, yaşı ve _ts üzerinde tanımlandığı aşağıdaki örneği göz önünde bulundurun:

| **Bileşik Dizin**     | **Örnek `ORDER BY` sorgu**      | **Bileşik dizin tarafından destekleniyor mu?** |
| ----------------------- | -------------------------------- | -------------- |
| ```(name ASC, age ASC)```   | ```SELECT * FROM c ORDER BY c.name ASC, c.age asc``` | ```Yes```            |
| ```(name ASC, age ASC)```   | ```SELECT * FROM c ORDER BY c.age ASC, c.name asc```   | ```No```             |
| ```(name ASC, age ASC)```    | ```SELECT * FROM c ORDER BY c.name DESC, c.age DESC``` | ```Yes```            |
| ```(name ASC, age ASC)```     | ```SELECT * FROM c ORDER BY c.name ASC, c.age DESC``` | ```No```             |
| ```(name ASC, age ASC, timestamp ASC)``` | ```SELECT * FROM c ORDER BY c.name ASC, c.age ASC, timestamp ASC``` | ```Yes```            |
| ```(name ASC, age ASC, timestamp ASC)``` | ```SELECT * FROM c ORDER BY c.name ASC, c.age ASC``` | ```No```            |

Tüm gerekli `ORDER BY` sorgulara hizmeti sağlamak için dizin oluşturma ilkenizi özelleştirmeniz gerekir.

### <a name="queries-with-filters-on-multiple-properties"></a>Birden çok özelliklerde filtre içeren sorgular

Bir sorgu iki veya daha fazla özelliğe filtre içeriyorsa, bu özellikler için bileşik bir dizin oluşturmak yararlı olabilir.

Örneğin, iki özellik üzerinde bir eşitlik filtresi bulunan aşağıdaki sorguyu göz önünde bulundurun:

```sql
SELECT * FROM c WHERE c.name = "John" AND c.age = 18
```

Bu sorgu daha verimli olacaktır, daha az zaman alır ve bir bileşik dizinden yararlanıyorsa daha az RU (ad ASC, Age ASC).

Aralık filtreleri içeren sorgular da bileşik bir dizinle iyileştirilebilir. Ancak, sorgu yalnızca tek bir Aralık filtresine sahip olabilir. `>`Aralık filtreleri `<` ,`>=`,, ve`!=`içerir. `<=` Aralık filtresi, en son bileşik dizinde tanımlanmalıdır.

Aşağıdaki sorguyu hem eşitlik hem de Aralık filtreleriyle göz önünde bulundurun:

```sql
SELECT * FROM c WHERE c.name = "John" AND c.age > 18
```

Bu sorgu, üzerinde bileşik bir dizin ile daha verimli olacaktır (ad ASC, Age ASC). Ancak, sorgu (Age ASC, ad ASC) üzerinde bir bileşik dizin kullanmaz, çünkü eşitlik filtreleri ilk olarak bileşik dizinde tanımlanmalıdır.

Birden çok özelliklerde filtre içeren sorgular için Bileşik dizinler oluşturulurken aşağıdaki noktalar kullanılır

- Sorgunun filtresindeki özellikler, bileşik dizinindekilerle eşleşmelidir. Bir özellik bileşik dizindaysa, ancak sorguya filtre olarak eklenmemelidir, sorgu bileşik dizinden yararlanmaz.
- Bir sorguda, bir bileşik dizinde tanımlanmayan ek özellikler varsa, sorguyu değerlendirmek için bileşik ve Aralık dizinlerinin bir birleşimi kullanılır. Bu, Aralık dizinleri kullanılarak özel olarak daha az RU gerektirir.
- Bir özelliğin Aralık filtresi varsa (`>` `<=`, `<` `>=`,, veya `!=`), bu özellik bileşik dizinde son olarak tanımlanmalıdır. Bir sorguda birden fazla Aralık filtresi varsa, bileşik dizinden yararlanmaz.
- Birden çok filtre içeren sorguları iyileştirmek için bir bileşik dizin oluştururken, `ORDER` bileşik dizinin sonuçları üzerinde hiçbir etkisi olmayacaktır. Bu özellik isteğe bağlıdır.
- Birden çok özelliklerde filtre içeren bir sorgu için bileşik dizin tanımlamadıysanız sorgu yine de başarılı olur. Ancak, sorgunun RU maliyeti bir bileşik dizinle azaltılabilir.

Bir bileşik dizinin özellikler adı, yaşı ve zaman damgasında tanımlandığı aşağıdaki örnekleri göz önünde bulundurun:

| **Bileşik Dizin**     | **Örnek sorgu**      | **Bileşik dizin tarafından destekleniyor mu?** |
| ----------------------- | -------------------------------- | -------------- |
| ```(name ASC, age ASC)```   | ```SELECT * FROM c WHERE c.name = "John" AND c.age = 18``` | ```Yes```            |
| ```(name ASC, age ASC)```   | ```SELECT * FROM c WHERE c.name = "John" AND c.age > 18```   | ```Yes```             |
| ```(name DESC, age ASC)```    | ```SELECT * FROM c WHERE c.name = "John" AND c.age > 18``` | ```Yes```            |
| ```(name ASC, age ASC)```     | ```SELECT * FROM c WHERE c.name != "John" AND c.age > 18``` | ```No```             |
| ```(name ASC, age ASC, timestamp ASC)``` | ```SELECT * FROM c WHERE c.name = "John" AND c.age = 18 AND c.timestamp > 123049923``` | ```Yes```            |
| ```(name ASC, age ASC, timestamp ASC)``` | ```SELECT * FROM c WHERE c.name = "John" AND c.age < 18 AND c.timestamp = 123049923``` | ```No```            |

### <a name="queries-with-a-filter-as-well-as-an-order-by-clause"></a>Bir filtrenin yanı sıra ORDER BY yan tümcesini içeren sorgular

Bir sorgu bir veya daha fazla özellik üzerinde filtreleyip order by yan tümcesinde farklı özelliklere sahipse, filtrenin `ORDER BY` içindeki özellikleri yan tümcesine eklemek yararlı olabilir.

Örneğin, filtreye ORDER BY yan tümcesine eklenen özellikleri ekleyerek, bir bileşik dizinden yararlanmak için aşağıdaki sorgu yeniden yazılabilir:

Aralık dizinini kullanan sorgu:

```sql
SELECT * FROM c WHERE c.name = "John" ORDER BY c.timestamp
```

Bileşik dizin kullanarak sorgula:

```sql
SELECT * FROM c WHERE c.name = "John" ORDER BY c.name, c.timestamp
```

Aynı model ve sorgu iyileştirmeleri, birden çok eşitlik filtresi içeren sorgular için genelleştirilerek bulunabilir:

Aralık dizinini kullanan sorgu:

```sql
SELECT * FROM c WHERE c.name = "John", c.age = 18 ORDER BY c.timestamp
```

Bileşik dizin kullanarak sorgula:

```sql
SELECT * FROM c WHERE c.name = "John", c.age = 18 ORDER BY c.name, c.age, c.timestamp
```

Bir sorguyu bir filtre ve `ORDER BY` yan tümcesiyle iyileştirmek için Bileşik dizinler oluşturulurken aşağıdaki noktalar kullanılır:

* Sorgu, özelliklere filtre uygular, bu, ilk olarak `ORDER BY` yan tümcesine eklenmelidir.
* Bir özellikte filtre içeren bir sorgu üzerinde bir bileşik dizin tanımlamadıysanız ve farklı bir özellik kullanarak ayrı `ORDER BY` bir yan tümce kullanırsanız, sorgu yine de başarılı olur. Ancak, özellikle `ORDER BY` yan tümcesindeki özelliğin yüksek bir kardinalite özelliği varsa, sorgunun ru maliyeti bileşik bir dizinle azaltılabilir.
* Birden çok özelliği olan sorgular için `ORDER BY` Bileşik dizinler oluşturmaya yönelik tüm hususlar ve birden çok özelliklerde filtre içeren sorgular hala geçerlidir.


| **Bileşik Dizin**                      | **Örnek `ORDER BY` sorgu**                                  | **Bileşik dizin tarafından destekleniyor mu?** |
| ---------------------------------------- | ------------------------------------------------------------ | --------------------------------- |
| ```(name ASC, timestamp ASC)```          | ```SELECT * FROM c WHERE c.name = "John" ORDER BY c.name ASC, c.timestamp ASC``` | `Yes` |
| ```(name ASC, timestamp ASC)```          | ```SELECT * FROM c WHERE c.name = "John" ORDER BY c.timestamp ASC, c.name ASC``` | `No`  |
| ```(name ASC, timestamp ASC)```          | ```SELECT * FROM c WHERE c.name = "John" ORDER BY c.timestamp ASC``` | ```No```   |
| ```(age ASC, name ASC, timestamp ASC)``` | ```SELECT * FROM c WHERE c.age = 18 and c.name = "John" ORDER BY c.age ASC, c.name ASC,c.timestamp ASC``` | `Yes` |
| ```(age ASC, name ASC, timestamp ASC)``` | ```SELECT * FROM c WHERE c.age = 18 and c.name = "John" ORDER BY c.timestamp ASC``` | `No` |

## <a name="modifying-the-indexing-policy"></a>Dizin oluşturma ilkesini değiştirme

Kapsayıcının dizin oluşturma ilkesi [, Azure Portal veya desteklenen SDK 'lardan birini kullanarak](how-to-manage-indexing-policy.md)dilediğiniz zaman güncelleştirilemeyebilir. Dizin oluşturma ilkesi için bir güncelleştirme, eski dizinden yeni bir dönüştürmeyi tetikler ve bu, çevrimiçi ve yerinde gerçekleştirilir (Bu nedenle işlem sırasında ek depolama alanı tüketilmelidir). Eski ilkenin dizini, yazma kullanılabilirliğini etkilemeden ve kapsayıcıda sağlanan aktarım hızını etkilemeden etkili bir şekilde yeni ilkeye dönüştürülür. Dizin dönüştürme zaman uyumsuz bir işlemdir ve tamamlanmak üzere gereken süre, sağlanan aktarım hızına, öğe sayısına ve bunların boyutuna bağlıdır.

> [!NOTE]
> Yeniden dizin oluşturma işlemi devam ederken, sorgular tüm eşleşen sonuçları döndürmeyebilir ve hata döndürmeksizin bunu yapacaktır. Bu, sorgu sonuçlarının Dizin dönüştürmesi tamamlanana kadar tutarlı olamayacağı anlamına gelir. [SDK 'lardan birini kullanarak](how-to-manage-indexing-policy.md)Dizin dönüşümünün ilerlemesini izlemek mümkündür.

Yeni dizin oluşturma ilkesinin modu tutarlı olarak ayarlandıysa, Dizin dönüştürme işlemi devam ederken başka bir dizin oluşturma ilkesi değişikliği uygulanabilir. Çalışan bir dizin dönüştürmesi, dizin oluşturma ilkesinin modu None olarak ayarlanarak iptal edilebilir (Bu, dizini hemen bırakacak).

## <a name="indexing-policies-and-ttl"></a>Dizin oluşturma ilkeleri ve TTL

[Yaşam süresi (TTL) özelliği](time-to-live.md) , dizin oluşturmanın açık olduğu kapsayıcıda etkin olmasını gerektirir. Bunun anlamı:

- Dizin oluşturma modunun None olarak ayarlandığı bir kapsayıcıda TTL 'yi etkinleştirmek mümkün değildir,
- TTL 'nin etkinleştirildiği bir kapsayıcıda dizin oluşturma modunun hiçbiri olarak ayarlanması mümkün değildir.

Hiçbir özellik yolunun dizine alınması gereken ancak TTL 'nin gerekli olduğu senaryolarda, ile bir dizin oluşturma ilkesi kullanabilirsiniz:

- bir dizin oluşturma modu tutarlı olarak ayarlanır ve
- dahil edilen yol yok ve
- `/*`yalnızca Dışlanan yol olarak.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde dizin oluşturma hakkında daha fazla bilgi edinin:

- [Dizine genel bakış](index-overview.md)
- [Dizin oluşturma ilkesini yönetme](how-to-manage-indexing-policy.md)
