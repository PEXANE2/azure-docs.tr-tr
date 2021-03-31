---
title: Azure Cosmos DB sorgu dilinde INDEX_OF
description: Azure Cosmos DB INDEX_OF SQL sistem işlevi hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 228e23cd94f6d903af63e59ba0333d78bd5eacfd
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "93341731"
---
# <a name="index_of-azure-cosmos-db"></a>INDEX_OF (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 İlk belirtilen dize ifadesinde ikinci dize ifadesinin ilk örneğinin başlangıç konumunu döndürür veya dize bulunamazsa-1.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
INDEX_OF(<str_expr1>, <str_expr2> [, <numeric_expr>])  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*str_expr1*  
   , Aranacak dize ifadesidir.  
  
*str_expr2*  
   , Aranacak dize ifadesidir.  

*numeric_expr* Aramanın başlayacağı konumu ayarlayan isteğe bağlı sayısal ifade. *Str_expr1* ilk konum 0 ' dır. 
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, "abc" içindeki çeşitli alt dizelerin dizinini döndürür.  
  
```sql
SELECT INDEX_OF("abc", "ab") AS i1, INDEX_OF("abc", "b") AS i2, INDEX_OF("abc", "c") AS i3 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"i1": 0, "i2": 1, "i3": -1}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
