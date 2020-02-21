---
title: Azure VM boyutları-bellek | Microsoft Docs
description: Azure 'daki sanal makineler için kullanılabilir olan farklı bellek için iyileştirilmiş boyutları listeler. Bu serideki boyutlarda vCPU sayısı, veri diskleri ve NIC 'lerin yanı sıra depolama aktarım hızı ve ağ bant genişliği hakkındaki bilgileri listeler.
services: virtual-machines
documentationcenter: ''
author: jonbeck7
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
keywords: VM yalıtımı, yalıtılmış VM, yalıtım, yalıtılmış
ms.assetid: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jonbeck
ms.openlocfilehash: 26a44cdf3e57508cebb070669a8c1f473a59df8a
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77493533"
---
# <a name="memory-optimized-virtual-machine-sizes"></a>Bellek için iyileştirilmiş sanal makine boyutları

Bellek için iyileştirilmiş VM boyutları, ilişkisel veritabanı sunucuları, orta ve büyük önbellekler ve bellek içi analizler için harika olan yüksek bir bellek-CPU oranı sunar. Bu makalede, bu gruplandırmadaki her bir boyut için sanal CPU sayısı, veri diskleri ve NIC 'lerin yanı sıra depolama aktarım hızı ve ağ bant genişliği hakkında bilgi sağlanır.

- Özgün D serisi için bir takip olan [dv2 ve DSv2 serisi](dv2-dsv2-series-memory.md), daha güçlü bir CPU sunar. Dv2 serisi, D serisinden daha hızlı %35 daha hızlıdır. Intel® Xeon® 8171M 2,1 GHz (ufuk Gölü) veya Intel® Xeon® E5-2673 v4 2,3 GHz (geniş) veya Intel® Xeon® E5-2673 v3 2,4 GHz (Haswell) işlemcileri üzerinde çalışır ve Intel Turbo Boost Technology 2,0. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

    Dv2 ve DSv2 serisi, daha hızlı vCPU 'Ları, daha iyi geçici depolama performansını talep eden veya daha yüksek bellek taleplerine sahip olan uygulamalar için idealdir. Bu seçenekler birçok kurumsal sınıf uygulama için güçlü bir bileşim sunar.

- [Eav4 ve Easv4 serisi](eav4-easv4-series.md) , en fazla 256 MB boyutlu önbellek kullanan çok iş parçacıklı BIR yapılandırmada AMD 'Nin 2.35 GHz epyıc<sup>TM</sup> 7452 işlemcisini kullanır ve en fazla bellek için iyileştirilmiş iş yüklerini çalıştırmaya yönelik seçenekleri artırır. Eav4-Series ve Easv4 serisi, Ev3 & Esv3 serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

- [Ev3 ve Esv3-Series](ev3-esv3-series.md) Intel® Xeon® 8171M 2,1 GHz (ufuk Gölü) veya bir hiper iş yükleri için Intel® Xeon® E5-2673 v4 2,3 GHz (çok iyi) işlemcisi, çoğu genel amaçlı iş yükü için daha iyi bir değer teklifi sağlar ve Ev3 diğer diğer bulutların genel amaçlı VM 'leri ile hizalı hale getiriliyor. Bellek ve ağ sınırları, Hyper-Threading ' e geçiş ile hizalamak için, disk ve ağ sınırları her bir çekirdek temelinde ayarlanırken, bellek ve ağ sınırları, her bir çekirdek için bir saniyede ayarlanmışsa (7 GiB/vCPU 'dan 8 GiB Ev3, D/dv2 ailelerinin yüksek bellek sanal makine boyutlarına göre takip edilir.

- [M serisi](m-series.md) , yüksek bir vCPU sayısı (128 vCPU kadar) ve büyük miktarda bellek (3,8 TİB 'ye kadar) sunar. Son derece büyük veritabanları veya yüksek sanal CPU sayılarından ve büyük miktarda bellekten faydalanabilir diğer uygulamalar için de idealdir.

- [Mv2-Series](mv2-series.md) , buluttaki HERHANGI bir VM 'nin en yüksek vCPU sayısını (416 vCPU 'ya kadar) ve en büyük bellek miktarını (8,19 TİB 'ye kadar) sunar. Son derece büyük veritabanları veya yüksek vCPU sayısı ve büyük miktarlarda belleğin yararlı olacağı diğer uygulamalar için idealdir.

Azure Işlem, belirli bir donanım türüne yalıtılmış ve tek bir müşteriye adanmış sanal makine boyutları sunar. Bu sanal makine boyutları, uyumluluk ve mevzuat gereksinimleri gibi öğeleri içeren iş yükleri için diğer müşterilerden yüksek derecede yalıtım gerektiren iş yükleri için idealdir. Müşteriler Ayrıca, [iç içe geçmiş sanal makineler Için Azure desteği](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)'ni kullanarak bu yalıtılmış sanal makinelerin kaynaklarını daha fazla alt bölümlere ayırmak da tercih edebilir. Yalıtılmış VM seçenekleriniz için aşağıdaki sanal makine aileleri sayfalarına bakın.

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.