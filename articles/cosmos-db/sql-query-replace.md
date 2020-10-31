---
title: Azure Cosmos DB sorgu dilinde DEĞIŞTIR
description: Azure Cosmos DB 'da SQL sistem işlevi DEĞIŞTIRME hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: fb3afd2524949b1209ac42542a012b74559234d5
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93098119"
---
# <a name="replace-azure-cosmos-db"></a>DEĞIŞTIR (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

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
- [Azure Cosmos DB'ye giriş](introduction.md)
