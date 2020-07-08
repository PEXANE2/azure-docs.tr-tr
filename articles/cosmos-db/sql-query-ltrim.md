---
title: Azure Cosmos DB sorgu dilinde LTRIM
description: Baştaki boşlukları kaldırdıktan sonra bir dize ifadesi döndürmek için Azure Cosmos DB içindeki LTRIM SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 08c069de70684a8562e86963ddb2e84ee889e7cc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78302262"
---
# <a name="ltrim-azure-cosmos-db"></a>LTRIM (Azure Cosmos DB)
 Baştaki boşlukları kaldırdıktan sonra bir dize ifadesi döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
LTRIM(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*str_expr*  
   Bir dize ifadesidir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir dize ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `LTRIM` bir sorgunun içinde nasıl kullanılacağını gösterir.  
  
```sql
SELECT LTRIM("  abc") AS l1, LTRIM("abc") AS l2, LTRIM("abc   ") AS l3 
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{"l1": "abc", "l2": "abc", "l3": "abc   "}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizinden yararlanmayacak.

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
