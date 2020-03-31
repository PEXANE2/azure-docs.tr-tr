---
title: mySQL için sys_schema yararlanın - Azure Veritabanı
description: MySQL için Azure Veritabanı'nda performans sorunlarını bulmak ve veritabanını korumak için sys_schema nasıl kullanacağınızı öğrenin.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: troubleshooting
ms.date: 3/18/2020
ms.openlocfilehash: a35a586a519ff78e8b32d986b92bd008b2c6b858
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067874"
---
# <a name="how-to-use-sys_schema-for-performance-tuning-and-database-maintenance-in-azure-database-for-mysql"></a>MySQL için Azure Veritabanı'nda performans alamı ve veritabanı bakımı için sys_schema nasıl kullanılır?

MySQL performance_schema, ilk olarak MySQL 5.5'te mevcuttur, bellek ayırma, depolanan programlar, meta veri kilitleme gibi birçok önemli sunucu kaynağı için enstrümantasyon sağlar. Ancak, performance_schema 80'den fazla tablo içerir ve gerekli bilgileri almak genellikle performance_schema tablolar yanı sıra information_schema tablolar katılmayı gerektirir. Hem performance_schema hem de information_schema üzerine inşa edilen sys_schema, yalnızca salt okunur veritabanında [kullanıcı dostu görünümlerden](https://dev.mysql.com/doc/refman/5.7/en/sys-schema-views.html) oluşan güçlü bir koleksiyon sağlar ve MySQL sürüm 5.7 için Azure Veritabanı'nda tam olarak etkinleştirilir.

![sys_schema görüşleri](./media/howto-troubleshoot-sys-schema/sys-schema-views.png)

sys_schema 52 görüntüleme vardır ve her görünümde aşağıdaki öneklerden biri vardır:

- Host_summary veya IO: I/O ile ilgili gecikmeler.
- InnoDB: InnoDB tampon durumu ve kilitleri.
- Bellek: Ana bilgisayar ve kullanıcılar tarafından bellek kullanımı.
- Şema: Otomatik artış, dizinler, vb. gibi şema ile ilgili bilgiler
- Deyim: SQL deyimleri hakkında bilgi; tam tablo tarası veya uzun sorgu süresi ile sonuçlanan ifade olabilir.
- Kullanıcı: Kullanıcılar tarafından tüketilen ve gruplandırılan kaynaklar. Örnekler dosya G/Ç'leri, bağlantılar ve bellektir.
- Bekle: Ana bilgisayar veya kullanıcıya göre gruplanmış olayları bekleyin.

Şimdi sys_schema bazı ortak kullanım kalıplarına bakalım. İlk olarak, kullanım modellerini iki kategoriye ayıracağız: **Performans ve** **Veritabanı bakımı.**

## <a name="performance-tuning"></a>Performans ayarlama

### <a name="sysuser_summary_by_file_io"></a>*sys.user_summary_by_file_io*

IO veritabanındaki en pahalı işlemdir. *Sys.user_summary_by_file_io* görünümünü sorgulayarak ortalama IO gecikmesini bulabiliriz. Varsayılan 125 GB'lık sağlanan depolama alanıyla, IO gecikme syonum yaklaşık 15 saniyedir.

![io gecikme: 125 GB](./media/howto-troubleshoot-sys-schema/io-latency-125GB.png)

MySQL için Azure Veritabanı depolama açısından IO'yu ölçeklendirdiği için, sağlanan depolama alanımı 1 TB'ye artırdıktan sonra, IO gecikme sürem 571 ms'e düşürür.

![io gecikme: 1 TB](./media/howto-troubleshoot-sys-schema/io-latency-1TB.png)

### <a name="sysschema_tables_with_full_table_scans"></a>*sys.schema_tables_with_full_table_scans*

Dikkatli planlamarağmen, birçok sorgu hala tam tablo taramaları neden olabilir. Dizin türleri ve bunları en iyi duruma getirme hakkında ek bilgi için şu makaleye başvurabilirsiniz: [Sorgu performansını nasıl giderebilirsiniz.](./howto-troubleshoot-query-performance.md) Tam tablo taramaları kaynak yoğundur ve veritabanı performansınızı düşürür. Tam tablo tarak tabloları bulmak için en hızlı yolu *sys.schema_tables_with_full_table_scans* görünümünü sorgulamaktır.

![tam masa taramaları](./media/howto-troubleshoot-sys-schema/full-table-scans.png)

### <a name="sysuser_summary_by_statement_type"></a>*sys.user_summary_by_statement_type*

Veritabanı performans sorunlarını gidermek için veritabanınızın içinde meydana gelen olayları tanımlamak yararlı olabilir ve *sys.user_summary_by_statement_type* görünümünü kullanmak işe yarayabilir.

![beyannameye göre özet](./media/howto-troubleshoot-sys-schema/summary-by-statement.png)

Bu örnekte MySQL için Azure Veritabanı, slog sorgu günlüğünü 44579 kez yıkamak için 53 dakika harcadı. Bu uzun bir süre ve birçok iOs. Yavaş sorgu günlüğünüzü devre dışı katarak veya yavaş sorgu girişi Azure portalının sıklığını azaltarak bu etkinliği azaltabilirsiniz.

## <a name="database-maintenance"></a>Veritabanı bakımı

### <a name="sysinnodb_buffer_stats_by_table"></a>*sys.innodb_buffer_stats_by_table*

InnoDB arabellek havuzu bellekte bulunur ve DBMS ve depolama arasındaki ana önbellek mekanizmasıdır. InnoDB arabellek havuzunun boyutu performans katmanına bağlıdır ve farklı bir ürün SKU seçilmedikçe değiştirilemez. İşletim sisteminizdeki bellekte olduğu gibi, eski sayfalar daha taze verilere yer açmak için değiştirilir. Hangi tabloların InnoDB arabellek havuzu belleği en tükettiğinizi bulmak *için, sys.innodb_buffer_stats_by_table* görünümünü sorgulayabilirsiniz.

![InnoDB arabellek durumu](./media/howto-troubleshoot-sys-schema/innodb-buffer-status.png)

Yukarıdaki grafikte, sistem tabloları ve görünümleri dışında, mysqldatabase033 veritabanında her tablo, benim WordPress sitelerinden birini barındıran, 16 KB veya 1 sayfa, bellekte veri kaplar açıktır.

### <a name="sysschema_unused_indexes--sysschema_redundant_indexes"></a>*Sys.schema_unused_indexes* & *sys.schema_redundant_indexes*

Dizinler okuma performansını artırmak için harika araçlardır, ancak ekler ve depolama için ek maliyetler ekleri vardır. *Sys.schema_unused_indexes* ve *sys.schema_redundant_indexes* kullanılmayan veya yinelenen dizinler hakkında bilgi sağlar.

![kullanılmayan dizinler](./media/howto-troubleshoot-sys-schema/unused-indexes.png)

![gereksiz dizinler](./media/howto-troubleshoot-sys-schema/redundant-indexes.png)

## <a name="conclusion"></a>Sonuç

Özetle, sys_schema hem performans atoklama hem de veritabanı bakımı için harika bir araçtır. MySQL için Azure Veritabanınızda bu özelliktan yararlandığından emin olun. 

## <a name="next-steps"></a>Sonraki adımlar
- En ilgili sorularınıza akran yanıtları bulmak veya yeni bir soru/yanıt göndermek için [MSDN forumunu](https://social.msdn.microsoft.com/forums/security/en-US/home?forum=AzureDatabaseforMySQL) veya Stack Overflow'u ziyaret [edin.](https://stackoverflow.com/questions/tagged/azure-database-mysql)
