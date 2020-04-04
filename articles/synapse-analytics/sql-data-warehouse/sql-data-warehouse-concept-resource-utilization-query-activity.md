---
title: Yönetilebilirlik ve izleme - sorgu etkinliği, kaynak kullanımı
description: Azure Synapse Analytics'i yönetmek ve izlemek için hangi özelliklerin kullanılabilip kullanılabilmelerini öğrenin. Sorgu etkinliğini ve veri ambarınızın kaynak kullanımını anlamak için Azure portalını ve Dinamik Yönetim Görünümlerini (DMV) kullanın.
services: synapse-analytics
author: kevinvngo
manager: craigg-msft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/11/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 61c823aee5d8a6755a3bd7971cd7cca952cbb911
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633611"
---
# <a name="monitoring-resource-utilization-and-query-activity-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te kaynak kullanımı ve sorgu etkinliğini izleme

Azure Synapse Analytics, veri ambarı iş yükünüzle ilgili yüzey öngörülerine Azure portalı içinde zengin bir izleme deneyimi sağlar. Azure portalı, ölçümler ve günlükler için yapılandırılabilir bekletme süreleri, uyarılar, öneriler ve özelleştirilebilir grafikler ve panolar sağladığı için veri ambarınızı izlerken önerilen araçtır. Portal ayrıca, yalnızca veri ambarınız için değil, tüm Azure analiz platformunuz için tüm Azure analiz platformunuz için tümsel bir izleme deneyimi sağlamak için Azure Monitor (günlükler) gibi diğer Azure izleme hizmetleriyle log analitiği ile tümleştirmenize olanak tanır. Bu dokümantasyon, SQL Analytics ile analiz platformunuzu optimize etmek ve yönetmek için hangi izleme yeteneklerinin kullanılabillerini açıklar.

## <a name="resource-utilization"></a>Kaynak kullanımı

