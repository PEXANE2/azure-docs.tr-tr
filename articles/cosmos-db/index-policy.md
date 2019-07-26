---
title: Azure Cosmos DB Dizin oluşturma ilkeleri
description: Azure Cosmos DB ' de otomatik dizin oluşturma ve daha fazla performans için varsayılan dizin oluşturma ilkesini yapılandırma ve değiştirme hakkında bilgi edinin.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: thweiss
ms.openlocfilehash: 01e3e1f1c9bffee0604de1260e8e466f5b1d229d
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68467869"
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

## <a name="including-and-excluding-property-paths"></a>Özellik yollarını dahil etme ve hariç tutma

Özel bir dizin oluşturma ilkesi, dizin oluşturma işleminden açıkça dahil edilen veya dışlanan Özellik yollarını belirtebilir. Dizini oluşturulmuş yolların sayısını en iyi duruma getirerek, Kapsayıcınız tarafından kullanılan depolama miktarını düşürebilirsiniz ve yazma işlemlerinin gecikme süresini artırabilirsiniz. Bu yollar, [Dizin oluşturma genel bakış bölümünde açıklanan yöntemi](index-overview.md#from-trees-to-property-paths) aşağıdaki eklemelerle izleyerek tanımlanmıştır:

- skaler bir değere (dize veya sayı) öndeki bir yol, şununla biter`/?`
- bir dizideki öğeler `/[]` `/0`, Gösterim (yerine, `/1` vb.) ile birlikte karşılanır
- `/*` joker karakter, düğümün altındaki herhangi bir öğeyi eşleştirmek için kullanılabilir

Aynı örneği yeniden almak:

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

- `headquarters`yolu `employees``/headquarters/employees/?`
- `locations`' yol`country``/locations/[]/country/?`
- altında `headquarters` herhangi bir şeyin yolu`/headquarters/*`

Dizin oluşturma ilkesine açıkça bir yol dahil edildiğinde, bu yola hangi Dizin türlerinin uygulanacağını ve her bir dizin türü için bu dizinin uygulandığı veri türünü de tanımlamak gerekir:

| Dizin türü | İzin verilen hedef veri türleri |
| --- | --- |
| Aralık | Dize veya sayı |
| Uzamsal | Nokta, LineString veya Çokgen |

Örneğin, `/headquarters/employees/?` yolu dahil etmemiz ve her iki `Number` değer için de `Range` `String` bu yola bir dizin uygulanması gerektiğini belirtmeniz gerekir.

### <a name="includeexclude-strategy"></a>Dahil etme/hariç tutma stratejisi

Herhangi bir dizin oluşturma ilkesinin kök yolu `/*` dahil edilen ya da hariç tutulmuş bir yol olarak içermesi gerekmez.

- Dizin oluşturma gerektirmeyen yolları seçmeli olarak hariç tutmak için kök yolu ekleyin. Bu, modelinize eklenebilen yeni bir özelliğin Azure Cosmos DB proaktif olarak dizinlemenizi sağlayan bu, önerilen yaklaşımdır.
- Dizine eklenmesi gereken yolları seçmeli olarak dahil etmek için kök yolu hariç tutun.

- : Alfasayısal karakterler ve _ (alt çizgi) içeren normal karakter içeren yollar için, yol dizesinin çift tırnak etrafında (örneğin, "/path/?") kaçış olması gerekmez. Diğer özel karakterlere sahip yollar için, yol dizesini çift tırnak etrafında (örneğin, "/\"Path-ABC\"/?") kaçış yapmanız gerekir. Yolunuzda özel karakterler bekleliyorsanız, güvenlik için her yolu da kaçış yapabilirsiniz. İşlevsel olarak, tüm yolları yalnızca özel karakterlere sahip olanlara karşı atladıysanız herhangi bir farklılık yapmaz.

- "ETag" sistem özelliği dizin oluşturma için eklenen yola eklenemediği takdirde varsayılan olarak dizin oluşturma işleminden çıkarılır.

Dizin oluşturma ilkesi örnekleri için [Bu bölüme](how-to-manage-indexing-policy.md#indexing-policy-examples) bakın.

## <a name="composite-indexes"></a>Bileşik dizinler

`ORDER BY` İki veya daha fazla özelliği bir bileşik dizin gerektirdiğini sorgular. Şu anda Bileşik dizinler yalnızca birden çok `ORDER BY` sorgu tarafından kullanılır. Varsayılan olarak, bir bileşik dizin tanımlanmadığında, gereken şekilde [Bileşik dizinler eklemelisiniz](how-to-manage-indexing-policy.md#composite-indexing-policy-examples) .

Bileşik dizin tanımlarken şunu belirtirsiniz:

- İki veya daha fazla özellik yolu. Özellik yollarının önemli olarak tanımlandığı sıra.
- Sıra (artan veya azalan).

Bileşik dizinler kullanılırken aşağıdaki noktalar kullanılır:

- Bileşik dizin yolları ORDER BY yan tümcesindeki özelliklerin dizisiyle eşleşmezse, bileşik dizin sorguyu desteklemiyor

- Bileşik dizin yollarının sırası (artan veya azalan) ORDER BY yan tümcesindeki sıra ile aynı olmalıdır.

- Bileşik dizin aynı zamanda tüm yollarda ters sırada olan ORDER BY yan tümcesini destekler.

A, b ve c özelliklerinde bir bileşik dizinin tanımlandığı aşağıdaki örneği göz önünde bulundurun:

| **Bileşik Dizin**     | **Örnek `ORDER BY` sorgu**      | **Dizin tarafından destekleniyor mu?** |
| ----------------------- | -------------------------------- | -------------- |
| ```(a asc, b asc)```         | ```ORDER BY  a asc, b asc```        | ```Yes```            |
| ```(a asc, b asc)```          | ```ORDER BY  b asc, a asc```        | ```No```             |
| ```(a asc, b asc)```          | ```ORDER BY  a desc, b desc```      | ```Yes```            |
| ```(a asc, b asc)```          | ```ORDER BY  a asc, b desc```       | ```No```             |
| ```(a asc, b asc, c asc)``` | ```ORDER BY  a asc, b asc, c asc``` | ```Yes```            |
| ```(a asc, b asc, c asc)``` | ```ORDER BY  a asc, b asc```        | ```No```            |

Tüm gerekli `ORDER BY` sorgulara hizmeti sağlamak için dizin oluşturma ilkenizi özelleştirmeniz gerekir.

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

## <a name="obsolete-attributes"></a>Kullanılmayan öznitelikler

Dizin oluşturma ilkeleriyle çalışırken artık kullanılmayan aşağıdaki özniteliklerle karşılaşabilirsiniz:

- `automatic`, dizin oluşturma ilkesinin kökünde tanımlanmış bir Boole değeri. Artık yoksayıldı ve kullandığınız araç gerektirdiğinde, olarak `true`ayarlanabilir.
- `precision`, eklenen yollar için dizin düzeyinde tanımlanmış bir sayıdır. Artık yoksayıldı ve kullandığınız araç gerektirdiğinde, olarak `-1`ayarlanabilir.
- `hash`, artık Aralık türüyle değiştirilmiş bir dizin türüdür.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde dizin oluşturma hakkında daha fazla bilgi edinin:

- [Dizine genel bakış](index-overview.md)
- [Dizin oluşturma ilkesini yönetme](how-to-manage-indexing-policy.md)
