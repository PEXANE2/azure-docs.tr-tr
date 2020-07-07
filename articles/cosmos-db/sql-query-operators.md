---
title: Azure Cosmos DB için SQL sorgu işleçleri
description: Azure Cosmos DB tarafından desteklenen eşitlik, karşılaştırma ve mantıksal işleçler gibi SQL işleçleri hakkında bilgi edinin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: tisande
ms.openlocfilehash: 8ef41edb687a5df39243880c897d12e83c008ec9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80063566"
---
# <a name="operators-in-azure-cosmos-db"></a>Azure Cosmos DB işleçler

Bu makalede Azure Cosmos DB tarafından desteklenen çeşitli işleçler ayrıntılı olarak açıklanır.

## <a name="equality-and-comparison-operators"></a>Eşitlik ve karşılaştırma Işleçleri

Aşağıdaki tabloda, iki JSON türü arasındaki SQL API 'sindeki eşitlik karşılaştırmalarının sonucu gösterilmektedir.

| **Üs** | **Tanımlayan** | **Null** | **Boole** | **Sayı** | **Dize** | **Nesne** | **Dizi** |
|---|---|---|---|---|---|---|---|
| **Tanımlayan** | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan |
| **Null** | Tanımlayan | **Tamam** | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan |
| **Boole** | Tanımlayan | Tanımlayan | **Tamam** | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan |
| **Sayı** | Tanımlayan | Tanımlayan | Tanımlayan | **Tamam** | Tanımlayan | Tanımlayan | Tanımlayan |
| **Dize** | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | **Tamam** | Tanımlayan | Tanımlayan |
| **Nesne** | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | **Tamam** | Tanımlayan |
| **Dizi** | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | Tanımlayan | **Tamam** |

,,, Ve gibi karşılaştırma işleçleri için, `>` `>=` `!=` `<` `<=` türler arasında veya iki nesne veya dizi arasında karşılaştırma üretir `Undefined` .  

Skaler ifadenin sonucu ise, `Undefined` eşit olmadığı için öğe sonuca dahil değildir `Undefined` `true` .

## <a name="logical-and-or-and-not-operators"></a>Mantıksal (ve, OR ve NOT) işleçleri

Mantıksal işleçler Boole değerleri üzerinde çalışır. Aşağıdaki tablolarda bu işleçler için mantıksal Truth tabloları gösterilmektedir:

**OR işleci**

`true`Koşullardan biri olduğunda döndürür `true` .

|  | **Değeri** | **Yanlýþ** | **Tanımlayan** |
| --- | --- | --- | --- |
| **Değeri** |True |True |True |
| **Yanlýþ** |True |False |Tanımlayan |
| **Tanımlayan** |True |Tanımlayan |Tanımlayan |

**AND işleci**

`true`Her iki ifadenin de ne zaman olduğunu döndürür `true` .

|  | **Değeri** | **Yanlýþ** | **Tanımlayan** |
| --- | --- | --- | --- |
| **Değeri** |True |False |Tanımlayan |
| **Yanlýþ** |False |False |False |
| **Tanımlayan** |Tanımlayan |False |Tanımlayan |

**NOT işleci**

Herhangi bir Boolean ifadesinin değerini tersine çevirir.

|  | **BAŞLATıLMADı** |
| --- | --- |
| **Değeri** |False |
| **Yanlýþ** |True |
| **Tanımlayan** |Tanımlayan |

**İşleç önceliği**

Mantıksal işleçler, `OR` `AND` ve `NOT` aşağıda gösterilen öncelik düzeyine sahiptir:

| **Operatör** | **Priority** |
| --- | --- |
| **BAŞLATıLMADı** |1 |
| **'** |2 |
| **VEYA** |3 |

## <a name="-operator"></a>* işleci

* Özel işleci, tüm öğeyi olduğu gibi projeler. Kullanıldığında, tek bir öngörülen alan olması gerekir. Benzer bir sorgu `SELECT * FROM Families f` geçerli, ancak `SELECT VALUE * FROM Families f` `SELECT *, f.id FROM Families f` geçerli değil.

## <a name="-and--operators"></a>? ve?? işleçler

C# ve JavaScript gibi programlama dillerinde olduğu gibi, Koşullu ifadeler oluşturmak için üçlü (?) ve birleşim (??) işleçlerini kullanabilirsiniz.

Kullanabilirsiniz. anında yeni JSON özellikleri oluşturmaya yönelik operatör. Örneğin, aşağıdaki sorgu, sınıf düzeylerini veya içine sınıflandırır `elementary` `other` :

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

??? yarı yapılandırılmış veya karma tür verilere göre sorgulama yaparken bir öğedeki özelliği etkin bir şekilde denetlemek için işleç. Örneğin, aşağıdaki sorgu varsa döndürür `lastName` veya yoksa `surname` `lastName` .

```sql
    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Anahtar sözcükler](sql-query-keywords.md)
- [SELECT yan tümcesi](sql-query-select.md)
