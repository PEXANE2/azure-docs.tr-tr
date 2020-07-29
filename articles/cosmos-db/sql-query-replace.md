---
title: Azure Cosmos DB sorgu dilinde DEĞIŞTIR
description: Azure Cosmos DB 'da SQL sistem işlevi DEĞIŞTIRME hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 758ac13530752df481d27e7e253f025f5c8d6430
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78302211"
---
# <a name="replace-azure-cosmos-db"></a>DEĞIŞTIR (Azure Cosmos DB)
 Belirtilen bir dize değerinin tüm oluşumlarını başka bir dize değeriyle değiştirir.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
REPLACE(<str_expr1>, <str_expr2>, <str_expr3>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*str_expr1*  
   , Aranacak dize ifadesidir.  
  
*str_expr2*  
   , Bulunan dize ifadesidir.  
  
*str_expr3*  
   , *Str_expr1* *str_expr2* tekrarlarının yerini alacak dize ifadesidir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir dize ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, bir sorguda nasıl kullanılacağını gösterir `REPLACE` .  
  
```sql
SELECT REPLACE("This is a Test", "Test", "desk") AS replace
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"replace": "This is a desk"}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
