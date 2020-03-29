---
title: Monitör ve tune - Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: Bu makalede, PostgreSQL - Hyperscale (Citus) için Azure Veritabanı'ndaki izleme ve ayarı özellikleri açıklanmaktadır
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: d2e9fcd6f6292c1da76e725e90deda4547b3682d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74975525"
---
# <a name="monitor-and-tune-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL - Hyperscale (Citus) için Azure Veritabanını izleyin ve ayarlayın

Sunucularınız hakkındaki verileri izlemek, iş yükünüz için sorun gidermenize ve en iyi duruma getirmenize yardımcı olur. Hyperscale (Citus), bir sunucu grubundaki düğümlerin davranışları hakkında bilgi sağlamak için çeşitli izleme seçenekleri sağlar.

## <a name="metrics"></a>Ölçümler

Hyperscale (Citus), sunucu grubundaki her düğüm için ölçümler sağlar. Ölçümler, destekleyici kaynakların davranışı hakkında bilgi verir. Her metrik bir dakikalık frekansta yayılır ve 30 güne kadar geçmişi vardır.

Ölçümlerin grafiklerini görüntülemeye ek olarak, uyarıları yapılandırabilirsiniz. Adım adım kılavuzluk için [uyarıları nasıl ayarlayabilirsiniz.](howto-hyperscale-alert-on-metric.md)  Diğer görevler arasında otomatik eylemler ayarlama, gelişmiş analitik çalıştırma ve geçmişi arşivleme yer alır. Daha fazla bilgi için [Azure Ölçümlerine Genel Bakış'a](../monitoring-and-diagnostics/monitoring-overview-metrics.md)bakın.

### <a name="list-of-metrics"></a>Ölçümler listesi

Bu ölçümler Hyperscale (Citus) düğümleri için kullanılabilir:

|Ölçüm|Metrik Görüntü Adı|Birim|Açıklama|
|---|---|---|---|
|active_connections|Etkin Bağlantılar|Sayı|Sunucuya etkin bağlantı sayısı.|
|cpu_percent|CPU yüzdesi|Yüzde|Cpu'nun kullanımdaki yüzdesi.|
|ıop|IOPS|Sayı|[IOPS tanımına](../virtual-machines/linux/premium-storage-performance.md#iops) ve [Hiper ölçek lendirme girdisine](concepts-hyperscale-configuration-options.md) bakın|
|memory_percent|Bellek yüzdesi|Yüzde|Kullanılan bellek yüzdesi.|
|network_bytes_ingress|Ağ Girişi|Bayt|Ağ Etkin bağlantılar arasında in.|
|network_bytes_egress|Ağ Çıkışı|Bayt|Etkin bağlantılar arasında Ağ Out.|
|storage_percent|Depolama yüzdesi|Yüzde|Sunucunun maksimum dışında kullanılan depolama yüzdesi.|
|storage_used|Kullanılan depolama|Bayt|Kullanımdaki depolama miktarı. Hizmet tarafından kullanılan depolama veritabanı dosyalarını, işlem günlüklerini ve sunucu günlüklerini içerebilir.|

Azure küme için bir bütün olarak toplu ölçüm ler sağlar, ancak birden çok düğüm için ölçümler aynı grafiğe yerleştirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

- Bir metrikte uyarı oluşturma konusunda rehberlik için [uyarılar nasıl ayarlayacağınız](howto-hyperscale-alert-on-metric.md) üzerinde görün.
