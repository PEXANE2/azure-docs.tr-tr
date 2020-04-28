---
title: Azure Cosmos DB LINQ to SQL çevirisi
description: Desteklenen LINQ işleçlerini ve LINQ sorgularının Azure Cosmos DB içindeki SQL sorgularıyla nasıl eşleştirildiğini öğrenin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: d43f95b91df7d0c9c442339de51936200f4688e2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75441248"
---
# <a name="linq-to-sql-translation"></a>LINQ to SQL çevirisi

Azure Cosmos DB sorgu sağlayıcısı, bir LINQ sorgusundan Cosmos DB SQL sorgusuna en iyi çaba eşlemesini gerçekleştirir. Aşağıdaki açıklamada, LINQ ile ilgili temel bir benzerlik varsayılmaktadır.

Sorgu sağlayıcısı tür sistemi yalnızca JSON ilkel türlerini destekler: numeric, Boolean, String ve null.

Sorgu sağlayıcısı aşağıdaki skaler ifadeleri destekler:

- Sorgu değerlendirme sırasında temel veri türlerinin sabit değerleri de dahil olmak üzere sabit değerler.
  
- Bir nesnenin veya dizi öğesinin özelliğine başvuran Özellik/dizi dizini ifadeleri. Örneğin:
  
  ```
    family.Id;
    family.children[0].familyName;
    family.children[0].grade;
    family.children[n].grade; //n is an int variable
  ```
  
- Sayısal ve Boole değerlerinde ortak aritmetik ifadeler de dahil olmak üzere aritmetik ifadeler. Tüm liste için [Azure Cosmos db SQL belirtimine](https://go.microsoft.com/fwlink/p/?LinkID=510612)bakın.
  
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

## <a name="supported-linq-operators"></a><a id="SupportedLinqOperators"></a>Desteklenen LINQ işleçleri

SQL .NET SDK 'sına dahil edilen LINQ sağlayıcısı aşağıdaki işleçleri destekler:

- **Seçim**: nesne oluşturma da dahil olmak üzere SQL SELECT için projeksiyonları çevir.
- **Burada**: filtreler SQL ' e çevirir ve SQL işleçleri arasındaki `&&` `||` `!` çeviriyi destekler
- **SelectMany**: dizilerin SQL JOIN yan tümcesine geri sarılamadığına izin verir. Dizi öğelerinde filtrelemek için ifadeleri zincirlemek veya iç içe yerleştirmek için kullanın.
- **OrderBy** ve **OrderByDescending**: ASC veya DESC ile order by 'a çevirin.
- Toplama için **Count**, **Sum**, **Min**, **Max**ve **Average** işleçleri ve zaman uyumsuz eşdeğerleri **Countasync**, **SumAsync**, **minasync**, **maxasync**ve **averageasync**.
- **CompareTo**: Aralık karşılaştırmaları ' ne çevirir. Genellikle dizeler için kullanılır, çünkü .NET ' de karşılaştırılabilir değildir.
- **Atla** ve **Al**: bir sorgudaki sonuçları SıNıRLAYAN ve sayfalandırma yapan SQL 'e çevirir ve sınırla.
- **Matematik işlevleri**: `Abs`.net `Acos`,, `Asin` `Atan` `Ceiling` `Cos` `Pow` `Round` `Sign` `Sin` `Sqrt` `Tan`,,,,, `Truncate` , `Log`, `Log10`,,,,,, ve ile eşdeğer SQL yerleşik işlevlerine çeviriyi destekler. `Exp` `Floor`
- **Dize işlevleri**: .net `Concat`, `Contains`, `Count` `EndsWith``IndexOf` `Replace` `SubString` `ToLower` `ToUpper` `TrimEnd`,,,,,,,,, ve ile `TrimStart` eşdeğer SQL yerleşik işlevlerine çeviriyi destekler. `Reverse` `StartsWith`
- **Dizi işlevleri**: .net `Concat`, `Contains`ve ile `Count` eşdeğer SQL yerleşik işlevlerine çeviriyi destekler.
- **Jeo-uzamsal uzantı işlevleri**: `Distance`saplama yöntemlerinden `IsValid`,, `IsValidDetailed`, ve `Within` eşdeğer SQL yerleşik işlevlerine çeviriyi destekler.
- **Kullanıcı tanımlı Işlev uzantısı işlevi**: saplama yönteminden `UserDefinedFunctionProvider.Invoke` karşılık gelen Kullanıcı tanımlı işleve çeviriyi destekler.
- **Çeşitli**: `Coalesce` ve koşullu işleçlerin çevirisini destekler. İçeriğine bağlı `Contains` olarak, IÇINDE dize içeren, ARRAY_CONTAINS veya SQL arasında çeviri yapabilir.

## <a name="examples"></a>Örnekler

Aşağıdaki örneklerde, bazı standart LINQ sorgu işleçlerinin Cosmos DB sorgulara nasıl çevrilebileceğini gösterir.

### <a name="select-operator"></a>İşleç seç

Sözdizimi `input.Select(x => f(x))`, burada `f` skaler bir ifadedir.

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

Sözdizimi `input.SelectMany(x => f(x))`, burada `f` bir kapsayıcı türü döndüren skaler bir ifadedir.

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

Sözdizimi `input.Where(x => f(x))`, `f` bir Boolean değer döndüren skaler bir ifadedir.

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

### <a name="concatenation"></a>Bitiştirme

Söz dizimi `input(.|.SelectMany())(.Select()|.Where())*`. Art arda eklenmiş bir sorgu, daha sonra `SelectMany` birden çok `Select` veya `Where` işleçle bir isteğe bağlı sorgu ile başlayabilir.

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

Sözdizimi `input.SelectMany(x=>x.Q())` `Q` `SelectMany`,, veya `Where` işleçtir. `Select`

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
