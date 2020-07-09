---
title: Ölçek sunucu grubu-hiper ölçek (Citus)-PostgreSQL için Azure veritabanı
description: Daha fazla yük ile başa çıkmak için sunucu grubu belleği, disk ve CPU kaynaklarını ayarlayın
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 3/16/2020
ms.openlocfilehash: 1173defa8bbe66cbeaaf6bd5264b0730160a197b
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86116837"
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

`rebalance_table_shards`İşlevi, bağımsız değişkeninde adlı tablonun birlikte bulundurma [colocation](concepts-hyperscale-colocation.md) grubundaki tüm tabloları yeniden dengeler. Bu nedenle, her dağıtılmış tablo için işlevi çağırmanız gerekmez, bunu her bir birlikte bulundurma grubundan temsili bir tabloda çağırmanız yeterlidir.

## <a name="increase-or-decrease-vcores-on-nodes"></a>Düğümlerdeki sanal çekirdekleri artırma veya azaltma

> [!NOTE]
> Bu özellik şu anda önizleme sürümündedir. Sunucu grubunuzdaki düğümlerin sanal çekirdekler üzerinde değişiklik istemek için lütfen [Azure desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

Yeni düğümler eklemenin yanı sıra, mevcut düğümlerin yeteneklerini de artırabilirsiniz. İşlem kapasitesini yukarı ve aşağı ayarlamak, performans denemeleri ve trafik taleplerine yönelik kısa veya uzun süreli değişiklikler için yararlı olabilir.

Tüm çalışan düğümlerinin sanal çekirdeğini değiştirmek için **yapılandırma (çalışan düğümü başına)** altındaki **vçekirdekler** kaydırıcısını ayarlayın. Düzenleyici düğümünün sanal çekirdekleri bağımsız olarak ayarlanabilir. **Düzenleyici düğümü**altındaki **yapılandırma Değiştir** bağlantısına tıklayın. Düzenleyicinin sanal çekirdekleri ve depolama kapasitesi için kaydırıcıların bulunduğu bir iletişim kutusu görüntülenir. Sürgüleri istediğiniz şekilde değiştirin ve **Tamam**' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Sunucu grubu [performans seçenekleri](concepts-hyperscale-configuration-options.md)hakkında daha fazla bilgi edinin.
