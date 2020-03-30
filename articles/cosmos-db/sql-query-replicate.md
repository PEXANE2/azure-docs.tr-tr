---
title: Azure Cosmos DB sorgu dilinde ÇOĞALTMA
description: Azure Cosmos DB'de SQL sistem fonksiyonu ÇOĞALTMA hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 19fcde522c5cb0355e53a5616145f27fada7dad9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302194"
---
# <a name="replicate-azure-cosmos-db"></a>ÇOĞALTMA (Azure Cosmos DB)
 Bir dize değerini belirli sayıda yineler.
  
## <a name="syntax"></a>Sözdizimi
  
```sql
REPLICATE(<str_expr>, <num_expr>)
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   Bir dize ifadesidir.
  
*num_expr*  
   Sayısal bir ifadedir. *num_expr* negatif veya sonlu değilse, sonuç tanımsızdır.
  
## <a name="return-types"></a>İade türleri
  
  Dize ifadesini döndürür.
  
## <a name="remarks"></a>Açıklamalar
  Sonucun maksimum uzunluğu 10.000 karakter yani (uzunluk(str_expr*str_expr*) * *num_expr*) <= 10.000'dir.

## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnekte, sorguda nasıl kullanılacağı `REPLICATE` gösterilmektedir.
  
```sql
SELECT REPLICATE("a", 3) AS replicate
```  
  
 Sonuç kümesini burada bulabilirsiniz.
  
```json
[{"replicate": "aaa"}]
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [String fonksiyonları Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
