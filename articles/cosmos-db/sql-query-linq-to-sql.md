---
title: Azure Cosmos DB LINQ to SQL çevirisi
description: Desteklenen LINQ işleçlerini ve LINQ sorgularının Azure Cosmos DB içindeki SQL sorgularıyla nasıl eşleştirildiğini öğrenin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 7/29/2020
ms.author: tisande
ms.openlocfilehash: f2a7570b7ebed26a06e1bd075c2904bc29061c21
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87498863"
---
# <a name="linq-to-sql-translation"></a>LINQ to SQL çevirisi

Azure Cosmos DB sorgu sağlayıcısı, bir LINQ sorgusundan Cosmos DB SQL sorgusuna en iyi çaba eşlemesini gerçekleştirir. LINQ 'dan çevrilen SQL sorgusunu almak istiyorsanız `ToString()` oluşturulan nesnede yöntemini kullanın `IQueryable` . Aşağıdaki açıklamada, [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/introduction-to-linq-queries)ile ilgili temel bir benzerlik varsayılmaktadır.

Sorgu sağlayıcısı tür sistemi yalnızca JSON ilkel türlerini destekler: numeric, Boolean, String ve null.

Sorgu sağlayıcısı aşağıdaki skaler ifadeleri destekler:

- Sorgu değerlendirme sırasında temel veri türlerinin sabit değerleri de dahil olmak üzere sabit değerler.
  
- Bir nesnenin veya dizi öğesinin özelliğine başvuran Özellik/dizi dizini ifadeleri. Örnek:
  
  ```
    family.Id;
    family.children[0].familyName;
    family.children[0].grade;
    family.children[n].grade; //n is an int variable
  ```
  
- Sayısal ve Boole değerlerinde ortak aritmetik ifadeler de dahil olmak üzere aritmetik ifadeler. Tüm liste için [Azure Cosmos db SQL belirtimine](sql-query-system-functions.md)bakın.
  
  ```
    2 * family.children[0].grade;
    x + y;
  ```
  
- Bir dize değerini bir sabit dize değerine karşılaştırmayı içeren dize karşılaştırma ifadeleri.  
  
  ```
    mother.familyName == "Wakefield";
    child.givenName == s; //s is a string variable
  ```
  
- Bileşik değer türü veya anonim türdeki bir nesneyi ya da bu nesnelerden oluşan bir diziyi döndüren nesne/dizi oluşturma ifadeleri. Bu değerleri iç içe geçirebilirsiniz.
  
  ```
    new Parent { familyName = "Wakefield", givenName = "Robin" };
    new { first = 1, second = 2 }; //an anonymous type with two fields  
    new int[] { 3, child.grade, 5 };
  ```

## <a name="using-linq"></a>LINQ kullanma

İle LINQ sorgusu oluşturabilirsiniz `GetItemLinqQueryable` . Bu örnek, LINQ sorgu oluşturma ve zaman uyumsuz yürütmeyi bir ile gösterir `FeedIterator` :

```csharp
using (FeedIterator<Book> setIterator = container.GetItemLinqQueryable<Book>()
                      .Where(b => b.Title == "War and Peace")
                      .ToFeedIterator<Book>())
 {
     //Asynchronous query execution
     while (setIterator.HasMoreResults)
     {
         foreach(var item in await setIterator.ReadNextAsync()){
         {
             Console.WriteLine(item.cost);
         }
       }
     }
 }
```

## <a name="supported-linq-operators"></a><a id="SupportedLinqOperators"></a>Desteklenen LINQ işleçleri

SQL .NET SDK 'sına dahil edilen LINQ sağlayıcısı aşağıdaki işleçleri destekler:

