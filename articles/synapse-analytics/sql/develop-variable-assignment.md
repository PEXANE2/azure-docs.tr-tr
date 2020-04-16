---
title: Synapse SQL ile değişken atama
description: Bu makalede, Synapse SQL ile T-SQL değişkenleri atama ipuçları bulacaksınız.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: b2a596b71ee7e5f58e01d5bc10b330f6f54a69d2
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81428673"
---
# <a name="assigning-variables-with-synapse-sql"></a>Synapse SQL ile değişken atama

Bu makalede, Synapse SQL ile T-SQL değişkenleri atama ipuçları bulacaksınız.

## <a name="setting-variables-with-declare"></a>DECLARE ile değişkenleri ayarlama

Synapse SQL'deki değişkenler `DECLARE` deyim veya `SET` deyim kullanılarak ayarlanır. DEĞIŞKENleri DECLARE ile başlatma, Synapse SQL'de değişken değeri belirlemenin en esnek yollarından biridir.

```sql
DECLARE @v  int = 0
;
```

Aynı anda birden fazla değişken ayarlamak için DECLARE'yi de kullanabilirsiniz. Aşağıdakileri yapmak için SELECT veya UPDATE'i kullanamazsınız:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Aynı DECLARE deyiminde bir değişkeni baş harfe ve kullanamazsın. Göstermek için, hem başharfe *@p1* başharfve aynı DECLARE deyiminde kullanıldığından aşağıdaki örneğe izin verilmez. Aşağıdaki örnekte bir hata verir.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>SET ile değerleri ayarlama

SET, tek bir değişkeni ayarlamak için yaygın bir yöntemdir.

Aşağıdaki ifadeler, SET ile bir değişken ayarlamak için geçerli yollardır:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

SET ile aynı anda yalnızca bir değişken ayarlayabilirsiniz. Ancak, bileşik işleçler izin verilir.

## <a name="limitations"></a>Sınırlamalar

Değişken atama için UPDATE kullanamazsınız.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için [Synapse SQL geliştirme genel bakış](develop-overview.md) makalesine bakın.