---
title: Ölçek sunucu grubu-hiper ölçek (Citus)-PostgreSQL için Azure veritabanı
description: Daha fazla yük ile başa çıkmak için sunucu grubu belleği, disk ve CPU kaynaklarını ayarlayın
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 3/16/2020
ms.openlocfilehash: fa48ca287c248155a0271b5134be782d8db1c785
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80063109"
---
# <a name="scale-a-hyperscale-citus-server-group"></a>Hiper ölçek (Citus) sunucu grubunu ölçeklendirme

PostgreSQL için Azure veritabanı-Hyperscale (Citus), daha fazla yük ile başa çıkmak için self servis Ölçeklendirmesi sağlar. Azure portal, yeni çalışan düğümleri eklemeyi ve mevcut düğümlerin sanal çekirdeğini artırmayı kolaylaştırır.

## <a name="add-worker-nodes"></a>Çalışan düğümleri Ekle

Düğüm eklemek için, Hyperscale (Citus) sunucu grubundaki **Yapılandır** sekmesine gidin.  **Çalışan düğümü sayısı** için kaydırıcıyı sürüklemek değeri değiştirir.

![Kaynak sürgüleri](./media/howto-hyperscale-scaling/01-sliders-workers.png)

Değiştirilen değerin etkili olması için **Kaydet** düğmesine tıklayın.

> [!NOTE]
> Arttırıldıktan ve kaydedildikten sonra, çalışan düğümlerinin sayısı kaydırıcı kullanılarak azaltılemez.

### <a name="rebalance-shards"></a>Parçaları yeniden dengeleme

Yeni eklenen düğümlerin avantajlarından yararlanmak için, [Dağıtılmış tablo parçaları](concepts-hyperscale-distributed-data.md#shards)'nı yeniden dengelemeniz gerekir, bu da bazı parçaları mevcut düğümlerden yeni olanlara taşımak anlamına gelir. İlk olarak, yeni çalışanların sağlamayı başarıyla bitirdiğini doğrulayın. Ardından, psql ile küme Düzenleyicisi düğümüne bağlanarak ve çalıştıran parça yeniden dengeleyiciyi başlatın:

```sql
SELECT rebalance_table_shards('distributed_table_name');
```

İşlevi, bağımsız değişkeninde adlı tablonun birlikte bulundurma [grubundaki tüm](concepts-hyperscale-colocation.md) tabloları yeniden dengeler. `rebalance_table_shards` Bu nedenle, her dağıtılmış tablo için işlevi çağırmanız gerekmez, bunu her bir birlikte bulundurma grubundan temsili bir tabloda çağırmanız yeterlidir.

## <a name="increase-or-decrease-vcores-on-nodes"></a>Düğümlerdeki sanal çekirdekleri artırma veya azaltma

> [!NOTE]
> Bu özellik şu anda önizleme sürümündedir. Sunucu grubunuzdaki düğümlerin sanal çekirdekler üzerinde değişiklik istemek için lütfen [Azure desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

Yeni düğümler eklemenin yanı sıra, mevcut düğümlerin yeteneklerini de artırabilirsiniz. İşlem kapasitesini yukarı ve aşağı ayarlamak, performans denemeleri ve trafik taleplerine yönelik kısa veya uzun süreli değişiklikler için yararlı olabilir.

Tüm çalışan düğümlerinin sanal çekirdeğini değiştirmek için **yapılandırma (çalışan düğümü başına)** altındaki **vçekirdekler** kaydırıcısını ayarlayın. Düzenleyici düğümünün sanal çekirdekleri bağımsız olarak ayarlanabilir. **Düzenleyici düğümü**altındaki **yapılandırma Değiştir** bağlantısına tıklayın. Düzenleyicinin sanal çekirdekleri ve depolama kapasitesi için kaydırıcıların bulunduğu bir iletişim kutusu görüntülenir. Sürgüleri istediğiniz şekilde değiştirin ve **Tamam**' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Sunucu grubu [performans seçenekleri](concepts-hyperscale-configuration-options.md)hakkında daha fazla bilgi edinin.
