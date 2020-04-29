---
title: Azure VM boyutları-Işlem için iyileştirilmiş | Microsoft Docs
description: Azure 'daki sanal makineler için kullanılabilen farklı işlem için iyileştirilmiş boyutları listeler. Bu serideki boyutlarda sanal CPU 'lar, veri diskleri ve NIC 'lerin yanı sıra depolama aktarım hızı ve ağ bant genişliği hakkındaki bilgileri listeler.
services: virtual-machines
documentationcenter: ''
author: mimckitt
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: mimckitt
ms.openlocfilehash: e08d593f641c42f9ad605fda013206e70a34e52f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81269644"
---
# <a name="compute-optimized-virtual-machine-sizes"></a>İşlem için iyileştirilmiş sanal makine boyutları

İşlem için iyileştirilmiş VM boyutları yüksek CPU-bellek oranına sahiptir. Bu boyutlar orta trafikli web sunucuları, ağ gereçleri, toplu süreçler ve uygulama sunucuları için uygundur. Bu makale, vCPU sayısı, veri diskleri ve NIC 'ler hakkında bilgi sağlar. Ayrıca, bu gruplandırmadaki her bir boyut için depolama alanı işleme ve ağ bant genişliği hakkında bilgiler içerir.

[Fsv2 serisi](fsv2-series.md) , Intel® Xeon® Platinum 8168 işlemcisini temel alır. Sürekli olarak 3,4 GHz 'nin tüm Core Turbo saat hızına ve en yüksek çekirdekli Turbo 3,7 GHz sıklığına sahiptir. Intel® AVX-512 yönergeleri Intel Ölçeklenebilir Işlemcilerde yenidir. Bu yönergeler, tek ve çift duyarlıklı kayan nokta işlemlerinde vektör işleme iş yükleri için bir 2X performans artışı sağlar. Diğer bir deyişle, tüm hesaplama iş yükleri oldukça hızlıdır.

Daha düşük bir saatlik liste fiyatına göre, Fsv2-Series, vCPU başına Azure Işlem birimi (ACU) temelinde Azure portföyündeki fiyat performansı için en iyi değerdir.

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
