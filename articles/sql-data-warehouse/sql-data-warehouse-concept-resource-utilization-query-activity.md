---
title: Yönetilebilirlik ve izleme-sorgu etkinliği, kaynak kullanımı
description: Azure SYNAPSE Analytics 'i yönetmek ve izlemek için kullanabileceğiniz özellikleri öğrenin. Sorgu etkinliğini ve veri ambarınızın kaynak kullanımını anlamak için Azure portal ve dinamik yönetim görünümlerini (DMVs) kullanın.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/04/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 47f142a19ac470fb29e9542941cd94a6b29ce240
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78195932"
---
# <a name="monitoring-resource-utilization-and-query-activity-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te kaynak kullanımını ve sorgu etkinliğini izleme
Azure SYNAPSE Analytics, veri ambarı iş yükünüze yönelik içgörüler için Azure portal içinde zengin bir izleme deneyimi sağlar. Azure portal, veri Ambarınızı izlerken, ölçüm ve Günlükler için yapılandırılabilir bekletme dönemleri, uyarılar, öneriler ve özelleştirilebilir grafikler ve panolar sağladığından önerilen araçtır. Portal ayrıca Operations Management Suite (OMS) ve Azure Izleyici (Günlükler) gibi diğer Azure izleme hizmetleri ile tümleştirmenize olanak tanıdığından, yalnızca veri Ambarınızla değil, tüm Azure analizlerinizi değil de bir bütünsel izleme deneyimi sağlar tümleşik izleme deneyimi için platform. Bu belgelerde, SQL Analytics ile analiz platformunuzu iyileştirmek ve yönetmek için kullanabileceğiniz izleme özellikleri açıklanmaktadır. 

## <a name="resource-utilization"></a>Kaynak kullanımı 
SQL Analytics için Azure portal aşağıdaki ölçümler mevcuttur. Bu ölçümler [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics)aracılığıyla ortaya çıkmış.


| Ölçüm Adı             | Açıklama                                                  | Toplama Türü |
| ----------------------- | ------------------------------------------------------------ | ---------------- |
| CPU yüzdesi          | Veri ambarı için tüm düğümlerde CPU kullanımı      | Ortalama, en düşük, en fazla    |
| Veri G/Ç yüzdesi      | Veri ambarı için tüm düğümlerde GÇ kullanımı       | Ortalama, en düşük, en fazla    |
| Bellek yüzdesi       | Veri ambarı için tüm düğümlerde bellek kullanımı (SQL Server) | Ortalama, en düşük, en fazla   |
| Etkin sorgular          | Sistemde yürütülen etkin sorguların sayısı             | Toplam              |
| Kuyruğa alınmış sorgular          | Yürütülmeye başlamak için bekleyen sıraya alınmış sorgu sayısı          | Toplam              |
| Başarılı bağlantılar  | Verilere yönelik başarılı bağlantı sayısı                 | Toplam, sayı       |
| Başarısız Bağlantılar      | Veri ambarına yönelik başarısız bağlantı sayısı           | Toplam, sayı       |
| Güvenlik duvarı tarafından engellendi     | Engellenen veri ambarına ait oturum açma sayısı     | Toplam, sayı       |
| DWU sınırı               | Veri ambarının hizmet düzeyi hedefi                | Ortalama, en düşük, en fazla    |
| DWU yüzdesi          | CPU yüzdesi ile veri GÇ yüzdesi arasındaki en fazla        | Ortalama, en düşük, en fazla    |
| Kullanılan DWU                | DWU sınırı * DWU yüzdesi                                   | Ortalama, en düşük, en fazla    |
| İsabetli önbellek okuması yüzdesi    | (önbellek isabet sayısı/önbellek isabetsizliği) * 100, önbellek isabetlerinin yerel SSD önbelleğindeki tüm columnstore segmentlerinin toplamı olduğu ve önbellek isabetsizlik, tüm düğümlerde toplanan yerel SSD önbelleğinde bulunan columnstore kesimlerinin isabetsizliği | Ortalama, en düşük, en fazla    |
| Önbellek kullanılan yüzde   | (kullanılan önbellek/önbellek kapasitesi) * 100, kullanılan önbellek tüm düğümlerde yerel SSD önbelleğindeki tüm baytların toplamı ve önbellek kapasitesi tüm düğümlerde yerel SSD önbelleğinin depolama kapasitesinin toplamıdır | Ortalama, en düşük, en fazla    |
| Yerel tempdb yüzdesi | Tüm işlem düğümlerinde yerel tempdb kullanımı-değerler her beş dakikada bir dağıtılır | Ortalama, en düşük, en fazla    |

Ölçümleri görüntülerken ve uyarıları ayarlarken dikkate alınması gereken noktalar:

- Başarısız ve başarılı bağlantılar, belirli bir veri ambarı için (mantıksal sunucu için değil) raporlanır
- Veri ambarı boşta durumunda olsa bile bellek yüzdesi kullanımı yansıtır; etkin iş yükü bellek tüketimini yansıtmaz. Ek önbellek kapasitesi için ölçeklendirmenin, gereksinimlerinizi karşılamak üzere iş yükü performansını artırabilmesi için bu ölçümü diğer kullanıcılarla birlikte (tempdb, Gen2 Cache) kullanın ve izleyin.


## <a name="query-activity"></a>Sorgu etkinliği
SQL analizini T-SQL aracılığıyla izlerken bir programlama deneyimi için, hizmet bir dizi dinamik yönetim görünümü (DMVs) sağlar. Bu görünümler, iş yükünüzün performans sorunlarını etkin bir şekilde gidermeye ve tanımlamaya yönelik olarak faydalıdır.

SQL Analytics 'in sağladığı DMVs listesini görüntülemek için bu [belgeye](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views#sql-data-warehouse-dynamic-management-views-dmvs)bakın. 

## <a name="metrics-and-diagnostics-logging"></a>Ölçümler ve tanılama günlükleri
Hem ölçümler hem de Günlükler Azure izleyici 'ye, özellikle de [Azure izleyici günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) bileşenine aktarılabilir ve [günlük sorgularıyla](https://docs.microsoft.com/azure/log-analytics/log-analytics-tutorial-viewdata)programlı olarak erişilebilir. SQL Analytics için günlük gecikme süresi yaklaşık 10-15 dakikadır. Gecikme süresini etkileyen faktörler hakkında daha fazla bilgi için aşağıdaki belgeleri ziyaret edin.


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki nasıl yapılır kılavuzlarında, veri Ambarınızı izlerken ve yönetirken yaygın senaryolar ve kullanım örnekleri açıklanır:

- [DMV 'ler ile veri ambarı iş yükünüzü izleme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-monitor)
