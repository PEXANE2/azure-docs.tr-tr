---
title: H serisi VM'lerde yüksek performanslı bilgi işlem - Azure Sanal Makineler
description: HPC için optimize edilmiş H serisi VM'lerin özellikleri ve yetenekleri hakkında bilgi edinin.
author: vermagit
ms.author: amverma
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: overview
ms.date: 07/02/2019
ms.openlocfilehash: b3d5d003db89a11a013c3236a3afbe03ffe68557
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76271013"
---
# <a name="high-performance-computing-on-h-series-vms"></a>H serisi VM'lerde yüksek performanslı bilgi işlem

HB serisi ve HC serisi VM'lerde yüksek performanslı bilgi işlem (HPC), Azure'daki tüm VM'lerin en optimize edilmiş HPC performansını sağlar. HPC optimize vm'ler, akışkanlar, yağ ve gaz simülasyonları ve hava modellemesi gibi en zor matematiksel problemlerden bazılarını çözmek için kullanılır.

Bu makalede HB serisi ve HC serisi VM'lerin bazı temel özellikleri, bu VM'lerin neden HPC senaryolarında iyi performans gösterdiği ve nasıl başlatılacakları yer alır.

## <a name="features-and-capabilities"></a>Özellikler ve yetenekler

HB serisi ve HC serisi VM'ler en iyi HPC performansını, ileti geçiş arabirimini (MPI) ölçeklenebilirliği ve HPC iş yükleri için maliyet verimliliğini sağlamak üzere tasarlanmıştır.

### <a name="message-passing-interface"></a>İleti geçme arabirimi

HB serisi ve HC serisi hemen hemen tüm MPI türleri ve sürümlerini destekler. OpenMPI, MVAPICH2, Platform MPI, Intel MPI ve tüm uzaktan doğrudan bellek erişimi (RDMA) fiilleri şunlardır: En yaygın, desteklenen MPI türleri. Daha fazla bilgi için [bkz.](setup-mpi.md)

### <a name="rdma-and-infiniband"></a>RDMA ve InfiniBand

RDMA arabirimi HB serisi ve HC serisi VM'lerde standarttır. RDMA özellikli örnekler, HB serisi ve HC serisi sanal makineler için gelişmiş veri hızlarında (EDR) çalışan bir InfiniBand ağı üzerinden iletişim kurar. RDMA özellikli örnekler, bazı MPI uygulamalarının ölçeklenebilirliğini ve performansını artırabilir.

HB serisi ve HC serisi VM'leri destekleyen InfiniBand konfigürasyonu, tutarlı RDMA performansı için düşük çaplı tasarıma sahip, engelleyici olmayan yağ ağaçlarıdır.

HB serisi veya HC serisi VM'lerinizde InfiniBand kurulumu hakkında daha fazla bilgi edinmek için [InfiniBand'ı etkinleştirme](enable-infiniband.md) bilgisini görün.

## <a name="get-started"></a>Kullanmaya başlayın

İlk olarak, hangi H-serisi VM'i kullanacağınıza karar verin. HPC optimize edilmiş VM'ler hakkında ayrıntılı bilgi için [HB serisi genel bakış](hb-series-overview.md) ve HC serisi genel [bakış](hc-series-overview.md)alabilirsiniz. Teknik özellikler için yüksek [performanslı işlem VM boyutlarına](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-hpc)bakın.

Uygulamanız için bir VM seçip oluşturduktan sonra, InfiniBand'ı etkinleştirerek bunu yapılandırmanız gerekir. Hem Windows hem de Linux VM'lerinde InfiniBand'ı nasıl etkinleştireceğimiz öğrenmek için [bkz.](enable-infiniband.md)

HPC iş yüklerinin önemli bir bileşeni MPI'dir. HB serisi ve HC serisi hemen hemen tüm MPI türleri ve sürümlerini destekler. Daha fazla bilgi için [bkz.](setup-mpi.md)

VM serinizi seçtikten, Infiniband ve MPI'yi kurduktan sonra, HPC iş yüklerinizi oluşturmaya hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

- Önemli farklılıklar ve teknik özellikler hakkında bilgi edinmek için [HB serisi genel bakışı](hb-series-overview.md) ve HC serisi genel [görünümünü](hc-series-overview.md) gözden geçirin.

- Daha yüksek bir düzey için, Çalışan HPC iş yüklerinin mimari görünümü için [Azure'da Yüksek Performanslı Bilgi İşlem 'e (HPC)](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/)bakın.
