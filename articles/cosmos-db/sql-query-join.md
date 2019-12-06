---
title: Azure Cosmos DB için SQL JOIN sorguları
description: Verileri sorgulamak için Azure Cosmos DB birden çok tabloya nasıl KATıLACAĞıNıZı öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: mjbrown
ms.openlocfilehash: 38e80f1597a08b8db7cbfa852d1bcf38ac768b1f
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871151"
---
# <a name="joins-in-azure-cosmos-db"></a>Azure Cosmos DB birleşimler

İlişkisel bir veritabanında, tablolar arasında birleşimler, Normalleştirilmemiş şemalar tasarlamanın mantıksal bir ilişkmalarıdır. Buna karşılık, SQL API 'SI, *kendinden JOIN*'in mantıksal eşdeğeri olan, şema içermeyen öğelerin Normalleştirilmemiş veri modelini kullanır.

İç birleştirmeler birleştirme işleminde katılan kümeleri, eksiksiz bir çapraz ürün sonuçlanır. Çok yönlü birleştirme sonucunu, burada her bir tanımlama grubu değeri katılan birleştirme işleminde ayarlamak diğer adlı ile ilişkilendirilir ve bu diğer ad diğer yan tümceleri içinde başvurarak erişilebilir N-öğe dizilerini kümesidir.

## <a name="syntax"></a>Sözdizimi

Dil `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`sözdizimini destekler. Bu sorgu, `N` değerleri olan bir dizi tanımlama grubu döndürür. Her bir tanımlama grubunu tüm kapsayıcı diğer adları kendi ilgili ayarlar yineleme tarafından üretilen değerler içeriyor. 

Aşağıdaki FROM yan tümcesi göz atalım: `<from_source1> JOIN <from_source2> JOIN ... JOIN <from_sourceN>`  
  
 Her kaynak tanımlamak olanak `input_alias1, input_alias2, …, input_aliasN`. Bu FROM yan tümcesi, N-tanımlama grubu (N değeri ile tanımlama grubu) kümesini döndürür. Her bir tanımlama grubunu tüm kapsayıcı diğer adları kendi ilgili ayarlar yineleme tarafından üretilen değerler içeriyor.  
  
**Örnek 1** -2 kaynakları  
  
- İzin `<from_source1>` kapsayıcı kapsamına- ve set {A, B, C} temsil eder.  
  
- İzin `<from_source2>` input_alias1 başvuran belge kapsamlı ve kümeleri temsil eder:  
  
    {1, 2} için `input_alias1 = A,`  
  
    {3} için `input_alias1 = B,`  
  
    {4, 5} için `input_alias1 = C,`  
  
- FROM yan tümcesi `<from_source1> JOIN <from_source2>` de aşağıdaki tanımlama gruplarına neden olur:  
  
    (`input_alias1, input_alias2`):  
  
    `(A, 1), (A, 2), (B, 3), (C, 4), (C, 5)`  
  
**Örnek 2** -3 kaynakları  
  
- İzin `<from_source1>` kapsayıcı kapsamına- ve set {A, B, C} temsil eder.  
  
- İzin `<from_source2>` belge kapsamlı başvuran olması `input_alias1` ve kümeleri temsil eder:  
  
    {1, 2} için `input_alias1 = A,`  
  
    {3} için `input_alias1 = B,`  
  
    {4, 5} için `input_alias1 = C,`  
  
- İzin `<from_source3>` belge kapsamlı başvuran olması `input_alias2` ve kümeleri temsil eder:  
  
    {100, 200} için `input_alias2 = 1,`  
  
    {300} için `input_alias2 = 3,`  
  
- FROM yan tümcesi `<from_source1> JOIN <from_source2> JOIN <from_source3>` de aşağıdaki tanımlama gruplarına neden olur:  
  
    (input_alias1, input_alias2, input_alias3):  
  
    (BİR, 1, 100), (A, 1, 200), (B, 3, 300)  
  
  > [!NOTE]
  > Diğer değerler için demetleri eksikliği `input_alias1`, `input_alias2`, kendisi için `<from_source3>` herhangi bir değer döndürmedi.  
  
