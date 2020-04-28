---
title: Azure Cosmos DB için SQL JOIN sorguları
description: Verileri sorgulamak için Azure Cosmos DB birden çok tabloya nasıl KATıLACAĞıNıZı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: mjbrown
ms.openlocfilehash: 38e80f1597a08b8db7cbfa852d1bcf38ac768b1f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74871151"
---
# <a name="joins-in-azure-cosmos-db"></a>Azure Cosmos DB birleşimler

İlişkisel bir veritabanında, tablolar arasında birleşimler, Normalleştirilmemiş şemalar tasarlamanın mantıksal bir ilişkmalarıdır. Buna karşılık, SQL API 'SI, *kendinden JOIN*'in mantıksal eşdeğeri olan, şema içermeyen öğelerin Normalleştirilmemiş veri modelini kullanır.

İç birleşimler, birleşime katılan kümelerden oluşan tüm çapraz ürünlere yol açabilir. N-Way JOIN 'in sonucu, kayıt kümesindeki her bir değerin birleşime katılan diğer ad kümesiyle ilişkilendirildiği ve diğer yan tümcelerde bu diğer ada başvuruda bulunarak erişilebilen N-element tanımlama gruplarının bir kümesidir.

## <a name="syntax"></a>Sözdizimi

Dil söz dizimini `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`destekler. Bu sorgu, değerleri olan `N` bir başlık kümesi döndürür. Her tanımlama grubu, tüm kapsayıcı diğer adlarını ilgili kümeleri üzerinde yineleerek oluşturulan değerlere sahiptir. 

Aşağıdaki FROM yan tümcesine göz atalım:`<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`  
  
 Her bir kaynağın tanımlamasına `input_alias1, input_alias2, …, input_aliasN`izin verin. Bu FROM yan tümcesi bir N-tanımlama grubu kümesi döndürür (N değeri olan tanımlama grubu). Her tanımlama grubu, tüm kapsayıcı diğer adlarını ilgili kümeleri üzerinde yineleerek oluşturulan değerlere sahiptir.  
  
**Örnek 1** -2 kaynak  
  
- Kapsayıcı `<from_source1>` kapsamlı olmasına izin verir ve {A, B, C} kümesini temsil eder.  
  
- Belge `<from_source2>` kapsamlı başvuru input_alias1 ve kümeleri temsil edelim:  
  
    için {1, 2}`input_alias1 = A,`  
  
    {3}bekleniyor`input_alias1 = B,`  
  
    için {4, 5}`input_alias1 = C,`  
  
- FROM yan tümcesi `<from_source1> JOIN <from_source2>` aşağıdaki başlıkların oluşmasına neden olur:  
  
    (`input_alias1, input_alias2`):  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
**Örnek 2** -3 kaynak  
  
- Kapsayıcı `<from_source1>` kapsamlı olmasına izin verir ve {A, B, C} kümesini temsil eder.  
  
- Belge `<from_source2>` kapsamlı başvuru `input_alias1` ve kümeleri temsil etmenize izin ver:  
  
    için {1, 2}`input_alias1 = A,`  
  
    {3}bekleniyor`input_alias1 = B,`  
  
    için {4, 5}`input_alias1 = C,`  
  
- Belge `<from_source3>` kapsamlı başvuru `input_alias2` ve kümeleri temsil etmenize izin ver:  
  
    için {100, 200}`input_alias2 = 1,`  
  
    {300}bekleniyor`input_alias2 = 3,`  
  
- FROM yan tümcesi `<from_source1> JOIN <from_source2> JOIN <from_source3>` aşağıdaki başlıkların oluşmasına neden olur:  
  
    (input_alias1, input_alias2, input_alias3):  
  
    (A, 1, 100), (A, 1, 200), (B, 3, 300)  
  
  > [!NOTE]
  > Diğer değerleri `input_alias1` `input_alias2`için, bir değer döndürmeyen, için `<from_source3>` tanımlama gruplarının bulunmaması.  
  
**Örnek 3** -3 kaynak  
  
- <from_source1> kapsayıcı kapsamlı olmasına izin verir ve {A, B, C} kümesini temsil eder.  
  
- Kapsayıcı `<from_source1>` kapsamlı olmasına izin verir ve {A, B, C} kümesini temsil eder.  
  
- <from_source2> belge kapsamlı başvuru input_alias1 ve kümeleri temsil etmesini sağlar:  
  
    için {1, 2}`input_alias1 = A,`  
  
    {3}bekleniyor`input_alias1 = B,`  
  
    için {4, 5}`input_alias1 = C,`  
  
- Kapsamlandırılmalıdır `<from_source3>` `input_alias1` ve kümeleri temsil edelim:  
  
    için {100, 200}`input_alias2 = A,`  
  
    {300}bekleniyor`input_alias2 = C,`  
  
- FROM yan tümcesi `<from_source1> JOIN <from_source2> JOIN <from_source3>` aşağıdaki başlıkların oluşmasına neden olur:  
  
    (`input_alias1, input_alias2, input_alias3`):  
  
    (A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200), (C, 4, 300), (C, 5, 300)  
  
  > [!NOTE]
  > Her ikisi de aynı `<from_source2>` `<from_source3>` `<from_source1>`kapsam içerdiğinden, bu, ile arasında çapraz ürün ile sonuçlandı.  Bu, 1 değerine sahip olan 4 (2x2) tanımlama grubu ile sonuçlanır. değer B (1x0) ve 2 (2x1) tanımlama gruplarına sahip, C değeri olan 0 tanımlama grubu.  
  
