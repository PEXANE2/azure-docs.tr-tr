---
title: Değişken atama
description: Bu makalede, SQL havuzunda T-SQL değişkenlerini atamaya yönelik temel ipuçları bulacaksınız.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 0cbadc10fb817c70bd259397c840aae68abc2d54
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85213338"
---
# <a name="assign-variables-in-synapse-sql-pool"></a>SYNAPSE SQL havuzunda değişken atama

Bu makalede, SQL havuzunda T-SQL değişkenlerini atamaya yönelik temel ipuçları bulacaksınız.

## <a name="set-variables-with-declare"></a>DECLARE ile değişkenleri ayarla

SQL havuzundaki değişkenler, `DECLARE` ifadesini veya ifadesini kullanarak ayarlanır `SET` . Değişkenleri DECLARE ile başlatmak, SQL havuzunda değişken değer ayarlamak için en esnek yollarla biridir.

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

Aynı DECLARE ifadesinde bir değişkeni başlatamıyor ve kullanamazsınız. Bu noktayı göstermek için, **not** @p1 aynı Declare ifadesinde hem başlatılmış hem de kullanıldığı için aşağıdaki örneğe izin verilmez. Bu nedenle, aşağıdaki örnek bir hata verir:

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

Daha fazla geliştirme ipucu için bkz. [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).
