---
title: Azure Cosmos DB sorgu dilinde DEĞIŞTIR
description: Azure Cosmos DB'de SQL sistem fonksiyonu REPLACE hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 758ac13530752df481d27e7e253f025f5c8d6430
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302211"
---
# <a name="replace-azure-cosmos-db"></a>REPLACE (Azure Cosmos DB)
 Belirtilen bir dize değerinin tüm oluşumlarını başka bir dize değeriyle değiştirir.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
REPLACE(<str_expr1>, <str_expr2>, <str_expr3>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr1*  
   Aranacak dize ifadesidir.  
  
*str_expr2*  
   Bulunacak dize ifadesidir.  
  
*str_expr3*  
   *str_expr1* *str_expr2* oluşumları yerine dize ifadesi dir.  
  
## <a name="return-types"></a>İade türleri
  
  Dize ifadesini döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnekte, sorguda nasıl kullanılacağı `REPLACE` gösterilmektedir.  
  
```sql
SELECT REPLACE("This is a Test", "Test", "desk") AS replace
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"replace": "This is a desk"}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [String fonksiyonları Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
