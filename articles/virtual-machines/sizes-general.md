---
title: Azure VM boyutları - Genel amaç | Microsoft Dokümanlar
description: Azure'daki sanal makineler için kullanılabilen farklı genel amaçlı boyutları listeler. VCPUs, veri diskleri ve NIC'lerin sayısı nın yanı sıra bu serideki boyutlar için depolama verime ve ağ bant genişliği hakkındaki bilgileri listeler.
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
ms.date: 02/20/2020
ms.author: jonbeck
ms.openlocfilehash: 4b36f456f70eb79cff1f615c7c136b8fe4b1b3d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78226715"
---
# <a name="general-purpose-virtual-machine-sizes"></a>Genel amaçlı sanal makine boyutları

Genel amaçlı VM boyutları dengeli CPU-bellek oranı sağlar. Test ve geliştirme, küçük-orta büyüklükteki veritabanları ve küçük-orta büyüklükte trafik hacmine sahip web sunucuları için idealdir. Bu makalede, genel amaçlı bilgi işlem teklifleri hakkında bilgi sağlar.

- [Av2 serisi](av2-series.md) VM'ler çeşitli donanım türlerine ve işlemcilere dağıtılabilir. A serisi VM'ler, geliştirme ve test gibi giriş düzeyi iş yükleri için en uygun CPU performansına ve bellek yapılandırmalarına sahiptir. Dağıtıldığı donanımdan bağımsız olarak, çalışan örneğe tutarlı işlemci performansı sunmak için boyut donanıma göre genişletilir. Bu boyutun dağıtıldığı fiziksel donanımı belirlemek için Sanal Makinenin içinden sanal donanımı sorgulayın. Örnek kullanım örnekleri geliştirme ve test sunucuları, düşük trafik web sunucuları, küçük ve orta veritabanları, kavram kanıtı ve kod depolarını içerir.

  > [!NOTE]
  > A8 – A11 VM'lerin 3/2021 tarihinde emekliye ayrılması planlanmaktadır. Daha fazla bilgi için [HPC Geçiş Kılavuzu'na](https://azure.microsoft.com/resources/hpc-migration-guide/)bakın.

- [B serisi patlanabilir](sizes-b-series-burstable.md) VM'ler, web sunucuları, küçük veritabanları ve geliştirme ve test ortamları gibi CPU'nun tam performansına sürekli olarak ihtiyaç duymayan iş yükleri için idealdir. Bu iş yükleri genellikle burstable performans gereksinimleri vardır. B-Serisi, bu müşterilere, VM temel performansından daha az ını kullanırken VM örneğinin kredi oluşturmasına olanak tanıyan fiyat bilinçli temel performansıyla Bir VM boyutu satın alma olanağı sağlar. VM kredi biriktirdiğinde, uygulamanız daha yüksek CPU performansı gerektirdiğinde VM CPU'nun %100'üne kadarını kullanarak VM'nin taban çizgisinin üzerine çıkabilir.

- [Dav4 ve Dasv4 serisi,](dav4-dasv4-series.md) AMD'nin 2.35Ghz EPYC<sup>TM</sup> 7452 işlemcisini 256 MB L3 önbelleğe sahip çok iş parçacıklı bir yapılandırmada kullanan ve bu L3 önbelleğinin 8 GB'ını genel amaçlı iş yüklerini çalıştırmak için müşteri seçeneklerini artıran her 8 çekirdekte kullanan yeni boyutlardır. Dav4 serisi ve Dasv4 serisi, D & Dsv3 serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

- [DCv2 serisi,](dcv2-series.md) genel bulutta işlenirken verilerinizin ve kodlarınızın gizliliğini ve bütünlüğünü korumaya yardımcı olabilir. Bu makineler, SGX teknolojisine sahip en son nesil Intel XEON E-2288G İşlemci tarafından desteklenen bir cihazdır. Intel Turbo Boost Teknolojisi ile bu makineler 5.0GHz'e kadar çıkabiliyor. DCv2 serisi örnekleri, müşterilerin kullanımdayken kodlarını ve verilerini korumak için güvenli enklav tabanlı uygulamalar oluşturmasına olanak tanır.

- [Dv2 ve Dsv2 serisi](dv2-dsv2-series.md) Orijinal D serisinin bir takipçisi olan VM'ler, daha güçlü bir IŞLEMCI ve en iyi CPU'dan belleğe yapılandırmaya sahiptir ve bunları çoğu üretim iş yükü için uygun hale getirir. Dv2 serisi D serisi yaklaşık% 35 daha hızlıdır. Dv2 serisi Intel Turbo Boost Technology 2.0 ile Intel® Xeon® 8171M 2.1GHz (Skylake), Intel® Xeon® E5-2673 v4 2.3 GHz (Broadwell) veya Intel® Xeon® E5-2673 v3 2.4 GHz (Haswell) işlemcileri üzerinde çalışır. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

- [Dv3 ve Dsv3 serisi](dv3-dsv3-series.md) VM'ler Intel® Xeon® 8171M 2.1GHz (Skylake), Intel® Xeon® E5-2673 v4 2.3 GHz (Broadwell) veya Intel® Xeon® E5-2673 v3 2.4 GHz (Haswell) işlemcilerinde hiper iş parçacığı yapılandırmasında çalışır ve en genel amaçlı iş yükleri için daha iyi bir değer teklifi sunar. Bellek genişletildi (~3.5 GiB/vCPU'dan 4 GiB/vCPU'ya) disk ve ağ sınırları ise hiperiş parçacığına geçmek için çekirdek bazında ayarlandı. Dv3 serisi artık D/Dv2 serisi yüksek bellek VM boyutlarına sahip, bu bellek optimize [Ev3 ve Esv3 serisi](ev3-esv3-series.md)taşındı .

Örnek D serisi kullanım örnekleri, kurumsal dereceli uygulamaları, ilişkisel veritabanlarını, bellek içi önbelleğe alma ve analizleri içerir.

## <a name="other-sizes"></a>Diğer boyutlar

- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure bilgi [işlem birimlerinin (ACU)](acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.