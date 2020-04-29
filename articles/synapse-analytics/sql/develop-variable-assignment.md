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
ms.openlocfilehash: b2a596b71ee7e5f58e01d5bc10b330f6f54a69d2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81428673"
---
# <a name="assigning-variables-with-synapse-sql"></a>SYNAPSE SQL ile değişken atama

Bu makalede, T-SQL değişkenlerini SYNAPSE SQL ile atamaya yönelik ipuçları bulacaksınız.

## <a name="setting-variables-with-declare"></a>DECLARE ile değişkenleri ayarlama

SYNAPSE SQL içindeki değişkenler, `DECLARE` veya `SET` ifadesini kullanarak ayarlanır. Değişkenleri DECLARE ile başlatmak, SYNAPSE SQL 'de değişken değer ayarlamak için en esnek yollarla biridir.

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

Aynı DECLARE ifadesinde bir değişkeni başlatamıyor ve kullanamazsınız. Göstermek için, aynı DECLARE ifadesinde hem başlatılmış hem *@p1* de kullanıldığı için aşağıdaki örneğe izin verilmez. Aşağıdaki örnek bir hata verir.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Değerleri ayarla olarak ayarlama

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