Aşağıdaki ölçümler SQL Analytics için Azure portalında kullanılabilir. Bu ölçümler Azure [Monitor](../../azure-monitor/platform/data-collection.md?toc=/azure/synapse-analytics/sql-data-warehouse?toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#metrics)aracılığıyla su yüzüne çıkar.

| Metrik Adı             | Açıklama                                                  | Toplama Türü |
| ----------------------- | ------------------------------------------------------------ | ---------------- |
| CPU yüzdesi          | Veri ambarı için tüm düğümlerde CPU kullanımı      | Avg, Min, Max    |
| Veri G/Ç yüzdesi      | Veri ambarı için tüm düğümlerde IO Kullanımı       | Avg, Min, Max    |
| Bellek yüzdesi       | Veri ambarı için tüm düğümlerde bellek kullanımı (SQL Server) | Avg, Min, Max   |
| Etkin Sorgular          | Sistemde çalıştırılabilen etkin sorgu sayısı             | Toplam              |
| Sıralı Sorgular          | Yürütmeyi başlatmak için bekleyen sıralı sorgu ların sayısı          | Toplam              |
| Başarılı Bağlantılar  | Verilere başarılı bağlantı sayısı                 | Toplam, Sayma       |
| Başarısız Bağlantılar      | Veri ambarına başarısız bağlantı sayısı           | Toplam, Sayma       |
| Güvenlik Duvarı tarafından engellendi     | Engellenen veri ambarına giriş sayısı     | Toplam, Sayma       |
| DWU sınırı               | Veri ambarının hizmet düzeyi hedefi                | Avg, Min, Max    |
| DWU yüzdesi          | CPU yüzdesi ile Veri IO yüzdesi arasında maksimum        | Avg, Min, Max    |
| Kullanılan DWU                | DWU sınırı * DWU yüzdesi                                   | Avg, Min, Max    |
| Önbellek isabet yüzdesi    | (önbellek isabetleri / önbellek miss) * 100 nerede önbellek vurur yerel SSD önbellek ve önbellek miss tüm sütun mağaza segmentleri isabet toplamıdır yerel SSD önbelleğinde kalır sütun mağazası segmentleri tüm düğümleri arasında özetlenebilir | Avg, Min, Max    |
| Önbellek kullanılan yüzde   | (kullanılan önbellek / önbellek kapasitesi) * Kullanılan önbelleğin tüm düğümler ve önbellek kapasitesi boyunca yerel SSD önbelleğindeki tüm baytların toplamı olduğu 100, tüm düğümler arasında yerel SSD önbelleğinin depolama kapasitesinin toplamıdır | Avg, Min, Max    |
| Yerel tempdb yüzdesi | Tüm işlem düğümleri arasında yerel tempdb kullanımı - değerler her beş dakikada bir yayılır | Avg, Min, Max    |
| Veri Depolama Boyutu (GB) | Veritabanına yüklenen toplam veri boyutu. Bu, CCI olmayan tabloların boyutunun toplam veritabanı dosya boyutuyla ölçüldüğü CCI ve CCI olmayan tablolarda bulunan verileri içerir | Toplam |
| Olağanüstü Durum Kurtarma Boyutu (GB) | Her 24 saatte bir alınan coğrafi yedeklemenin toplam boyutu | Toplam |
| Anlık Görüntü Depolama boyutu (GB) | Veritabanı geri yükleme noktaları sağlamak için alınan anlık görüntü toplam boyutu. Bu, otomatik ve kullanıcı tanımlı anlık görüntüleri içerir. | Toplam |

Ölçümleri görüntülerken ve uyarıları ayarlarken göz önünde bulundurulması gerekenler:

- Kullanılan DWU, SQL havuzunda yalnızca üst düzey bir **kullanım temsilini** temsil eder ve kapsamlı bir kullanım göstergesi değildir. Yukarı veya aşağı ölçeklendirmek için, eşzamanlılık, bellek, tempdb ve uyarlanabilir önbellek kapasitesi gibi DWU tarafından etkilenebilir tüm faktörleri göz önünde bulundurun. İş yükünüzü, iş hedeflerinize ulaşmak için en iyi neyin işe yaradığını belirlemek için [farklı DWU ayarlarında çalıştırmanızı](sql-data-warehouse-manage-compute-overview.md#finding-the-right-size-of-data-warehouse-units) öneririz.
- Belirli bir veri ambarı için başarısız ve başarılı bağlantılar bildirilir - mantıksal sunucu için değil
- Bellek yüzdesi, veri ambarı boşta olsa bile kullanımı yansıtır - etkin iş yükü bellek tüketimini yansıtmaz. Ek önbellek kapasitesi için ölçeklemenin gereksinimlerinizi karşılamak için iş yükü performansını artırıp artıracağına ilişkin bütünsel bir karar vermek için bu ölçütünüzü başkalarıyla birlikte (tempdb, gen2 önbelleği) kullanın ve izleyin.

## <a name="query-activity"></a>Sorgu etkinliği

T-SQL üzerinden SQL Analytics'i izlerken programlı bir deneyim için, hizmet dinamik yönetim görünümleri (DMV) kümesi sağlar. Bu görünümler, iş yükünüzle ilgili performans sorunlarını etkin olarak giderirken ve tanımlarken yararlıdır.

SQL Analytics'in sağladığı DMV listesini görüntülemek için bu [belgelere](sql-data-warehouse-reference-tsql-system-views.md#sql-data-warehouse-dynamic-management-views-dmvs)bakın.

## <a name="metrics-and-diagnostics-logging"></a>Ölçümler ve tanılama günlükleri

Hem ölçümler hem de günlükler Azure Monitor'a, özellikle [Azure Monitor günlükleri](../../azure-monitor/log-query/log-query-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) bileşenine ihraç edilebilir ve [günlük sorguları](../../azure-monitor/log-query/get-started-portal.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)aracılığıyla programlanabilir. SQL Analytics için günlük gecikmesi yaklaşık 10-15 dakikadır. Gecikme gecikmesini etkileyen faktörler hakkında daha fazla bilgi için aşağıdaki belgeleri ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki Nasıl Yap'ı kullanma kılavuzları, veri ambarınızı izlerken ve yönetirken sık karşılaşılan senaryoları ve kullanım durumlarını açıklar:

- [Veri ambarı iş yükünüzü DMV'lerle izleme](sql-data-warehouse-manage-monitor.md)
