---
title: Azure Cosmos DB sorgu dilinde DateTimeToTimestamp
description: Azure Cosmos DB 'de SQL sistem işlevi DateTimeToTimestamp hakkında bilgi edinin.
author: timsander1
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 08/18/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: 11e0150089fd979cbf840d3b117c0d796ecc68f3
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93335730"
---
# <a name="datetimetotimestamp-azure-cosmos-db"></a>DateTimeToTimestamp (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Belirtilen tarih/saati bir zaman damgasına dönüştürür.
  
## <a name="syntax"></a>Söz dizimi
  
```sql
DateTimeToTimestamp (<DateTime>)
```

## <a name="arguments"></a>Bağımsız değişkenler

*Tarih Saat*  
   Şu biçimdeki UTC Tarih ve saat ISO 8601 dize değeri `YYYY-MM-DDThh:mm:ss.fffffffZ` :
  
  |Biçimlendir|Açıklama|
  |-|-|
  |YYYY|dört basamaklı yıl|
  |MM|iki basamaklı ay (01 = Ocak, vb.)|
  |DD|iki basamaklı ayın günü (01 ile 31 arasında)|
  |T|zaman öğelerinin başlangıcı için signifier|
  |hh|iki basamaklı saat (00 ile 23 arasında)|
  |mm|iki basamaklı dakika (00 ila 59)|
  |ss|iki basamaklı saniyeler (00 ila 59)|
  |. fffffff|yedi basamaklı kesirli saniye|
  |Z|UTC (Eşgüdümlü Evrensel Saat) göstergesi||
  
  ISO 8601 biçimi hakkında daha fazla bilgi için bkz. [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601)

## <a name="return-types"></a>Dönüş türleri

İşaretli bir sayısal değeri döndürür. bu yana geçen milisaniye sayısı, örneğin, UNIX dönemi (00:00:00, 1 Ocak 1970).

## <a name="remarks"></a>Açıklamalar

`undefined`Belirtilen DateTime değeri geçersizse DateTimeToTimestamp döndürülür

## <a name="examples"></a>Örnekler
  
Aşağıdaki örnek, tarih ve saati bir zaman damgasına dönüştürür:

```sql
SELECT DateTimeToTimestamp("2020-07-09T23:20:13.4575530Z") AS Timestamp
```

```json
[
    {
        "Timestamp": 1594336813457
    }
]
```  

Aşağıda başka bir örnek verilmiştir:

```sql
SELECT DateTimeToTimestamp("2020-07-09") AS Timestamp
```

```json
[
    {
        "Timestamp": 1594252800000
    }
]
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Tarih ve saat işlevleri Azure Cosmos DB](sql-query-date-time-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
