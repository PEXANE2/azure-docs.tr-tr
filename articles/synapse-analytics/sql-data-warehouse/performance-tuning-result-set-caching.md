---
title: Sonuç kümesini önbelleğe ile performans ayarlama
description: Azure Synapse Analytics'teki Synapse SQL havuzu için sonuç kümesi önbelleğe alma özelliğine genel bakış
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 10/10/2019
ms.author: xiaoyul
ms.reviewer: nidejaco;
ms.custom: azure-synapse
ms.openlocfilehash: 4eef8a3a83456a9f2066311b9339b26b83afa009
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633798"
---
# <a name="performance-tuning-with-result-set-caching"></a>Sonuç kümesini önbelleğe ile performans ayarlama

Sonuç kümesi önbelleğe alma etkinleştirildiğinde, SQL Analytics yinelenen kullanım için sorgu sonuçlarını kullanıcı veritabanında otomatik olarak önbelleğe verir.  Bu, sonraki sorgu yürütmelerinin doğrudan kalıcı önbellekten sonuç almalarına olanak tanır, bu nedenle yeniden hesaplama gerekmez.   Sonuç kümesi önbelleğe alma sorgu performansını artırır ve bilgi işlem kaynağı kullanımını azaltır.  Ayrıca, önbelleğe alınan sonuçlar kümesini kullanan sorgular eşzamanlılık yuvaları kullanmaz ve bu nedenle varolan eşzamanlılık sınırlarına dahil edilmez. Güvenlik için, kullanıcılar önbelleğe alınan sonuçlara yalnızca önbelleğe alınan sonuçları oluşturan kullanıcılarla aynı veri erişim izinlerine sahiplerse erişebilirler.  

## <a name="key-commands"></a>Anahtar komutları

[Kullanıcı veritabanı için AÇMA/KAPALI sonuç kümesini açma](/sql/t-sql/statements/alter-database-transact-sql-set-options?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

[Kullanıcı veritabanı için AÇMA/KAPALI sonuç kümesini açma](/sql/t-sql/statements/alter-database-transact-sql-set-options?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

[Oturum için önbelleğe alma ayarını AÇMA/KAPAMA](/sql/t-sql/statements/set-result-set-caching-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

[Önbelleğe alınmış sonuç kümesinin boyutunu denetleme](/sql/t-sql/database-console-commands/dbcc-showresultcachespaceused-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)  

[Önbelleği temizleme](/sql/t-sql/database-console-commands/dbcc-dropresultsetcache-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="whats-not-cached"></a>Önbelleğe alınmayanlar  

Sonuç kümesi önbelleğe alma veritabanı için Açık döndükten sonra, önbellek bu sorgular dışında tüm sorgular için önbelleğe alınır:

- DateTime.Now() gibi deterministik olmayan işlevleri kullanan sorgular
- Kullanıcı tanımlı işlevlerini kullanan sorgular
- Satır düzeyi güvenliği etkin olan tabloları kullanan sorgular veya sütun düzeyi güvenliği etkin
- Satır boyutu 64KB'den büyük olan verileri döndüren sorgular

> [!IMPORTANT]
> Sonuç kümesi önbelleği oluşturmak ve önbellekten veri almak için yapılan işlemler, Synapse SQL havuz örneğinin denetim düğümünde gerçekleşir.
> Sonuç kümesi önbelleğe alma açık olduğunda, büyük sonuç kümesini döndüren sorguları çalıştırmak (örneğin, 1 milyon satır >) denetim düğümünde yüksek CPU kullanımına neden olabilir ve örnekteki genel sorgu yanıtını yavaşlatabilir.  Bu sorgular genellikle veri arama veya ETL işlemleri sırasında kullanılır. Denetim düğümünün vurgulanmasından ve performans sorununa neden olmak için, kullanıcıların bu tür sorguları çalıştırmadan önce veritabanında kapkaç sonucu kümesini açmaları gerekir.  

Bu sorguyu, bir sorgu için önbelleğe alma işlemlerini ayarlayarak sonuç olarak alınan süre için çalıştırın:

```sql
SELECT step_index, operation_type, location_type, status, total_elapsed_time, command
FROM sys.dm_pdw_request_steps
WHERE request_id  = <'request_id'>;
```

Burada, sonuç kümesi devre dışı bırakılmış olarak yürütülen bir sorgu için örnek bir çıktı verilmiştir.

![Sorgu adımları-rsc-devre dışı](./media/performance-tuning-result-set-caching/query-steps-with-rsc-disabled.png)

Burada, sonuç kümesi önbelleğe alma etkinken yürütülen bir sorgu için örnek bir çıktı verilmiştir.

![Rsc etkin sorgu adımları](./media/performance-tuning-result-set-caching/query-steps-with-rsc-enabled.png)

## <a name="when-cached-results-are-used"></a>Önbelleğe alınmış sonuçlar kullanıldığında

Önbelleğe alınmış sonuç kümesi, aşağıdaki gereksinimlerin tümü karşılanırsa sorgu için yeniden kullanılır:

- Sorguyu çalıştıran kullanıcı, sorguda başvurulan tüm tablolara erişebilir.
- Yeni sorgu ile sonuç kümesi önbelleğini oluşturan önceki sorgu arasında tam bir eşleşme vardır.
- Önbelleğe alınan sonuç kümesinin oluşturulduğu tablolarda veri veya şema değişikliği yoktur.

Bir sorgunun bir sonuç önbelleği isabetiyle mi yoksa kaçırArak mı yürütüldeceğini denetlemek için bu komutu çalıştırın. result_set_cache sütunu önbellek isabeti için 1, önbellek kaçlaması için 0 ve sonuç kümesi önbelleğinin neden kullanılmadığı nedenlerden dolayı negatif değerler döndürür. Ayrıntılar için [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kontrol edin.

```sql
SELECT request_id, command, result_set_cache FROM sys.dm_pdw_exec_requests
WHERE request_id = <'Your_Query_Request_ID'>
```

## <a name="manage-cached-results"></a>Önbelleğe alınmış sonuçları yönetme

Sonuç kümesi önbelleğinin maksimum boyutu veritabanı başına 1 TB'dir.  Önbelleğe alınan sonuçlar, temel sorgu verileri değiştiğinde otomatik olarak geçersiz kılınur.  

Önbellek tahliyesi, bu zamanlamayı izleyerek otomatik olarak SQL Analytics tarafından yönetilir:

- Sonuç kümesi kullanılmamışsa veya geçersiz kılınmışsa her 48 saatte bir.
- Sonuç kümesi önbellek maksimum boyuta yaklaştığında.

Kullanıcılar, aşağıdaki seçeneklerden birini kullanarak tüm sonuç kümesi önbelleğini el ile boşaltabilir:

- Veritabanı için sonuç kümesi önbellek özelliğini KAPAT
- Veritabanına bağlıyken DBCC DROPRESULTSETCACHE çalıştırın

Veritabanını duraklatma önbelleğe alınmış sonuç kümesini boşaltmaz.  

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla geliştirme ipucu için [geliştirme genel bakış](sql-data-warehouse-overview-develop.md)ına bakın.
