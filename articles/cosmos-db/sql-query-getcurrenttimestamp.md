---
title: Azure Cosmos DB sorgu dilinde GetCurrentTimestamp
description: Azure Cosmos DB 'de SQL sistem işlevi GetCurrentTimestamp hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/19/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: c1e0a8d69edab0c01005268ee49c23625236f03a
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88606923"
---
# <a name="getcurrenttimestamp-azure-cosmos-db"></a>GetCurrentTimestamp (Azure Cosmos DB)

 00:00:00 Perşembe, 1 Ocak 1970 tarihinden itibaren geçen milisaniye sayısını döndürür.
  
## <a name="syntax"></a>Syntax
  
```sql
GetCurrentTimestamp ()  
```  
  
## <a name="return-types"></a>Dönüş türleri
  
İşaretli bir sayısal değeri döndürür. bu yana geçen milisaniye sayısı, örneğin, UNIX dönemi (00:00:00, 1 Ocak 1970).

## <a name="remarks"></a>Açıklamalar

GetCurrentTimestamp () belirleyici olmayan bir işlevdir. Döndürülen sonuç UTC 'dir (Eşgüdümlü Evrensel Saat).

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, GetCurrentTimestamp () yerleşik işlevini kullanarak geçerli zaman damgasının nasıl alınacağını gösterir.
  
```sql
SELECT GetCurrentTimestamp() AS currentUtcTimestamp
```  
  
 Örnek bir sonuç kümesi aşağıda verilmiştir.
  
```json
[{
  "currentUtcTimestamp": 1556916469065
}]  
```

## <a name="next-steps"></a>Sonraki adımlar

- [Tarih ve saat işlevleri Azure Cosmos DB](sql-query-date-time-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
