---
title: Azure Cosmos DB sorgu dilinde GetCurrentTicks
description: Azure Cosmos DB 'de SQL sistem işlevi GetCurrentTicks hakkında bilgi edinin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/14/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 2ca76d75edba6688dbe93f11a51a0ad67942677a
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88606938"
---
# <a name="getcurrentticks-azure-cosmos-db"></a>GetCurrentTicks (Azure Cosmos DB)

00:00:00 Perşembe, 1 Ocak 1970 ve bu yana geçen 100-nanosaniyelik ticks sayısını döndürür.
  
## <a name="syntax"></a>Syntax
  
```sql
GetCurrentTicks ()
```

## <a name="return-types"></a>Dönüş türleri

, UNIX dönemi bu yana geçen 100-nanosaniyelik Tick 'in geçerli sayısı olan işaretli bir sayısal değer döndürür. Diğer bir deyişle GetCurrentTicks, 00:00:00 Perşembe, 1 Ocak 1970 100, bu yana geçen nanosaniyelik ticks sayısını döndürür.

## <a name="remarks"></a>Açıklamalar

GetCurrentTicks () belirleyici olmayan bir işlevdir. Döndürülen sonuç UTC 'dir (Eşgüdümlü Evrensel Saat).

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="examples"></a>Örnekler

Aşağıda, zaman işaretleri cinsinden ölçülen geçerli zamanı döndüren bir örnek verilmiştir:

```sql
SELECT GetCurrentTicks() AS CurrentTimeInTicks
```

```json
[
    {
        "CurrentTimeInTicks": 15973607943002652
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Tarih ve saat işlevleri Azure Cosmos DB](sql-query-date-time-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
