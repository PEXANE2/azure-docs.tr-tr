---
title: Azure VM boyutları - Bellek | Microsoft Dokümanlar
description: Azure'daki sanal makineler için kullanılabilen farklı bellek optimize boyutları listeler. VCPUs, veri diskleri ve NIC'lerin sayısı nın yanı sıra bu serideki boyutlar için depolama verime ve ağ bant genişliği hakkındaki bilgileri listeler.
services: virtual-machines
documentationcenter: ''
author: jonbeck7
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
keywords: VM izolasyon, izole VM, izolasyon, izole
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jonbeck
ms.openlocfilehash: 0936e3049b2bc23e2f98244f29c962f6db65b8df
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115263"
---
# <a name="memory-optimized-virtual-machine-sizes"></a>Bellek optimize edilmiş sanal makine boyutları

Bellek optimize edilmiş VM boyutları, ilişkisel veritabanı sunucuları, orta ve büyük önbellekler ve bellek içi analizler için mükemmel olan yüksek bellek-CPU oranı sunar. Bu makalede, vCPUs sayısı, veri diskleri ve NIC'ler yanı sıra depolama iş ve ağ bant genişliği bu gruplandırma her boyutu hakkında bilgi sağlar.

- [Dv2 ve DSv2 serisi,](dv2-dsv2-series-memory.md)orijinal D-serisinin devamı, daha güçlü bir Işlemci özellikleri. Dv2 serisi D serisi yaklaşık% 35 daha hızlıdır. Intel Xeon&reg; &reg; 8171M 2.1 GHz (Skylake) veya&reg; Intel&reg; Xeon E5-2673 v4 2.3 GHz&reg; (Broadwell) veya Intel Xeon&reg; E5-2673 v3 2.4 GHz (Haswell) işlemcilerde ve Intel Turbo Boost Technology 2.0 ile çalışır. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

    Dv2 ve DSv2 serisi, daha hızlı vCPUs, daha iyi geçici depolama performansı veya daha yüksek bellek taleplerine sahip uygulamalar için idealdir. Bu seçenekler birçok kurumsal sınıf uygulama için güçlü bir bileşim sunar.

- [Eav4 ve Easv4 serisi,](eav4-easv4-series.md) AMD'nin 2.35Ghz EPYC<sup>TM</sup> 7452 işlemcisini 256 MB L3 önbelleğe kadar çok iş parçacığı yapılandırmasında kullanarak en çok bellek için optimize edilmiş iş yükünü çalıştırma seçeneklerini artırır. Eav4 serisi ve Easv4 serisi, Ev3 & Esv3 serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

- [Ev3 ve Esv3](ev3-esv3-series.md) serisi&reg; Intel&reg; Xeon 8171M 2.1 GHz&reg; (Skylake) veya Intel Xeon&reg; E5-2673 v4 2.3 GHz (Broadwell) işlemci, en genel amaçlı iş yükleri için daha iyi bir değer teklifi sağlayan ve diğer birçok bulutun genel amaçlı VM'leri ile uyumlu hale Ev3 getiriyor. Bellek genişletildi (7 GiB/vCPU'dan 8 GiB/vCPU'ya) disk ve ağ sınırları her çekirdek bazında hiper iş parçacığına geçmek üzere ayarlandı. Ev3, D/Dv2 ailelerinin yüksek bellekli VM boyutlarının takibidir.

- [M serisi](m-series.md) yüksek vCPU sayısı (128 vCPUs'a kadar) ve büyük miktarda bellek (3,8 TiB'ye kadar) sunar. Ayrıca, yüksek vCPU sayılarından ve büyük miktarda bellekten yararlanan son derece büyük veritabanları veya diğer uygulamalar için de idealdir.

- [Mv2 serisi,](mv2-series.md) buluttaki herhangi bir VM'nin en yüksek vCPU sayısını (416 vCPUs'a kadar) ve en büyük belleği (11,4 TiB'ye kadar) sunar. Yüksek vCPU sayılarından ve büyük miktarda bellekten yararlanan son derece büyük veritabanları veya diğer uygulamalar için idealdir.

Azure Compute, belirli bir donanım türüne yalıtılmış ve tek bir müşteriye adanmış sanal makine boyutları sunar. Bu sanal makine boyutları, uyumluluk ve mevzuat gereksinimleri gibi öğeleri içeren iş yükleri için diğer müşterilerden yüksek derecede yalıtım gerektiren iş yükleri için en uygundur. Müşteriler ayrıca iç içe sanal makineler için Azure desteğini kullanarak bu Yalıtılmış sanal makinelerin kaynaklarını daha fazla alt bölmeyi de [seçebilirler.](https://azure.microsoft.com/blog/nested-virtualization-in-azure/) İzole VM seçenekleriniz için aşağıdaki sanal makine ailelerinin sayfalarına bakın.

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure bilgi [işlem birimlerinin (ACU)](acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.