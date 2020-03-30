---
title: Azure Cosmos DB sorgu dilinde Kİ
description: Azure Cosmos DB'de SQL sistem fonksiyonu PI hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 27832008e8922e339a648985192a58b111555bc9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71349659"
---
# <a name="pi-azure-cosmos-db"></a>PI (Azure Cosmos DB)
 PI sabit değerini verir.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
PI ()  
```  
   
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnekte `PI`. değerini döndürür.  
  
```sql
SELECT PI() AS pi 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"pi": 3.1415926535897931}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Matematiksel fonksiyonlar Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
