---
title: H serisi VM 'lerde yüksek performanslı bilgi işlem-Azure sanal makineleri
description: HPC için iyileştirilmiş H serisi VM 'lerinin özellikleri ve özellikleri hakkında bilgi edinin.
author: vermagit
ms.author: amverma
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: overview
ms.date: 07/02/2019
ms.openlocfilehash: 1f88496a6c474194089f20e128b2b92ec61e8559
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87083438"
---
# <a name="high-performance-computing-on-h-series-vms"></a>H serisi VM 'lerde yüksek performanslı bilgi işlem

HB Serisi ve HC Serisi VM 'lerde yüksek performanslı bilgi işlem (HPC), Azure 'daki tüm VM 'Lerde en iyi duruma getirilmiş HPC performansını etkinleştirir. HPC için iyileştirilmiş VM 'Ler: akışkan dinamikleri, yağ ve gaz benzetimleri ve hava durumu modellemesi gibi en zor matematik sorunlarını gidermek için kullanılır.

Bu makalede, HB Serisi ve HC Serisi sanal makinelerin bazı önemli özellikleri, bu VM 'Lerin HPC senaryolarında iyi bir şekilde nasıl çalıştığı ve nasıl başlatılabileceği ele alınmaktadır.

## <a name="features-and-capabilities"></a>Özellikler ve yetenekler

HB Serisi ve HC Serisi VM 'Ler, HPC iş yükleri için en iyi HPC performansı, ileti geçirme arabirimi (MPı) ölçeklenebilirliğini ve maliyet verimliliğini sağlamak üzere tasarlanmıştır.

### <a name="message-passing-interface"></a>İleti geçirme arabirimi

HB Serisi ve HC Serisi neredeyse tüm MPı türlerini ve sürümlerini destekler. En yaygın, desteklenen MPı türlerinden bazıları şunlardır: OpenMPI, MVAPICH2, platform MPı, Intel MPı ve tüm uzak doğrudan bellek erişimi (RDMA) fiilleri. Daha fazla bilgi için bkz. [HPC Için Ileti geçirme arabirimini ayarlama](setup-mpi.md).

### <a name="rdma-and-infiniband"></a>RDMA ve InfiniBand

RDMA arabirimi, HB Serisi ve HC Serisi VM 'lerde standarttır. RDMA özellikli örnekler, HB Serisi ve HC Serisi sanal makineler için geliştirilmiş veri ücretleri (EDR) ile çalışan bir InfiniBand ağı üzerinden iletişim kurar. RDMA özellikli örnekler, bazı MPı uygulamalarının ölçeklenebilirliğini ve performansını artırabilir.

HB Serisi ve HC Serisi VM 'Leri destekleyen InfiniBand yapılandırması, tutarlı RDMA performansı için düşük çapta bir tasarıma sahip engellenmeyen FAT ağaçları.

HB Serisi veya HC Serisi sanal makinelerinize InfiniBand ayarlama hakkında daha fazla bilgi edinmek için bkz. [InfiniBand 'Yi etkinleştirme](enable-infiniband.md) .

## <a name="get-started"></a>başlarken

İlk olarak, hangi H serisi sanal makineyi kullanacağınızı belirleyin. HPC iyileştirilmiş VM 'Ler hakkında daha fazla bilgi için bkz. [HB-serinin genel bakış](hb-series-overview.md) ve [HC Serisi genel bakış](hc-series-overview.md). Belirtimler için bkz. [yüksek performanslı Işlem VM boyutları](../../sizes-hpc.md).

Uygulamanız için bir VM seçtikten ve oluşturduktan sonra, InfiniBand 'yi etkinleştirerek yapılandırmanız gerekir. InfiniBand 'i hem Windows hem de Linux VM 'lerde etkinleştirmeyi öğrenmek için bkz. [InfiniBand 'Yi etkinleştirme](enable-infiniband.md).

HPC iş yüklerinin kritik bir bileşeni MPı 'dir. HB Serisi ve HC Serisi neredeyse tüm MPı türlerini ve sürümlerini destekler. Daha fazla bilgi için bkz. [HPC Için Ileti geçirme arabirimini ayarlama](setup-mpi.md).

VM serisini seçtikten sonra, InfiniBand ve MPı 'yi ayarladıktan sonra HPC iş yüklerinizi oluşturmaya başlamaya hazırsınız demektir.

## <a name="next-steps"></a>Sonraki adımlar

- Temel farklılıklar ve özellikler hakkında bilgi edinmek için [HB Serisi genel bakış](hb-series-overview.md) ve [HC Serisi genel bakış](hc-series-overview.md) konusunu gözden geçirin.

- Çalıştırılan HPC iş yüklerinin daha yüksek bir düzeyi olan mimari görünümü için bkz. [Azure 'Da yüksek performanslı bilgi işlem (HPC)](/azure/architecture/topics/high-performance-computing/).
