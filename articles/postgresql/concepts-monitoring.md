---
title: İzleme ve ayarlama-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede PostgreSQL için Azure veritabanı-tek sunucu 'da izleme ve ayarlama özellikleri açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/19/2019
ms.openlocfilehash: 283ffdd32dbb5b2c80106da98b846ab81aca9608
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74768563"
---
# <a name="monitor-and-tune-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nı izleme ve ayarlama-tek sunucu
Sunucularınız hakkındaki izleme verileri, iş yükünüz için sorun gidermenize ve iyileştirmenize yardımcı olur. PostgreSQL için Azure veritabanı, sunucunuzun davranışına ilişkin Öngörüler sağlamak üzere çeşitli izleme seçenekleri sunar.

## <a name="metrics"></a>Ölçümler
PostgreSQL için Azure veritabanı, PostgreSQL sunucusunu destekleyen kaynakların davranışına fikir veren çeşitli ölçümler sağlar. Her ölçüm bir dakikalık sıklıkta yayılır ve 30 güne kadar geçmiş olur. Ölçümler üzerinde uyarılar yapılandırabilirsiniz. Adım adım yönergeler için bkz. [uyarıları ayarlama](howto-alert-on-metric.md). Diğer görevler otomatik eylemleri ayarlamayı, gelişmiş analiz gerçekleştirmeyi ve arşivleme geçmişini içerir. Daha fazla bilgi için bkz. [Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Ölçüm listesi
Bu ölçümler PostgreSQL için Azure veritabanı 'nda kullanılabilir:

|Ölçüm|Ölçüm görünen adı|Birim|Açıklama|
|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Kullanımdaki CPU yüzdesi.|
|memory_percent|Bellek yüzdesi|Yüzde|Kullanımdaki belleğin yüzdesi.|
|io_consumption_percent|GÇ yüzdesi|Yüzde|Kullanımdaki GÇ yüzdesi.|
|storage_percent|Depolama yüzdesi|Yüzde|Sunucunun en yüksek sınırının dışında kullanılan depolama alanı yüzdesi.|
|storage_used|Kullanılan depolama|Sayacının|Kullanımdaki depolama miktarı. Hizmet tarafından kullanılan depolama alanı, veritabanı dosyalarını, işlem günlüklerini ve sunucu günlüklerini içerebilir.|
|storage_limit|Depolama sınırı|Sayacının|Bu sunucu için en fazla depolama alanı.|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Yüzde|Sunucunun en yüksek sunucu günlük depolama alanı dışında kullanılan sunucu günlük depolama alanı yüzdesi.|
|serverlog_storage_usage|Kullanılan sunucu günlüğü depolaması|Sayacının|Kullanımdaki sunucu günlüğü depolama miktarı.|
|serverlog_storage_limit|Sunucu günlüğü depolama sınırı|Sayacının|Bu sunucu için en fazla sunucu günlük depolama alanı.|
|active_connections|Etkin bağlantılar|Sayı|Sunucuya etkin bağlantı sayısı.|
|connections_failed|Başarısız Bağlantılar|Sayı|Sunucuya yönelik başarısız bağlantı sayısı.|
|network_bytes_egress|Ağ Çıkışı|Sayacının|Etkin bağlantılar arasında ağ çıkışı.|
|network_bytes_ingress|Ağ Girişi|Sayacının|Etkin bağlantılar genelinde ağ.|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Sayacının|Kullanılan yedekleme depolama alanı miktarı.|
|pg_replica_log_delay_in_bytes|Çoğaltmalar genelinde en fazla gecikme|Sayacının|Ana ve en fazla çoğaltma çoğaltması arasındaki bayt cinsinden gecikme. Bu ölçüm yalnızca ana sunucuda kullanılabilir.|
|pg_replica_log_delay_in_seconds|Çoğaltma gecikmesi|Saniye|Son yeniden yürütülmüş işlemden bu yana geçen süre. Bu ölçüm yalnızca çoğaltma sunucuları için kullanılabilir.|

## <a name="server-logs"></a>Sunucu günlükleri
Sunucunuzda günlük kaydını etkinleştirebilirsiniz. Bu Günlükler [Azure izleyici günlükleri](../azure-monitor/log-query/log-query-overview.md), Event Hubs ve depolama hesabı 'Ndaki Azure tanılama günlükleri aracılığıyla da kullanılabilir. Günlüğe kaydetme hakkında daha fazla bilgi için [sunucu günlükleri](concepts-server-logs.md) sayfasını ziyaret edin.

## <a name="query-store"></a>Sorgu Deposu
Sorgu [deposu](concepts-query-store.md) sorgu çalışma zamanı istatistikleri ve bekleme olayları dahil olmak üzere zaman içinde sorgu performansını izler. Bu özellik, query_store şeması altında **azure_sys** adlı bir sistem veritabanında çalışma zamanı performans bilgilerini sorgular. Verilerin toplanmasını ve depolanmasını çeşitli yapılandırma düğmelerini kullanarak denetleyebilirsiniz.

## <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri
[Sorgu performansı içgörüleri](concepts-query-performance-insight.md) , Azure Portal erişilebilir görselleştirmeler sağlamak üzere sorgu deposuyla birlikte çalışarak. Bu grafikler, performansı etkileyen anahtar sorgularını tanımlamanızı sağlar. PostgreSQL için Azure veritabanı sunucunuzun portal sayfasındaki **destek + sorun giderme** bölümünden erişilen sorgu performansı öngörülü.

## <a name="performance-recommendations"></a>Performans Önerileri
[Performans önerileri](concepts-performance-recommendations.md) özelliği, iş yükü performansını artırmaya yönelik fırsatları tanımlar. Performans önerileri, iş yüklerinizin performansını geliştirmek için potansiyel olarak yeni dizinler oluşturmaya yönelik öneriler sağlar. Dizin önerileri oluşturmak için, özelliği şema ve sorgu deposu tarafından bildirilen iş yükü dahil çeşitli veritabanı özelliklerini dikkate alır. Herhangi bir performans önerisi uygulandıktan sonra, müşteriler bu değişikliklerin etkisini değerlendirmek için performansı test etmelidir. 

## <a name="service-health"></a>Hizmet durumu
[Azure hizmet durumu](../service-health/overview.md) , aboneliğinizdeki tüm hizmet durumu bildirimlerinin bir görünümünü sağlar. Kullandığınız Azure hizmetlerini ve bölgelerini etkileyebilecek sorunlar veya değişiklikler olduğunda tercih ettiğiniz iletişim kanallarınız aracılığıyla sizi bilgilendirmek üzere hizmet durumu uyarıları ayarlayabilirsiniz.

PostgreSQL için Azure veritabanı için zamanlanmış bakım olaylarını, **Planlı bakım** olay türünü kullanarak görüntüleyebilirsiniz. **Hizmet durumu uyarıları**oluşturma hakkında bilgi edinmek için [hizmet bildirimleri üzerinde etkinlik günlüğü uyarıları oluştur](../service-health/alerts-activity-log-service-notifications.md) makalesini ziyaret edin.

> [!IMPORTANT]
> Planlı bakım bildirimleri yalnızca Doğu ABD ve UK Güney önizleme sürümünde kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
- Bir ölçüm üzerinde uyarı oluşturma konusunda rehberlik için [uyarıları ayarlama](howto-alert-on-metric.md) bölümüne bakın.
- Azure portal, REST API veya CLı kullanarak ölçümlere erişme ve dışarı aktarma hakkında daha fazla bilgi için bkz. [Azure ölçümleri 'Ne genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
- [Sunucunuzu izlemek için en iyi uygulamalardan](https://azure.microsoft.com/blog/best-practices-for-alerting-on-metrics-with-azure-database-for-postgresql-monitoring/)blogumuzu okuyun.
