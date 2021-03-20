---
title: Azure Cosmos DB sorgu dilinde RTRIM
description: Azure Cosmos DB 'de SQL sistem işlevi RTRIM hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 0765269fb085c8d4fd8485790772a91c9f2e6373
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93341544"
---
# <a name="rtrim-azure-cosmos-db"></a>RTRIM (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 Sondaki boşlukları kaldırdıktan sonra bir dize ifadesi döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
RTRIM(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*str_expr*  
   Herhangi bir geçerli dize ifadesi.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir dize ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `RTRIM` bir sorgunun içinde nasıl kullanılacağını gösterir.  
  
```sql
SELECT RTRIM("  abc") AS r1, RTRIM("abc") AS r2, RTRIM("abc   ") AS r3  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"r1": "   abc", "r2": "abc", "r3": "abc"}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye giriş](introduction.md)
