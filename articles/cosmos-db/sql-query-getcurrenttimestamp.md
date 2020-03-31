---
title: Azure Cosmos DB sorgu dilinde GetCurrentTimestamp
description: Azure Cosmos DB'de SQL sistem fonksiyonu GetCurrentTimestamp hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: b816c63da56025fe6e9cbaece2cde5dcd01585d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71351001"
---
# <a name="getcurrenttimestamp-azure-cosmos-db"></a>GetCurrentTimestamp (Azure Cosmos DB)
 1 Ocak 1970 Perşembe günü saat 00:00:00'dan bu yana geçen milisaniye sayısını verir. 
  
## <a name="syntax"></a>Sözdizimi
  
```sql
GetCurrentTimestamp ()  
```  
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir değer verir, Unix döneminden bu yana geçen milisaniyelerin geçerli sayısını yani 1 Ocak 1970 Perşembe günü 00:00:00'dan bu yana geçen milisaniye sayısını verir.

## <a name="remarks"></a>Açıklamalar

  GetCurrentTimestamp() nondeterministic bir işlevdir.
  
  Döndürülen sonuç UTC (Eşgüdümlü Evrensel Zaman) olduğunu.

## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, GetCurrentTimestamp() yerleşik işlevini kullanarak geçerli zaman damgasınınasıl elde edilebildiğini gösterir.
  
```sql
SELECT GetCurrentTimestamp() AS currentUtcTimestamp
```  
  
 Burada bir örnek sonuç kümesidir.
  
```json
[{
  "currentUtcTimestamp": 1556916469065
}]  
```

## <a name="next-steps"></a>Sonraki adımlar

- [Tarih ve saat fonksiyonları Azure Cosmos DB](sql-query-date-time-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