**Örnek 3** -3 kaynakları  
  
- < {A, B, C} kümesini temsil eder ve kapsayıcı kapsamlı from_source1 > sağlar.  
  
- İzin `<from_source1>` kapsayıcı kapsamına- ve set {A, B, C} temsil eder.  
  
- < Belge kapsamlı başvuru input_alias1 olması ve kümelerini göstermek from_source2 > sağlar:  
  
    {1, 2} için `input_alias1 = A,`  
  
    {3} için `input_alias1 = B,`  
  
    {4, 5} için `input_alias1 = C,`  
  
- İzin `<from_source3>` kapsamı `input_alias1` ve kümeleri temsil eder:  
  
    {100, 200} için `input_alias2 = A,`  
  
    {300} için `input_alias2 = C,`  
  
- FROM yan tümcesi `<from_source1> JOIN <from_source2> JOIN <from_source3>` de aşağıdaki tanımlama gruplarına neden olur:  
  
    (`input_alias1, input_alias2, input_alias3`):  
  
    (BİR, 1, 100), (A, 1, 200), (A, 2, 100), (A, 2, 200), (C, 4, 300), (C, 5, 300)  
  
  > [!NOTE]
  > Bu arasında çapraz ürün içinde sonuçlanan `<from_source2>` ve `<from_source3>` her ikisi de aynı belirlenir çünkü `<from_source1>`.  (2 x 2) 4'te bu durum diziler değerini 0 tanımlama grubu B (1 x 0) değerine sahip olan ve 2 (2 x 1) değeri c diziler  
  
## <a name="examples"></a>Örnekler

Aşağıdaki örnekler, JOIN yan tümcesi nasıl çalıştığını gösterir. Bu örnekleri çalıştırmadan önce örnek [Aile verilerini](sql-query-getting-started.md#upload-sample-data)karşıya yükleyin. Aşağıdaki örnekte, kaynak ve boş bir kümeden her öğenin çapraz çarpımı boş olduğundan sonuç boştur:

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

Aşağıdaki örnekte, JOIN iki JSON nesnesi, öğe kökü `id` ve `children` alt kökü arasındaki bir çapraz üründür. `children` bir dizi, `children` dizi olan tek bir kökle uğradığı için, birleşimde geçerli değildir. Sonuç yalnızca iki sonuç içerir, çünkü dizi ile her öğenin çapraz çarpımı yalnızca tek bir öğe oluşturur.

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

JOIN yan tümcesinin KIMDEN kaynağı bir yineleyici. Bu nedenle, önceki örnekteki akış:  

1. Dizideki her bir alt öğe `c` genişletin.
2. Öğe `f` köküne sahip bir çapraz ürün uygulama, her bir alt öğe ile düzleştirilmiş `c`.
3. Son olarak, kök nesne `f` `id` özelliğini tek başına proje.

İlk öğe `AndersenFamily`, yalnızca bir `children` öğesi içerir, bu nedenle sonuç kümesi yalnızca tek bir nesne içerir. İkinci öğe olan `WakefieldFamily`iki `children`içerir, bu nedenle, iki nesne de her `children` öğesi için bir tane olmak üzere iki nesne üretir. Bir çapraz ürün beklediğiniz gibi bu her iki öğe kök alanları aynıdır.

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

`AndersenFamily`, tek bir evcil hayvan içeren bir alt öğeye sahiptir, bu nedenle çapraz ürün bu aileden bir satır (1\*1\*1) verir. `WakefieldFamily` iki çocuğu vardır, ancak bu alt öğe iki Evcil hayvan 'a sahiptir. Bu aileye yönelik çapraz ürün 1\*1\*2 = 2 satır verir.

Sonraki örnekte, `pet`üzerinde ek bir filtre vardır ve bu, Evcil hayvan adının `Shadow`olmadığı tüm başlıkları dışlar. Dizilerden tanımlama grupları oluşturabilir, kayıt düzeni öğelerinin herhangi birine filtre uygulayabilir ve öğelerin herhangi bir birleşimini proje aracılığıyla yapabilirsiniz.

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