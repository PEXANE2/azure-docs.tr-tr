---
title: Azure Cosmos DB sorgu dilinde Gİrİş YAP
description: Belirtilen sayısal ifadenin doğal logaritmasını döndürmek için Azure Cosmos DB'deki LOG SQL sistem işlevi hakkında bilgi edinin
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 9213ef03f383dec7109652246411fac154b4a7f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302517"
---
# <a name="log-azure-cosmos-db"></a>GİR (Azure Cosmos DB)
 Belirtilen sayısal ifadenin doğal logaritmasını verir.  
  
## <a name="syntax"></a>Sözdizimi
  
```sql
LOG (<numeric_expr> [, <base>])  
```  
  
## <a name="arguments"></a>Bağımsız Değişkenler
  
*numeric_expr*  
   Sayısal bir ifadedir.  
  
*base*  
   Logaritma için temel ayarlar isteğe bağlı sayısal bağımsız değişken.  
  
## <a name="return-types"></a>İade türleri
  
  Sayısal bir ifade verir.  
  
## <a name="remarks"></a>Açıklamalar
  
  Varsayılan olarak, LOG() doğal logaritma döndürür. İsteğe bağlı temel parametreyi kullanarak logaritmiktabanını başka bir değerle değiştirebilirsiniz.  
  
  Doğal logaritma temel **e**logaritma , **e** yaklaşık 2.718281828 eşit irrasyonel bir sabittir.  
  
  Bir sayının üstel kısmının doğal logaritması sayının kendisidir: LOG (EXP(n ) ) = n. Ve bir sayının doğal logaritma üslü sayının kendisidir: EXP (LOG(n ) ) = n.  
  
## <a name="examples"></a>Örnekler
  
  Aşağıdaki örnekbir değişken bildirir ve belirtilen değişkenin logaritma değerini döndürür (10).  
  
```sql
SELECT LOG(10) AS log  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{log: 2.3025850929940459}]  
```  
  
 Aşağıdaki örnek, `LOG` bir sayının üsiçin hesaplar.  
  
```sql
SELECT EXP(LOG(10)) AS expLog  
```  
  
 Sonuç kümesini burada bulabilirsiniz.  
  
```json
[{expLog: 10.000000000000002}]  
```  

## <a name="remarks"></a>Açıklamalar

Bu sistem işlevi dizini kullanmaz.

## <a name="next-steps"></a>Sonraki adımlar

- [Matematiksel fonksiyonlar Azure Cosmos DB](sql-query-mathematical-functions.md)
- [Sistem fonksiyonları Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB'ye Giriş](introduction.md)
