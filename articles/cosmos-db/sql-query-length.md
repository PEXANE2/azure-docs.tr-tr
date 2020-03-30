---
title: Azure Cosmos DB sorgu dilinde UZUNLUK
description: Azure Cosmos DB'de SQL sistem fonksiyonu LENGTH hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: e020555b0c706b5577bd20ac9bd537604d43ba3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303724"
---
# <a name="length-azure-cosmos-db"></a>UZUNLUK (Azure Cosmos DB)
 Belirtilen dize ifadesinin karakter sayısını döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
LENGTH(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   Değerlendirilecek dize ifadesidir.  
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, bir dize uzunluğunu döndürür.  
  
```sql
SELECT LENGTH("abc") AS len 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"len": 3}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [String fonksiyonları Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
