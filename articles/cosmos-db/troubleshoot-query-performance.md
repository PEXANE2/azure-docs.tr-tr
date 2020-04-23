---
title: Azure Cosmos DB kullanırken sorgu sorunlarını giderme
description: Azure Cosmos DB SQL sorgu sorunlarını nasıl tanımlayabildiğini, tanılamayı ve sorun gidermeyi öğrenin.
author: timsander1
ms.service: cosmos-db
ms.topic: troubleshooting
ms.date: 04/20/2020
ms.author: tisande
ms.subservice: cosmosdb-sql
ms.reviewer: sngun
ms.openlocfilehash: 4a8b61f3719a60af567d10f8839987e613babc9e
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81870449"
---
# <a name="troubleshoot-query-issues-when-using-azure-cosmos-db"></a>Azure Cosmos DB kullanırken sorgu sorunlarını giderme

Bu makale, Azure Cosmos DB'deki sorun giderme sorguları için genel olarak önerilen bir yaklaşımdan geçer. Bu makalede özetlenen adımları olası sorgu sorunlarına karşı tam bir savunma olarak düşünmeseniz de, en yaygın performans ipuçlarını buraya dahil ettik. Bu makaleyi, Azure Cosmos DB çekirdeği (SQL) API'sinde yavaş veya pahalı sorguları giderme için başlangıç noktası olarak kullanmalısınız. Yavaş olan veya önemli miktarda iş tükeden sorguları tanımlamak için [tanılama günlüklerini](cosmosdb-monitor-resource-logs.md) de kullanabilirsiniz.

Azure Cosmos DB'de sorgu optimizasyonlarını geniş bir şekilde kategorilere ayırabilirsiniz:

- Sorgunun İstek Birimi (RU) ücretini azaltan optimizasyonlar
- Gecikme süresini azaltan optimizasyonlar

Bir sorgunun RU ücretini azaltırsanız, gecikme süresini de neredeyse kesinlikle azaltırsınız.

Bu makalede, [beslenme](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json) veri kümesini kullanarak yeniden oluşturabileceğiniz örnekler verilmektedir.

## <a name="common-sdk-issues"></a>Ortak SDK sorunları

- En iyi performans için [Performans ipuçlarını](performance-tips.md)izleyin.
    > [!NOTE]
    > Daha iyi performans için Windows 64 bit ana bilgisayar işlemeyi öneririz. SQL SDK, sorguları yerel olarak ayrışdırmak ve optimize etmek için yerel serviceInterop.dll içerir. ServiceInterop.dll yalnızca Windows x64 platformunda desteklenir. Linux ve ServiceInterop.dll'nin kullanılamadığı diğer desteklenmeyen platformlar için, optimize edilmiş sorguyu almak için ağ geçidine ek bir ağ çağrısı yapılır.
- Sorgularınız `MaxItemCount` için a ayarlayabilirsiniz, ancak minimum öğe sayısı belirtemezsiniz.
    - Kod, sıfırdan `MaxItemCount`.
    - Bir sayfadaki öğe sayısı her zaman belirtilenden `MaxItemCount`daha az olacaktır. Ancak, `MaxItemCount` kesinlikle maksimum ve bu miktardan daha az sonuç olabilir.
- Bazen sorgularda, gelecekteki bir sayfada sonuç olsa bile boş sayfalar olabilir. Bunun nedenleri:
    - SDK birden fazla ağ araması yapıyor olabilir.
    - Sorgunun belgeleri alması uzun zaman alabilir.
- Tüm sorgularda, sorgunun devam etmesine izin verecek bir devam belirteci vardır. Sorguyu tamamen boşalttın. SDK örneklerine bakın ve `while` tüm `FeedIterator.HasMoreResults` sorguyu boşaltmak için bir döngü kullanın.

## <a name="get-query-metrics"></a>Sorgu ölçümlerini alın

