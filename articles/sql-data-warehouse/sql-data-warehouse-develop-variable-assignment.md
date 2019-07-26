---
title: Azure SQL veri ambarı 'nda değişken atama | Microsoft Docs
description: Azure SQL veri ambarı 'nda çözüm geliştirmeye yönelik T-SQL değişkenlerini atamaya yönelik ipuçları.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 6c943478f3904aac17a572f012f2b2b69ffa2223
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479556"
---
# <a name="assigning-variables-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı 'nda değişken atama

Azure SQL veri ambarı 'nda çözüm geliştirmeye yönelik T-SQL değişkenlerini atamaya yönelik ipuçları.

## <a name="setting-variables-with-declare"></a>DECLARE ile değişkenleri ayarlama

SQL veri ambarı 'ndaki değişkenler, `DECLARE` `SET` veya ifadesini kullanarak ayarlanır. Değişkenleri DECLARE ile başlatmak, SQL veri ambarı 'nda değişken değer ayarlamak için en esnek yollarla biridir.

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

Aynı DECLARE ifadesinde bir değişkeni başlatamıyor ve kullanamazsınız. Bu@p1 noktayı göstermek için, aynı Declare ifadesinde hem başlatılmış hem de kullanıldığı için aşağıdaki **örneğe izin verilmez** . Aşağıdaki örnek bir hata verir.

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

Değişken atama için GÜNCELLEŞTIRME kullanamazsınız.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için bkz. [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).
