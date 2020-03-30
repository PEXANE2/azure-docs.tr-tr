---
title: Azure Cosmos DB sorgu dilinde LEFT
description: Azure Cosmos DB'de LEFT SQL sistem işlevi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 0eac35a91e4d5158335d6797d49a09f8f6f391e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303758"
---
# <a name="left-azure-cosmos-db"></a>SOL (Azure Cosmos DB)
 Belirtilen karakter sayısıyla bir dize sol bölümünü döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
LEFT(<str_expr>, <num_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   Karakterleri ayıklamak için dize ifadesidir.  
  
*num_expr*  
   Karakter sayısını belirten sayısal bir ifadedir.  
  
## <a name="return-types"></a>İade türleri
  
  Dize ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, çeşitli uzunluk değerleri için "abc"nin sol kısmını döndürür.  
  
```sql
SELECT LEFT("abc", 1) AS l1, LEFT("abc", 2) AS l2 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"l1": "a", "l2": "ab"}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi bir [aralık dizini](index-policy.md#includeexclude-strategy)yararlanacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [String fonksiyonları Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
