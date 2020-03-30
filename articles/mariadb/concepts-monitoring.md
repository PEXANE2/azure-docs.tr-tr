---
title: İzleme - MariaDB için Azure Veritabanı
description: Bu makalede, CPU, depolama ve bağlantı istatistikleri de dahil olmak üzere MariaDB için Azure Veritabanı için izleme ve uyarı ölçümleri açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 20a2066dc0228fe9c2fee09387d96bf6aafeb32f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79531983"
---
# <a name="monitoring-in-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanında İzleme
Sunucularınız hakkındaki verileri izlemek, iş yükünüz için sorun gidermenize ve en iyi duruma getirmenize yardımcı olur. MariaDB için Azure Veritabanı, sunucunuzun davranışı hakkında bilgi veren çeşitli ölçümler sağlar.

## <a name="metrics"></a>Ölçümler
Tüm Azure ölçümleri bir dakikalık sıklıkla çalışır ve her metrik 30 günlük geçmiş sağlar. Ölçümlerdeki uyarıları yapılandırabilirsiniz. Diğer görevler arasında otomatik eylemler ayarlama, gelişmiş analitik gerçekleştirme ve geçmişi arşivleme yer alır. Daha fazla bilgi için [Azure Ölçümlerine Genel Bakış'a](../monitoring-and-diagnostics/monitoring-overview-metrics.md)bakın.

Adım adım kılavuzluk için [uyarıları nasıl ayarlayabilirsiniz.](howto-alert-metric.md)

### <a name="list-of-metrics"></a>Ölçümler listesi
Bu ölçümler, MariaDB için Azure Veritabanı için kullanılabilir:

|Ölçüm|Metrik Görüntü Adı|Birim|Açıklama|
|---|---|---|---|
|cpu_percent|CPU yüzdesi|Yüzde|Cpu'nun kullanımdaki yüzdesi.|
|memory_percent|Bellek yüzdesi|Yüzde|Kullanılan bellek yüzdesi.|
|io_consumption_percent|IO yüzdesi|Yüzde|Kullanılan IO yüzdesi.|
|storage_percent|Depolama yüzdesi|Yüzde|Sunucunun maksimum dışında kullanılan depolama yüzdesi.|
|storage_used|Kullanılan depolama|Bayt|Kullanımdaki depolama miktarı. Hizmet tarafından kullanılan depolama veritabanı dosyalarını, işlem günlüklerini ve sunucu günlüklerini içerebilir.|
|serverlog_storage_percent|Sunucu Günlüğü depolama yüzdesi|Yüzde|Sunucu günlük depolama yüzdesi sunucunun maksimum sunucu günlüğü depolama dışında kullanılır.|
|serverlog_storage_usage|Kullanılan Server Log depolama|Bayt|Kullanılan sunucu günlüğü depolama miktarı.|
|serverlog_storage_limit|Sunucu Günlüğü depolama sınırı|Bayt|Bu sunucu için maksimum sunucu günlüğü depolama.|
|storage_limit|Depolama sınırı|Bayt|Bu sunucu için maksimum depolama alanı.|
|active_connections|Etkin Bağlantılar|Sayı|Sunucuya etkin bağlantı sayısı.|
|connections_failed|Başarısız Bağlantılar|Sayı|Sunucuya başarısız bağlantı sayısı.|
|network_bytes_egress|Ağ Çıkışı|Bayt|Etkin bağlantılar arasında Ağ Out.|
|network_bytes_ingress|Ağ Girişi|Bayt|Ağ Etkin bağlantılar arasında in.|

## <a name="server-logs"></a>Sunucu günlükleri

Sunucunuzda yavaş sorgu günlüğe kaydetmeyi etkinleştirebilirsiniz. Bu günlükler, Azure Monitor günlüklerinde, Etkinlik Hub'larında ve Depolama Hesabındaki Azure Tanı Günlükleri'nde de kullanılabilir. Günlüğe kaydetme hakkında daha fazla bilgi edinmek için [sunucu günlükleri](concepts-server-logs.md) sayfasını ziyaret edin.

## <a name="query-store"></a>Sorgu Deposu

[Sorgu Deposu,](concepts-query-store.md) sorgu çalışma zamanı istatistikleri ve bekleme olayları da dahil olmak üzere zaman içinde sorgu performansını izler. Özellik, **mysql** şemasında sorgu çalışma zamanı performans bilgilerini devam ettir. Çeşitli yapılandırma topuzları aracılığıyla verilerin toplanmasını ve depolanmasını denetleyebilirsiniz.

## <a name="query-performance-insight"></a>Sorgu Performansı İçgörüleri

[Sorgu Performans Öngörüsü,](concepts-query-performance-insight.md) Azure portalından erişilebilen görselleştirmeler sağlamak için Sorgu Mağazası ile birlikte çalışır. Bu grafikler, performansı etkileyen anahtar sorguları belirlemenize olanak tanır. Sorgu Performans Öngörüsü'ne MariaDB sunucunun portal sayfası için Azure Veritabanınızın **Akıllı Performans** bölümünden erişilebilir.

## <a name="performance-recommendations"></a>Performans Önerileri

[Performans Önerileri](concepts-performance-recommendations.md) özelliği, iş yükü performansını artırmak için fırsatları tanımlar. Performans Önerileri, iş yüklerinizin performansını artırma potansiyeline sahip yeni dizinler oluşturmak için öneriler sağlar. Dizin önerileri üretmek için özellik, şeması ve Query Store tarafından bildirilen iş yükü de dahil olmak üzere çeşitli veritabanı özelliklerini dikkate alır. Herhangi bir performans önerisi uygulandıktan sonra, müşteriler bu değişikliklerin etkisini değerlendirmek için performansı test etmelidir.

## <a name="planned-maintenance-notification"></a>Planlı bakım bildirimi

**Planlı bakım bildirimleri,** MariaDB için Azure Veritabanınızda planlanan planlı bakım için uyarılar almanıza olanak tanır. Bu bildirimler Service [Health'in](../service-health/overview.md) planlanan bakımıyla tümleştirilir ve abonelikleriniz için planlanan tüm bakımı tek bir yerde görüntülemenize olanak sağlar. Ayrıca, farklı kaynaklardan sorumlu farklı kişilerinuz olabileceğinden, bildirimi farklı kaynak grupları için doğru hedef kitlelere ölçeklendirmenize de yardımcı olur. Yaklaşan bakım hakkında bildirim olaydan 72 saat önce alacaksınız.

> [!Note]
> Tüm etkinlikler için **Planlı bakım bildirimi72** saat bildirimi sağlamak için her türlü girişimi gerçekleştireceğiz. Ancak, kritik veya güvenlik düzeltme eki durumlarda bildirimler olaya daha yakın gönderilebilir veya atlanabilir.

### <a name="to-receive-planned-maintenance-notification"></a>Planlı bakım bildirimi almak için

1. [Portalda,](https://portal.azure.com) **Hizmet Durumu'nu**seçin.
2. **Uyarılar** bölümünde, Sistem **Durumu uyarılarını**seçin.
3. **+ Hizmet durumu uyarısı ekleyin** ve alanları doldurun.
4. Gerekli alanları doldurun. 
5. Olay **türünü**seçin, **Planlı bakımı** seçin veya **Tümünü Seçin**
6. **Eylem gruplarında** uyarıyı nasıl almak istediğinizi tanımlar (e-posta alın, bir mantık uygulaması tetikleme vb.)  
7. Oluşturma üzerine etkinleştir kuralının Evet olarak ayarlandığından emin olun.
8. Uyarınızı tamamlamak için **uyarı kuralı nı oluştur'u** seçin

**Hizmet durumu uyarılarının**nasıl oluşturulaca ilişkin ayrıntılı adımlar için, [hizmet bildirimlerinde etkinlik günlüğü uyarıları oluşturma'ya](../service-health/alerts-activity-log-service-notifications.md)bakın.

> [!IMPORTANT]
> Planlanan bakım bildirimleri şu anda önizlemede

## <a name="next-steps"></a>Sonraki adımlar

- Azure portalı, REST API veya CLI'yi kullanarak ölçümlere nasıl erişeceğimiz ve dışa aktarılabilenler hakkında daha fazla bilgi için [Azure Ölçümlere Genel Bakış'a](../monitoring-and-diagnostics/monitoring-overview-metrics.md)bakın.
  - Bkz. Bir metrikte uyarı oluşturma konusunda kılavuz [için uyarılar nasıl ayarlayınız.](howto-alert-metric.md)
