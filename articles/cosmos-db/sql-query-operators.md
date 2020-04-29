---
title: Azure Cosmos DB için SQL sorgu işleçleri
description: Azure Cosmos DB tarafından desteklenen eşitlik, karşılaştırma ve mantıksal işleçler gibi SQL işleçleri hakkında bilgi edinin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: tisande
ms.openlocfilehash: 8ef41edb687a5df39243880c897d12e83c008ec9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80063566"
---
# <a name="operators-in-azure-cosmos-db"></a>Azure Cosmos DB işleçler

Bu makalede Azure Cosmos DB tarafından desteklenen çeşitli işleçler ayrıntılı olarak açıklanır.

## <a name="equality-and-comparison-operators"></a>Eşitlik ve karşılaştırma Işleçleri

Aşağıdaki tabloda, iki JSON türü arasındaki SQL API 'sindeki eşitlik karşılaştırmalarının sonucu gösterilmektedir.

| **Üs** | **Tanımlayan** | **Değer** | **Boole** | **Sayısından** | **Dize** | **Nesne** | **Dizi** |
|---|---|---|---|---|---|---|---|
| **Tanımlayan** | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan |
| **Değer** | Tanımlayan | **Tamam** | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan |
| **Boole** | Tanımlayan | Tanımlayan | **Tamam** | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan |
| **Sayısından** | Tanımlayan | Tanımlayan | Tanımlayan | **Tamam** | Tanımlayan | Tanımlayan | Tanımlayan |
| **Dize** | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | **Tamam** | Tanımlayan | Tanımlayan |
| **Nesne** | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | **Tamam** | Tanımlayan |
| **Dizi** | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | **Tamam** |

`>` `>=`, `!=` `<=`,, Ve gibi karşılaştırma işleçleri için, türler arasında veya iki nesne veya dizi arasında karşılaştırma üretir `Undefined` `<`  

Skaler ifadenin sonucu ise `Undefined`, eşit `Undefined` `true`olmadığı için öğe sonuca dahil değildir.

## <a name="logical-and-or-and-not-operators"></a>Mantıksal (ve, OR ve NOT) işleçleri

Mantıksal işleçler Boole değerleri üzerinde çalışır. Aşağıdaki tablolarda bu işleçler için mantıksal Truth tabloları gösterilmektedir:

**OR işleci**

Koşullardan `true` biri olduğunda döndürür `true`.

|  | **True** | **False** | **Tanımlayan** |
| --- | --- | --- | --- |
| **True** |True |True |True |
| **False** |True |False |Tanımlayan |
| **Tanımlayan** |True |Tanımlayan |Tanımlayan |

**AND işleci**

Her `true` iki ifadenin de ne `true`zaman olduğunu döndürür.

|  | **True** | **False** | **Tanımlayan** |
| --- | --- | --- | --- |
| **True** |True |False |Tanımlayan |
| **False** |False |False |False |
| **Tanımlayan** |Tanımlayan |False |Tanımlayan |

**NOT işleci**

Herhangi bir Boolean ifadesinin değerini tersine çevirir.

|  | **BAŞLATıLMADı** |
| --- | --- |
| **True** |False |
| **False** |True |
| **Tanımlayan** |Tanımlayan |

**İşleç Önceliği**

Mantıksal işleçler `OR` `AND`, ve `NOT` aşağıda gösterilen öncelik düzeyine sahiptir:

| **İşleç** | **Öncelik** |
| --- | --- |
| **BAŞLATıLMADı** |1 |
| **'** |2 |
| **VEYA** |3 |

## <a name="-operator"></a>* işleci

* Özel işleci, tüm öğeyi olduğu gibi projeler. Kullanıldığında, tek bir öngörülen alan olması gerekir. Benzer `SELECT * FROM Families f` bir sorgu geçerli, ancak `SELECT VALUE * FROM Families f` `SELECT *, f.id FROM Families f` geçerli değil.

## <a name="-and--operators"></a>? ve?? işleçler

C# ve JavaScript gibi programlama dillerinde olduğu gibi, Koşullu ifadeler oluşturmak için üçlü (?) ve birleşim (??) işleçlerini kullanabilirsiniz.

Kullanabilirsiniz. anında yeni JSON özellikleri oluşturmaya yönelik operatör. Örneğin, aşağıdaki sorgu, sınıf düzeylerini veya `elementary` `other`içine sınıflandırır:

```sql
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel
     FROM Families.children[0] c
```

Ayrıca, çağrılarını iç içe geçirebilirsiniz. işlecini, aşağıdaki sorguda olduğu gibi: 

```sql
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high") AS gradeLevel
    FROM Families.children[0] c
```

Diğer sorgu işleçleri gibi? başvurulan Özellikler eksikse veya karşılaştırılan türler farklıysa işleç öğeleri dışlar.

??? yarı yapılandırılmış veya karma tür verilere göre sorgulama yaparken bir öğedeki özelliği etkin bir şekilde denetlemek için işleç. Örneğin, aşağıdaki sorgu varsa döndürür `lastName` veya `surname` `lastName` yoksa.

```sql
    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Anahtar Sözcükler](sql-query-keywords.md)
- [SELECT yan tümcesi](sql-query-select.md)
