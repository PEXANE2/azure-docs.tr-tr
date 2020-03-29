---
title: Azure Cosmos DB'de LINQ'dan SQL'e çeviri
description: DESTEKLENEN LINQ operatörlerini ve LINQ sorgularının Azure Cosmos DB'deki SQL sorgularına nasıl eşlendirilmelerini öğrenin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: d43f95b91df7d0c9c442339de51936200f4688e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75441248"
---
# <a name="linq-to-sql-translation"></a>LINQ - SQL çevirisi

Azure Cosmos DB sorgu sağlayıcısı, BIR LINQ sorgusundan Cosmos DB SQL sorgusuna en iyi çaba eşlemisini gerçekleştirir. Aşağıdaki açıklama LINQ ile temel bir aşinalık varsayar.

Sorgu sağlayıcı türü sistemi yalnızca JSON ilkel türlerini destekler: sayısal, Boolean, dize ve null.

Sorgu sağlayıcısı aşağıdaki skaler ifadeleri destekler:

- Sorgu değerlendirme zamanındaki ilkel veri türlerinin sabit değerleri de dahil olmak üzere sabit değerler.
  
- Bir nesnenin veya bir dizi öğesinin özelliğine başvuran özellik/dizi diziifadeleri ifadeleri. Örnek:
  
  ```
    family.Id;
    family.children[0].familyName;
    family.children[0].grade;
    family.children[n].grade; //n is an int variable
  ```
  
