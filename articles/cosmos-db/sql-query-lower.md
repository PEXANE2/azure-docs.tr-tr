---
title: Azure Cosmos DB sorgu dilinde daha düşük
description: Büyük harfli karakter verilerini küçük harfe dönüştürdükten sonra bir dize ifadesi döndürmek için Azure Cosmos DB alt SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 163b893b74b5f6d3950ae92e209f96abde8cd90e
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93075288"
---
# <a name="lower-azure-cosmos-db"></a>DAHA düşük (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Büyük harfli karakter verilerini küçük harfe dönüştürdükten sonra bir dize ifadesi döndürür.  

ALT sistem işlevi dizin kullanmaz. Büyük/küçük harfe duyarsız karşılaştırmalar yapmak için plan yaparsanız, alt sistem işlevi önemli miktarda RU 'yı kullanabilir. Bu durumda, karşılaştırmalar için her seferinde verileri normalleştirmek için alt sistem işlevini kullanmak yerine, ekleme sırasında büyük/küçük harfleri normalleştirin. Ardından, SELECT * FROM c, LOWER (c. Name) = ' Bob ' gibi bir sorgu yalnızca c.name = ' Bob ' olarak SELECT * FROM c.

## <a name="syntax"></a>Söz dizimi
  
```sql
LOWER(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*str_expr*  
   Bir dize ifadesidir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir dize ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, bir sorguda nasıl kullanılacağını gösterir `LOWER` .  
  
```sql
SELECT LOWER("Abc") AS lower
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"lower": "abc"}]  
  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
