---
title: İzleme-MySQL için Azure veritabanı
description: Bu makalede CPU, depolama ve bağlantı istatistikleri gibi MySQL için Azure veritabanı için izleme ve uyarı verme ölçümleri açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: ec99db9406c5c83cdcbf322c45cea40c6643ee8f
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74770892"
---
# <a name="monitoring-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda izleme
Sunucularınız hakkındaki izleme verileri, iş yükünüz için sorun gidermenize ve iyileştirmenize yardımcı olur. MySQL için Azure veritabanı, sunucunuzun davranışına ilişkin Öngörüler sağlayan çeşitli ölçümler sağlar.

## <a name="metrics"></a>Ölçümler
Tüm Azure ölçümlerinin bir dakikalık sıklığı vardır ve her ölçüm 30 gün geçmiş sağlar. Ölçümler üzerinde uyarılar yapılandırabilirsiniz. Adım adım yönergeler için bkz. [uyarıları ayarlama](howto-alert-on-metric.md). Diğer görevler otomatik eylemleri ayarlamayı, gelişmiş analiz gerçekleştirmeyi ve arşivleme geçmişini içerir. Daha fazla bilgi için bkz. [Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Ölçüm listesi
MySQL için Azure veritabanı 'nda bu ölçümler mevcuttur:

|Ölçüm|Ölçüm görünen adı|Birim|Açıklama|
|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Kullanımdaki CPU yüzdesi.|
|memory_percent|Bellek yüzdesi|Yüzde|Kullanımdaki belleğin yüzdesi.|
|io_consumption_percent|GÇ yüzdesi|Yüzde|Kullanımdaki GÇ yüzdesi.|
|storage_percent|Depolama yüzdesi|Yüzde|Sunucunun en yüksek sınırının dışında kullanılan depolama alanı yüzdesi.|
|storage_used|Kullanılan depolama|Sayacının|Kullanımdaki depolama miktarı. Hizmet tarafından kullanılan depolama alanı, veritabanı dosyalarını, işlem günlüklerini ve sunucu günlüklerini içerebilir.|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Yüzde|Sunucunun en yüksek sunucu günlük depolama alanı dışında kullanılan sunucu günlük depolama alanı yüzdesi.|
|serverlog_storage_usage|Kullanılan sunucu günlüğü depolaması|Sayacının|Kullanımdaki sunucu günlüğü depolama miktarı.|
|serverlog_storage_limit|Sunucu günlüğü depolama sınırı|Sayacının|Bu sunucu için en fazla sunucu günlük depolama alanı.|
|storage_limit|Depolama sınırı|Sayacının|Bu sunucu için en fazla depolama alanı.|
|active_connections|Etkin bağlantılar|Sayı|Sunucuya etkin bağlantı sayısı.|
|connections_failed|Başarısız Bağlantılar|Sayı|Sunucuya yönelik başarısız bağlantı sayısı.|
|seconds_behind_master|Saniye cinsinden çoğaltma gecikmesi|Sayı|Çoğaltma sunucusunun ana sunucuya yönelik olarak geçmesi gereken saniye sayısı.|
|network_bytes_egress|Ağ Çıkışı|Sayacının|Etkin bağlantılar arasında ağ çıkışı.|
|network_bytes_ingress|Ağ Girişi|Sayacının|Etkin bağlantılar genelinde ağ.|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Sayacının|Kullanılan yedekleme depolama alanı miktarı.|

## <a name="server-logs"></a>Sunucu günlükleri
Sunucunuzda yavaş sorgu ve denetim günlüğünü etkinleştirebilirsiniz. Bu Günlükler Azure Izleyici günlükleri, Event Hubs ve depolama hesabı 'ndaki Azure tanılama günlükleri aracılığıyla da kullanılabilir. Günlüğe kaydetme hakkında daha fazla bilgi edinmek için [Denetim günlükleri](concepts-audit-logs.md) ' ni ve [yavaş sorgu günlüğü](concepts-server-logs.md) makalelerini ziyaret edin.

## <a name="query-store"></a>Sorgu Deposu
[Sorgu deposu](concepts-query-store.md) sorgu çalışma zamanı istatistikleri ve bekleme olayları dahil olmak üzere zaman içinde sorgu performansını izlemeyi tutan bir özelliktir. Özellik, **MySQL** şemasında çalışma zamanı performans bilgilerini sorgu ile devam ettirir. Verilerin toplanmasını ve depolanmasını çeşitli yapılandırma düğmelerini kullanarak denetleyebilirsiniz.

## <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri
[Sorgu performansı içgörüleri](concepts-query-performance-insight.md) , Azure Portal erişilebilir görselleştirmeler sağlamak üzere sorgu deposuyla birlikte çalışarak. Bu grafikler, performansı etkileyen anahtar sorgularını tanımlamanızı sağlar. Sorgu Performansı İçgörüleri, MySQL için Azure veritabanı sunucunuzun portal sayfasındaki **akıllı performans** bölümünde erişilebilir.

## <a name="performance-recommendations"></a>Performans Önerileri
[Performans önerileri](concepts-performance-recommendations.md) özelliği, iş yükü performansını artırmaya yönelik fırsatları tanımlar. Performans önerileri, iş yüklerinizin performansını geliştirmek için potansiyel olarak yeni dizinler oluşturmaya yönelik öneriler sağlar. Dizin önerileri oluşturmak için, özelliği şema ve sorgu deposu tarafından bildirilen iş yükü dahil çeşitli veritabanı özelliklerini dikkate alır. Herhangi bir performans önerisi uygulandıktan sonra, müşteriler bu değişikliklerin etkisini değerlendirmek için performansı test etmelidir.

## <a name="service-health"></a>Hizmet durumu
[Azure hizmet durumu](../service-health/overview.md) , aboneliğinizdeki tüm hizmet durumu bildirimlerinin bir görünümünü sağlar. Kullandığınız Azure hizmetlerini ve bölgelerini etkileyebilecek sorunlar veya değişiklikler olduğunda tercih ettiğiniz iletişim kanallarınız aracılığıyla sizi bilgilendirmek üzere hizmet durumu uyarıları ayarlayabilirsiniz.

**Planlı bakım** olay türünü kullanarak MySQL Için Azure veritabanı için zamanlanmış bakım olaylarını görüntüleyebilirsiniz. **Hizmet durumu uyarıları**oluşturma hakkında bilgi edinmek için [hizmet bildirimleri üzerinde etkinlik günlüğü uyarıları oluştur](../service-health/alerts-activity-log-service-notifications.md) makalesini ziyaret edin.

> [!IMPORTANT]
> Planlı bakım bildirimleri yalnızca Doğu ABD ve UK Güney önizleme sürümünde kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
- Bir ölçüm üzerinde uyarı oluşturma konusunda rehberlik için [uyarıları ayarlama](howto-alert-on-metric.md) bölümüne bakın.
- Azure portal, REST API veya CLı kullanarak ölçümlere erişme ve dışarı aktarma hakkında daha fazla bilgi için bkz. [Azure ölçümleri 'Ne genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
- [Sunucunuzu izlemek için en iyi uygulamalardan](https://azure.microsoft.com/blog/best-practices-for-alerting-on-metrics-with-azure-database-for-mysql-monitoring/)blogumuzu okuyun.
