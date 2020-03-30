---
title: Azure Cosmos DB sorgu dilinde GetCurrentDateTime
description: Azure Cosmos DB'de SQL sistem fonksiyonu GetCurrentDateTime hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: d50b08ab85c7e299c465c3eb6f34e867d6634006
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303911"
---
# <a name="getcurrentdatetime-azure-cosmos-db"></a>GetCurrentDateTime (Azure Cosmos DB)
 Geçerli UTC (Eşgüdümlü Evrensel Saat) tarih ve saatini ISO 8601 dizesi olarak döndürür.
  
## <a name="syntax"></a>Sözdizimi
  
```sql
GetCurrentDateTime ()
```
  
## <a name="return-types"></a>İade türleri
  
  Geçerli UTC tarih ve saati ISO 8601 `YYYY-MM-DDThh:mm:ss.fffffffZ` dize değerini aşağıdaki biçimde verir:
  
  |||
  |-|-|
  |YYYY|dört basamaklı yıl|
  |MM|iki basamaklı ay (01 = Ocak, vb.)|
  |DD|ayın iki basamaklı günü (01 ile 31 arası)|
  |T|zaman öğelerinin başlangıcı için işaret|
  |hh|iki basamaklı saat (00 -23)|
  |mm|iki basamaklı dakika (00 -59)|
  |ss|iki basamaklı saniye (00 - 59)|
  |.fffffff|yedi basamaklı kesirli saniye|
  |Z|UTC (Eşgüdümlü Evrensel Zaman) tasnörü||
  
  ISO 8601 formatı hakkında daha fazla bilgi için [ISO_8601 bkz.](https://en.wikipedia.org/wiki/ISO_8601)

## <a name="remarks"></a>Açıklamalar

  GetCurrentDateTime() nondeterministic bir işlevdir. 
  
  Döndürülen sonuç UTC'dir.

  Hassasiyet, 100 nanosaniye lik bir doğrulukla 7 basamaktır.

## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, GetCurrentDateTime() yerleşik işlevini kullanarak geçerli UTC Tarih Saatini nasıl elde edilebildiğini gösterir.
  
```sql
SELECT GetCurrentDateTime() AS currentUtcDateTime
```  
  
 Burada bir örnek sonuç kümesidir.
  
```json
[{
  "currentUtcDateTime": "2019-05-03T20:36:17.1234567Z"
}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Tarih ve saat fonksiyonları Azure Cosmos DB](sql-query-date-time-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
