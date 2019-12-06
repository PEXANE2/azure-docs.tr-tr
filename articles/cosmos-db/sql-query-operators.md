---
title: Azure Cosmos DB için SQL sorgu işleçleri
description: Azure Cosmos DB tarafından desteklenen eşitlik, karşılaştırma ve mantıksal işleçler gibi SQL işleçleri hakkında bilgi edinin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: f3efe4bee749f0d3132206ca68a33a60f0e16b81
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74870947"
---
# <a name="operators-in-azure-cosmos-db"></a>Azure Cosmos DB işleçler

Bu makalede Azure Cosmos DB tarafından desteklenen çeşitli işleçler ayrıntılı olarak açıklanır.

## <a name="equality-and-comparison-operators"></a>Eşitlik ve karşılaştırma Işleçleri

Aşağıdaki tabloda, her iki JSON türünden SQL API eşitlik karşılaştırmaları sonucunu gösterir.

| **OP** | **Tanımsız** | **Null** | **Boole değeri** | **Sayı** | **dize** | **Nesne** | **Dizi** |
|---|---|---|---|---|---|---|---|
| **Tanımsız** | Undefined | Undefined | Undefined | Undefined | Undefined | Undefined | Undefined |
| **Null** | Undefined | **Tamam** | Undefined | Undefined | Undefined | Undefined | Undefined |
| **Boole değeri** | Undefined | Undefined | **Tamam** | Undefined | Undefined | Undefined | Undefined |
| **Sayı** | Undefined | Undefined | Undefined | **Tamam** | Undefined | Undefined | Undefined |
| **dize** | Undefined | Undefined | Undefined | Undefined | **Tamam** | Undefined | Undefined |
| **Nesne** | Undefined | Undefined | Undefined | Undefined | Undefined | **Tamam** | Undefined |
| **Dizi** | Undefined | Undefined | Undefined | Undefined | Undefined | Undefined | **Tamam** |

`>`, `>=`, `!=`, `<`ve `<=`gibi karşılaştırma işleçleri için, türler arasında veya iki nesne veya dizi arasında karşılaştırma `Undefined`üretir.  

Skaler ifadenin sonucu `Undefined`, `Undefined` `true`eşit olmadığı için öğe sonuca dahil değildir.

## <a name="logical-and-or-and-not-operators"></a>Mantıksal (AND, OR ve NOT) işleçleri

Mantıksal işleçler Boole değerleri üzerinde çalışır. Aşağıdaki tablolarda bu işleçler için mantıksal Truth tabloları gösterilmektedir:

**OR işleci**

| VEYA | Doğru | Yanlış | Undefined |
| --- | --- | --- | --- |
| Doğru |Doğru |Doğru |Doğru |
| Yanlış |Doğru |Yanlış |Undefined |
| Undefined |Doğru |Undefined |Undefined |

**AND işleci**

| VE | Doğru | Yanlış | Undefined |
| --- | --- | --- | --- |
| Doğru |Doğru |Yanlış |Undefined |
| Yanlış |Yanlış |Yanlış |Yanlış |
| Undefined |Undefined |Yanlış |Undefined |

**NOT işleci**

| DEĞİL |  |
| --- | --- |
| Doğru |Yanlış |
| Yanlış |Doğru |
| Undefined |Undefined |


## <a name="-operator"></a>* işleci

\* Özel işleci, tüm öğeyi olduğu gibi projeler. Kullanıldığında yansıtılan tek alan olması gerekir. `SELECT * FROM Families f` gibi bir sorgu geçerli, ancak `SELECT VALUE * FROM Families f` ve `SELECT *, f.id FROM Families f` geçerli değil.

## <a name="-and--operators"></a>? ve?? işleçler

C# Ve JavaScript gibi programlama dillerinde olduğu gibi, Koşullu ifadeler oluşturmak için üçlü (?) ve birleşim (??) işleçlerini kullanabilirsiniz. 

Kullanabilirsiniz. anında yeni JSON özellikleri oluşturmaya yönelik operatör. Örneğin, aşağıdaki sorgu, sınıf düzeylerini `elementary` veya `other`sınıflandırır:

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

??? yarı yapılandırılmış veya karma tür verilere göre sorgulama yaparken bir öğedeki özelliği etkin bir şekilde denetlemek için işleç. Örneğin, aşağıdaki sorgu varsa `lastName` döndürür veya `lastName` yoksa `surname`.

```sql
    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [anahtar sözcükler](sql-query-keywords.md)
- [SELECT yan tümcesi](sql-query-select.md)
