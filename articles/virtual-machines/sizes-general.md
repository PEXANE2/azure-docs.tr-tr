---
title: Azure VM boyutları-genel amaçlı | Microsoft Docs
description: Azure 'daki sanal makineler için kullanılabilir olan farklı genel amaçlı boyutları listeler. Bu serideki boyutlarda sanal CPU 'lar, veri diskleri ve NIC 'lerin yanı sıra depolama aktarım hızı ve ağ bant genişliği hakkındaki bilgileri listeler.
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
ms.openlocfilehash: 674197bd4d1562d8492a2605d8929572d450af90
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77493585"
---
# <a name="general-purpose-virtual-machine-sizes"></a>Genel amaçlı sanal makine boyutları

Genel amaçlı VM boyutları dengeli CPU-bellek oranını sağlar. Test ve geliştirme için idealdir, küçük ve orta ölçekli veritabanları ve düşük düzeyli trafik Web sunucuları. Bu makalede genel amaçlı bilgi işlem teklifleri hakkında bilgi sağlanır.

- [AV2 serisi](av2-series.md) VM 'ler, çeşitli donanım türlerinde ve işlemcilerde dağıtılabilir. A serisi VM 'Ler, geliştirme ve test gibi giriş düzeyi iş yükleri için en uygun CPU performansına ve bellek yapılandırmalarına sahiptir. Dağıtıldığı donanımdan bağımsız olarak, çalışan örneğe tutarlı işlemci performansı sunmak için boyut donanıma göre genişletilir. Bu boyutun dağıtıldığı fiziksel donanımı belirlemek için Sanal Makinenin içinden sanal donanımı sorgulayın. Örnek kullanım örnekleri arasında geliştirme ve test sunucuları, düşük trafikli web sunucuları, küçük ve orta ölçekli veritabanları, kavram kanıtı ve kod depoları bulunur.

- [B serisi Burstable](sizes-b-series-burstable.md) VM 'Ler, Web sunucuları, küçük veritabanları ve geliştirme ve test ortamları gibi sürekli olarak CPU 'nun tam performansına gerek olmayan iş yükleri için idealdir. Bu iş yükleri genellikle ani performans gereksinimlerine sahiptir. B serisi, bu müşterilere VM 'nin temel performansından daha küçük bir performans üzerinden sanal makine örneğinin krediler oluşturmasını sağlayan bir fiyat bilincine sahip bir VM boyutu satın alma olanağı sağlar. VM 'nin birikmiş kredisi olduğunda, uygulamanız daha yüksek CPU performansı gerektirdiğinde CPU 'nun en fazla %100 ' ü kullanarak VM 'nin ana hat üzerinde veri bloğu oluşturabilir.

- [Dav4 ve Dasv4-Series](dav4-dasv4-series.md) , 128 MB 256 'a kadar L3 önbellek kullanan çok iş parçacıklı bir yapılandırmada, en fazla 128 MB 'lık, L3 önbelleğinin, genel amaçlı iş yüklerini çalıştırmaya yönelik müşteri seçeneklerini artıran her 8 çekirdeğe sahip olan<sup>Yeni 7452</sup> boyutlardır. Dav4-Series ve Dasv4 serisi, D & Dsv3 serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

- [DC Serisi](dc-series.md) , Azure 'daki bir sanal makine ailesidir ve bu, genel bulutta işlendiği sırada verilerinizin ve kodunuzun gizliliğini ve bütünlüğünü korumanıza yardımcı olabilir. Bu makineler, SGX teknolojisine sahip 3.7 GHz Intel® Xeon® E-2176G Işlemcisinin en son nesil tarafından desteklenir. Intel Turbo Boost teknolojisiyle bu makineler, 4.7 GHz 'ye kadar sürebilir. DC serisi örnekleri, müşterilerin, kullanıldığı sırada kod ve verilerini korumak için güvenli şifreleme tabanlı uygulamalar oluşturmasına imkan tanır.

- [Dv2 ve Dsv2 serisi](dv2-dsv2-series.md) Özgün D serisi için bir takip eden VM 'Ler, daha güçlü bir CPU ve en iyi CPU-bellek yapılandırmasına sahiptir ve bu da üretim iş yüklerinin çoğu için uygun hale getirir. Dv2 serisi, D serisinden daha hızlı %35 daha hızlıdır. Dv2 serisi, Intel Turbo Boost Technology 2,0 ile Intel® Xeon® 8171M 2.1 GHz (ufuk Gölü), Intel® Xeon® E5-2673 v4 2,3 GHz (çok Iyi) veya Intel® Xeon® E5-2673 v3 2,4 GHz (Haswell) işlemcileri üzerinde çalışır. Dv2 Serisi, D Serisi ile aynı bellek ve disk yapılandırmalarına sahiptir.

- [Dv3 ve Dsv3 serisi](dv3-dsv3-series.md) VM 'Ler, bir hiper iş yükleri için daha iyi bir değer teklifi sağlayan Intel® Xeon® 8171M 2.1 GHz (ufuk Gölü), Intel® Xeon® E5-2673 v4 2,3 GHz (çok Iyi) veya Intel® Xeon® E5-2673 v3 2,4 GHz (Haswell) işlemcileri üzerinde çalışır. Bellek ve ağ sınırları, hiper iş parçacığına geçme ile hizalanmak üzere bir çekirdek temelinde ayarlanırken, bellek (~ 3,5 GiB/vCPU 'dan 4 GiB/vCPU 'ya) genişletildi. Dv3 serisi artık D/dv2-Series ' in yüksek bellek sanal makine boyutlarına sahiptir; bunlar bellek için iyileştirilmiş [Ev3 ve Esv3-serisine](ev3-esv3-series.md)taşınmıştır.

Örnek D Serisi Kullanım örnekleri arasında kurumsal düzeyde uygulamalar, ilişkisel veritabanları, bellek içi önbelleğe alma ve analiz vardır.

## <a name="other-sizes"></a>Diğer boyutlar

- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.