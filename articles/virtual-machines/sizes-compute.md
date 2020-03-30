---
title: Azure VM boyutları - En iyi iletişimi en iyi duruma getirin | Microsoft Dokümanlar
description: Azure'daki sanal makineler için kullanılabilen farklı bilgi işlem optimize boyutları listeler. VCPUs, veri diskleri ve NIC'lerin sayısı nın yanı sıra bu serideki boyutlar için depolama verime ve ağ bant genişliği hakkındaki bilgileri listeler.
services: virtual-machines
documentationcenter: ''
author: jonbeck7
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jonbeck
ms.openlocfilehash: d709d621341ef14ec158ed5af1c2df4297d66b8b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77493637"
---
# <a name="compute-optimized-virtual-machine-sizes"></a>Optimize edilmiş sanal makine boyutlarını hesaplama

İşlem ekien VM boyutları, yüksek CPU-bellek oranına sahiptir. Bu boyutlar orta trafik web sunucuları, ağ cihazları, toplu işlem ve uygulama sunucuları için iyidir. Bu makalede, vCPUs, veri diskleri ve NIC'lerin sayısı hakkında bilgi sağlar. Ayrıca, bu gruplandırmadaki her boyut için depolama iş liği ve ağ bant genişliği hakkında bilgi içerir.

[Fsv2 serisi](fsv2-series.md) Intel® Xeon® Platinum 8168 işlemcidayanmaktadır. Tüm çekirdekli Turbo saat hızı 3.4 GHz ve maksimum tek çekirdekli turbo frekansı 3.7 GHz'dir. Intel® AVX-512 talimatları Intel Ölçeklenebilir İşlemciler yeni. Bu yönergeler, hem tek hem de çift hassas kayan nokta işlemlerinde vektör işleme iş yüklerine 2 kata kadar performans artışı sağlar. Başka bir deyişle, herhangi bir hesaplama iş yükü için gerçekten hızlı.

Fsv2 serisi, saat başına daha düşük bir liste fiyatıyla, vCPU başına Azure İşlem Birimi'ni (ACU) temel alan Azure portföyündeki fiyat performansı açısından en iyi değerdir.

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaç](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure bilgi [işlem birimlerinin (ACU)](acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.