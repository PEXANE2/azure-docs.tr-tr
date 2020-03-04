---
title: İzleme ve ayarlama-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede PostgreSQL için Azure veritabanı-tek sunucu 'da izleme ve ayarlama özellikleri açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/19/2019
ms.openlocfilehash: 2d2bb5440e6d23a4cb5781244ba33ab4c5be2612
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78252576"
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
|storage_used|Kullanılan depolama alanı|Bayt|Kullanımdaki depolama miktarı. Hizmet tarafından kullanılan depolama alanı, veritabanı dosyalarını, işlem günlüklerini ve sunucu günlüklerini içerebilir.|
|storage_limit|Depolama sınırı|Bayt|Bu sunucu için en fazla depolama alanı.|
|serverlog_storage_percent|Sunucu günlüğü depolama yüzdesi|Yüzde|Sunucunun en yüksek sunucu günlük depolama alanı dışında kullanılan sunucu günlük depolama alanı yüzdesi.|
|serverlog_storage_usage|Kullanılan sunucu günlüğü depolaması|Bayt|Kullanımdaki sunucu günlüğü depolama miktarı.|
|serverlog_storage_limit|Sunucu günlüğü depolama sınırı|Bayt|Bu sunucu için en fazla sunucu günlük depolama alanı.|
|active_connections|Etkin bağlantılar|Sayı|Sunucuya etkin bağlantı sayısı.|
|connections_failed|Başarısız Bağlantılar|Sayı|Sunucuya yönelik başarısız bağlantı sayısı.|
|network_bytes_egress|Ağ Çıkışı|Bayt|Etkin bağlantılar arasında ağ çıkışı.|
|network_bytes_ingress|Ağ Girişi|Bayt|Etkin bağlantılar genelinde ağ.|
|backup_storage_used|Kullanılan yedekleme depolama alanı|Bayt|Kullanılan yedekleme depolama alanı miktarı.|
|pg_replica_log_delay_in_bytes|Çoğaltmalar genelinde en fazla gecikme|Bayt|Ana ve en fazla çoğaltma çoğaltması arasındaki bayt cinsinden gecikme. Bu ölçüm yalnızca ana sunucuda kullanılabilir.|
|pg_replica_log_delay_in_seconds|Çoğaltma gecikmesi|Saniyeden|Son yeniden yürütülmüş işlemden bu yana geçen süre. Bu ölçüm yalnızca çoğaltma sunucuları için kullanılabilir.|

## <a name="server-logs"></a>Sunucu günlükleri
Sunucunuzda günlük kaydını etkinleştirebilirsiniz. Bu Günlükler [Azure izleyici günlükleri](../azure-monitor/log-query/log-query-overview.md), Event Hubs ve depolama hesabı 'Ndaki Azure tanılama günlükleri aracılığıyla da kullanılabilir. Günlüğe kaydetme hakkında daha fazla bilgi için [sunucu günlükleri](concepts-server-logs.md) sayfasını ziyaret edin.

## <a name="query-store"></a>Sorgu Deposu
Sorgu [deposu](concepts-query-store.md) sorgu çalışma zamanı istatistikleri ve bekleme olayları dahil olmak üzere zaman içinde sorgu performansını izler. Bu özellik, query_store şeması altında **azure_sys** adlı bir sistem veritabanında çalışma zamanı performans bilgilerini sorgular. Verilerin toplanmasını ve depolanmasını çeşitli yapılandırma düğmelerini kullanarak denetleyebilirsiniz.

## <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri
[Sorgu performansı içgörüleri](concepts-query-performance-insight.md) , Azure Portal erişilebilir görselleştirmeler sağlamak üzere sorgu deposuyla birlikte çalışarak. Bu grafikler, performansı etkileyen anahtar sorgularını tanımlamanızı sağlar. Sorgu Performansı İçgörüleri, PostgreSQL için Azure veritabanı sunucunuzun portal sayfasındaki **destek + sorun giderme** bölümünden erişilebilir.

## <a name="performance-recommendations"></a>Performans Önerileri
[Performans önerileri](concepts-performance-recommendations.md) özelliği, iş yükü performansını artırmaya yönelik fırsatları tanımlar. Performans önerileri, iş yüklerinizin performansını geliştirmek için potansiyel olarak yeni dizinler oluşturmaya yönelik öneriler sağlar. Dizin önerileri oluşturmak için, özelliği şema ve sorgu deposu tarafından bildirilen iş yükü dahil çeşitli veritabanı özelliklerini dikkate alır. Herhangi bir performans önerisi uygulandıktan sonra, müşteriler bu değişikliklerin etkisini değerlendirmek için performansı test etmelidir. 

## <a name="planned-maintenance-notification"></a>Planlı bakım bildirimi

**Planlı bakım bildirimleri** PostgreSQL Için Azure veritabanı-tek sunucu için Yaklaşan planlı bakım uyarılarını almanızı sağlar. Bu bildirimler [hizmet durumunun](../service-health/overview.md) planlanmış bakımıyla tümleşiktir ve abonelikleriniz için tüm zamanlanmış bakımı tek bir yerden görüntülemenize olanak sağlar. Ayrıca, farklı kaynaklardan sorumlu farklı kişileriniz olabileceğinden farklı kaynak grupları için bildirimin doğru kitlelere ölçeklendirilmesine de yardımcı olur. Olaydan önce yaklaşan bakım 72 saati hakkında bildirim alacaksınız.

> [!Note]
> Tüm olaylar için **Planlı bakım bildirimi** 72 saat bildirimi sağlamak için her girişimde bulunacağız. Bununla birlikte, kritik veya güvenlik düzeltme ekleri durumlarında, bildirim olaya yakın bir şekilde gönderilebilir veya atlanamaz.

### <a name="to-receive-planned-maintenance-notification"></a>Planlı bakım bildirimi almak için

1. [Portalda](https://portal.azure.com) **hizmet durumu**' nu seçin.
2. **Uyarılar** bölümünde **sistem durumu uyarıları**' nı seçin.
3. **+ Hizmet sistem durumu uyarısı Ekle** ' yi seçin ve alanları girin.
4. Gerekli alanları doldurun. 
5. **Olay türünü**seçin, **Planlı bakım** ' ı seçin veya **tümünü seçin**
6. **Eylem gruplarında** , uyarıyı nasıl almak istediğinizi tanımlar (bir e-posta alın, mantıksal uygulama tetiklemesi vb.)  
7. Oluşturma sonrasında kuralın etkinleştir ' in Evet olarak ayarlandığından emin olun.
8. Uyarınızı gerçekleştirmek için **Uyarı kuralı oluştur** ' u seçin

**Hizmet durumu uyarıları**oluşturma hakkında ayrıntılı adımlar için, [hizmet bildirimlerinde etkinlik günlüğü uyarıları oluşturma](../service-health/alerts-activity-log-service-notifications.md)konusuna bakın.

> [!IMPORTANT]
> Planlı bakım bildirimleri şu anda önizleme aşamasındadır

## <a name="next-steps"></a>Sonraki adımlar
- Bir ölçüm üzerinde uyarı oluşturma konusunda rehberlik için [uyarıları ayarlama](howto-alert-on-metric.md) bölümüne bakın.
- Azure portal, REST API veya CLı kullanarak ölçümlere erişme ve dışarı aktarma hakkında daha fazla bilgi için bkz. [Azure ölçümleri 'Ne genel bakış](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
- [Sunucunuzu izlemek için en iyi uygulamalardan](https://azure.microsoft.com/blog/best-practices-for-alerting-on-metrics-with-azure-database-for-postgresql-monitoring/)blogumuzu okuyun.
