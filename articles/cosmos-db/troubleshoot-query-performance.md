---
title: Azure Cosmos DB kullanırken sorgu sorunlarını giderme
description: Azure Cosmos DB SQL sorgu sorunlarını belirlemeyi, tanılamayı ve sorun gidermeyi öğrenin.
author: ginamr
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 01/14/2020
ms.author: girobins
ms.subservice: cosmosdb-sql
ms.reviewer: sngun
ms.openlocfilehash: c004031ec40bedcf83d77d08a34ce1d0e28fecd8
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76157040"
---
# <a name="troubleshoot-query-issues-when-using-azure-cosmos-db"></a>Azure Cosmos DB kullanırken sorgu sorunlarını giderme

Bu makalede, Azure Cosmos DB sorgularda sorun giderme için önerilen genel bir yaklaşım gösterilmektedir. Bu belgede özetlenen adımlar olası sorgu sorunları için "tümünü yakala" olarak düşünülmemelidir, ancak en sık kullanılan performans ipuçlarını buradan sunuyoruz. Bu belgeyi, Azure Cosmos DB Çekirdek (SQL) API 'sindeki yavaş veya pahalı sorguların giderilmesi için bir başlangıç yeri olarak kullanmanız gerekir. [Tanılama günlüklerini](cosmosdb-monitor-resource-logs.md) , yavaş olan veya önemli miktarda üretilen işi kullanan sorguları belirlemek için de kullanabilirsiniz.

Sorgu iyileştirmelerini büyük ölçüde kategorilere ayırarak Azure Cosmos DB: Istek birimi (RU), sorgu ve gecikme süresini azaltan iyileştirmeler. Bir sorgunun RU ücreti düşürülerek gecikme süresini de neredeyse tamamen azaltabilirsiniz.

Bu belge, [beslenme](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json) veri kümesi kullanılarak yeniden oluşturulabilen örnekleri kullanacaktır.

### <a name="obtaining-query-metrics"></a>Sorgu ölçümleri alma:

Azure Cosmos DB bir sorguyu iyileştirirken, ilk adım her zaman sorgunuzun [sorgu ölçümlerini elde](profile-sql-api-query.md) etmek için kullanılır. Bunlar ayrıca aşağıda gösterildiği gibi Azure portal aracılığıyla da kullanılabilir:

[sorgu ölçümleri almak ![](./media/troubleshoot-query-performance/obtain-query-metrics.png)](./media/troubleshoot-query-performance/obtain-query-metrics.png#lightbox)

Sorgu ölçümlerini aldıktan sonra, sorgularınız için çıktı belge sayısıyla alınan belge sayısını karşılaştırın. Aşağıda başvurulacak ilgili bölümleri belirlemek için bu karşılaştırmayı kullanın.

Alınan belge sayısı sorgunun yüklemesi için gereken belge sayısıdır. Çıktı belgesi sayısı, sorgunun sonuçları için gereken belge sayısıdır. Alınan belge sayısı, çıktı belge sayısından önemli ölçüde daha yüksekse, sorgunuzun dizinini kullanmayan ve tarama yapmak için gereken en az bir bölümü vardı.

Senaryonuza yönelik ilgili sorgu iyileştirmelerini anlamak için aşağıdaki bölüme başvurabilirsiniz:

### <a name="querys-ru-charge-is-too-high"></a>Sorgunun RU ücreti çok yüksek

#### <a name="retrieved-document-count-is-significantly-greater-than-output-document-count"></a>Alınan belge sayısı, çıktı belge sayısından önemli ölçüde büyük

- [Dizin oluşturma ilkesinin gerekli yolları içerdiğinden emin olun](#ensure-that-the-indexing-policy-includes-necessary-paths)

- [Hangi sistem işlevlerinin dizinden yararlankullandığını anlayın](#understand-which-system-functions-utilize-the-index)

- [Sorguları hem filtre hem de ORDER BY yan tümcesiyle iyileştirme](#optimize-queries-with-both-a-filter-and-an-order-by-clause)

- [AYRı kullanan sorguları iyileştirme](#optimize-queries-that-use-distinct)

- [Alt sorgu kullanarak JOIN ifadelerini iyileştirme](#optimize-join-expressions-by-using-a-subquery)

<br>

#### <a name="retrieved-document-count-is-approximately-equal-to-output-document-count"></a>Alınan belge sayısı, çıktı belge sayısına yaklaşık olarak eşit

- [Çapraz bölüm sorgularının kaçının](#avoid-cross-partition-queries)

- [Birden çok özelliklerde filtre bulunan sorguları iyileştirin](#optimize-queries-that-have-a-filter-on-multiple-properties)

- [Sorguları hem filtre hem de ORDER BY yan tümcesiyle iyileştirme](#optimize-queries-with-both-a-filter-and-an-order-by-clause)

<br>

### <a name="querys-ru-charge-is-acceptable-but-latency-is-still-too-high"></a>Sorgunun RU ücreti kabul edilebilir ancak gecikme hala çok yüksek

- [Uygulamanız ve Azure Cosmos DB arasında yakınlık artırma](#improving-proximity-between-your-app-and-azure-cosmos-db)

- [Sağlanan aktarım hızını artırma](#increasing-provisioned-throughput)

- [MaxConcurrency 'yi artırma](#increasing-maxconcurrency)

- [MaxBufferedItemCount artırma](#increasing-maxbuffereditemcount)

## <a name="optimizations-for-queries-where-retrieved-document-count-significantly-exceeds-output-document-count"></a>Alınan belge sayısı önemli ölçüde çıkış belgesi sayısını aştığında sorgulara yönelik iyileştirmeler:

 Alınan belge sayısı sorgunun yüklemesi için gereken belge sayısıdır. Çıktı belgesi sayısı, sorgunun sonuçları için gereken belge sayısıdır. Alınan belge sayısı, çıktı belge sayısından önemli ölçüde daha yüksekse, sorgunuzun dizinini kullanmayan ve tarama yapmak için gereken en az bir bölümü vardı.

 Aşağıda, dizin tarafından tamamen hizmet edilmemiş tarama sorgusunun bir örneği verilmiştir.

Sorgu:

 ```sql
SELECT VALUE c.description
FROM c
WHERE UPPER(c.description) = "BABYFOOD, DESSERT, FRUIT DESSERT, WITHOUT ASCORBIC ACID, JUNIOR"
 ```

Sorgu ölçümleri:

```
Retrieved Document Count                 :          60,951
Retrieved Document Size                  :     399,998,938 bytes
Output Document Count                    :               7
Output Document Size                     :             510 bytes
Index Utilization                        :            0.00 %
Total Query Execution Time               :        4,500.34 milliseconds
  Query Preparation Times
    Query Compilation Time               :            0.09 milliseconds
    Logical Plan Build Time              :            0.05 milliseconds
    Physical Plan Build Time             :            0.04 milliseconds
    Query Optimization Time              :            0.01 milliseconds
  Index Lookup Time                      :            0.01 milliseconds
  Document Load Time                     :        4,177.66 milliseconds
  Runtime Execution Times
    Query Engine Times                   :          322.16 milliseconds
    System Function Execution Time       :           85.74 milliseconds
    User-defined Function Execution Time :            0.00 milliseconds
  Document Write Time                    :            0.01 milliseconds
Client Side Metrics
  Retry Count                            :               0
  Request Charge                         :        4,059.95 RUs
```

Alınan belge sayısı (60.951), çıktı belge sayısından (7) çok büyük olduğundan, bu sorgunun bir tarama yapması gerekir. Bu durumda, [Upper ()](sql-query-upper.md) sistem işlevi dizinden kullanmaz.

## <a name="ensure-that-the-indexing-policy-includes-necessary-paths"></a>Dizin oluşturma ilkesinin gerekli yolları içerdiğinden emin olun

Dizin oluşturma ilkeniz `WHERE` tümceleri, `ORDER BY` yan tümceleri, `JOIN`ve çoğu sistem Işlevlerini içeren özellikleri kapsamalıdır. Dizin ilkesinde belirtilen yol, JSON belgelerindeki özelliği ile eşleşmelidir (büyük/küçük harfe duyarlı).

[Beslenme](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json) veri kümesinde basit bir sorgu çalıştırırsanız, `WHERE` yan tümcesindeki Özellik dizine eklendiğinde daha düşük bir ru ücreti gözlemliyoruz.

### <a name="original"></a>Özgün

Sorgu:

```sql
SELECT * FROM c WHERE c.description = "Malabar spinach, cooked"
```

Dizin oluşturma ilkesi:

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*"
        }
    ],
    "excludedPaths": [
        {
            "path": "/description/*"
        }
    ]
}
```

**Ru ücreti:** 409,51 ru

### <a name="optimized"></a>İyileştirilmiş

Dizin oluşturma ilkesi güncelleştirildi:

```json
{
    "indexingMode": "consistent",
    "automatic": true,
    "includedPaths": [
        {
            "path": "/*"
        }
    ],
    "excludedPaths": []
}
```

**Ru ücreti:** 2,98 ru

Herhangi bir zamanda, kullanılabilirlik veya performans yazmak için herhangi bir etkisi olmadan dizin oluşturma ilkesine ek özellikler ekleyebilirsiniz. Dizine yeni bir özellik eklerseniz, bu özelliği kullanan sorgular hemen yeni kullanılabilir dizini kullanır. Sorgu oluşturulurken yeni dizin kullanılır. Sonuç olarak, dizin yeniden oluşturma devam ettiğinden sorgu sonuçları tutarsız olabilir. Yeni bir özellik dizine alınmışsa, dizin yeniden oluşturma işlemi sırasında yalnızca var olan dizinleri kullanan sorgular etkilenmez. [Dizin dönüştürme ilerlemesini izleyebilirsiniz](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-indexing-policy#use-the-net-sdk-v3).

## <a name="understand-which-system-functions-utilize-the-index"></a>Hangi sistem işlevlerinin dizinden yararlankullandığını anlayın

İfade, bir dize değerleri aralığına çevrilebiliyorsa dizini kullanabilir; aksi takdirde dizini kullanamaz.

Dizini kullanılabilecek dize işlevlerinin listesi aşağıdadır:

- STARTSWITH(str_expr, str_expr)
- LEFT(str_expr, num_expr) = str_expr
- SUBSTRING(str_expr, num_expr, num_expr) = str_expr, but only if first num_expr is 0

Dizini kullanmayan ve her bir belgeyi yüklemesi gereken bazı ortak sistem işlevleri aşağıda verilmiştir:

| **Sistem Işlevi**                     | **Iyileştirme için fikirler**             |
| --------------------------------------- |------------------------------------------------------------ |
| CONTAINS                                | Tam metin araması için Azure Search kullan                        |
| ÜST/ALT                             | Karşılaştırma sırasında her seferinde verileri normalleştirmek için sistem işlevini kullanmak yerine, ekleme sırasında büyük/küçük harfleri normalleştirin. ```SELECT * FROM c WHERE UPPER(c.name) = 'BOB'``` gibi bir sorgu ```SELECT * FROM c WHERE c.name = 'BOB'``` olur. |
| Matematik işlevleri (toplamasız olmayan) | Sorgunuzun bir değeri sıklıkla hesaplamanız gerekiyorsa, bu değeri JSON belgenizde bir özellik olarak depolamayı düşünün. |

------

Sorgunun diğer kısımları, sistem işlevlerinin dizinini kullanmamasına rağmen yine de dizinden yararlanabilir.

## <a name="optimize-queries-with-both-a-filter-and-an-order-by-clause"></a>Sorguları hem filtre hem de ORDER BY yan tümcesiyle iyileştirme

Filtre ve bir `ORDER BY` yan tümcesinin bulunduğu sorgular normalde bir Aralık dizinini kullanır, ancak bu, bileşik dizinden sunulabilen daha verimli olacaktır. Dizin oluşturma ilkesini değiştirmenin yanı sıra, bileşik dizindeki tüm özellikleri `ORDER BY` yan tümcesine eklemeniz gerekir. Bu sorgu değişikliği, bileşik dizinden yararlandığından emin olur.  Bir sorgu [çalıştırarak, bu](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json) etkiyi gözlemleyebilirsiniz.

### <a name="original"></a>Özgün

Sorgu:

```sql
SELECT * FROM c WHERE c.foodGroup = “Soups, Sauces, and Gravies” ORDER BY c._ts ASC
```

Dizin oluşturma ilkesi:

```json
{

        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[]
}
```

**Ru ücreti:** 44,28 ru

### <a name="optimized"></a>İyileştirilmiş

Sorgu güncelleştirildi (`ORDER BY` yan tümcesindeki her iki özelliği de içerir):

```sql
SELECT * FROM c 
WHERE c.foodGroup = “Soups, Sauces, and Gravies” 
ORDER BY c.foodGroup, c._ts ASC
```

Dizin oluşturma ilkesi güncelleştirildi:

```json
{  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/foodGroup",
                    "order":"ascending"
        },
                {  
                    "path":"/_ts",
                    "order":"ascending"
                }
            ]
        ]
    }

```

**Ru ücreti:** 8,86 ru

## <a name="optimize-queries-that-use-distinct"></a>AYRı kullanan sorguları iyileştirme

Yinelenen sonuçlar ardışık olduğunda `DISTINCT` sonuç kümesini bulmak daha verimli olacaktır. Sorguya ve bileşik dizine `ORDER BY` bir yan tümce eklemek, yinelenen sonuçların ardışık olmasını sağlar. Birden çok özellik `ORDER BY` gerekiyorsa, bileşik bir dizin ekleyin. Bir sorgu [çalıştırarak, bu](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json) etkiyi gözlemleyebilirsiniz.

### <a name="original"></a>Özgün

Sorgu:

```sql
SELECT DISTINCT c.foodGroup 
FROM c
```

**Ru ücreti:** 32,39 ru

### <a name="optimized"></a>İyileştirilmiş

Güncelleştirilmiş sorgu:

```sql
SELECT DISTINCT c.foodGroup 
FROM c 
ORDER BY c.foodGroup
```

**Ru ücreti:** 3,38 ru

## <a name="optimize-join-expressions-by-using-a-subquery"></a>Alt sorgu kullanarak JOIN ifadelerini iyileştirme
Çoklu değer alt sorguları, `WHERE` yan tümcesindeki tüm çapraz birleşimlerden sonra olmak yerine her bir SELECT-many ifadesinden sonra koşullar göndererek `JOIN` ifadelerini en iyileştirebilir.

Şu sorguyu inceleyin:

```sql
SELECT Count(1) AS Count
FROM c
JOIN t IN c.tags
JOIN n IN c.nutrients
JOIN s IN c.servings
WHERE t.name = 'infant formula' AND (n.nutritionValue > 0
AND n.nutritionValue < 10) AND s.amount > 1
```

**Ru ücreti:** 167,62 ru

Bu sorgu için dizin, "Infant Formula", nutritionValue 0 ' dan büyük ve 1 ' den büyük olan bir etikete sahip herhangi bir belgeyle eşleşir. Buradaki `JOIN` ifadesi, herhangi bir filtre uygulanmadan önce, eşleşen her belge için tüm etiket, nutristalar ve servilerler dizilerinin çapraz çarpımını gerçekleştirir. `WHERE` yan tümcesi, her `<c, t, n, s>` kayıt noktasında filtre koşulunu uygular.

Örneğin, eşleşen bir belge üç dizinin her birinde 10 öğe içeriyorsa, 1 x 10 x 10 x 10 (diğer bir deyişle, 1.000) tanımlama gruplarına genişletilir. Burada alt sorguları kullanmak, birleştirilmiş dizi öğelerinin sonraki ifadeyle katılmadan önce filtrelemeye yardımcı olabilir.

Bu sorgu, önceki bir ile eşdeğerdir, ancak alt sorgular kullanır:

```sql
SELECT Count(1) AS Count
FROM c
JOIN (SELECT VALUE t FROM t IN c.tags WHERE t.name = 'infant formula')
JOIN (SELECT VALUE n FROM n IN c.nutrients WHERE n.nutritionValue > 0 AND n.nutritionValue < 10)
JOIN (SELECT VALUE s FROM s IN c.servings WHERE s.amount > 1)
```

**Ru ücreti:** 22,17 ru

Etiketler dizisindeki yalnızca bir öğe filtreyle eşleşen ve hem nutristalar hem de servi dizileri için beş öğe olduğunu varsayalım. `JOIN` ifadeler, ilk sorgudaki 1.000 öğe aksine, 1 x 1 x 5 x 5 = 25 öğe olacak şekilde genişletilir.

## <a name="optimizations-for-queries-where-retrieved-document-count-is-approximately-equal-to-output-document-count"></a>Alınan belge sayısı, çıktı belge sayısına yaklaşık olarak eşit olan sorgulara yönelik iyileştirmeler:

Alınan belge sayısı yaklaşık olarak çıktı belge sayısına eşitse, sorgunun birçok gereksiz belgeyi taramak zorunda olmadığı anlamına gelir. En üstteki anahtar sözcüğü kullananlar gibi birçok sorgu için, alınan belge sayısı, çıkış belgesi sayısını 1 ' den fazla olabilir. Bu sorun için neden olmamalıdır.

## <a name="avoid-cross-partition-queries"></a>Çapraz bölüm sorgularının kaçının

Azure Cosmos DB, Istek birimi ve veri depolama alanı artışına göre bağımsız kapsayıcıları ölçeklendirmek için [bölümleme](partitioning-overview.md) kullanır. Her fiziksel bölümün ayrı ve bağımsız bir dizini vardır. Sorgunuzun, kapsayıcının bölüm anahtarı ile eşleşen bir eşitlik filtresi varsa, yalnızca ilgili bölümün dizinini denetlemeniz gerekir. Bu iyileştirme, sorgunun gerektirdiği toplam RU sayısını azaltır.

Çok sayıda sağlanan RU (30.000 üzerinde) veya büyük miktarda veri (yaklaşık 100 GB) varsa, sorgu RU ücretlerinde önemli bir düşüş görmeniz için büyük olasılıkla çok sayıda kapsayıcınıza sahip olursunuz.

Örneğin, bölüm anahtarı \ Grup olan bir kapsayıcı oluşturuyoruz, aşağıdaki sorguların yalnızca tek bir fiziksel bölümü denetlemesi gerekir:

```sql
SELECT * FROM c
WHERE c.foodGroup = “Soups, Sauces, and Gravies” and c.description = "Mushroom, oyster, raw"
```

Bu sorgular, bölüm anahtarı sorguya dahil ederek de iyileştirilir:

```sql
SELECT * FROM c
WHERE c.foodGroup IN(“Soups, Sauces, and Gravies”, “"Vegetables and Vegetable Products”) and  c.description = "Mushroom, oyster, raw"
```

Bölüm anahtarında Aralık filtrelerine sahip olan veya bölüm anahtarında filtre bulunmayan sorgular, "fan" ve sonuçlar için her fiziksel bölümün dizinini denetleyecek.

```sql
SELECT * FROM c
WHERE c.description = "Mushroom, oyster, raw"
```

```sql
SELECT * FROM c
WHERE c.foodGroup > “Soups, Sauces, and Gravies” and c.description = "Mushroom, oyster, raw"
```

## <a name="optimize-queries-that-have-a-filter-on-multiple-properties"></a>Birden çok özelliklerde filtre bulunan sorguları iyileştirin

Birden çok özelliklerde filtreler içeren sorgular normalde bir Aralık dizini kullanır, ancak bu, bileşik dizinden sunulabilen daha verimli olacaktır. Küçük miktarlarda veri için bu iyileştirme önemli bir etkiye sahip olmayacaktır. Ancak, büyük miktarlarda veri için yararlı olabilir. Bileşik dizin başına yalnızca en çok bir eşitlik olmayan filtreyi en iyi hale getirebilirsiniz. Sorgunuzun birden çok eşitlik olmayan filtresi varsa, bileşik dizin kullanacak şekilde bunlardan birini seçmelisiniz. Kalan Aralık dizinleri kullanmaya devam edecektir. Eşitlik olmayan filtrenin en son bileşik dizinde tanımlanması gerekir. [Bileşik dizinler hakkında daha fazla bilgi edinin](index-policy.md#composite-indexes)

Bir bileşik dizin ile iyileştirilen bazı sorgu örnekleri aşağıda verilmiştir:

```sql
SELECT * FROM c
WHERE c.foodGroup = "Vegetables and Vegetable Products" AND c._ts = 1575503264
```

```sql
SELECT * FROM c
WHERE c.foodGroup = "Vegetables and Vegetable Products" AND c._ts > 1575503264
```

İlgili bileşik dizin aşağıda verilmiştir:

```json
{  
        "automatic":true,
        "indexingMode":"Consistent",
        "includedPaths":[  
            {  
                "path":"/*"
            }
        ],
        "excludedPaths":[],
        "compositeIndexes":[  
            [  
                {  
                    "path":"/foodGroup",
                    "order":"ascending"
                },
                {  
                    "path":"/_ts",
                    "order":"ascending"
                }
            ]
        ]
}
```

## <a name="common-optimizations-that-reduce-query-latency-no-impact-on-ru-charge"></a>Sorgu gecikmesini azaltan genel iyileştirmeler (RU ücreti üzerinde etki yok):

Çoğu durumda RU ücreti kabul edilebilir ancak sorgu gecikmesi hala çok yüksek olabilir. Aşağıdaki bölümlerde sorgu gecikmesini azaltmaya yönelik ipuçlarına genel bir bakış sunulmaktadır. Aynı sorguyu aynı veri kümesinde birden çok kez çalıştırırsanız, her seferinde aynı RU ücretine sahip olur. Ancak sorgu gecikmesi sorgu yürütmeleri arasında farklılık gösterebilir.

## <a name="improving-proximity-between-your-app-and-azure-cosmos-db"></a>Uygulamanız ve Azure Cosmos DB arasında yakınlık artırma

Azure Cosmos DB hesabından farklı bir bölgeden çalıştırılan sorguların aynı bölge içinde çalıştırıldıklarından daha yüksek bir gecikme süresi olacaktır. Örneğin, masaüstü bilgisayarınızda kod çalıştırıyorsanız, sorgunun aynı Azure bölgesindeki bir sanal makineden Azure Cosmos DB kadar büyük veya yüzlerce (veya daha fazla) milisaniyelik gecikme süresi beklemelisiniz. Verilerinizi uygulamanıza yakın bir şekilde getirebilmeniz için [Azure Cosmos DB verileri küresel olarak dağıtmak](distribute-data-globally.md) basittir.

## <a name="increasing-provisioned-throughput"></a>Sağlanan aktarım hızını artırma

Azure Cosmos DB, sağlanan aktarım hızı Istek birimleri (RU) cinsinden ölçülür. 5 RU 'ın aktarım hızını tüketen bir sorgunuz olduğunu düşünelim. Örneğin, 1.000 RU 'yi sağlarsanız, bu sorguyu saniye başına 200 kez çalıştırabilirsiniz. Yeterli kullanılabilir üretilen iş olmadığında sorguyu çalıştırmaya çalıştınız, Azure Cosmos DB bir HTTP 429 hatası döndürür. Geçerli çekirdek (SQL) API SDK 'sının herhangi biri, kısa bir süre bekledikten sonra bu sorguyu otomatik olarak yeniden dener. Kısıtlanmış isteklerin süresi daha uzun sürer, bu nedenle sağlanan verimlilik arttırıldığında sorgu gecikmesi iyileştirebilirler. Azure portal, [Kısıtlanmış isteklerin toplam sayısını](use-metrics.md#understand-how-many-requests-are-succeeding-or-causing-errors) , ölçüm dikey penceresinde gözlemleyebilirsiniz.

## <a name="increasing-maxconcurrency"></a>MaxConcurrency 'yi artırma

Paralel sorgular birden çok bölümü paralel olarak sorgulayarak çalışır. Ancak, tek bir bölümlenmiş koleksiyondaki veriler, sorguya göre işlem içine alınır. Bu nedenle, MaxConcurrency 'ın bölüm sayısına göre ayarlanması en iyi performansı elde etmek için en yüksek performanslı bir sorgu sağlar, ancak diğer tüm sistem koşulları aynı kalır. Bölüm sayısını bilmiyorsanız, MaxConcurrency (veya daha eski SDK sürümlerindeki Maxdegreesofparalellik) değerini yüksek bir sayı olarak ayarlayabilir ve sistem en az paralellik derecesi olarak en düşük (bölüm sayısını, Kullanıcı tarafından girilen girişi) seçer.

## <a name="increasing-maxbuffereditemcount"></a>MaxBufferedItemCount artırma

Sorgular, geçerli sonuç toplu işi istemci tarafından işlendiği sırada sonuçları önceden getirmek üzere tasarlanmıştır. Önceden getirme, bir sorgunun genel gecikme artışında yardımcı olur. MaxBufferedItemCount ayarı, önceden getirilen sonuçların sayısını sınırlar. Bu değeri beklenen sonuç sayısı (veya daha yüksek bir sayı) olarak ayarlayarak, sorgu ön alma işleminden en fazla avantajı alabilir. Bu değerin-1 olarak ayarlanması, sistemin arabelleğe eklenecek öğe sayısını otomatik olarak karar vermesine olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar
Sorgu başına ru ölçüsünde aşağıdaki belgelere bakın, sorgularınızı ayarlamak için yürütme istatistiklerini alın ve daha fazlasını yapın:

* [.NET SDK kullanarak SQL sorgusu yürütme ölçümlerini alma](profile-sql-api-query.md)
* [Azure Cosmos DB ile sorgu performansını ayarlama](sql-api-sql-query-metrics.md)
* [.NET SDK’sı için performans ipuçları](performance-tips.md)