- **Seçim**: nesne oluşturma dahil olmak üzere [Seçilecek](sql-query-select.md)projeksiyonler çeviri.
- **Burada**: filtreler [nereye](sql-query-where.md)çeviriye çevirir ve `&&` ,, `||` ve ile `!` SQL işleçleri arasındaki çeviriyi destekler
- **SelectMany**: dizilerin [JOIN](sql-query-join.md) yan tümcesine geri sarıya izin verir. Dizi öğelerinde filtrelemek için ifadeleri zincirlemek veya iç içe yerleştirmek için kullanın.
- **OrderBy** ve **OrderByDescending**: ASC veya DESC ile [order by](sql-query-order-by.md) 'a çevirin.
- [Toplama](sql-query-aggregates.md)için **Count**, **Sum**, **Min**, **Max**ve **Average** işleçleri ve zaman uyumsuz eşdeğerleri **countasync**, **SumAsync**, **minasync**, **maxasync**ve **averageasync**.
- **CompareTo**: Aralık karşılaştırmaları ' ne çevirir. Genellikle dizeler için kullanılır, çünkü .NET ' de karşılaştırılabilir değildir.
- **Atla** ve **Al**: sorgunun sonuçlarını sınırlandırma ve sayfalandırma IŞLEMLERI için [uzaklığa ve sınıra](sql-query-offset-limit.md) çevirir.
- **Matematik işlevleri**: .net,,,,,,,, `Abs` `Acos` ,, `Asin` `Atan` `Ceiling` `Cos` `Exp` `Floor` `Log` `Log10` `Pow` , `Round` , `Sign` , `Sin` , `Sqrt` , `Tan` ve ile `Truncate` eşdeğer [yerleşik matematik işlevlerine](sql-query-mathematical-functions.md)çeviriyi destekler.
- **Dize işlevleri**: .net,,,,,,,, `Concat` `Contains` ,, `Count` `EndsWith` `IndexOf` `Replace` `Reverse` `StartsWith` `SubString` `ToLower` `ToUpper` , `TrimEnd` ve ile `TrimStart` eşdeğer [yerleşik dize işlevlerine](sql-query-string-functions.md)çeviriyi destekler.
- **Dizi işlevleri**: .net `Concat` , `Contains` ve ile `Count` eşdeğer [yerleşik dizi işlevlerine](sql-query-array-functions.md)çeviriyi destekler.
- **Jeo-uzamsal uzantı işlevleri**: saplama yöntemlerinden, `Distance` , `IsValid` `IsValidDetailed` , ve `Within` eşdeğer [yerleşik Jeo uzamsal işlevlere](sql-query-geospatial-query.md)çeviriyi destekler.
- **Kullanıcı tanımlı Işlev uzantısı işlevi**: saplama yönteminden `UserDefinedFunctionProvider.Invoke` karşılık gelen [Kullanıcı tanımlı işleve](sql-query-udfs.md)çeviriyi destekler.
- **Çeşitli**: `Coalesce` ve koşullu [işleçlerin](sql-query-operators.md)çevirisini destekler. `Contains`İçeriğe bağlı olarak, ' nın içerdiği, ARRAY_CONTAINS veya Içinde dize ile çeviri yapılabilir.

## <a name="examples"></a>Örnekler

Aşağıdaki örneklerde, bazı standart LINQ sorgu işleçlerinin Azure Cosmos DB içindeki sorgulara nasıl çevrilebileceğini gösterir.

### <a name="select-operator"></a>İşleç seç

Sözdizimi `input.Select(x => f(x))` , burada `f` skaler bir ifadedir. `input`Bu durumda, bir `IQueryable` nesnesi olur.

**İşleç seç, örnek 1:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Select(family => family.parents[0].familyName);
  ```
  
- **SQL**
  
  ```sql
      SELECT VALUE f.parents[0].familyName
      FROM Families f
    ```
  
**İşleç seç, örnek 2:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Select(family => family.children[0].grade + c); // c is an int variable
  ```
  
- **SQL**
  
  ```sql
      SELECT VALUE f.children[0].grade + c
      FROM Families f
  ```
  
**İşleç seç, örnek 3:**

