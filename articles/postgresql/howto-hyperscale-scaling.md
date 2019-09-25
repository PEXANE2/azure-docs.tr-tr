---
title: PostgreSQL için Azure veritabanı-hiper ölçek (Citus) sunucu grubu ölçeklendirme
description: Daha fazla yük ile başa çıkmak için sunucu grubu belleği, disk ve CPU kaynaklarını ayarlayın
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 9/17/2019
ms.openlocfilehash: a69b4988a5ee835381de986edaa5899b09aa9e4e
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71262498"
---
# <a name="scale-a-hyperscale-citus-server-group"></a>Hiper ölçek (Citus) sunucu grubunu ölçeklendirme

PostgreSQL için Azure veritabanı-Hyperscale (Citus), daha fazla yük ile başa çıkmak için self servis Ölçeklendirmesi sağlar. Azure portal yeni çalışan düğümleri eklemeyi kolaylaştırır.

Bunu yapmak için, Hyperscale (Citus) sunucu grubunuzda **Yapılandır** sekmesine gidin.
Değeri değiştirmek için **çalışan düğümü sayısı** kaydırıcısını sürükleyin.

![Kaynak sürgüleri](./media/howto-hyperscale-scaling/01-sliders-workers.png)

Değiştirilen değerin etkili olması için "Kaydet" düğmesine tıklayın.

> [!NOTE]
> Arttırıldıktan ve kaydedildikten sonra, çalışan düğümlerinin sayısı kaydırıcı kullanılarak azaltılemez.
>
> Ayrıca, sanal çekirdekler ve depolama henüz bu kullanıcı arabirimiyle düzenleyici veya çalışanlar üzerinde ayarlanamaz. Düzenleyici veya çalışan düğümlerinde ölçeklendirme işlemi gerekiyorsa bir destek bileti açın.

Yeni eklenen düğümlerin avantajlarından yararlanmak için, [Dağıtılmış tablo parçaları](concepts-hyperscale-distributed-data.md#shards)'nı yeniden dengelemeniz gerekir, bu da bazı parçaları mevcut düğümlerden yeni olanlara taşımak anlamına gelir. Parça yeniden dengeleyiciyi başlatmak için psql ile küme Düzenleyicisi düğümüne bağlanın ve şunu çalıştırın:

```sql
SELECT rebalance_table_shards('distributed_table_name');
```

İşlevi, bağımsız değişkeninde adlı tablonun birlikte bulundurma [grubundaki tüm](concepts-hyperscale-colocation.md) tabloları yeniden dengeler. `rebalance_table_shards` Bu nedenle, her dağıtılmış tablo için işlevi çağırmanız gerekmez, bunu her bir birlikte bulundurma grubundan temsili bir tabloda çağırmanız yeterlidir.

## <a name="next-steps"></a>Sonraki adımlar

Sunucu grubu [performans seçenekleri](concepts-hyperscale-configuration-options.md)hakkında daha fazla bilgi edinin.