Azure Cosmos DB'de bir sorguyu en iyi duruma getirdiğinizde, ilk adım her zaman sorgunuzun [sorgu ölçümlerini almaktır.](profile-sql-api-query.md) Bu ölçümler, Azure portalı üzerinden de kullanılabilir:

[![Sorgu ölçümlerini](./media/troubleshoot-query-performance/obtain-query-metrics.png) alma](./media/troubleshoot-query-performance/obtain-query-metrics.png#lightbox)

Sorgu ölçümlerini aldıktan sonra, Alınan Belge Sayısı ile sorgunuzun Çıktı Belge Sayısı'nı karşılaştırın. Bu makalede gözden geçirmek için ilgili bölümleri tanımlamak için bu karşılaştırmayı kullanın.

Alınan Belge Sayısı, sorgunun yüklenmesi için gereken belge sayısıdır. Çıktı Belge Sayısı, sorgunun sonuçları için gereken belge sayısıdır. Alınan Belge Sayısı Çıktı Belge Sayısı'ndan önemli ölçüde yüksekse, sorgunuzun dizinkullanamayan ve tarama yapması gereken en az bir bölümü vardı.

Senaryonuzla ilgili sorgu optimizasyonlarını anlamak için aşağıdaki bölümlere bakın.

### <a name="querys-ru-charge-is-too-high"></a>Sorgunun RU ücreti çok yüksek

#### <a name="retrieved-document-count-is-significantly-higher-than-output-document-count"></a>Alınan Belge Sayısı Çıktı Belge Sayısı'ndan önemli ölçüde daha yüksektir

- [Dizin oluşturma ilkesine gerekli yolları ekleyin.](#include-necessary-paths-in-the-indexing-policy)

- [Dizini hangi sistem işlevlerinin kullandığını anlayın.](#understand-which-system-functions-use-the-index)

- [Dizin imi hangi toplu sorguların kullandığını anlayın.](#understand-which-aggregate-queries-use-the-index)

- [Hem filtre hem de ORDER BY yan tümcesi olan sorguları değiştirin.](#modify-queries-that-have-both-a-filter-and-an-order-by-clause)

- [Bir alt sorgu kullanarak JOIN ifadelerini optimize edin.](#optimize-join-expressions-by-using-a-subquery)

<br>

#### <a name="retrieved-document-count-is-approximately-equal-to-output-document-count"></a>Alınan Belge Sayısı, Çıktı Belge Sayısı'na yaklaşık olarak eşittir

- [Çapraz bölüm sorgularından kaçının.](#avoid-cross-partition-queries)

- [Birden çok özellikte filtreleri olan sorguları en iyi duruma getirin.](#optimize-queries-that-have-filters-on-multiple-properties)

- [Hem filtre hem de ORDER BY yan tümcesi olan sorguları değiştirin.](#modify-queries-that-have-both-a-filter-and-an-order-by-clause)

<br>

### <a name="querys-ru-charge-is-acceptable-but-latency-is-still-too-high"></a>Sorgunun RU ücreti kabul edilebilir ama gecikme si hala çok yüksek

- [Yakınlığı geliştirin.](#improve-proximity)

- [Sağlanan iş bilgili iş buzun artirililmesi.](#increase-provisioned-throughput)

- [MaxConcurrency'i artırın.](#increase-maxconcurrency)

- [MaxBufferedItemCount'ı artırın.](#increase-maxbuffereditemcount)

## <a name="queries-where-retrieved-document-count-exceeds-output-document-count"></a>Alınan Belge Sayısının Çıktı Belge Sayısını aştığı sorgular

 Alınan Belge Sayısı, sorgunun yüklenmesi için gereken belge sayısıdır. Çıktı Belge Sayısı, sorgunun sonuçları için gereken belge sayısıdır. Alınan Belge Sayısı Çıktı Belge Sayısı'ndan önemli ölçüde yüksekse, sorgunuzun dizinkullanamayan ve tarama yapması gereken en az bir bölümü vardı.

Dizin tarafından tam olarak sunulmayan bir scan sorgusu örneği aşağıda verilmiştir:

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

Alınan Belge Sayısı (60.951) Çıktı Belge Sayısı'ndan (7) önemli ölçüde daha yüksektir, bu nedenle bu sorgunun taranıyor olması gerekir. Bu durumda, sistem işlevi [UPPER()](sql-query-upper.md) dizini kullanmaz.

### <a name="include-necessary-paths-in-the-indexing-policy"></a>Dizin oluşturma ilkesine gerekli yolları ekleme

Dizin oluşturma ilkeniz yan `WHERE` tümcelerde, `ORDER BY` yan `JOIN`tümcelerde ve çoğu sistem işlevinde yer alan özellikleri kapsamalıdır. Dizin ilkesinde belirtilen yol JSON belgelerindeki özelliği eşleştirmelidir (büyük/küçük harf duyarlıdır).

[Beslenme](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json) veri kümesinde basit bir sorgu çalıştırDığınızda, `WHERE` yan tümcedeki özellik dizine eklendiğinde çok daha düşük bir RU ücreti gözlemlersiniz:

#### <a name="original"></a>Özgün

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

**RU şarj:** 409,51 RUs

#### <a name="optimized"></a>İyileştirilmiş

Güncelleştirilmiş dizin oluşturma ilkesi:

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

**RU şarj:** 2.98 RUs

Dizin oluşturma ilkesine, yazma kullanılabilirliği veya performansı üzerinde hiçbir etkisi olmadan, istediğiniz zaman özellikler ekleyebilirsiniz. Dizin için yeni bir özellik eklerseniz, özelliği kullanan sorgular hemen yeni kullanılabilir dizini kullanır. Sorgu, oluşturulurken yeni dizini kullanır. Bu nedenle, dizin yeniden oluşturma devam ederken sorgu sonuçları tutarsız olabilir. Yeni bir özellik dizine eklenirse, dizin yeniden oluşturma sırasında yalnızca varolan dizinleri kullanan sorgular etkilenmez. [Dizin dönüşüm ilerlemeyi izleyebilirsiniz.](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-indexing-policy#use-the-net-sdk-v3)

### <a name="understand-which-system-functions-use-the-index"></a>Dizini hangi sistem işlevlerinin kullandığını anlama

Bir ifade dize değerleri aralığına çevrilebiliyorsa, dizini kullanabilir. Yoksa olamaz.

Dizin kullanabilen bazı yaygın dize işlevlerinin listesi aşağıda veda edebilirsiniz:

- STARTSWITH(str_expr, str_expr)
- LEFT(str_expr, num_expr) = str_expr
- SUBSTRING(str_expr, num_expr, num_expr) = str_expr, ancak ilk num_expr 0 ise

Dizin kullanmayan ve her belgeyi yüklemesi gereken bazı yaygın sistem işlevleri aşağıda verilmelidir:

| **Sistem fonksiyonu**                     | **Optimizasyon için fikirler**             |
| --------------------------------------- |------------------------------------------------------------ |
| CONTAINS                                | Tam metin arama için Azure Arama'yı kullanın.                        |
| ÜST/ALT                             | Karşılaştırmalar için verileri normalleştirmek için sistem işlevini kullanmak yerine, ekleme üzerine kasanormalleştirin. Gibi ```SELECT * FROM c WHERE UPPER(c.name) = 'BOB'``` bir ```SELECT * FROM c WHERE c.name = 'BOB'```sorgu olur. |
| Matematiksel fonksiyonlar (toplam dışı) | Sorgunuzda sık sık bir değer hesaplamanız gerekiyorsa, değeri JSON belgenizde bir özellik olarak depolamayı düşünün. |

------

Sorgunun diğer bölümleri, sistem işlevleri olmasa bile dizini kullanmaya devam edebilir.

### <a name="understand-which-aggregate-queries-use-the-index"></a>Dizini hangi toplu sorguların kullandığını anlama

Çoğu durumda, Azure Cosmos DB'deki toplu sistem işlevleri dizini kullanır. Ancak, bir toplu sorgudaki filtrelere veya ek yan tümcelere bağlı olarak, sorgu motorunun çok sayıda belge yüklemesi gerekebilir. Genellikle, sorgu motoru önce eşitlik ve aralık filtreleri uygular. Bu filtreleri uyguladıktan sonra, sorgu motoru ek filtreleri değerlendirebilir ve gerekirse toplamı hesaplamak için kalan belgeleri yüklemeye başvurabilir.

Örneğin, bu iki örnek sorgu göz önüne alındığında, `CONTAINS` hem eşitlik hem de sistem işlev filtresi olan `CONTAINS` sorgu genellikle sadece sistem işlev filtresi olan bir sorgudan daha verimli olacaktır. Bunun nedeni, eşitlik filtresinin önce uygulanması ve belgelerin daha pahalı `CONTAINS` filtre için yüklenmesi gerekmeden önce dizini kullanmasıdır.

Yalnızca `CONTAINS` filtre ile sorgula - daha yüksek RU ücreti:

```sql
SELECT COUNT(1) FROM c WHERE CONTAINS(c.description, "spinach")
```

Hem eşitlik filtresi hem `CONTAINS` de filtre ile sorgula - RU ücretini düşür:

```sql
SELECT AVG(c._ts) FROM c WHERE c.foodGroup = "Sausages and Luncheon Meats" AND CONTAINS(c.description, "spinach")
```

Dizin tam olarak kullanılmayacak toplam sorgularının ek örnekleri aşağıda verilmiştir:

#### <a name="queries-with-system-functions-that-dont-use-the-index"></a>Dizini kullanmayan sistem işlevlerine sahip sorgular

Dizinin kullanıp kullanıp kullanmadığınızı görmek için ilgili [sistem işlevinin sayfasına](sql-query-system-functions.md) başvurmalısınız.

```sql
SELECT MAX(c._ts) FROM c WHERE CONTAINS(c.description, "spinach")
```

#### <a name="aggregate-queries-with-user-defined-functionsudfs"></a>Kullanıcı tanımlı işlevlere (UDF'ler) sahip toplu sorgular

```sql
SELECT AVG(c._ts) FROM c WHERE udf.MyUDF("Sausages and Luncheon Meats")
```

#### <a name="queries-with-group-by"></a>GROUP BY ile sorgular

Maddedeki mülklerin kardinalliği arttıkça RU ücreti `GROUP BY` artacaktır. `GROUP BY` Bu örnekte, sorgu motorunun `c.foodGroup = "Sausages and Luncheon Meats"` filtreyle eşleşen her belgeyi yüklemesi gerekir, böylece RU ücretinin yüksek olması beklenir.

```sql
SELECT COUNT(1) FROM c WHERE c.foodGroup = "Sausages and Luncheon Meats" GROUP BY c.description
```

Sık sık aynı toplu sorguları çalıştırmayı planlıyorsanız, [Azure Cosmos DB değişiklik akışıyla](change-feed.md) tek tek sorguları çalıştırmaktan daha gerçek zamanlı bir gerçek zamanlı görüntü oluşturmak daha verimli olabilir.

### <a name="modify-queries-that-have-both-a-filter-and-an-order-by-clause"></a>Hem filtre hem de ORDER BY yan tümcesi olan sorguları değiştirme

Filtresi ve `ORDER BY` yan tümcesi olan sorgular normalde bir aralık dizini kullansa da, bileşik dizinden sunulabilirse daha verimli olurlar. Dizin oluşturma ilkesini değiştirmeye ek olarak, bileşik dizindeki tüm `ORDER BY` özellikleri yan tümeceye eklemeniz gerekir. Sorgudaki bu değişiklik, bileşik dizini kullandığından emin olur.  [Beslenme](https://github.com/CosmosDB/labs/blob/master/dotnet/setup/NutritionData.json) veri kümesinde bir sorgu çalıştırarak etkisini gözlemleyebilirsiniz:

#### <a name="original"></a>Özgün

Sorgu:

```sql
SELECT * FROM c WHERE c.foodGroup = "Soups, Sauces, and Gravies" ORDER BY c._ts ASC
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

**RU şarj:** 44,28 RUs

#### <a name="optimized"></a>İyileştirilmiş

Güncelleştirilmiş sorgu `ORDER BY` (yan tümcedeki her iki özelliği de içerir):

```sql
SELECT * FROM c
WHERE c.foodGroup = "Soups, Sauces, and Gravies"
ORDER BY c.foodGroup, c._ts ASC
```

Güncelleştirilmiş dizin oluşturma ilkesi:

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

**RU şarj:** 8.86 RUs

### <a name="optimize-join-expressions-by-using-a-subquery"></a>Bir alt sorgu kullanarak JOIN ifadelerini optimize edin
Çok değerli alt sorgular, `JOIN` `WHERE` tüm çapraz birleştirmeler yerine her seç-çok ifadesinden sonra yüklemleri iterek ifadeleri en iyi duruma getirebilir.

Bu sorguyı göz önünde bulundurun:

```sql
SELECT Count(1) AS Count
FROM c
JOIN t IN c.tags
JOIN n IN c.nutrients
JOIN s IN c.servings
WHERE t.name = 'infant formula' AND (n.nutritionValue > 0
AND n.nutritionValue < 10) AND s.amount > 1
```

**RU şarj:** 167,62 RUs

Bu sorgu için dizin, "bebek formülü" adı, nutritionValue Değeri 0'dan büyük ve 1'den büyük hizmet miktarı ile etikete sahip tüm belgelerle eşleşir. Buradaki `JOIN` ifade, herhangi bir filtre uygulanmadan önce her eşleşen belge için etiket, besin ve porsiyon dizilerinin tüm öğelerinin çapraz ürünlerini gerçekleştirecektir. Madde `WHERE` daha sonra her `<c, t, n, s>` tuple üzerinde filtre yüklemi uygular.

Örneğin, eşleşen bir belgenin üç dizinin her birinde 10 öğesi varsa, bu belge 1 x 10 x 10 x 10 (yani 1.000) tuples'e genişletilir. Burada alt sorguların kullanımı, birsonraki ifadeyle katılmadan önce birleştirilmiş dizi öğelerini filtrelemene yardımcı olabilir.

Bu sorgu bir öncekine eşdeğerdir, ancak alt sorguları kullanır:

```sql
SELECT Count(1) AS Count
FROM c
JOIN (SELECT VALUE t FROM t IN c.tags WHERE t.name = 'infant formula')
JOIN (SELECT VALUE n FROM n IN c.nutrients WHERE n.nutritionValue > 0 AND n.nutritionValue < 10)
JOIN (SELECT VALUE s FROM s IN c.servings WHERE s.amount > 1)
```

**RU şarj:** 22.17 RUs

Etiket dizisindeyalnızca bir öğenin filtreyle eşleştiğini ve hem besin hem de porsiyon dizileri için beş öğe olduğunu varsayalım. İfadeler, `JOIN` ilk sorgudaki 1.000 öğeyerine 1 x 1 x 5 x 5 = 25 öğeye genişletir.

## <a name="queries-where-retrieved-document-count-is-equal-to-output-document-count"></a>Alınan Belge Sayısının Çıktı Belge Sayısına eşit olduğu sorgular

Alınan Belge Sayısı çıktı belge sayısına yaklaşık eşitse, sorgunun çok sayıda gereksiz belgeyi tetmesi gerekmez. TOP anahtar sözcük kullananlar gibi birçok sorgu için, Alınan Belge Sayısı Çıktı Belge Sayısı'nı 1'e kadar aşabilir. Bu konuda endişelenmene gerek yok.

### <a name="avoid-cross-partition-queries"></a>Çapraz bölüm sorgularından kaçının

Azure Cosmos DB, İstek Birimi ve veri depolama gereksinimleri arttıkça tek tek kapsayıcıları ölçeklendirmek için [bölümleme](partitioning-overview.md) kullanır. Her fiziksel bölüm ayrı ve bağımsız bir dizin vardır. Sorgunuzun kapsayıcınızın bölüm anahtarıyla eşleşen bir eşitlik filtresi varsa, yalnızca ilgili bölümün dizinini denetlemeniz gerekir. Bu optimizasyon, sorgunun gerektirdiği toplam RUs sayısını azaltır.

Çok sayıda verilen RUs'un (30.000'den fazla) veya depolanan büyük miktarda veriniz (yaklaşık 100 GB'dan fazla) varsa, büyük olasılıkla ru sorgu ücretlerinde önemli bir azalma görecek kadar büyük bir kapsayıcınız vardır.

Örneğin, bölüm tuşu foodGroup'a sahip bir kapsayıcı oluşturursanız, aşağıdaki sorguların yalnızca tek bir fiziksel bölümü denetlemesi gerekir:

```sql
SELECT * FROM c
WHERE c.foodGroup = "Soups, Sauces, and Gravies" and c.description = "Mushroom, oyster, raw"
```

Bu sorgular, sorgudaki bölüm anahtarının eklenmesiyle de en iyi duruma getirilebilir:

```sql
SELECT * FROM c
WHERE c.foodGroup IN("Soups, Sauces, and Gravies", "Vegetables and Vegetable Products") and c.description = "Mushroom, oyster, raw"
```

Bölüm anahtarında aralık filtreleri olan veya bölüm anahtarında filtre olmayan sorguların sonuçlar için her fiziksel bölümün dizinini denetlemesi gerekir:

```sql
SELECT * FROM c
WHERE c.description = "Mushroom, oyster, raw"
```

```sql
SELECT * FROM c
WHERE c.foodGroup > "Soups, Sauces, and Gravies" and c.description = "Mushroom, oyster, raw"
```

### <a name="optimize-queries-that-have-filters-on-multiple-properties"></a>Birden çok özellikte filtreleri olan sorguları en iyi duruma getirin

Birden çok özellik üzerinde filtreleri olan sorgular normalde bir aralık dizini kullanacak olsa da, bileşik dizinten sunulabilirse daha verimli olurlar. Küçük miktarda veri için bu optimizasyon önemli bir etkiye sahip olmaz. Ancak, büyük miktarda veri için yararlı olabilir. Bileşik dizin başına en fazla bir eşit olmayan filtreyi en iyi duruma getirebilirsiniz. Sorgunuzda birden çok eşitlik olmayan filtre varsa, bileşik dizini kullanacak bunlardan birini seçin. Geri kalanı aralık dizinleri kullanmaya devam edecektir. Eşitlik süzgecin indekste son olarak tanımlanması gerekir. [Bileşik dizinler hakkında daha fazla bilgi edinin.](index-policy.md#composite-indexes)

Bileşik dizinle en iyi duruma getirilebilen sorgulara bazı örnekler aşağıda verilmiştir:

```sql
SELECT * FROM c
WHERE c.foodGroup = "Vegetables and Vegetable Products" AND c._ts = 1575503264
```

```sql
SELECT * FROM c
WHERE c.foodGroup = "Vegetables and Vegetable Products" AND c._ts > 1575503264
```

İşte ilgili bileşik indeks:

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

## <a name="optimizations-that-reduce-query-latency"></a>Sorgu gecikmesüresini azaltan optimizasyonlar

Çoğu durumda, sorgu gecikmesi hala çok yüksek olduğunda RU ücreti kabul edilebilir olabilir. Aşağıdaki bölümler, sorgu gecikmesini azaltmaya yönelik ipuçlarına genel bir bakış sağlar. Aynı sorguyu aynı veri kümesinde birden çok kez çalıştırın, her seferinde aynı RU ücretine sahip olur. Ancak sorgu gecikmesi sorgu yürütmeleri arasında değişebilir.

### <a name="improve-proximity"></a>Yakınlığı geliştirin

Azure Cosmos DB hesabından farklı bir bölgeden çalıştırılan sorgular, aynı bölge içinde çalıştırıldıklarından daha yüksek gecikme sürelerine sahip olur. Örneğin, masaüstü bilgisayarınızda kod çalıştırıyorsanız, sorgunun Azure Cosmos DB ile aynı Azure bölgesinde bulunan sanal bir makineden gelmesinden on veya yüzlerce milisaniye (veya daha fazla) gecikme stoğunun olmasını beklemelisiniz. Verilerinizi uygulamanıza yaklaştırabileceğinizden emin olmak için [Azure Cosmos DB'de verileri](distribute-data-globally.md) genel olarak dağıtmak kolaydır.

### <a name="increase-provisioned-throughput"></a>Sağlanan iş artış

Azure Cosmos DB'de, sağlanan iş ortası İstek Birimleri (RUs) cinsinden ölçülür. 5 RUs'luk iş gücü tüketen bir sorgunuz olduğunu düşünün. Örneğin, 1.000 RUs sağlarsanız, bu sorguycýsý saniyede 200 kez çalıştırabilirsiniz. Yeterli iş ortası olmadığında sorguyu çalıştırmayı denediyseniz, Azure Cosmos DB bir HTTP 429 hatası döndürdü. Geçerli Core (SQL) API SDK'lardan herhangi biri, kısa bir süre bekledikten sonra bu sorguyu otomatik olarak yeniden dener. Daraltılmış istekler daha uzun sürer, bu nedenle verilen iş taminin artırılması sorgu gecikmeliğini artırabilir. Azure portalının **Ölçümler** bıçak üzerinde [toplam daraltılmış istek sayısını](use-metrics.md#understand-how-many-requests-are-succeeding-or-causing-errors) gözlemleyebilirsiniz.

### <a name="increase-maxconcurrency"></a>MaxConcurrency'i artırın

Paralel sorgular, birden çok bölümü paralel olarak sorgulayarak çalışır. Ancak, tek tek bölümlenmiş koleksiyondaki veriler sorguyla ilgili olarak seri olarak getirilir. Bu nedenle, MaxConcurrency'i bölüm sayısına ayarlarsanız, diğer tüm sistem koşullarının aynı kalması koşuluyla en çok performans gösteren sorguyu elde etme şansınız en yüksek tir. Bölüm sayısını bilmiyorsanız, MaxConcurrency'i (veya eski SDK sürümlerinde MaxDegreesOfParallelism) yüksek bir sayıya ayarlayabilirsiniz. Sistem en az (bölüm sayısı, kullanıcı sağlanan giriş) paralellik maksimum derecesi olarak seçecektir.

### <a name="increase-maxbuffereditemcount"></a>MaxBufferedItemCount'ı artırın

Geçerli sonuç grubu istemci tarafından işlenirken sorgular sonuçları önceden almak üzere tasarlanmıştır. Ön alma, bir sorgunun genel gecikmesini iyileştirmeye yardımcı olur. MaxBufferedItemCount'in ayarlanması, önceden getirilen sonuç sayısını sınırlar. Bu değeri, döndürülen sonuç sayısına (veya daha yüksek bir sayıya) ayarlarsanız, sorgu ön almadan en fazla faydayı elde edebilir. Bu değeri -1 olarak ayarlarsanız, sistem arabellek için öğe sayısını otomatik olarak belirler.

## <a name="next-steps"></a>Sonraki adımlar
Sorgu başına RUs'u nasıl ölçtükçe, sorgularınızı ayarlamak için yürütme istatistiklerini alma ve daha fazlası hakkında bilgi için aşağıdaki makalelere bakın:

* [.NET SDK kullanarak SQL sorgu yürütme ölçümlerini alın](profile-sql-api-query.md)
* [Azure Cosmos DB ile sorgu performansını ayarlama](sql-api-sql-query-metrics.md)
* [.NET SDK’sı için performans ipuçları](performance-tips.md)
