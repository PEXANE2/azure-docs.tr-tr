---
title: Azure Cosmos DB sorgu dilinde DateTimePart
description: Azure Cosmos DB 'de SQL sistem işlevi DateTimePart hakkında bilgi edinin.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/14/2020
ms.author: tisande
ms.custom: query-reference
ms.openlocfilehash: dd88f8d6a0f5271da97d6f2f95eecb60bf94ed78
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93095824"
---
# <a name="datetimepart-azure-cosmos-db"></a>DateTimePart (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Belirtilen tarih/saat arasındaki belirtilen DateTimePart 'ın değerini döndürür.
  
## <a name="syntax"></a>Söz dizimi
  
```sql
DateTimePart (<DateTimePart> , <DateTime>)
```

## <a name="arguments"></a>Bağımsız değişkenler
  
*DateTimePart*  
   Tarih, DateTimePart 'ın değeri döndürdüğü bölümüdür. Bu tablo tüm geçerli DateTimePart bağımsız değişkenlerini listeler:

| DateTimePart | kısaltmaları        |
| ------------ | -------------------- |
| Yıl         | "Year", "yyyy", "yy" |
| Ay        | "ay", "AA", "m"   |
| Gün          | "gün", "gg", "d"     |
| Saat         | "saat", "hh"         |
| Dakika       | "Minute", "mı", "n"  |
| Second       | "saniye", "ss", "s"  |
| Milisaniy  | "milisaniyelik", "MS"  |
| Mikrosaniye ölçeğinde  | "mikro saniye", "MCS" |
| Nanosaniyelik   | "nanosaniyelik", "NS"   |

*Tarih Saat*  
   Biçimde UTC Tarih ve saat ISO 8601 dize değeri `YYYY-MM-DDThh:mm:ss.fffffffZ`

## <a name="return-types"></a>Dönüş türleri

Pozitif bir tamsayı değeri döndürür.

## <a name="remarks"></a>Açıklamalar

DateTimePart `undefined` aşağıdaki nedenlerle döndürülür:

- Belirtilen DateTimePart değeri geçersiz
- DateTime geçerli bir ISO 8601 tarih saati değil

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="examples"></a>Örnekler

Ayın tamsayı değerini döndüren bir örnek aşağıda verilmiştir:

```sql
SELECT DateTimePart("m", "2020-01-02T03:04:05.6789123Z") AS MonthValue
```

```json
[
    {
        "MonthValue": 1
    }
]
```

Mikrosaniye sayısını döndüren bir örnek aşağıda verilmiştir:

```sql
SELECT DateTimePart("mcs", "2020-01-02T03:04:05.6789123Z") AS MicrosecondsValue
```

```json
[
    {
        "MicrosecondsValue": 678912
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

- [Tarih ve saat işlevleri Azure Cosmos DB](sql-query-date-time-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
