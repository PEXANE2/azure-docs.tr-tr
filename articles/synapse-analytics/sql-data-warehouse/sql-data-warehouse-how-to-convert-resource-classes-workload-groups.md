---
title: Kaynak sınıfLarını iş yükü grubuna dönüştürme
description: Azure SQL Veri Ambarı'ndaki kaynak sınıfına benzer bir iş yükü grubu oluşturmayı öğrenin.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.subservice: ''
ms.topic: conceptual
ms.date: 11/4/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 8cee874106598c7d81b923d7dd32ba91902d9326
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80745180"
---
# <a name="convert-resource-classes-to-workload-groups"></a>Kaynak Sınıflarını İş Yükü Gruplarına Dönüştürme

İş yükü grupları, sistem kaynaklarını yalıtmak ve içeren bir mekanizma sağlar.  Ayrıca, iş yükü grupları, içinde çalışan istekler için yürütme kuralları belirlemenize olanak tanır.  Sorgu zaman ekme yürütme kuralı, kaçak sorguların kullanıcı müdahalesi olmadan iptal edilmesine olanak tanır.  Bu makalede, varolan bir kaynak sınıfının nasıl alınır ve benzer bir yapılandırmaya sahip bir iş yükü grubu nasıl oluşturulur.  Ayrıca, isteğe bağlı bir sorgu zaman alakart kuralı eklenir.

## <a name="understanding-the-existing-resource-class-configuration"></a>Varolan kaynak sınıfı yapılandırmasını anlama

İş yükü grupları, `REQUEST_MIN_RESOURCE_GRANT_PERCENT` istek başına ayrılan genel sistem kaynaklarının yüzdesini belirten bir parametre gerektirir.  Kaynak ayırma, [eşzamanlılık](resource-classes-for-workload-management.md#what-are-resource-classes) yuvaları ayrılarak kaynak sınıfları için yapılır.  Belirtecek değeri belirlemek `REQUEST_MIN_RESOURCE_GRANT_PERCENT`için sys.dm_workload_management_workload_groups_stats <link tbd> DMV'yi kullanın.  Örneğin, aşağıdaki sorgu sorgusu statikrc40 benzer `REQUEST_MIN_RESOURCE_GRANT_PERCENT` bir iş yükü grubu oluşturmak için parametre için kullanılabilecek bir değer döndürür.

```sql
SELECT Request_min_resource_grant_percent = Effective_request_min_resource_grant_percent
  FROM sys.dm_workload_management_workload_groups_stats
  WHERE name = 'staticrc40'
```

> [!NOTE]
> İş yükü grupları, genel sistem kaynaklarının yüzdesini temel alınca çalışır.  

İş yükü grupları, siz ölçeklendikçe ve küçültttünkçe, genel sistem kaynakları değişikliklerine göre statik kaynak sınıflarına ayrılan kaynakların yüzdesi genel sistem kaynaklarının yüzdesine göre çalışır.  Örneğin, DW1000c'deki staticrc40 toplam sistem kaynaklarının %9,6'sını ayırır.  DW2000c'de %19,2'si ayrılmıştır.  Bu model, istek başına daha fazla kaynak ayırmayerine eşzamanlılık için ölçeklendirmek istiyorsanız benzer.

## <a name="create-workload-group"></a>İş Yükü Grubu Oluşturma

Bilinen `REQUEST_MIN_RESOURCE_GRANT_PERCENT`, iş yükü grubu oluşturmak <link> için İş YÜKÜ GRUBU sözdizimini kullanabilirsiniz.  İş yükü grubu `MIN_PERCENTAGE_RESOURCE` için kaynakları yalıtmak için isteğe bağlı olarak sıfırdan büyük bir kaynak belirtebilirsiniz.  Ayrıca, iş yükü `CAP_PERCENTAGE_RESOURCE` grubunun tüketebileceği kaynak miktarını sınırlamak için isteğe bağlı olarak 100'den az belirtebilirsiniz.  

Aşağıdaki örnekte, `MIN_PERCENTAGE_RESOURCE` sistem kaynaklarının %9,6'sını `wgDataLoads` ayıracağı ve bir sorgunun her zaman çalıştırılabilme garantisi verilmiştir.  Ayrıca, `CAP_PERCENTAGE_RESOURCE` %38,4 olarak ayarlanır ve bu iş yükü grubunu dört eşzamanlı istekle sınırlar.  `QUERY_EXECUTION_TIMEOUT_SEC` Parametre 3600 olarak ayarlayarak, 1 saatten fazla çalışan tüm sorgu otomatik olarak iptal edilir.

```sql
CREATE WORKLOAD GROUP wgDataLoads WITH  
( REQUEST_MIN_RESOURCE_GRANT_PERCENT = 9.6
 ,MIN_PERCENTAGE_RESOURCE = 9.6
 ,CAP_PERCENTAGE_RESOURCE = 38.4
 ,QUERY_EXECUTION_TIMEOUT_SEC = 3600)
```

## <a name="create-the-classifier"></a>Sınıflandırıcı yı oluşturma

Daha önce, kaynak sınıflarına sorguların eşleme [sp_addrolemember](resource-classes-for-workload-management.md#change-a-users-resource-class)ile yapıldı.  Aynı işlevselliği elde etmek ve istekleri iş yükü gruplarına eşlemek için [CREATE WORKLOAD CLASSIFIER](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) sözdizimini kullanın.  sp_addrolemember kullanmak, yalnızca kaynakları bir girişe dayalı bir isteğe eşlemenize olanak sağlar.  Bir sınıflandırıcı, oturum açmanın yanı sıra şunları gibi ek seçenekler de sağlar:
    - etiket
    - oturum
    - zaman Aşağıdaki örnek, oturum açma `AdfLogin` dan yukarıda oluşturulan iş yükü `factloads` grubuna `wgDataLoads` ayarlanmış [OPTION LABEL](sql-data-warehouse-develop-label.md) sorguları atar.

```sql
CREATE WORKLOAD CLASSIFIER wcDataLoads WITH  
( WORKLOAD_GROUP = 'wgDataLoads'
 ,MEMBERNAME = 'AdfLogin'
 ,WLM_LABEL = 'factloads')
```

## <a name="test-with-a-sample-query"></a>Örnek sorguyla test

Aşağıda, iş yükü grubu ve sınıflandırıcının doğru şekilde yapılandırıldığından emin olmak için örnek bir sorgu ve Bir DMV sorgusu yer almaktadır.

```sql
SELECT SUSER_SNAME() --should be 'AdfLogin'

--change to a valid table AdfLogin has access to
SELECT TOP 10 *
  FROM nation
  OPTION (label='factloads')

SELECT request_id, [label], classifier_name, group_name, command
  FROM sys.dm_pdw_exec_requests
  WHERE [label] = 'factloads'
  ORDER BY submit_time DESC
```

## <a name="next-steps"></a>Sonraki adımlar

- [İş Yükü Yalıtımı](sql-data-warehouse-workload-isolation.md)
- [İş Yükü Grubu bağlantısı oluşturma](quickstart-configure-workload-isolation-tsql.md)
