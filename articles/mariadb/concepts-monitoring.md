---
title: İzleme-MariaDB için Azure veritabanı
description: Bu makalede CPU, depolama ve bağlantı istatistikleri dahil olmak üzere MariaDB için Azure veritabanı izleme ve uyarma ölçümleri açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 15a396a86103f41f49d3b49878ec51c1e71add40
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74772488"
---
# <a name="monitoring-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı 'nda izleme
Sunucularınız hakkındaki izleme verileri, iş yükünüz için sorun gidermenize ve iyileştirmenize yardımcı olur. MariaDB için Azure veritabanı, sunucunuzun davranışına ilişkin Öngörüler sağlayan çeşitli ölçümler sunar.

## <a name="metrics"></a>Ölçümler
Tüm Azure ölçümlerinin bir dakikalık sıklığı vardır ve her ölçüm 30 gün geçmiş sağlar. Ölçümler üzerinde uyarılar yapılandırabilirsiniz. Diğer görevler otomatik eylemleri ayarlamayı, gelişmiş analiz gerçekleştirmeyi ve arşivleme geçmişini içerir. Daha fazla bilgi için bkz. [Azure ölçümlerine genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Adım adım yönergeler için bkz. [uyarıları ayarlama](howto-alert-metric.md).

### <a name="list-of-metrics"></a>Ölçüm listesi
Bu ölçümler, MariaDB için Azure veritabanı 'nda kullanılabilir:

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
|network_bytes_egress|Ağ Çıkışı|Sayacının|Etkin bağlantılar arasında ağ çıkışı.|
|network_bytes_ingress|Ağ Girişi|Sayacının|Etkin bağlantılar genelinde ağ.|

## <a name="server-logs"></a>Sunucu günlükleri

Sunucunuzda yavaş sorgu günlüğünü etkinleştirebilirsiniz. Bu Günlükler Azure Izleyici günlükleri, Event Hubs ve depolama hesabı 'ndaki Azure tanılama günlükleri aracılığıyla da kullanılabilir. Günlüğe kaydetme hakkında daha fazla bilgi için [sunucu günlükleri](concepts-server-logs.md) sayfasını ziyaret edin.

## <a name="query-store"></a>Sorgu Deposu

Sorgu [deposu](concepts-query-store.md) sorgu çalışma zamanı istatistikleri ve bekleme olayları dahil olmak üzere zaman içinde sorgu performansını izler. Özellik, **MySQL** şemasında çalışma zamanı performans bilgilerini sorgu ile devam ettirir. Verilerin toplanmasını ve depolanmasını çeşitli yapılandırma düğmelerini kullanarak denetleyebilirsiniz.

## <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri

[Sorgu performansı içgörüleri](concepts-query-performance-insight.md) , Azure Portal erişilebilir görselleştirmeler sağlamak üzere sorgu deposuyla birlikte çalışarak. Bu grafikler, performansı etkileyen anahtar sorgularını tanımlamanızı sağlar. Sorgu Performansı İçgörüleri, MariaDB sunucunuzun portal sayfası için Azure veritabanı 'nın **akıllı performans** bölümünde erişilebilir.

## <a name="performance-recommendations"></a>Performans Önerileri

[Performans önerileri](concepts-performance-recommendations.md) özelliği, iş yükü performansını artırmaya yönelik fırsatları tanımlar. Performans önerileri, iş yüklerinizin performansını geliştirmek için potansiyel olarak yeni dizinler oluşturmaya yönelik öneriler sağlar. Dizin önerileri oluşturmak için, özelliği şema ve sorgu deposu tarafından bildirilen iş yükü dahil çeşitli veritabanı özelliklerini dikkate alır. Herhangi bir performans önerisi uygulandıktan sonra, müşteriler bu değişikliklerin etkisini değerlendirmek için performansı test etmelidir.

## <a name="service-health"></a>Hizmet durumu
[Azure hizmet durumu](../service-health/overview.md) , aboneliğinizdeki tüm hizmet durumu bildirimlerinin bir görünümünü sağlar. Kullandığınız Azure hizmetlerini ve bölgelerini etkileyebilecek sorunlar veya değişiklikler olduğunda tercih ettiğiniz iletişim kanallarınız aracılığıyla sizi bilgilendirmek üzere hizmet durumu uyarıları ayarlayabilirsiniz.

MariaDB için Azure veritabanı için zamanlanmış bakım olaylarını **Planlı bakım** olay türünü kullanarak görüntüleyebilirsiniz. **Hizmet durumu uyarıları**oluşturma hakkında bilgi edinmek için [hizmet bildirimleri üzerinde etkinlik günlüğü uyarıları oluştur](../service-health/alerts-activity-log-service-notifications.md) makalesini ziyaret edin.

> [!IMPORTANT]
> Planlı bakım bildirimleri yalnızca Doğu ABD ve UK Güney önizleme sürümünde kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure portal, REST API veya CLı kullanarak ölçümlere erişme ve dışarı aktarma hakkında daha fazla bilgi için bkz. [Azure ölçümleri 'Ne genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
  - Bir ölçüm üzerinde uyarı oluşturma konusunda rehberlik için [uyarıları ayarlama](howto-alert-metric.md) bölümüne bakın.
