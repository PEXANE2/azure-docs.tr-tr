---
title: Ölçek sunucu grubu-hiper ölçek (Citus)-PostgreSQL için Azure veritabanı
description: Daha fazla yük ile başa çıkmak için sunucu grubu belleği, disk ve CPU kaynaklarını ayarlayın
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 11/17/2020
ms.openlocfilehash: 59e6e73c99569b0a35c56d65c1a7ccdfcb394c0f
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95026430"
---
# <a name="scale-a-hyperscale-citus-server-group"></a>Hiper ölçek (Citus) sunucu grubunu ölçeklendirme

PostgreSQL için Azure veritabanı-Hyperscale (Citus), daha fazla yük ile başa çıkmak için self servis Ölçeklendirmesi sağlar. Azure portal, yeni çalışan düğümleri eklemeyi ve mevcut düğümlerin sanal çekirdeğini artırmayı kolaylaştırır. Düğüm eklemek kapalı kalma gerektirmez ve hatta [parçaları](howto-hyperscale-scale-rebalance.md)yeni düğümlere (parça olarak adlandırılır), sorguları kesintiye uğramadan hareket ettirilmesine neden olur.

## <a name="add-worker-nodes"></a>Çalışan düğümleri Ekle

Düğüm eklemek için, Hyperscale (Citus) sunucu grubundaki **işlem + depolama** sekmesine gidin.  **Çalışan düğümü sayısı** için kaydırıcıyı sürüklemek değeri değiştirir.

:::image type="content" source="./media/howto-hyperscale-scaling/01-sliders-workers.png" alt-text="Kaynak sürgüleri":::

Değiştirilen değerin etkili olması için **Kaydet** düğmesine tıklayın.

> [!NOTE]
> Arttırıldıktan ve kaydedildikten sonra, çalışan düğümlerinin sayısı kaydırıcı kullanılarak azaltılemez.

> [!NOTE]
> Yeni eklenen düğümlerin avantajlarından yararlanmak için, [Dağıtılmış tablo](howto-hyperscale-scale-rebalance.md)parçaları 'nı yeniden dengelemeniz gerekir, bu [da bazı parçaları](concepts-hyperscale-distributed-data.md#shards) mevcut düğümlerden yeni olanlara taşımak anlamına gelir.

## <a name="increase-or-decrease-vcores-on-nodes"></a>Düğümlerdeki sanal çekirdekleri artırma veya azaltma

Yeni düğümler eklemenin yanı sıra, mevcut düğümlerin yeteneklerini de artırabilirsiniz. İşlem kapasitesini yukarı ve aşağı ayarlamak, performans denemeleri ve trafik taleplerine yönelik kısa veya uzun süreli değişiklikler için yararlı olabilir.

Tüm çalışan düğümlerinin sanal çekirdeğini değiştirmek için **yapılandırma (çalışan düğümü başına)** altındaki **vçekirdekler** kaydırıcısını ayarlayın. Düzenleyici düğümünün sanal çekirdekleri bağımsız olarak ayarlanabilir. **Yapılandırma (düzenleyici düğümü)** altında **vçekirdekler** kaydırıcısını ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

- Sunucu grubu [performans seçenekleri](concepts-hyperscale-configuration-options.md)hakkında daha fazla bilgi edinin.
- Tüm çalışan düğümlerinin paralel sorgulara katılmasını sağlamak için [Dağıtılmış tablo parçaları yeniden dengeleyin](howto-hyperscale-scale-rebalance.md)
