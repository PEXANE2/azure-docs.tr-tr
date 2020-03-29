---
title: Azure Cosmos DB için SQL JOIN sorguları
description: Verileri sorgulamak için Azure Cosmos DB'deki birden çok tabloya nasıl katılacağınızı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: mjbrown
ms.openlocfilehash: 38e80f1597a08b8db7cbfa852d1bcf38ac768b1f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74871151"
---
# <a name="joins-in-azure-cosmos-db"></a>Azure Cosmos DB'ye katılıyor

İlişkisel bir veritabanında, tablolar arasında birleşen ler, normalleştirilmiş şemaların tasarlanmasının mantıksal sonucudur. Buna karşılık, SQL API şema içermeyen öğelerin normalize edilmiş veri modelini kullanır, bu da *kendi kendine birleştirmenin*mantıksal eşdeğeridir.

İç birleştirmeler, birleştirmeye katılan kümelerin tam bir çapraz çarpımıyla sonuçlanır. N-way birleştirme sonucu, tuple'daki her değerin birleştirmeye katılan diğer ad kümesiyle ilişkili olduğu ve diğer tümcelerde bu diğer adatıfta bulunularak erişilebildiği N-öğesi tuples kümesidir.

## <a name="syntax"></a>Sözdizimi

Dil sözdizimini `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`destekler. Bu sorgu değerleri ile tuples kümesi döndürür. `N` Her tuple, tüm kapsayıcı diğer adlarını kendi kümeleri üzerinde yineleyerek üretilen değerlere sahiptir. 

Aşağıdaki FROM yan tümcesine bakalım:`<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`  
  
 Her kaynağın `input_alias1, input_alias2, …, input_aliasN`. Bu FROM yan tümcesi N-tuples kümesini döndürür (N değerleri ile tuple). Her tuple, tüm kapsayıcı diğer adlarını kendi kümeleri üzerinde yineleyerek üretilen değerlere sahiptir.  
  
**Örnek 1** - 2 kaynak  
  
- Kapsayıcı `<from_source1>` kapsamına alın ve {A, B, C} kümesini temsil edelim.  
  
- input_alias1 `<from_source2>` başvuru ve temsil kümeleri belge kapsamı:  
  
    {1, 2} için`input_alias1 = A,`  
  
    {3}Için`input_alias1 = B,`  
  
    {4, 5} için`input_alias1 = C,`  
  
- FROM yan `<from_source1> JOIN <from_source2>` tümcesi aşağıdaki tuples neden olacaktır:  
  
    (`input_alias1, input_alias2`):  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
**Örnek 2** - 3 kaynak  
  
- Kapsayıcı `<from_source1>` kapsamına alın ve {A, B, C} kümesini temsil edelim.  
  
- Belge `<from_source2>` kapsamı referansı `input_alias1` ve temsil kümeleri olsun:  
  
    {1, 2} için`input_alias1 = A,`  
  
    {3}Için`input_alias1 = B,`  
  
    {4, 5} için`input_alias1 = C,`  
  
- Belge `<from_source3>` kapsamı referansı `input_alias2` ve temsil kümeleri olsun:  
  
    {100, 200} için`input_alias2 = 1,`  
  
    {300}Için`input_alias2 = 3,`  
  
- FROM yan `<from_source1> JOIN <from_source2> JOIN <from_source3>` tümcesi aşağıdaki tuples neden olacaktır:  
  
    (input_alias1, input_alias2, input_alias3):  
  
    (A, 1, 100), (A, 1, 200), (B, 3, 300)  
  
  > [!NOTE]
  > Diğer değerler için tuples `input_alias1` `input_alias2`eksikliği , `<from_source3>` , hangi herhangi bir değer döndürmedi.  
  
**Örnek 3** - 3 kaynakları  
  
- <from_source1> kapsayıcı kapsamına alın ve {A, B, C} kümesini temsil etsin.  
  
- Kapsayıcı `<from_source1>` kapsamına alın ve {A, B, C} kümesini temsil edelim.  
  
- from_source2> belge kapsamına> <input_alias1 ve kümeleri temsil edelim:  
  
    {1, 2} için`input_alias1 = A,`  
  
    {3}Için`input_alias1 = B,`  
  
    {4, 5} için`input_alias1 = C,`  
  
