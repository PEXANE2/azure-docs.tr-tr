---
title: Azure Cosmos DB sorgu dilinde oturum aç
description: Belirtilen sayısal ifadenin doğal logaritmasını döndürmek için Azure Cosmos DB SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: ae7812670da836efa326b9224547e4d1b64374c2
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873293"
---
# <a name="log-azure-cosmos-db"></a>GÜNLÜK (Azure Cosmos DB)
 Belirtilen sayısal ifadenin doğal logaritmasını döndürür.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
LOG (<numeric_expr> [, <base>])  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
*base*  
   Logaritmanın tabanı ayarlayan isteğe bağlı sayısal bağımsız değişken.  
  
## <a name="return-types"></a>Dönüş türleri
  
  Sayısal bir ifade döndürür.  
  
## <a name="remarks"></a>Açıklamalar
  
  Varsayılan olarak, LOG() doğal logaritmasını döndürür. Logaritmanın tabanı, isteğe bağlı temel parametresini kullanarak başka bir değere değiştirebilirsiniz.  
  
  Logaritmanın tabanı için doğal logaritmasını olan **e**burada **e** bir Irrational 2.718281828 için yaklaşık olarak eşit sabittir.  
  
  Üstel bir sayının doğal logaritmasını sayıdır kendisini: günlük (EXP (n)) = n. Ve üstel bir sayının doğal logaritma sayı kendisini: EXP (günlüğü (n)) = n.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnek, bir değişken bildirir ve belirtilen değişkeni (10) logaritmasını döndürür.  
  
```sql
SELECT LOG(10) AS log  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{log: 2.3025850929940459}]  
```  
  
 Aşağıdaki örnek bir sayının üs değeri için `LOG` hesaplar.  
  
```sql
SELECT EXP(LOG(10)) AS expLog  
```  
  
 Sonuç kümesini burada verilmiştir.  
  
```json
[{expLog: 10.000000000000002}]  
```  

## <a name="next-steps"></a>Sonraki adımlar

- [Matematik işlevleri Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem işlevleri Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB giriş](introduction.md)