- Sayısal ve Boolean değerleri üzerinde ortak aritmetik ifadeler de dahil olmak üzere aritmetik ifadeler. Tam liste için [Azure Cosmos DB SQL belirtimine](https://go.microsoft.com/fwlink/p/?LinkID=510612)bakın.
  
  ```
    2 * family.children[0].grade;
    x + y;
  ```
  
- Dize değerini bazı sabit dize değeriyle karşılaştırmayı içeren dize karşılaştırma ifadeleri.  
  
  ```
    mother.familyName == "Wakefield";
    child.givenName == s; //s is a string variable
  ```
  
- Bileşik değer türüveya anonim türdeki bir nesneyi veya bu tür nesnelerin bir dizisini döndüren nesne/dizi oluşturma ifadeleri. Bu değerleri iç içe.
  
  ```
    new Parent { familyName = "Wakefield", givenName = "Robin" };
    new { first = 1, second = 2 }; //an anonymous type with two fields  
    new int[] { 3, child.grade, 5 };
  ```

## <a name="supported-linq-operators"></a><a id="SupportedLinqOperators"></a>Desteklenen LINQ operatörleri

SQL .NET SDK'ya dahil olan LINQ sağlayıcısı aşağıdaki operatörleri destekler:

- **Select**: Projeksiyonlar nesne yapısı da dahil olmak üzere SQL SELECT'e çevirir.
- **Nerede**: Filtreler SQL WHERE'e çevirir `&&` `||`ve `!` , , ve SQL işleçleri arasındaki mütercim
- **SelectMany**: SQL JOIN yan tümcesi için dizilerin gevşemesine izin verir. Dizi öğelerine filtre dağıtmak için ifadeleri zincirlemek veya yuvalamak için kullanın.
- **OrderBy** ve **OrderByDescending**: ASC veya DESC ile ORDER BY'ye çevirin.
- **Count**, **Sum**, **Min**, **Max**, ve toplama için **ortalama** operatörler ve onların async eşdeğerleri **CountAsync**, **SumAsync**, **MinAsync**, **MaxAsync**, ve **AverageAsync**.
- **CompareTo**: Aralık karşılaştırmalarına çevirir. .NET'te karşılaştırılabilir olmadıklarından, dizeleri için yaygın olarak kullanılır.
- **Atla** ve **Al**: Bir sorgudan elde edilen sonuçları sınırlamak ve pagination yapmak için SQL OFSET ve LIMIT'e çevirir.
- **Matematik fonksiyonları**: .NET `Abs` `Acos`, `Asin` `Atan`, `Ceiling` `Cos` `Exp`, `Floor`, `Log` `Log10`, `Pow` `Round`, `Sign` `Sin` `Sqrt` `Tan`, , `Truncate` , , , , , , , ve eşdeğer SQL dahili işlevlerinden çeviridestekler.
- **String fonksiyonları**: .NET `Concat` `Contains`, `Count` `EndsWith`,`IndexOf` `Replace` `Reverse` `StartsWith` `SubString` `ToLower`, , , `ToUpper` `TrimEnd`, `TrimStart` , , , , , , , ve eşdeğer SQL yerleşik işlevlerinden çeviri destekler.
- **Dizi işlevleri**: .NET `Concat` `Contains`ve `Count` eşdeğer SQL yerleşik işlevlerinden çeviri destekler.
- **Jeouzamsal Uzantı işlevleri**: `Distance` `IsValid`Saplama `Within` yöntemlerinden , , ve `IsValidDetailed`eşdeğer SQL yerleşik işlevlerinden çeviridestekler.
- **Kullanıcı Tanımlı İşlev Uzantısı fonksiyonu**: `UserDefinedFunctionProvider.Invoke` Saplama yönteminden ilgili kullanıcı tanımlı işleve çeviri destekler.
- **Çeşitli**: Koşullu operatörlerin `Coalesce` çevirisini ve koşullu operatörleri destekler. Bağlama `Contains` bağlı olarak String CONTAINS, ARRAY_CONTAINS veya SQL IN'e çevirebilir.

## <a name="examples"></a>Örnekler

Aşağıdaki örnekler, bazı standart LINQ sorgu işleçlerinin Cosmos DB sorgularına nasıl çevrildiği gösterilmektedir.

### <a name="select-operator"></a>Operatör seçin

Sözdizimi, `input.Select(x => f(x))`skaler bir ifadenin olduğu yerdir. `f`

**Operatör seçin, örnek 1:**

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Select(family => family.parents[0].familyName);
  ```
  
- **SQL**
  
  ```sql
      SELECT VALUE f.parents[0].familyName
      FROM Families f
    ```
  
**Operatör seçin, örnek 2:** 

- **LINQ lambda ifadesi**
  
  ```csharp
      input.Select(family => family.children[0].grade + c); // c is an int variable
  ```
  
- **SQL**
  
  ```sql
      SELECT VALUE f.children[0].grade + c
      FROM Families f
  ```
  
**Operatör seçin, örnek 3:**

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

### <a name="selectmany-operator"></a>SelectMany operatörü

Sözdizimi, `input.SelectMany(x => f(x))`kapsayıcı `f` türünü döndüren skaler bir ifadenin olduğu yerdir.

- **LINQ lambda ifadesi**
  
  ```csharp
      input.SelectMany(family => family.children);
  ```
  
- **SQL**

  ```sql
      SELECT VALUE child
      FROM child IN Families.children
  ```

### <a name="where-operator"></a>Nerede operatör

Sözdizimi, `input.Where(x => f(x))`Boolean değerini döndüren skaler bir ifadenin olduğu yerdir. `f`

**Nerede işleç, örnek 1:**

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
  
**Nerede işleç, örnek 2:**

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

Daha güçlü sorgular oluşturmak için önceki işleçleri oluşturabilirsiniz. Cosmos DB iç içe kapsayıcıları desteklediğinden, kompozisyonu eşiçe veya yuvaya yerleştirmeyapabilirsiniz.

### <a name="concatenation"></a>Bitiştirme

Sözdizimi. `input(.|.SelectMany())(.Select()|.Where())*` Concatenated sorgusu isteğe bağlı `SelectMany` bir sorgu `Select` ile `Where` başlatılabilir, ardından birden çok veya işleçler.

**Concatenation, örnek 1:**

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

**Concatenation, örnek 2:**

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

**Concatenation, örnek 3:**

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

**Concatenation, örnek 4:**

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

### <a name="nesting"></a>Iç içe

Sözdizimi, `input.SelectMany(x=>x.Q())` `Q` bir `Select`, `SelectMany`veya `Where` işleç nerededir.

İç içe geçen bir sorgu, iç sorguyu dış kapsayıcının her öğesine uygular. Önemli bir özellik, iç sorgunun kendi kendine birleştirme gibi dış kapsayıcıdaki öğelerin alanlarına başvurabiliyor olmasıdır.

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
