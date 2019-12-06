---
title: Azure Cosmos DB sorgu dilinde daha düşük
description: Büyük harfli karakter verilerini küçük harfe dönüştürdükten sonra bir dize ifadesi döndürmek için Azure Cosmos DB alt SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8f7cd864d90b8ab17a180a00a7a6d4bb683da9be
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873310"
---
# <a name="lower-azure-cosmos-db"></a>DAHA düşük (Azure Cosmos DB)
 Büyük harf karakter verileri küçük harfe dönüştürmenin sonra bir dize ifadesi döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
LOWER(<str_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*str_expr*  
   Bir dize ifadesidir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Bir dize ifadesi döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, `LOWER` bir sorguda nasıl kullanılacağını gösterir.  
  
```sql
SELECT LOWER("Abc") AS lower
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{"lower": "abc"}]  
  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Dize işlevleri Azure Cosmos DB](sql-query-string-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
