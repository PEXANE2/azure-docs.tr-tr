---
title: Değişkenleri atama
description: Bu makalede, SQL havuzunda T-SQL değişkenleri atamak için gerekli ipuçlarını bulacaksınız.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 2dcf706ea59657abc2718a69e59191604dc2849d
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633418"
---
# <a name="assign-variables-in-synapse-sql-pool"></a>Synapse SQL havuzunda değişken atama

Bu makalede, SQL havuzunda T-SQL değişkenleri atamak için gerekli ipuçlarını bulacaksınız.

## <a name="set-variables-with-declare"></a>DECLARE ile değişkenleri ayarlama

SQL havuzundaki değişkenler `DECLARE` deyim veya `SET` deyim kullanılarak ayarlanır. DEĞIŞKENleri DECLARE ile başlatma, SQL havuzunda değişken değeri ayarlamanın en esnek yollarından biridir.

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

Aynı DECLARE deyiminde bir değişkeni baş harfe ve kullanamazsın. Noktayı göstermek için, hem başharfe @p1 hem de aynı DECLARE deyiminde kullanıldığından aşağıdaki örneğe izin **verilmez.** Bu nedenle, aşağıdaki örnekte bir hata verir:

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="set-values-with-set"></a>SET ile değerleri ayarlama

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

Daha fazla geliştirme ipucu için [geliştirme genel bakış](sql-data-warehouse-overview-develop.md)ına bakın.