- **LINQ lambda ifadesi**
  
  ```csharp
    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });
  ```
  
- **SQL**
  
  ```sql
      SELECT VALUE {"name":f.children[0].familyName,
                    "grade": f.children[0].grade + 3 }
      FROM Families f
  ```

### <a name="selectmany-operator"></a>SelectMany işleci

Sözdizimi `input.SelectMany(x => f(x))` , burada `f` bir kapsayıcı türü döndüren skaler bir ifadedir.

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family => family.children);
  ```
  
- **SQL**

  ```sql
      SELECT VALUE child
      FROM child IN Families.children
  ```

### <a name="where-operator"></a>WHERE işleci

Sözdizimi `input.Where(x => f(x))` , `f` bir Boolean değer döndüren skaler bir ifadedir.

**WHERE işleci, örnek 1:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Where(family=> family.parents[0].familyName == "Wakefield");
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
  ```
  
**WHERE işleci, örnek 2:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Where(
          family => family.parents[0].familyName == "Wakefield" &&
          family.children[0].grade < 3);
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
      AND f.children[0].grade < 3
  ```

## <a name="composite-sql-queries"></a>Bileşik SQL sorguları

Daha güçlü sorgular oluşturmak için önceki işleçleri oluşturabilirsiniz. Cosmos DB iç içe kapsayıcıları desteklediğinden, kompozisyonu birleştirebilir veya iç içe yerleştirebilirsiniz.

### <a name="concatenation"></a>Birleştirme

Söz dizimi `input(.|.SelectMany())(.Select()|.Where())*` . Art arda eklenmiş bir sorgu `SelectMany` , daha sonra birden çok `Select` veya `Where` işleçle bir isteğe bağlı sorgu ile başlayabilir.

**Birleştirme, örnek 1:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Select(family => family.parents[0])
          .Where(parent => parent.familyName == "Wakefield");
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE f.parents[0].familyName = "Wakefield"
  ```

**Birleştirme, örnek 2:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Where(family => family.children[0].grade > 3)
          .Select(family => family.parents[0].familyName);
  ```

- **SQL**
  
  ```sql
      SELECT VALUE f.parents[0].familyName
      FROM Families f
      WHERE f.children[0].grade > 3
  ```

**Birleştirme, örnek 3:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Select(family => new { grade=family.children[0].grade}).
          Where(anon=> anon.grade < 3);
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      WHERE ({grade: f.children[0].grade}.grade > 3)
  ```

**Birleştirme, örnek 4:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family => family.parents)
          .Where(parent => parents.familyName == "Wakefield");
  ```
  
- **SQL**
  
  ```sql
      SELECT *
      FROM p IN Families.parents
      WHERE p.familyName = "Wakefield"
  ```

### <a name="nesting"></a>İç içe geçme

Sözdizimi,, `input.SelectMany(x=>x.Q())` `Q` `Select` `SelectMany` veya `Where` işleçtir.

İç içe sorgu, iç sorguyu dış kapsayıcının her öğesine uygular. Önemli bir özellik, iç sorgunun bir kendi kendine birleşim gibi dış kapsayıcıdaki öğelerin alanlarına başvurabileceği bir özelliktir.

**İç içe, örnek 1:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family=>
          family.parents.Select(p => p.familyName));
  ```

- **SQL**
  
  ```sql
      SELECT VALUE p.familyName
      FROM Families f
      JOIN p IN f.parents
  ```

**İç içe, örnek 2:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family =>
          family.children.Where(child => child.familyName == "Jeff"));
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      JOIN c IN f.children
      WHERE c.familyName = "Jeff"
  ```

**İç içe, örnek 3:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family => family.children.Where(
          child => child.familyName == family.parents[0].familyName));
  ```

- **SQL**
  
  ```sql
      SELECT *
      FROM Families f
      JOIN c IN f.children
      WHERE c.familyName = f.parents[0].familyName
  ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Belge verilerini modelleme](modeling-data.md)
