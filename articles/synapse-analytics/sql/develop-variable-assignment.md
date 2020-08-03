---
title: SYNAPSE SQL ile değişken atama
description: Bu makalede, T-SQL değişkenlerini SYNAPSE SQL ile atamaya yönelik ipuçları bulacaksınız.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: c3bd04d037f7852bd7d9ea51719abf27f09093b0
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87495684"
---
# <a name="assign-variables-with-synapse-sql"></a>SYNAPSE SQL ile değişken atama

Bu makalede, T-SQL değişkenlerini SYNAPSE SQL ile atamaya yönelik ipuçları bulacaksınız.

## <a name="set-variables-with-declare"></a>DECLARE ile değişkenleri ayarla

SYNAPSE SQL içindeki değişkenler, veya ifadesini kullanarak ayarlanır `DECLARE` `SET` . Değişkenleri DECLARE ile başlatmak, SYNAPSE SQL 'de değişken değer ayarlamak için en esnek yollarla biridir.

```sql
DECLARE @v  int = 0
;
```

Aynı anda birden fazla değişken ayarlamak için BILDIR ' i de kullanabilirsiniz. Şunları yapmak için SELECT veya UPDATE kullanamazsınız:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Aynı DECLARE ifadesinde bir değişkeni başlatamıyor ve kullanamazsınız. Örneğin, * \@ P1* hem başlatılmış hem de aynı Declare ifadesinde kullanıldığı için aşağıdaki örneğe izin verilmez. Aşağıdaki örnek bir hata verir.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="set-values-with-set"></a>Değerleri ayarla ayarla

SET, tek bir değişken ayarlamak için yaygın bir yöntemdir.

Aşağıdaki deyimler, SET ile bir değişken ayarlamaya yönelik tüm geçerli yollardır:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Tek seferde yalnızca bir değişken ayarlayabilirsiniz. Ancak, bileşik işleçlere izin verilir.

## <a name="limitations"></a>Sınırlamalar

Değişken atama için UPDATE kullanamazsınız.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için [SYNAPSE SQL geliştirme genel bakış](develop-overview.md) makalesine bakın.
