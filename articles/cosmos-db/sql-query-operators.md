---
title: Azure Cosmos DB için SQL sorgu operatörleri
description: Azure Cosmos DB tarafından desteklenen eşitlik, karşılaştırma ve mantıksal işleçler gibi SQL işleçleri hakkında bilgi edinin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: tisande
ms.openlocfilehash: 8ef41edb687a5df39243880c897d12e83c008ec9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063566"
---
# <a name="operators-in-azure-cosmos-db"></a>Azure Cosmos DB'deki operatörler

Bu makalede, Azure Cosmos DB tarafından desteklenen çeşitli işleçler ayrıntıları.

## <a name="equality-and-comparison-operators"></a>Eşitlik ve Karşılaştırma Operatörleri

Aşağıdaki tablo, SQL API'de iki JSON türü arasındaki eşitlik karşılaştırmalarının sonucunu gösterir.

| **Op** | **Tanımsız** | **Null** | **Boole** | **Numarası** | **Dize** | **Nesne** | **Dizi** |
|---|---|---|---|---|---|---|---|
| **Tanımsız** | Tanımsız | Tanımsız | Tanımsız | Tanımsız | Tanımsız | Tanımsız | Tanımsız |
| **Null** | Tanımsız | **Tamam** | Tanımsız | Tanımsız | Tanımsız | Tanımsız | Tanımsız |
| **Boole** | Tanımsız | Tanımsız | **Tamam** | Tanımsız | Tanımsız | Tanımsız | Tanımsız |
| **Numarası** | Tanımsız | Tanımsız | Tanımsız | **Tamam** | Tanımsız | Tanımsız | Tanımsız |
| **Dize** | Tanımsız | Tanımsız | Tanımsız | Tanımsız | **Tamam** | Tanımsız | Tanımsız |
| **Nesne** | Tanımsız | Tanımsız | Tanımsız | Tanımsız | Tanımsız | **Tamam** | Tanımsız |
| **Dizi** | Tanımsız | Tanımsız | Tanımsız | Tanımsız | Tanımsız | Tanımsız | **Tamam** |

Türler arasında `>`veya `>=` `!=`iki `<`nesne `<=`veya dizi arasında karşılaştırma gibi karşılaştırma `Undefined`işleçleri için üretir.  

Skaler ifadenin sonucu `Undefined`ise, öğe eşit `Undefined` `true`olmadığından, sonuca dahil değildir.

## <a name="logical-and-or-and-not-operators"></a>Mantıksal (VE, VEYA ve Değİl) işleçleri

Mantıksal işleçler Boolean değerleri üzerinde çalışır. Aşağıdaki tablolar bu işleçler için mantıksal doğruluk tablolarını gösterir:

**VEYA operatörü**

`true` Koşullardan biri `true`.

|  | **True** | **False** | **Tanımsız** |
| --- | --- | --- | --- |
| **True** |True |True |True |
| **False** |True |False |Tanımsız |
| **Tanımsız** |True |Tanımsız |Tanımsız |

**VE operatörü**

Her `true` iki ifade `true`de .

|  | **True** | **False** | **Tanımsız** |
| --- | --- | --- | --- |
| **True** |True |False |Tanımsız |
| **False** |False |False |False |
| **Tanımsız** |Tanımsız |False |Tanımsız |

**NOT operatörü**

Herhangi bir Boolean ifadesinin değerini tersine çevirir.

|  | **Değil** |
| --- | --- |
| **True** |False |
| **False** |True |
| **Tanımsız** |Tanımsız |

**İşleç Önceliği**

Mantıksal işleçler `OR`, `AND`ve `NOT` öncelik düzeyi aşağıda gösterilmiştir:

| **Işleç** | **Öncelik** |
| --- | --- |
| **Değil** |1 |
| **Ve** |2 |
| **Veya** |3 |

## <a name="-operator"></a>* işleci

Özel işleç * tüm öğeyi olduğu gibi projeler. Kullanıldığında, yansıtılan tek alan olmalıdır. Benzer `SELECT * FROM Families f` bir sorgu geçerlidir, ancak `SELECT VALUE * FROM Families f` `SELECT *, f.id FROM Families f` geçerli değildir.

## <a name="-and--operators"></a>? Ve?? işleçler

C# ve JavaScript gibi programlama dillerinde olduğu gibi koşullu ifadeler oluşturmak için Ternary (?) ve Coalesce (??) işleçlerini kullanabilirsiniz.

Bunu kullanabilir misin? operatör anında yeni JSON özellikleri inşa etmek. Örneğin, aşağıdaki sorgu sınıf düzeylerini `elementary` veya `other`aşağıdakileri sınıflar:

```sql
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel
     FROM Families.children[0] c
```

Ayrıca aramaları yuva olabilir? işleci, aşağıdaki sorguda olduğu gibi: 

```sql
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high") AS gradeLevel
    FROM Families.children[0] c
```

Diğer sorgu işleçleri gibi, ? başvurulan özellikler eksikse veya karşılaştırılan türler farklıysa, işleç öğeleri hariç tutar.

Kullanın ?? yarı yapılandırılmış veya karma türdeki verilere karşı sorgu yaparken bir maddedeki bir özelliği etkin bir şekilde denetlemek için işleç. Örneğin, varsa veya `lastName` `surname` `lastName` yoksa aşağıdaki sorgu döndürür.

```sql
    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Anahtar Sözcükler](sql-query-keywords.md)
- [SELECT yan tümcesi](sql-query-select.md)
