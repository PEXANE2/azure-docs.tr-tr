---
title: InfiniBand etkin H serisi ve N serisi VM 'lerde yüksek performanslı bilgi işlem-Azure sanal makineleri
description: ", HPC için iyileştirilmiş, InfiniBand etkin H serisi ve N serisi VM 'lerin özellikleri ve özellikleri hakkında bilgi edinin."
author: vermagit
ms.author: amverma
ms.service: virtual-machines
ms-subservice: hpc
ms.workload: infrastructure-services
ms.topic: overview
ms.date: 07/29/2020
ms.openlocfilehash: f9a672777fbc8a75cb6af10ba25232f8a274c894
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87543733"
---
# <a name="high-performance-computing-on-infiniband-enabled-h-series-and-n-series-vms"></a>InfiniBand etkin H serisi ve N serisi VM 'lerde yüksek performanslı bilgi işlem

Azure 'un InfiniBand özellikli H serisi ve N serisi VM 'Ler, çok sayıda gerçek bir HPC ve AI iş yükleri için liderlik sınıfı performans, Ileti geçirme arabirimi (MPı) ölçeklenebilirliği ve maliyet verimliliği sunmak üzere tasarlanmıştır. Bu yüksek performanslı bilgi işlem (HPC) en iyi duruma getirilmiş VM 'Ler, bilim ve mühendislik açısından, örneğin: akışkan Dynamics, dünya modellemesi, hava durumu benzetimleri gibi en yoğun en zorlu sorunlardan bazılarını çözümlemek için kullanılır.

Bu makaleler, Azure 'daki InfiniBand özellikli H serisi ve N serisi VM 'lerde ve ölçeklenebilirlik için VM 'lerde HPC ve AI iş yüklerinin en iyi yapılandırmasına nasıl başlamanızı açıklamaktadır.

## <a name="features-and-capabilities"></a>Özellikler ve yetenekler

InfiniBand etkin H serisi ve N serisi VM 'Ler, HPC iş yükleri için en iyi HPC performansını, MPı ölçeklenebilirliğini ve maliyet verimliliğini sağlamak üzere tasarlanmıştır. VM 'lerin özellikleri ve yetenekleri hakkında daha fazla bilgi edinmek için bkz. [H serisi](../../sizes-hpc.md) ve [N serisi](../../sizes-gpu.md) VM 'ler.

### <a name="rdma-and-infiniband"></a>RDMA ve InfiniBand

[RDMA özellikli](../../sizes-hpc.md#rdma-capable-instances) [H serisi](../../sizes-hpc.md) ve [N serisi](../../sizes-gpu.md) VM 'ler, düşük gecikme süresi ve yüksek bant genişliğine sahip InfiniBand ağı üzerinden iletişim kurar. Bu tür bir bağlantı üzerinden RDMA özelliği, dağıtılmış düğüm HPC ve AI iş yüklerinin ölçeklenebilirliğini ve performansını artırmak için önemlidir. InfiniBand etkin H serisi ve N serisi VM 'Ler, en iyileştirilmiş ve tutarlı RDMA performansı için düşük çapta bir tasarıma sahip engelleyici olmayan bir FAT ağacına bağlanır.
InfiniBand özellikli VM 'lerde InfiniBand ayarlama hakkında daha fazla bilgi edinmek için bkz. [InfiniBand 'Yi etkinleştirme](enable-infiniband.md) .

### <a name="message-passing-interface"></a>İleti geçirme arabirimi

SR-ıOV etkin H serisi ve N serisi, neredeyse tüm MPı kitaplıklarını ve sürümlerini destekler. En yaygın, desteklenen MPı kitaplıklarının bazıları şunlardır: Intel MPI, OpenMPI, MPICH, MVAPICH2, platform MPı ve tüm uzak doğrudan bellek erişimi (RDMA) fiilleri.
Desteklenen çeşitli MPı kitaplıklarını ve en iyi yapılandırmalarını yükleme hakkında daha fazla bilgi edinmek için bkz. [MPI ayarlama](setup-mpi.md) .

## <a name="get-started"></a>başlarken

İlk adım, VM belirtimleri ve [RDMA özelliğine](../../sizes-hpc.md#rdma-capable-instances)göre iş yükü için en uygun [h serisi](../../sizes-hpc.md) ve [N serisi](../../sizes-gpu.md) sanal makine türünü seçmemize yöneliktir.
İkinci olarak, InfiniBand 'yi etkinleştirerek sanal makineyi yapılandırın. Bu işlemi gerçekleştirmek için, sürücülerle birlikte iyileştirilmiş VM görüntülerini kullanma gibi çeşitli yöntemler vardır; bkz. [Linux iyileştirmesi](configure.md) ve Ayrıntılar için [InfiniBand 'ı etkinleştirin](enable-infiniband.md) .
Üçüncü olarak, dağıtılmış düğüm iş yükleri için MPı seçme ve yapılandırma kritiktir. Ayrıntılar için bkz. [MPI 'Yi ayarlama](setup-mpi.md) .
Dördüncü, performans ve ölçeklenebilirlik için, VM ailesine özgü kılavuzlardan yararlanarak [HB-Serisi genel bakış](hb-series-overview.md) ve [HC Serisi genel bakış](hc-series-overview.md)gibi iş yüklerini en iyi şekilde yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

- InfiniBand etkin [H serisi](../../sizes-hpc.md) ve [N serisi](../../sizes-gpu.md) VM 'ler [yapılandırmak ve iyileştirmek](configure.md) hakkında bilgi edinin.
- Performans ve ölçeklenebilirlik için iş yüklerini en iyi şekilde yapılandırma hakkında bilgi edinmek için [HB Serisi genel bakış](hb-series-overview.md) ve [HC Serisi genel bakışı](hc-series-overview.md) gözden geçirin.
- En son duyurular ve bazı HPC örnekleri hakkında bilgi edinin ve [Azure Işlem teknik topluluk bloglarında](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute)bu sonuçları elde edin.
- Çalıştırılan HPC iş yüklerinin daha yüksek düzey mimari görünümü için bkz. [Azure 'Da yüksek performanslı bilgi işlem (HPC)](/azure/architecture/topics/high-performance-computing/).
