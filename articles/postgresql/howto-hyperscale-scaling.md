---
title: Ölçek sunucu grubu - Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: Artan yükle başa çıkmak için sunucu grubu belleği, disk ve CPU kaynaklarını ayarlama
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 3/16/2020
ms.openlocfilehash: fa48ca287c248155a0271b5134be782d8db1c785
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063109"
---
# <a name="scale-a-hyperscale-citus-server-group"></a>Bir Hyperscale (Citus) sunucu grubunu ölçeklendirin

PostgreSQL için Azure Veritabanı - Hyperscale (Citus), artan yükle başa çıkmak için self servis ölçeklendirme sağlar. Azure portalı, yeni alt düğümler eklemeyi ve varolan düğümlerin vCore'larını artırmayı kolaylaştırır.

## <a name="add-worker-nodes"></a>İşçi düğümleri ekleme

Düğüm eklemek için, Hyperscale (Citus) sunucu grubunuzun **Yapıla** sekmesine gidin.  **İşçi düğümü sayısı** için kaydırıcıyı sürüklemek değeri değiştirir.

![Kaynak kaydırıcıları](./media/howto-hyperscale-scaling/01-sliders-workers.png)

Değiştirilen değerin etkili olması için **Kaydet** düğmesini tıklatın.

> [!NOTE]
> Artırıldıktan ve kaydedildikten sonra, kaydırıcı kullanılarak alt düğüm sayısı azaltılamaz.

### <a name="rebalance-shards"></a>Yeniden dengeleme parçaları

Yeni eklenen düğümlerden yararlanmak için dağıtılmış tablo [kırıklarını](concepts-hyperscale-distributed-data.md#shards)yeniden dengelemeniz gerekir, bu da bazı parçaları mevcut düğümlerden yeni düğümlere taşımak anlamına gelir. İlk olarak, yeni işçilerin tedariki başarıyla tamamladığını doğrulayın. Sonra psql ile küme koordinatörü düğümbağlanarak ve çalışan, shard rebalancer başlatın:

```sql
SELECT rebalance_table_shards('distributed_table_name');
```

İşlev, `rebalance_table_shards` bağımsız değişkeninde adı geçen tablonun [birlikte konum](concepts-hyperscale-colocation.md) grubundaki tüm tabloları yeniden dengeler. Böylece her dağıtılmış tablo için işlevi aramak zorunda kalmadığınızı, her bir konum grubundan temsili bir tabloya çağırmanız gerekir.

## <a name="increase-or-decrease-vcores-on-nodes"></a>Düğümlerde vCore'ları artırma veya azaltma

> [!NOTE]
> Bu özellik şu anda önizleme sürümündedir. Sunucu grubunuzun düğümleri için vCore'larda değişiklik isteğinde bulunmak için lütfen [Azure desteğine başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)

Yeni düğümeklemeye ek olarak, varolan düğümlerin yeteneklerini artırabilirsiniz. İşlem kapasitesinin yukarı ve aşağı ayarlanması, performans denemelerine ve trafik taleplerinde kısa veya uzun vadeli değişiklikler için yararlı olabilir.

Tüm alt düğümler için vCores değiştirmek için, **Yapılandırma (işçi düğümü başına)** altında **vCores** kaydırıcısını ayarlayın. Koordinatör düğümün vCore'ları bağımsız olarak ayarlanabilir. **Koordinatör düğümü**altında **yapılandırmayı değiştir** bağlantısını tıklatın. Koordinatörün vCores ve Depolama kapasitesi için kaydırıcıları ile bir iletişim kutusu görüntülenir. Kaydırıcıları istediğiniz gibi değiştirin ve **Tamam'ı**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Sunucu grubu [performans seçenekleri](concepts-hyperscale-configuration-options.md)hakkında daha fazla bilgi edinin.