## <a name="examples"></a>Örnekler

Aşağıdaki örneklerde JOIN yan tümcesinin nasıl çalıştığı gösterilmektedir. Bu örnekleri çalıştırmadan önce örnek [Aile verilerini](sql-query-getting-started.md#upload-sample-data)karşıya yükleyin. Aşağıdaki örnekte, kaynak ve boş bir kümeden her öğenin çapraz çarpımı boş olduğundan sonuç boştur:

```sql
    SELECT f.id
    FROM Families f
    JOIN f.NonExistent
```

Sonuç:

```json
    [{
    }]
```

Aşağıdaki örnekte, JOIN iki JSON nesnesi, öğe kökü `id` ve `children` alt kök arasında bir çapraz üründür. Dizi olan olgu `children` , `children` dizi olan tek bir kök ile ilişkili olduğundan, birleşimde geçerli değildir. Sonuç yalnızca iki sonuç içerir, çünkü dizi ile her öğenin çapraz çarpımı yalnızca tek bir öğe oluşturur.

```sql
    SELECT f.id
    FROM Families f
    JOIN f.children
```

Sonuçlar:

```json
    [
      {
        "id": "AndersenFamily"
      },
      {
        "id": "WakefieldFamily"
      }
    ]
```

Aşağıdaki örnekte daha geleneksel bir JOIN gösterilmektedir:

```sql
    SELECT f.id
    FROM Families f
    JOIN c IN f.children
```

Sonuçlar:

```json
    [
      {
        "id": "AndersenFamily"
      },
      {
        "id": "WakefieldFamily"
      },
      {
        "id": "WakefieldFamily"
      }
    ]
```

JOIN yan tümcesinin KIMDEN kaynağı bir yineleyici. Bu nedenle, önceki örnekteki akış:  

1. Dizideki her bir alt `c` öğeyi genişletin.
2. Birinci adımdan oluşan her bir alt öğesi `f` `c` olan öğenin köküne sahip bir çapraz ürün uygulayın.
3. Son olarak, kök nesne `f` `id` özelliğini tek başına proje.

İlk öğe `AndersenFamily`yalnızca bir `children` öğesi içerir, bu nedenle sonuç kümesi yalnızca tek bir nesne içerir. İkinci öğe, `WakefieldFamily`, iki `children`içerir, bu nedenle çapraz ürün her `children` öğe için bir tane olmak üzere iki nesne üretir. Bu öğelerin her ikisinde de, bir çapraz üründe bekledikleri gibi, kök alanları aynıdır.

JOIN yan tümcesinin gerçek yardımcı programı, başka bir şekilde proje için zor olan bir şekildeki çapraz üründen tanımlama grupları oluşturmak için kullanılır. Aşağıdaki örnek, kullanıcının tanımlama grupları tarafından karşılanan bir koşul seçmesini sağlayan bir tanımlama grubu birleşimine filtre uygular.

```sql
    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName
    FROM Families f
    JOIN c IN f.children
    JOIN p IN c.pets
```

Sonuçlar:

```json
    [
      {
        "familyName": "AndersenFamily",
        "childFirstName": "Henriette Thaulow",
        "petName": "Fluffy"
      },
      {
        "familyName": "WakefieldFamily",
        "childGivenName": "Jesse",
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily",
       "childGivenName": "Jesse",
       "petName": "Shadow"
      }
    ]
```

Önceki örneğin aşağıdaki uzantısı bir Double JOIN gerçekleştirir. Geçici ürünü aşağıdaki sözde kod olarak görüntüleyebilirsiniz:

```
    for-each(Family f in Families)
    {
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName,
                  c.givenName AS childGivenName,
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }
```

`AndersenFamily`tek bir evcil hayvan içeren bir alt öğesi vardır; bu nedenle, çapraz ürün bu aileden bir satır (1\*1\*1) verir. `WakefieldFamily`iki alt öğeye sahiptir ve bunlardan yalnızca biri pets 'e sahiptir, ancak bu alt öğe iki Evcil hayvan 'a sahiptir. Bu aile için çapraz ürün 1\*1\*2 = 2 satır verir.

Sonraki örnekte, Evcil hayvan adının olmadığı `pet` `Shadow`tüm başlıkları dışlayan ek bir filtre vardır. Dizilerden tanımlama grupları oluşturabilir, kayıt düzeni öğelerinin herhangi birine filtre uygulayabilir ve öğelerin herhangi bir birleşimini proje aracılığıyla yapabilirsiniz.

```sql
    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName
    FROM Families f
    JOIN c IN f.children
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"
```

Sonuçlar:

```json
    [
      {
       "familyName": "WakefieldFamily",
       "childGivenName": "Jesse",
       "petName": "Shadow"
      }
    ]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Başlarken](sql-query-getting-started.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet)
- [Alt](sql-query-subquery.md)