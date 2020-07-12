---
title: Azure Cosmos DB sorgu dilinde GetCurrentDateTime
description: Azure Cosmos DB 'de SQL sistem işlevi GetCurrentDateTime hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/09/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 4db2ec7bbd05d982af57c05fd46ef7da7c682e15
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86258802"
---
# <a name="getcurrentdatetime-azure-cosmos-db"></a>GetCurrentDateTime (Azure Cosmos DB)

ISO 8601 dizesi olarak geçerli UTC (Eşgüdümlü Evrensel Saat) Tarih ve saati döndürür.
  
## <a name="syntax"></a>Sözdizimi
  
```sql
GetCurrentDateTime ()
```

## <a name="return-types"></a>Dönüş türleri
  
  Şu biçimdeki geçerli UTC Tarih ve saat ISO 8601 dize değerini döndürür `YYYY-MM-DDThh:mm:ss.fffffffZ` :
  
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

## <a name="remarks"></a>Açıklamalar

  GetCurrentDateTime () belirleyici olmayan bir işlevdir.
  
  Döndürülen sonuç UTC 'dir.

  Duyarlık, 100 nanosaniye değeriyle doğru 7 haneye sahiptir.

## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, GetCurrentDateTime () yerleşik işlevini kullanarak geçerli UTC Tarih zamanının nasıl alınacağını gösterir.
  
```sql
SELECT GetCurrentDateTime() AS currentUtcDateTime
```  
  
 Örnek bir sonuç kümesi aşağıda verilmiştir.
  
```json
[{
  "currentUtcDateTime": "2019-05-03T20:36:17.1234567Z"
}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Tarih ve saat işlevleri Azure Cosmos DB](sql-query-date-time-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
