---
title: Azure Cosmos DB sorgu dilinde yuvarla
description: Azure Cosmos DB SQL sistem işlevi YUVARLA hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: b6aac5a963d0f58a3b21b9fb0958793169a3d444
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "78302126"
---
# <a name="round-azure-cosmos-db"></a>YUVARLAK (Azure Cosmos DB)
 En yakın tamsayı değerine yuvarlanmış sayısal bir değer döndürür.  
  
## <a name="syntax"></a>Söz dizimi
  
```sql
ROUND(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="remarks"></a>Açıklamalar
  
  Yuvarlama işlemi, sıfırdan uzağa yuvarlanan orta nokta yuvarlama işlemini gerçekleştirdi. Giriş iki tamsayı arasında tam olarak denk gelen sayısal bir ifadesiyse sonuç, sıfırdan bir en yakın tamsayı değeri olacaktır.  
  
  |<numeric_expr>|Yuvarlanır|
  |-|-|
  |-6,5000|-7|
  |-0,5|-1|
  |0,5|1|
  |6,5000|7||
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, aşağıdaki pozitif ve negatif sayıları en yakın tamsayıya yuvarlar.  
  
```sql
SELECT ROUND(2.4) AS r1, ROUND(2.6) AS r2, ROUND(2.5) AS r3, ROUND(-2.4) AS r4, ROUND(-2.6) AS r5  
```  
  
  Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{r1: 2, r2: 3, r3: 3, r4: -2, r5: -3}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi, bir [Aralık dizininden](index-policy.md#includeexclude-strategy)faydalanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
