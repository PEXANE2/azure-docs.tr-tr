---
title: Yönetilebilirlik ve izleme-sorgu etkinliği, kaynak kullanımı
description: Azure SYNAPSE Analytics 'i yönetmek ve izlemek için kullanabileceğiniz özellikleri öğrenin. Sorgu etkinliğini ve veri ambarınızın kaynak kullanımını anlamak için Azure portal ve dinamik yönetim görünümlerini (DMVs) kullanın.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 03/11/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 82bf6f9a78a46659cc2e0955895c6e1a6e6eb3aa
ms.sourcegitcommit: be53e74cd24bbabfd34597d0dcb5b31d5e7659de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79096632"
---
# <a name="monitoring-resource-utilization-and-query-activity-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te kaynak kullanımını ve sorgu etkinliğini izleme
Azure SYNAPSE Analytics, veri ambarı iş yükünüz ile ilgili öngörülere yönelik Azure portal içinde zengin bir izleme deneyimi sağlar. Azure portal, veri Ambarınızı izlerken, ölçüm ve Günlükler için yapılandırılabilir bekletme dönemleri, uyarılar, öneriler ve özelleştirilebilir grafikler ve panolar sağladığından önerilen araçtır. Portal Ayrıca, Azure Izleyici (Günlükler) gibi diğer Azure izleme hizmetleriyle, Günlük Analizi ile tümleştirmenize olanak sağlar. Ayrıca, yalnızca veri Ambarınızla değil, tümleşik bir Azure Analytics platformunuz izleme deneyimi. Bu belgelerde, SQL Analytics ile analiz platformunuzu iyileştirmek ve yönetmek için kullanabileceğiniz izleme özellikleri açıklanmaktadır. 

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
| Veri depolama boyutu | Veritabanına yüklenen toplam veri boyutu. Bu, CCı 'da bulunan ve CCI tablolarının boyutunun toplam veritabanı dosya boyutuna göre ölçülen verileri içerir | Toplam |
| Olağanüstü durum kurtarma boyutu | Her 24 saatte bir alınan coğrafi yedeklemenin toplam boyutu | Toplam |
| Anlık görüntü depolama boyutu | Veritabanı geri yükleme noktaları sağlamak için alınan anlık görüntülerin toplam boyutu. Buna otomatik ve Kullanıcı tanımlı anlık görüntüler dahildir. | Toplam |

Ölçümleri görüntülerken ve uyarıları ayarlarken dikkate alınması gereken noktalar:

- Kullanılan DWU, SQL havuzundaki **kullanım için yalnızca üst düzey bir temsili** temsil eder ve kapsamlı bir kullanım göstergesi olması anlamına gelir. Ölçeği artırma veya azaltma yapılıp yapılmayacağını anlamak için eşzamanlılık, bellek, tempdb ve Uyarlamalı önbellek kapasitesi gibi DWU tarafından etkilenen tüm faktörleri göz önünde bulundurun. İş hedeflerinizi karşılayacak en iyi şeyi belirlemek için iş [yükünüzü farklı DWU ayarlarında çalıştırmayı](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-manage-compute-overview#finding-the-right-size-of-data-warehouse-units) öneririz.
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
