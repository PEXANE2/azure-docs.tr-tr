---
title: İzleme-MySQL için Azure veritabanı-esnek sunucu
description: Bu makalede, MySQL için Azure veritabanı esnek sunucusu için, CPU, depolama ve bağlantı istatistikleri dahil olmak üzere izleme ve uyarı ölçümleri açıklanmaktadır.
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: conceptual
ms.date: 9/21/2020
ms.openlocfilehash: f3a58a6766c42385dd6611c2014ba36fd1078710
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941630"
---
# <a name="monitor-azure-database-for-mysql-flexible-servers-with-built-in-metrics"></a>Yerleşik ölçümler ile MySQL için Azure veritabanı esnek sunucularını izleme

> [!IMPORTANT] 
> MySQL için Azure veritabanı-esnek sunucu şu anda genel önizlemededir.

MySQL için Azure veritabanı esnek sunucu, Azure Izleyici aracılığıyla sunucuları izlemeyi sağlar. Ölçümler, belirli bir zamanda sunucunuzun kaynaklarının bir bölümünü tanımlayan sayısal değerlerdir. Sunucunuzun kaynaklarını izlemek, en önemli şeyi izlemenize olanak tanıyarak iş yükünüzü sorun gidermenize ve iyileştirmenize yardımcı olur. Doğru ölçümleri izlemek, sunucunuzun ve uygulamalarınızın performansını, güvenilirliğini ve kullanılabilirliğini korumanıza yardımcı olur.

Bu makalede, esnek sunucunuz için, sunucunuzun davranışına ilişkin Öngörüler sağlayan çeşitli ölçümler hakkında bilgi edineceksiniz.

## <a name="available-metrics"></a>Mevcut ölçümler

MySQL için Azure veritabanı esnek sunucu, iş yükünüzün nasıl çalıştığını ve bu verileri temel alarak, sunucu ve uygulamanızın etkisini anlayabilmeniz için çeşitli ölçümler sunar. Örneğin, esnek sunucu 'da, bir performans etkisi olduğunu belirlemek için **ana bilgisayar CPU 'su yüzdesini**, **Etkin bağlantıları**, **GÇ yüzdesini**ve **ana bilgisayar belleğini** izleyebilirsiniz. Buradan, iş yükünüzü en uygun hale getirmenize, işlem katmanlarını değiştirerek veya okuma çoğaltması kullanarak yatay olarak ölçeklendirmeniz gerekebilir.

Tüm Azure ölçümlerinin bir dakikalık sıklığı vardır ve her ölçüm 30 gün geçmiş sağlar. Ölçümler üzerinde uyarılar yapılandırabilirsiniz. Adım adım yönergeler için bkz. [uyarıları ayarlama](./how-to-alert-on-metric.md). Diğer görevler otomatik eylemleri ayarlamayı, gelişmiş analiz gerçekleştirmeyi ve arşivleme geçmişini içerir. Daha fazla bilgi için bkz. [Azure ölçümlerine genel bakış](../../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Ölçüm listesi
MySQL için Azure veritabanı 'nda bu ölçümler mevcuttur:

|Ölçüm görünen adı|Ölçüm|Birim|Açıklama|
|---|---|---|---|
|Ana bilgisayar CPU 'SU yüzdesi|cpu_percent|Yüzde|Hem müşteri iş yüklerinden hem de Azure MySQL işlemlerinden CPU kullanımı dahil olmak üzere, sunucudaki CPU kullanımının yüzdesi|
|Konak ağı |network_bytes_ingress|Bayt|Hem müşteri veritabanından gelen trafik hem de çoğaltma, izleme, Günlükler gibi Azure MySQL özellikleri dahil olmak üzere, sunucudaki gelen ağ trafiği.|
|Ana bilgisayar Ağdan giden|network_bytes_egress|Bayt|Hem müşteri veritabanından gelen trafik hem de çoğaltma, izleme, Günlükler vb. gibi Azure MySQL özellikleri dahil olmak üzere, sunucudaki giden ağ trafiği.|
|Çoğaltma gecikmesi|replication_lag|Saniye|Son yeniden yürütülmüş işlemden bu yana geçen süre. Bu ölçüm yalnızca çoğaltma sunucuları için kullanılabilir.|
|Etkin Bağlantılar|active_connection|Count|Sunucuya etkin bağlantı sayısı.|
|Kullanılan yedekleme depolama alanı|backup_storage_used|Bayt|Kullanılan yedekleme depolama alanı miktarı.|
|GÇ yüzdesi|io_consumption_percent|Yüzde|Kullanımdaki GÇ yüzdesi.|
|Ana bilgisayar belleği yüzdesi|memory_percent|Yüzde|Hem müşteri iş yüklerinden hem de Azure MySQL işlemlerinden bellek kullanımı da dahil olmak üzere, sunucuda kullanılan belleğin yüzdesi|
|Depolama sınırı|storage_limit|Bayt|Bu sunucu için en fazla depolama alanı.|
|Depolama alanı yüzdesi|storage_percent|Yüzde|Sunucunun en yüksek sınırının dışında kullanılan depolama alanı yüzdesi.|
|Kullanılan depolama alanı|storage_used|Bayt|Kullanımdaki depolama miktarı. Hizmet tarafından kullanılan depolama alanı, veritabanı dosyalarını, işlem günlüklerini ve sunucu günlüklerini içerebilir.|
|Toplam bağlantı sayısı|total_connections|Count|Sunucuya toplam bağlantı sayısı|
|Durdurulan Bağlantılar|aborted_connections|Count|MySQL 'e bağlanmak için başarısız girişim sayısı, örneğin hatalı kimlik bilgileri nedeniyle bağlantı başarısız oldu.|
|Sorgular|lardır|Count|Saniye başına sorgu sayısı|

## <a name="next-steps"></a>Sonraki adımlar
- Bir ölçüm üzerinde uyarı oluşturma konusunda rehberlik için [uyarıları ayarlama](./how-to-alert-on-metric.md) bölümüne bakın.
- Performansı artırmak için [IOPS ölçekleme](./concepts/../concepts-compute-storage.md#iops) hakkında daha fazla bilgi edinin.
