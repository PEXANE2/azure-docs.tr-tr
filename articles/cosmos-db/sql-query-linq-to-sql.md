---
title: Azure Cosmos DB LINQ to SQL çevirisi
description: Desteklenen LINQ işleçlerini ve LINQ sorgularının Azure Cosmos DB içindeki SQL sorgularıyla nasıl eşleştirildiğini öğrenin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: d8dd6392cf22852a10c1dc2600edcbc647f3c510
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871168"
---
# <a name="linq-to-sql-translation"></a>LINQ to SQL çeviri

Azure Cosmos DB sorgu sağlayıcısı, bir LINQ sorgusundan Cosmos DB SQL sorgusuna en iyi çaba eşlemesini gerçekleştirir. Aşağıdaki açıklamada, LINQ ile ilgili temel bir benzerlik varsayılmaktadır.

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

## <a id="SupportedLinqOperators"></a>Desteklenen LINQ işleçleri

SQL .NET SDK 'sına dahil edilen LINQ sağlayıcısı aşağıdaki işleçleri destekler:

- **Seçim**: nesne oluşturma da dahil olmak üzere SQL SELECT için projeksiyonları çevir.
- **Burada**: filtreler SQL 'e ÇEVIRIR ve sql işleçleri `&&`, `||`ve `!` arasında çeviri destekler
- **SelectMany**: SQL JOIN yan tümcesine dizi geriye doğru izleme sağlar. Dizi öğelerinde filtrelemek için ifadeleri zincirlemek veya iç içe yerleştirmek için kullanın.
- **OrderBy** ve **OrderByDescending**: ASC veya DESC ile order by 'a çevirin.
- **Sayısı**, **toplam**, **Min**, **Max**, ve **ortalama** işleçleri toplama ve zaman uyumsuz eşdeğerlerine **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, ve **AverageAsync**.
- **CompareTo**: aralık karşılaştırmalar çevirir. Genellikle dizeler için kullanılır, çünkü .NET ' de karşılaştırılabilir değildir.
- **Al**: sorgunun sonuçlarını KıSıTLAMAK için SQL 'e çevirir.
- **Matematik işlevleri**: .net `Abs`, `Acos`, `Asin`, `Atan`, `Ceiling`, `Cos`, `Exp`, `Floor`, `Log`, `Log10`, `Pow`, `Round`, `Sign`, `Sin`, `Sqrt`, `Tan`ve `Truncate` Ile eşdeğer SQL yerleşik işlevlerine kadar çeviriyi destekler.
- **Dize işlevleri**: .net `Concat`, `Contains`, `Count`, `EndsWith`,`IndexOf`, `Replace`, `Reverse`, `StartsWith`, `SubString`, `ToLower`, `ToUpper`, `TrimEnd`ve `TrimStart` 'nın eşdeğer SQL yerleşik işlevlerine çevirisini destekler.
- **Dizi işlevleri**: .net `Concat`, `Contains`ve `Count` aynı SQL yerleşik işlevlerine çeviriyi destekler.
- **Jeo-uzamsal uzantı işlevleri**: `Distance`, `IsValid`, `IsValidDetailed`ve `Within` eşdeğer SQL yerleşik işlevlerine çeviriyi destekler.
- **Kullanıcı tanımlı Işlev uzantısı işlevi**: `UserDefinedFunctionProvider.Invoke` saplama yönteminden çeviriyi, karşılık gelen Kullanıcı tanımlı işleve göre destekler.
- **Çeşitli**: `Coalesce` ve koşullu işleçlerin çevirisini destekler. , Bağlama göre `Contains`, ARRAY_CONTAINS veya SQL IÇEREN dizeye çevirebilir.

## <a name="examples"></a>Örnekler

Aşağıdaki örneklerde, bazı standart LINQ sorgu işleçlerinin Cosmos DB sorgulara nasıl çevrilebileceğini gösterir.

### <a name="select-operator"></a>İşleç seç

Söz dizimi `input.Select(x => f(x))`burada `f` skaler bir ifade.

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

Söz dizimi `input.SelectMany(x => f(x))`burada `f` bir kapsayıcı türü döndüren bir skaler ifade.

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family => family.children);
  ```
  
- **SQL**

  ```sql
      SELECT VALUE child
      FROM child IN Families.children
  ```

### <a name="where-operator"></a>Burada işleci

Söz dizimi `input.Where(x => f(x))`burada `f` bir Boole değeri döndüren bir skaler ifade.

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

Söz dizimi `input(.|.SelectMany())(.Select()|.Where())*`. Art arda eklenmiş bir sorgu, isteğe bağlı bir `SelectMany` sorgusuyla ve ardından birden çok `Select` veya `Where` işleçleriyle başlayabilir.

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

### <a name="nesting"></a>İç içe geçirme

Sözdizimi, `Q` bir `Select`, `SelectMany`veya `Where` işleci olduğu `input.SelectMany(x=>x.Q())`.

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
