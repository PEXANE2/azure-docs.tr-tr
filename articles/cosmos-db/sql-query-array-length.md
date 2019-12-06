---
title: Azure Cosmos DB sorgu dilinde ARRAY_LENGTH
description: Azure Cosmos DB ' deki dizi uzunluğu SQL sistem işlevinin, belirtilen dizi ifadesinin öğe sayısını nasıl döndürdüğü hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9a8bf33befdd842a2979151fef3d54679ee03de1
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74871780"
---
# <a name="array_length-azure-cosmos-db"></a>ARRAY_LENGTH (Azure Cosmos DB)
 Belirtilen bir dizi ifadesinin öğelerin sayısını döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
ARRAY_LENGTH(<arr_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*arr_expr*  
   Bir dizi ifadesidir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `ARRAY_LENGTH`kullanarak bir dizinin uzunluğunu alma.  
  
```sql
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"]) AS len  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{"len": 3}]  
```  
  

## <a name="next-steps"></a>Sonraki adımlar

- [Dizi işlevleri Azure Cosmos DB](sql-query-array-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
