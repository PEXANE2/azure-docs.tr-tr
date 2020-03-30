---
title: Değişkenleri atama
description: Çözümler geliştirmek için Azure SQL Veri Ambarı'nda T-SQL değişkenleri atamaipuçları.
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
ms.openlocfilehash: 0adcd9bdf92b7ec649b7d91ca0e655fc006b3549
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351656"
---
# <a name="assigning-variables-in-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarında değişken atama

Çözümler geliştirmek için Azure SQL Veri Ambarı'nda T-SQL değişkenleri atamaipuçları.

## <a name="setting-variables-with-declare"></a>DECLARE ile değişkenleri ayarlama

SQL Veri Ambarı'ndaki değişkenler `DECLARE` deyim `SET` veya deyim kullanılarak ayarlanır. DEĞIŞKENleri DECLARE ile başlatma, SQL Veri Ambarı'nda değişken değeri belirlemenin en esnek yollarından biridir.

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

Aynı DECLARE deyiminde bir değişkeni baş harfe ait hale alamaz ve kullanamazsınız. Noktayı göstermek için, hem başharfe @p1 hem de aynı DECLARE deyiminde kullanıldığından aşağıdaki örneğe izin **verilmez.** Aşağıdaki örnekte bir hata verir.

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

Daha fazla geliştirme ipucu için [geliştirme genel bakış](sql-data-warehouse-overview-develop.md)ına bakın.
