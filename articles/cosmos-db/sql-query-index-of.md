---
title: Azure Cosmos DB sorgu dilinde INDEX_OF
description: Azure Cosmos DB'de SQL sistem fonksiyonu INDEX_OF hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 50e489fdf57398d486b07944782ecbb3fd1d6a43
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71350994"
---
# <a name="index_of-azure-cosmos-db"></a>INDEX_OF (Azure Cosmos DB)
 İlk belirtilen dize ifadesi içinde ikinci dize ifadesinin ilk oluşumunun başlangıç konumunu döndürür veya dize bulunamazsa -1.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
INDEX_OF(<str_expr1>, <str_expr2> [, <numeric_expr>])  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr1*  
   Aranacak dize ifadesidir.  
  
*str_expr2*  
   Aranacak dize ifadesidir.  

*numeric_expr* Aramanın başlatacağı konumu ayarlayan isteğe bağlı sayısal ifade. *str_expr1'da* ilk pozisyon 0'dır. 
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnekte "abc" içindeki çeşitli alt dizeleri dizini döndürür.  
  
```sql
SELECT INDEX_OF("abc", "ab") AS i1, INDEX_OF("abc", "b") AS i2, INDEX_OF("abc", "c") AS i3 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"i1": 0, "i2": 1, "i3": -1}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [String fonksiyonları Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
