---
title: Azure VM boyutları-genel amaçlı | Microsoft Docs
description: Azure 'daki sanal makineler için kullanılabilir olan farklı genel amaçlı boyutları listeler. Bu serideki boyutlarda sanal CPU 'lar, veri diskleri ve NIC 'lerin yanı sıra depolama aktarım hızı ve ağ bant genişliği hakkındaki bilgileri listeler.
author: mimckitt
ms.service: virtual-machines
ms.subservice: sizes
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/20/2020
ms.author: mimckitt
ms.openlocfilehash: e1e4d3c896fadc60e4c0492691eaa1e0bb8e929a
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84678486"
---
# <a name="general-purpose-virtual-machine-sizes"></a>Genel amaçlı sanal makine boyutları

Genel amaçlı VM boyutları dengeli CPU-bellek oranını sağlar. Test ve geliştirme, küçük-orta büyüklükteki veritabanları ve küçük-orta büyüklükte trafik hacmine sahip web sunucuları için idealdir. Bu makalede genel amaçlı bilgi işlem teklifleri hakkında bilgi sağlanır.

- [AV2 serisi](av2-series.md) VM 'ler, çeşitli donanım türlerinde ve işlemcilerde dağıtılabilir. A serisi VM 'Ler, geliştirme ve test gibi giriş düzeyi iş yükleri için en uygun CPU performansına ve bellek yapılandırmalarına sahiptir. Dağıtıldığı donanımdan bağımsız olarak, çalışan örneğe tutarlı işlemci performansı sunmak için boyut donanıma göre genişletilir. Bu boyutun dağıtıldığı fiziksel donanımı belirlemek için Sanal Makinenin içinden sanal donanımı sorgulayın. Örnek kullanım örnekleri arasında geliştirme ve test sunucuları, düşük trafikli web sunucuları, küçük ve orta ölçekli veritabanları, kavram kanıtı ve kod depoları bulunur.

  > [!NOTE]
  > A8 – A11 VM 'Leri 3/2021 tarihinde kullanımdan kaldırma için planlanmaktadır. Daha fazla bilgi için bkz. [HPC geçiş kılavuzu](https://azure.microsoft.com/resources/hpc-migration-guide/).

- [B serisi Burstable](sizes-b-series-burstable.md) VM 'Ler, Web sunucuları, küçük veritabanları ve geliştirme ve test ortamları gibi sürekli olarak CPU 'nun tam performansına gerek olmayan iş yükleri için idealdir. Bu iş yükleri genellikle ani performans gereksinimlerine sahiptir. B serisi, bu müşterilere VM 'nin temel performansından daha küçük bir performans üzerinden sanal makine örneğinin krediler oluşturmasını sağlayan bir fiyat bilincine sahip bir VM boyutu satın alma olanağı sağlar. VM 'nin birikmiş kredisi olduğunda, uygulamanız daha yüksek CPU performansı gerektirdiğinde CPU 'nun en fazla %100 ' ü kullanarak VM 'nin ana hat üzerinde veri bloğu oluşturabilir.

- [Dav4 ve Dasv4 serisi](dav4-dasv4-series.md) , çok iş parçacıklı BIR yapılandırmada AMD 'Nin 2.35 GHz EPıC<sup>TM</sup> 7452 işlemcisini kullanan yeni boyutlardır. bu L3 önbelleği, her 8 çekirdeğe 256 kadar, genel amaçlı iş yüklerini çalıştırmaya yönelik müşteri seçeneklerini artırır. Dav4-Series ve Dasv4 serisi, D & Dsv3 serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

- [DCv2 serisi](dcv2-series.md) , genel bulutta işlendiği sırada verilerinizin ve kodunuzun gizliliğini ve bütünlüğünü korumaya yardımcı olabilir. Bu makineler, SGX teknolojisini içeren en son Intel XEON E-2288G Işlemcisi tarafından desteklenir. Intel Turbo Boost teknolojisiyle bu makineler, 5.0 GHz 'ye kadar sürebilir. DCv2 serisi örnekler, müşterilerin, kullanıldığı sırada kod ve verilerini korumak için güvenli şifreleme tabanlı uygulamalar oluşturmasına imkan tanır.

- [Dv2 ve Dsv2 serisi](dv2-dsv2-series.md) Özgün D serisi için bir takip eden VM 'Ler, daha güçlü bir CPU ve en iyi CPU-bellek yapılandırmasına sahiptir ve bu da üretim iş yüklerinin çoğu için uygun hale getirir. Dv2 serisi, D serisinden daha hızlı %35 daha hızlıdır. Dv2 serisi, Intel Turbo Boost Technology 2,0 ile Intel® Xeon® 8171M 2.1 GHz (ufuk Gölü), Intel® Xeon® E5-2673 v4 2,3 GHz (çok Iyi) veya Intel® Xeon® E5-2673 v3 2,4 GHz (Haswell) işlemcileri üzerinde çalışır. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

- [Dv3 ve Dsv3 serisi](dv3-dsv3-series.md) VM 'Ler, bir hiper iş yükleri için daha iyi bir değer teklifi sağlayan Intel® Xeon® 8171M 2.1 GHz (ufuk Gölü), Intel® Xeon® E5-2673 v4 2,3 GHz (çok Iyi) veya Intel® Xeon® E5-2673 v3 2,4 GHz (Haswell) işlemcileri üzerinde çalışır. Bellek ve ağ sınırları, hiper iş parçacığına geçme ile hizalanmak üzere bir çekirdek temelinde ayarlanırken, bellek (~ 3,5 GiB/vCPU 'dan 4 GiB/vCPU 'ya) genişletildi. Dv3 serisi artık D/dv2-Series ' in yüksek bellek sanal makine boyutlarına sahiptir; bunlar bellek için iyileştirilmiş [Ev3 ve Esv3-serisine](ev3-esv3-series.md)taşınmıştır.

- [Ddv4 ve Ddsv4 serisi](ddv4-ddsv4-series.md) VM 'Ler 2. nesil Intel &reg; Xeon &reg; Platinum 8272CL (Cascade Lake) işlemcileri üzerinde çalışarak genel amaçlı iş yükleri için daha iyi bir değer teklifi sağlar. 3,4 GHz 'nin tüm çekirdek Turbo saat hızına sürekli olarak sahiptir ve [Intel &reg; Turbo Boost Technology 2,0](https://www.intel.com/content/www/us/en/architecture-and-technology/turbo-boost/turbo-boost-technology.html), [Intel &reg; hiper iş parçacıklı teknoloji](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html) ve [Intel &reg; Gelişmiş vektör Uzantıları 512 (Intel AVX-512)](https://www.intel.com/content/www/us/en/architecture-and-technology/avx-512-overview.html)özelliklerine sahiptir. 


## <a name="other-sizes"></a>Diğer boyutlar

- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