- Kümeleri `<from_source3>` kapsamaa `input_alias1` ve temsil edelim:  
  
    {100, 200} için`input_alias2 = A,`  
  
    {300}Için`input_alias2 = C,`  
  
- FROM yan `<from_source1> JOIN <from_source2> JOIN <from_source3>` tümcesi aşağıdaki tuples neden olacaktır:  
  
    (`input_alias1, input_alias2, input_alias3`):  
  
    (A, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200), (C, 4, 300) , (C, 5, 300)  
  
  > [!NOTE]
  > Bu, her ikisi `<from_source2>` `<from_source3>` de aynı kapsamda olduğundan `<from_source1>`ve her ikisi de kapsama çünkü arasında çapraz ürün sonuçlandı.  Bu, 4 (2x2) tupüllerin A değerine sahip olması, 0 tupüllerin B (1x0) ve 2 (2x1) tupüllerin C değerine sahip olmasıyla sonuçlandı.  
  
## <a name="examples"></a>Örnekler

Aşağıdaki örnekler, JOIN yan tümcesinin nasıl çalıştığını göstermektedir. Bu örnekleri çalıştırmadan önce örnek [aile verilerini](sql-query-getting-started.md#upload-sample-data)yükleyin. Aşağıdaki örnekte, her maddenin kaynaktan çapraz çarpımı ve boş bir küme boş olduğundan, sonuç boştur:

```sql
    SELECT f.id
    FROM Families f
    JOIN f.NonExistent
```

Sonuç şudur:

```json
    [{
    }]
```

Aşağıdaki örnekte, birleştirme iki JSON nesnesi, öğe kökü `id` ve `children` alt kök arasında çapraz bir üründür. Dizi olması `children` birleştirmede etkili değildir, çünkü `children` dizi olan tek bir kökle ilgilenir. Diziye sahip her öğenin çapraz ürünü tam olarak yalnızca bir öğe verimleri olduğundan, sonuç yalnızca iki sonuç içerir.

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

Aşağıdaki örnek, daha geleneksel bir birleştirme gösterir:

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

JOIN yan tümcesinin FROM kaynağı bir yineleyicidir. Yani, önceki örnekte akış:  

1. Dizideki her `c` alt öğeyi genişletin.
2. İlk adımDüzleştirilmiş her alt `f` öğe `c` ile öğenin kökü ile bir çapraz ürün uygulayın.
3. Son olarak, kök `f` `id` nesne özelliğini tek başına yansıtın.

İlk öğe, `AndersenFamily`yalnızca bir `children` öğe içerir, bu nedenle sonuç kümesi yalnızca tek bir nesne içerir. İkinci öğe, `WakefieldFamily`iki `children`içerir, böylece çapraz ürün iki nesne, `children` her öğe için bir üretir. Her iki öğedeki kök alanları, çapraz üründe beklediğiniz gibi aynıdır.

JOIN yan tümcesinin gerçek yararı, çapraz üründen yansıtılaması zor bir şekilde tuples oluşturmaktır. Aşağıdaki örnek, kullanıcının genel olarak tuples tarafından tatmin edilen bir koşul seçmesine olanak tanıyan bir tuple kombinasyonuna yönelik filtreler.

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

Önceki örneğin aşağıdaki uzantısı bir çift birleştirme gerçekleştirir. Çapraz ürünü aşağıdaki sözde kod olarak görüntüleyebilirsiniz:

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

`AndersenFamily`bir evcil hayvanı olan bir çocuk vardır, bu nedenle\*\*çapraz ürün bu aileden bir satır (1 1 1 1) verir. `WakefieldFamily`sadece birinin evcil hayvanı olan iki çocuğu var, ama o çocuğun iki evcil hayvanı var. Bu aile için çapraz ürün\*\*1 1 2 = 2 satır verir.

Sonraki örnekte, evcil hayvan adının `pet`olmadığı `Shadow`tüm tuples hariç , üzerinde ek bir filtre vardır. Dizilerden tuples oluşturabilir, tuple'ın herhangi bir öğesine filtre uygulayabilir ve öğelerin herhangi bir birleşimini yansıtabilirsiniz.

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
- [Alt sorgular](sql-query-subquery.md)