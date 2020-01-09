---
title: Ölçek sunucu grubu-hiper ölçek (Citus)-PostgreSQL için Azure veritabanı
description: Daha fazla yük ile başa çıkmak için sunucu grubu belleği, disk ve CPU kaynaklarını ayarlayın
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: bec2a40d8cf5fb178418ec6bb59a52a0bfe3eb8c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453048"
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

`rebalance_table_shards` işlevi, bağımsız değişkeninde adlı tablonun birlikte bulundurma [grubundaki tüm](concepts-hyperscale-colocation.md) tabloları yeniden dengeler. Bu nedenle, her dağıtılmış tablo için işlevi çağırmanız gerekmez, bunu her bir birlikte bulundurma grubundan temsili bir tabloda çağırmanız yeterlidir.

## <a name="increase-vcores"></a>Sanal çekirdekleri artırma

Yeni düğümler eklemenin yanı sıra, mevcut düğümlerin yeteneklerini de artırabilirsiniz. Bu özellik şu anda önizleme aşamasındadır. sunucu grubunuzdaki düğümler için daha fazla sanal çekirdek istemek üzere lütfen [Azure desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="next-steps"></a>Sonraki adımlar

Sunucu grubu [performans seçenekleri](concepts-hyperscale-configuration-options.md)hakkında daha fazla bilgi edinin.
