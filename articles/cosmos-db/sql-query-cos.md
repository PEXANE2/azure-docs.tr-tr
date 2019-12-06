---
title: Azure Cosmos DB sorgu dilinde COS
description: Azure Cosmos DB içindeki kosinüs (COS) SQL sistem işlevinin, belirtilen ifadede radyan cinsinden belirtilen açıdaki trigonometrik kosinüs değerini nasıl döndürdüğünü öğrenin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 05546ef531059e05a1426a288a48446d63d8e5df
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873412"
---
# <a name="cos-azure-cosmos-db"></a>COS (Azure Cosmos DB)
 Radyan cinsinden belirtilen ifade trigonometrik belirtilen bir açının kosinüsünü döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
COS(<numeric_expr>)  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, belirtilen açının `COS` hesaplar.  
  
```sql
SELECT COS(14.78) AS cos  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{"cos": -0.59946542619465426}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
