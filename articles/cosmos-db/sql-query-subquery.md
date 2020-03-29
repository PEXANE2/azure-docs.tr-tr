---
title: Azure Cosmos DB için SQL alt sorguları
description: Azure Cosmos DB'deki SQL alt sorguları ve bunların yaygın kullanım örnekleri ve farklı alt sorgu türleri hakkında bilgi edinin
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: 42d9e8b190747a3ffaf0e46ea1eddda33d09bb24
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74870573"
---
# <a name="sql-subquery-examples-for-azure-cosmos-db"></a>Azure Cosmos DB için SQL alt sorgu örnekleri

Alt sorgu, başka bir sorgunun içinde iç içe olan bir sorgudur. Alt sorgu, iç sorgu veya iç seçim olarak da adlandırılır. Alt sorgu içeren deyim genellikle dış sorgu olarak adlandırılır.

Bu makalede, Azure Cosmos DB'de SQL alt sorguları ve bunların yaygın kullanım örnekleri açıklanmaktadır. Bu dokümandaki tüm örnek [sorgular, Azure Cosmos DB Query Playground'da](https://www.documentdb.com/sql/demo)önceden yüklenmiş bir beslenme veri kümesine karşı çalıştırılabilir.

## <a name="types-of-subqueries"></a>Alt sorgu türleri

İki ana alt sorgu türü vardır:

* **Correlated**: Dış sorgudaki değerlere başvuran bir alt sorgu. Alt sorgu, dış sorgunun işlediği her satır için bir kez değerlendirilir.
* **İlişkili olmayan**: Dış sorgudan bağımsız bir alt sorgu. Dış sorguya dayanmadan kendi başına çalıştırılabilir.

> [!NOTE]
> Azure Cosmos DB yalnızca ilişkili alt sorguları destekler.

Alt sorgular, döndükleri satır ve sütun sayısına göre daha da sınıflandırılabilir. Üç türü vardır:
* **Tablo**: Birden çok satır ve birden çok sütun döndürür.
* **Çok değerli**: Birden çok satır ve tek bir sütun verir.
* **Skaler**: Tek bir satır ve tek bir sütun verir.

Azure Cosmos DB'deki SQL sorguları her zaman tek bir sütunu döndürer (basit bir değer veya karmaşık bir belge). Bu nedenle, Azure Cosmos DB'de yalnızca çok değerli ve skaler alt sorgular uygulanabilir. Çok değerli bir alt sorguyu yalnızca FROM yan tümcesinde ilişkisel ifade olarak kullanabilirsiniz. SELECT veya WHERE yan tümcesinde skaler ifade olarak veya FROM yan tümcesinde ilişkisel bir ifade olarak skaler alt sorgu kullanabilirsiniz.

## <a name="multi-value-subqueries"></a>Çok değerli alt sorgular

Çok değerli alt sorgular bir belge kümesini döndürer ve her zaman FROM yan tümcesi içinde kullanılır. Şular için kullanılırlar:

* JOIN ifadelerini en iyi duruma alma. 
* Pahalı ifadeleri bir kez değerlendirme ve birden çok kez başvurma.

## <a name="optimize-join-expressions"></a>JOIN ifadelerini optimize edin

Çok değerli alt sorgular, WHERE yan tümcesindeki tüm çapraz birleştirmelerden sonra değil, her seç-çok ifadesinden sonra yüklemleri iterek JOIN ifadelerini en iyi duruma getirebilir.

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

Bu sorgu için dizin, "bebek formülü" adı ile etiketi olan tüm belgelerle eşleşir. Değeri 0 ile 10 arasında olan bir besin maddesi ve 1'den büyük bir porsiyon maddesidir. Burada JOIN ifadesi, herhangi bir filtre uygulanmadan önce her eşleşen belge için etiket, besin ve porsiyon dizilerinin tüm öğelerinin çapraz ürünlerini gerçekleştirecektir. 

WHERE yan tümcesi daha sonra her <c, t, n, s> tuple filtre yüklemi uygular. Örneğin, eşleşen bir belgenin üç dizinin her birinde 10 öğesi varsa, bu belge 1 x 10 x 10 x 10 (yani 1.000) tuples'e genişletilir. Burada alt sorguları kullanmak, birleştirilmiş dizi öğelerini bir sonraki ifadeyle birleştirmeden önce filtrelemede yardımcı olabilir.

Bu sorgu bir öncekine eşdeğerdir, ancak alt sorguları kullanır:

```sql
SELECT Count(1) AS Count
FROM c
JOIN (SELECT VALUE t FROM t IN c.tags WHERE t.name = 'infant formula')
JOIN (SELECT VALUE n FROM n IN c.nutrients WHERE n.nutritionValue > 0 AND n.nutritionValue < 10)
JOIN (SELECT VALUE s FROM s IN c.servings WHERE s.amount > 1)
```

Etiket dizisinde yalnızca bir öğenin filtreyle eşleştiğini ve hem besin hem de porsiyon dizileri için beş öğe olduğunu varsayalım. JOIN ifadeleri daha sonra ilk sorguda 1.000 öğeyerine 1 x 1 x 5 x 5 = 25 öğeye genişletilir.

## <a name="evaluate-once-and-reference-many-times"></a>Bir kez değerlendirin ve birçok kez başvuru

Alt sorgular, kullanıcı tanımlı işlevler (UDF'ler), karmaşık dizeleri veya aritmetik ifadeler gibi pahalı ifadeler içeren sorguları en iyi duruma getirmede yardımcı olabilir. İfadeyi bir kez değerlendirmek için JOIN ifadesiyle birlikte bir alt sorgu kullanabilirsiniz, ancak birçok kez başvuruda bulunabilirsiniz.

Aşağıdaki sorgu UDF'yi `GetMaxNutritionValue` iki kez çalıştırın:

```sql
SELECT c.id, udf.GetMaxNutritionValue(c.nutrients) AS MaxNutritionValue
FROM c
WHERE udf.GetMaxNutritionValue(c.nutrients) > 100
```

UDF'yi yalnızca bir kez çalıştıran eşdeğer bir sorgu aşağıda veda eder:

```sql
SELECT TOP 1000 c.id, MaxNutritionValue
FROM c
JOIN (SELECT VALUE udf.GetMaxNutritionValue(c.nutrients)) MaxNutritionValue
WHERE MaxNutritionValue > 100
``` 

> [!NOTE] 
> JOIN ifadelerinin ürün ötesi davranışını unutmayın. UDF ifadesi tanımlanmamış olarak değerlendirilebilirse, JOIN ifadesinin doğrudan değer yerine alt sorgudan bir nesne döndürerek her zaman tek bir satır oluşturduğundan emin olmalısınız.
>

Aşağıda, değer yerine bir nesneyi döndüren benzer bir örnek verilmiştir:

```sql
SELECT TOP 1000 c.id, m.MaxNutritionValue
FROM c
JOIN (SELECT udf.GetMaxNutritionValue(c.nutrients) AS MaxNutritionValue) m
WHERE m.MaxNutritionValue > 100
```

Bu yaklaşım UDF'lerle sınırlı değildir. Herhangi bir potansiyel pahalı ifade için geçerlidir. Örneğin, matematiksel fonksiyon `avg`ile aynı yaklaşımı alabilir:

```sql
SELECT TOP 1000 c.id, AvgNutritionValue
FROM c
JOIN (SELECT VALUE avg(n.nutritionValue) FROM n IN c.nutrients) AvgNutritionValue
WHERE AvgNutritionValue > 80
```

## <a name="mimic-join-with-external-reference-data"></a>Dış başvuru verileriyle birleştirme

Ölçüm birimleri veya ülke kodları gibi nadiren değişen statik verilere genellikle başvurmanız gerekebilir. Bu tür verileri her belge için yinelememek daha iyidir. Bu yinelemeden kaçınmak depolamadan tasarruf etmenizi sağlar ve belge boyutunu daha küçük tutarak yazma performansını artırır. Bir alt sorguyu, bir başvuru verikoleksiyonuyla iç birleştirme semantikini taklit etmek için kullanabilirsiniz.

Örneğin, bu başvuru veri kümesini göz önünde bulundurun:

| **Birim** | **Adı**            | **Çarpanı** | **Taban birimi** |
| -------- | ------------------- | -------------- | ------------- |
| Ng       | Nanogram            | 1.00E-09 arası       | Gram          |
| Μg       | Mikrogram           | 1.00E-06 arası       | Gram          |
| Mg       | Miligram           | 1.00E-03 arası       | Gram          |
| g        | Gram                | 1.00E+00       | Gram          |
| Kg       | Kilogram            | 1.00E+03       | Gram          |
| Mg       | Megagram            | 1.00E+06       | Gram          |
| Gg       | Gigagram            | 1.00E+09       | Gram          |
| Nj       | Nanojoule           | 1.00E-09 arası       | Joule         |
| μJ       | Mikrojoule          | 1.00E-06 arası       | Joule         |
| mJ       | Millijoule          | 1.00E-03 arası       | Joule         |
| J        | Joule               | 1.00E+00       | Joule         |
| Koç       | Kilojoule           | 1.00E+03       | Joule         |
| MJ       | Megajoule           | 1.00E+06       | Joule         |
| Gj       | Gigajoule           | 1.00E+09       | Joule         |
| Cal      | Kalori             | 1.00E+00       | Kalori       |
| Kcal     | Kalori             | 1.00E+03       | Kalori       |
| ıu       | Uluslararası birimler |                |               |


Aşağıdaki sorgu, birimin adını çıktıya eklemek için bu verilerle birleştirmeyi taklit eder:

```sql
SELECT TOP 10 n.id, n.description, n.nutritionValue, n.units, r.name
FROM food
JOIN n IN food.nutrients
JOIN r IN (
    SELECT VALUE [
        {unit: 'ng', name: 'nanogram', multiplier: 0.000000001, baseUnit: 'gram'},
        {unit: 'µg', name: 'microgram', multiplier: 0.000001, baseUnit: 'gram'},
        {unit: 'mg', name: 'milligram', multiplier: 0.001, baseUnit: 'gram'},
        {unit: 'g', name: 'gram', multiplier: 1, baseUnit: 'gram'},
        {unit: 'kg', name: 'kilogram', multiplier: 1000, baseUnit: 'gram'},
        {unit: 'Mg', name: 'megagram', multiplier: 1000000, baseUnit: 'gram'},
        {unit: 'Gg', name: 'gigagram', multiplier: 1000000000, baseUnit: 'gram'},
        {unit: 'nJ', name: 'nanojoule', multiplier: 0.000000001, baseUnit: 'joule'},
        {unit: 'µJ', name: 'microjoule', multiplier: 0.000001, baseUnit: 'joule'},
        {unit: 'mJ', name: 'millijoule', multiplier: 0.001, baseUnit: 'joule'},
        {unit: 'J', name: 'joule', multiplier: 1, baseUnit: 'joule'},
        {unit: 'kJ', name: 'kilojoule', multiplier: 1000, baseUnit: 'joule'},
        {unit: 'MJ', name: 'megajoule', multiplier: 1000000, baseUnit: 'joule'},
        {unit: 'GJ', name: 'gigajoule', multiplier: 1000000000, baseUnit: 'joule'},
        {unit: 'cal', name: 'calorie', multiplier: 1, baseUnit: 'calorie'},
        {unit: 'kcal', name: 'Calorie', multiplier: 1000, baseUnit: 'calorie'},
        {unit: 'IU', name: 'International units'}
    ]
)
WHERE n.units = r.unit
```

## <a name="scalar-subqueries"></a>Skaler alt sorgular

Skaler alt sorgu ifadesi, tek bir değere değer biçen bir alt sorgudur. Skaler alt sorgu ifadesinin değeri, alt sorgunun projeksiyonunun (SELECT yan tümcesi) değeridir.  Skaler ifadenin geçerli olduğu birçok yerde skaler alt sorgu ifadesi kullanabilirsiniz. Örneğin, SELECT ve WHERE yan tümcelerinde herhangi bir ifadede skaler bir alt sorgu kullanabilirsiniz.

Skaler alt sorgu kullanmak her zaman en iyi duruma getirmenin yardımcı olmuyor. Örneğin, bir scalar alt sorgunun bir bağımsız değişken olarak bir sisteme veya kullanıcı tarafından tanımlanan işlevlere geçirilmesi kaynak birimi (RU) tüketiminde veya gecikmesinde hiçbir fayda sağlamaz.

Skaler alt sorgular daha fazla olarak sınıflandırılabilir:
* Basit ifade skaler alt sorguları
* Toplam skaler alt sorgular

## <a name="simple-expression-scalar-subqueries"></a>Basit ifade skaler alt sorguları

Basit ifadeskalar alt sorgu, toplu ifadeler içermeyen bir SELECT yan tümcesi olan ilişkili bir alt sorgudur. Derleyici bunları daha büyük bir basit ifadeye dönüştürdüğü için bu alt sorgular hiçbir optimizasyon avantajı sağlamaz. İç ve dış sorgular arasında ilişkili bir bağlam yoktur.

Aşağıda birkaç örnek verilmiştir:

**Örnek 1**

```sql
SELECT 1 AS a, 2 AS b
```

Bu sorgu, basit ifade scalar alt sorgu kullanarak, yeniden yazabilirsiniz:

```sql
SELECT (SELECT VALUE 1) AS a, (SELECT VALUE 2) AS b
```

Her iki sorgu da bu çıktıyı üretir:

```json
[
  { "a": 1, "b": 2 }
]
```

**Örnek 2**

```sql
SELECT TOP 5 Concat('id_', f.id) AS id
FROM food f
```

Bu sorgu, basit ifade scalar alt sorgu kullanarak, yeniden yazabilirsiniz:

```sql
SELECT TOP 5 (SELECT VALUE Concat('id_', f.id)) AS id
FROM food f
```

Sorgu çıktısı:

```json
[
  { "id": "id_03226" },
  { "id": "id_03227" },
  { "id": "id_03228" },
  { "id": "id_03229" },
  { "id": "id_03230" }
]
```

**Örnek 3**

```sql
SELECT TOP 5 f.id, Contains(f.description, 'fruit') = true ? f.description : undefined
FROM food f
```

Bu sorgu, basit ifade scalar alt sorgu kullanarak, yeniden yazabilirsiniz:

```sql
SELECT TOP 10 f.id, (SELECT f.description WHERE Contains(f.description, 'fruit')).description
FROM food f
```

Sorgu çıktısı:

```json
[
  { "id": "03230" },
  { "id": "03238", "description":"Babyfood, dessert, tropical fruit, junior" },
  { "id": "03229" },
  { "id": "03226", "description":"Babyfood, dessert, fruit pudding, orange, strained" },
  { "id": "03227" }
]
```

### <a name="aggregate-scalar-subqueries"></a>Toplam skaler alt sorgular

Toplu skaler alt sorgu, projeksiyonunda veya filtresinde tek bir değere göre değerlendiren bir toplu işlev olan bir alt sorgudur.

**Örnek 1:**

Projeksiyonunda tek bir toplam işlev ifadesi bulunan bir alt sorgu aşağıda veda eder:

```sql
SELECT TOP 5 
    f.id, 
    (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg'
) AS count_mg
FROM food f
```

Sorgu çıktısı:

```json
[
  { "id": "03230", "count_mg": 13 },
  { "id": "03238", "count_mg": 14 },
  { "id": "03229", "count_mg": 13 },
  { "id": "03226", "count_mg": 15 },
  { "id": "03227", "count_mg": 19 }
]
```

**Örnek 2**

Burada birden çok toplam işlev ifadeleri olan bir alt sorgu:

```sql
SELECT TOP 5 f.id, (
    SELECT Count(1) AS count, Sum(n.nutritionValue) AS sum 
    FROM n IN f.nutrients 
    WHERE n.units = 'mg'
) AS unit_mg
FROM food f
```

Sorgu çıktısı:

```json
[
  { "id": "03230","unit_mg": { "count": 13,"sum": 147.072 } },
  { "id": "03238","unit_mg": { "count": 14,"sum": 107.385 } },
  { "id": "03229","unit_mg": { "count": 13,"sum": 141.579 } },
  { "id": "03226","unit_mg": { "count": 15,"sum": 183.91399999999996 } },
  { "id": "03227","unit_mg": { "count": 19,"sum": 94.788999999999987 } }
]
```

**Örnek 3**

Hem projeksiyonda hem de filtrede toplu alt sorgu içeren bir sorgu aşağıda veda eder:

```sql
SELECT TOP 5 
    f.id, 
    (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg') AS count_mg
FROM food f
WHERE (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg') > 20
```

Sorgu çıktısı:

```json
[
  { "id": "03235", "count_mg": 27 },
  { "id": "03246", "count_mg": 21 },
  { "id": "03267", "count_mg": 21 },
  { "id": "03269", "count_mg": 21 },
  { "id": "03274", "count_mg": 21 }
]
```

Bu sorguyu yazmanın daha uygun bir yolu, alt sorguya katılmak ve hem SELECT hem de WHERE yan tümcelerinde alt sorgu takma adını başvurmaktır. Bu sorgu, alt sorguyu hem projeksiyon hem de filtrede değil, yalnızca birleştirme deyimi içinde yürütmeniz gerektiğinden daha verimlidir.

```sql
SELECT TOP 5 f.id, count_mg
FROM food f
JOIN (SELECT VALUE Count(1) FROM n IN f.nutrients WHERE n.units = 'mg') AS count_mg
WHERE count_mg > 20
```

## <a name="exists-expression"></a>EXISTS ifadesi

Azure Cosmos DB, EXISTS ifadelerini destekler. Bu, Azure Cosmos DB SQL API'de yerleşik bir toplu skaler alt sorgudur. EXISTS, alt sorgu ifadesini alan ve alt sorgu satırları döndürürse doğru döndüren bir Boolean ifadesidir. Aksi takdirde, yanlış döndürür.

Azure Cosmos DB SQL API Boolean ifadeleri ile diğer skaler ifadeler arasında ayrım yapmadığından, HEM SELECT hem de WHERE yan tümcelerinde EXISTS'i kullanabilirsiniz. Bu, Boolean ifadesinin (örneğin, EXISTS, BETWEEN ve IN) filtreyle sınırlı olduğu T-SQL'den farklı değildir.

EXISTS alt sorgusu tanımlanmamış tek bir değer döndürürse, EXISTS false olarak değerlendirilir. Örneğin, yanlış ı değerlendiren aşağıdaki sorguyu göz önünde bulundurun:
```sql
SELECT EXISTS (SELECT VALUE undefined)
```   


Önceki alt sorgudaki VALUE anahtar sözcüğü atlanırsa, sorgu doğru olarak değerlendirilir:
```sql
SELECT EXISTS (SELECT undefined) 
```

Alt sorgu, seçili listedeki değerler listesini bir nesneye içine alacaktır. Seçili listenin değeri yoksa, alt sorgu tek{}değer ' ''yi döndürür. Bu değer tanımlanır, bu nedenle EXISTS doğru değerlendirir.

### <a name="example-rewriting-array_contains-and-join-as-exists"></a>Örnek: ARRAY_CONTAINS yeniden yazma ve VAR OLARAK BİrLEŞTİr

ARRAY_CONTAINS yaygın kullanım örneği, bir dizideki bir öğenin varlığına göre belgeyi filtrelemektir. Bu durumda, etiket dizisinin "turuncu" adlı bir öğe içerip içermedikini kontrol ediyoruz.

```sql
SELECT TOP 5 f.id, f.tags
FROM food f
WHERE ARRAY_CONTAINS(f.tags, {name: 'orange'})
```

EXISTS'i kullanmak için aynı sorguyu yeniden yazabilirsiniz:

```sql
SELECT TOP 5 f.id, f.tags
FROM food f
WHERE EXISTS(SELECT VALUE t FROM t IN f.tags WHERE t.name = 'orange')
```

Ayrıca, ARRAY_CONTAINS yalnızca bir değerin diziiçindeki herhangi bir öğeye eşit olup olmadığını denetleyebilir. Dizi özellikleri nde daha karmaşık filtrelere ihtiyacınız varsa, JOIN'i kullanın.

Dizideki birimleri ve `nutritionValue` özellikleri temel alan filtreleyen aşağıdaki sorguyu göz önünde bulundurun: 

```sql
SELECT VALUE c.description
FROM c
JOIN n IN c.nutrients
WHERE n.units= "mg" AND n.nutritionValue > 0
```

Koleksiyondaki belgelerin her biri için, dizi öğeleriyle bir çapraz ürün gerçekleştirilir. Bu JOIN işlemi, dizi içindeki özelliklere filtre uygulamayı mümkün kılar. Ancak, bu sorgunun RU tüketimi önemli olacaktır. Örneğin, her dizide 1.000 belge 100 öğe varsa, bu öğe 1.000 x 100 'e (yani 100.000) tuples'a genişletilir.

EXISTS'i kullanmak bu pahalı çapraz ürünü önlemeye yardımcı olabilir:

```sql
SELECT VALUE c.description
FROM c
WHERE EXISTS(
    SELECT VALUE n
    FROM n IN c.nutrients
    WHERE n.units = "mg" AND n.nutritionValue > 0
)
```

Bu durumda, EXISTS alt sorgusu içindeki dizi öğelerine filtre uygulayabilirsiniz. Bir dizi öğesi filtreyle eşleşirse, bunu yansıtMış olursunuz ve EXISTS doğru değerlendirir.

Ayrıca diğer ad VAR ve projeksiyon başvuru:

```sql
SELECT TOP 1 c.description, EXISTS(
    SELECT VALUE n
    FROM n IN c.nutrients
    WHERE n.units = "mg" AND n.nutritionValue > 0) as a
FROM c
```

Sorgu çıktısı:

```json
[
    {
        "description": "Babyfood, dessert, fruit pudding, orange, strained",
        "a": true
    }
]
```

## <a name="array-expression"></a>ARRAY ifadesi

Bir sorgunun sonuçlarını dizi olarak yansıtmak için ARRAY ifadesini kullanabilirsiniz. Bu ifadeyi yalnızca sorgunun SELECT yan tümcesi içinde kullanabilirsiniz.

```sql
SELECT TOP 1   f.id, ARRAY(SELECT VALUE t.name FROM t in f.tags) AS tagNames
FROM  food f
```

Sorgu çıktısı:

```json
[
    {
        "id": "03238",
        "tagNames": [
            "babyfood",
            "dessert",
            "tropical fruit",
            "junior"
        ]
    }
]
```

Diğer alt sorgularda olduğu gibi, ARRAY ifadesine sahip filtreler mümkündür.

```sql
SELECT TOP 1 c.id, ARRAY(SELECT VALUE t FROM t in c.tags WHERE t.name != 'infant formula') AS tagNames
FROM c
```

Sorgu çıktısı:

```json
[
    {
        "id": "03226",
        "tagNames": [
            {
                "name": "babyfood"
            },
            {
                "name": "dessert"
            },
            {
                "name": "fruit pudding"
            },
            {
                "name": "orange"
            },
            {
                "name": "strained"
            }
        ]
    }
]
```

Dizi ifadeleri, alt sorgularda FROM yan tümcesinden sonra da gelebilir.

```sql
SELECT TOP 1 c.id, ARRAY(SELECT VALUE t.name FROM t in c.tags) as tagNames
FROM c
JOIN n IN (SELECT VALUE ARRAY(SELECT t FROM t in c.tags WHERE t.name != 'infant formula'))
```

Sorgu çıktısı:

```json
[
    {
        "id": "03238",
        "tagNames": [
            "babyfood",
            "dessert",
            "tropical fruit",
            "junior"
        ]
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Belge verilerini modelleme](modeling-data.md)
