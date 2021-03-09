---
title: HB serisi
description: HB Serisi VM 'Ler için Özellikler.
author: vermagit
ms.service: virtual-machines
ms.subservice: hpc
ms.topic: conceptual
ms.date: 03/08/2021
ms.author: amverma
ms.reviewer: jushiman
ms.openlocfilehash: 65bf943c57151d209e60aff67892c3ef9c71cb36
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102502187"
---
# <a name="hb-series"></a>HB serisi

HB Serisi VM 'Ler, akıcı Dynamics, açık sınırlı öğe analizi ve hava durumu modelleme gibi bellek bant genişliğine göre çalışan uygulamalar için iyileştirilmiştir. HB VM 'Ler özelliği 60 AMD EPIC 7551 işlemci çekirdekleri, CPU çekirdeği başına 4 GB RAM ve eşzamanlı çoklu iş parçacığı yok. Bir HB VM, en fazla 260 GB/sn bellek bant genişliği sağlar.

HB Serisi VM 'Ler özelliği 100 GB/sn Mellanox EDR InfiniBand. Bu VM 'Ler, iyileştirilmiş ve tutarlı RDMA performansı için engelleyici olmayan bir FAT ağacına bağlanır. Bu VM 'Ler, uyarlamalı yönlendirmeyi ve dinamik bağlı taşımayı (Standart RC ve UD aktarımlara ek olarak DCT) destekler. Bu özellikler uygulama performansını, ölçeklenebilirliği ve tutarlılığı geliştirir ve kullanımları önerilir.

[Acu](acu.md): 199-216<br>
[Premium Depolama](premium-storage-performance.md): desteklenir<br>
[Premium depolama önbelleği](premium-storage-performance.md): desteklenir<br>
[Dinamik geçiş](maintenance-and-updates.md): desteklenmiyor<br>
[Güncelleştirmeleri koruyan bellek](maintenance-and-updates.md): desteklenmiyor<br>
[VM oluşturma desteği](generation-2.md): 1. ve 2. nesil<br>
[Hızlandırılmış ağ](../virtual-network/create-vm-accelerated-networking-cli.md): desteklenir (performans ve olası sorunlar hakkında[daha fazla bilgi edinin](https://techcommunity.microsoft.com/t5/azure-compute/accelerated-networking-on-hb-hc-hbv2-and-ndv2/ba-p/2067965) ) <br>
[Kısa ömürlü işletim sistemi diskleri](ephemeral-os-disks.md): desteklenmez <br>
<br>

| Boyut | Sanal işlemci | İşlemci | Bellek (GiB) | Bellek bant genişliği GB/sn | Taban CPU sıklığı (GHz) | Tüm çekirdekler sıklığı (GHz, tepe) | Tek çekirdekli sıklık (GHz, tepe) | RDMA performansı (GB/sn) | MPı desteği | Geçici depolama (GiB) | Maksimum veri diskleri | En fazla Ethernet vNIC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HB60rs | 60 | AMD EPYıC 7551 | 228 | 263 | 2.0 | 2.55 | 2.55 | 100 | Tümü | 700 | 4 | 8 |

Temeldeki [mimari](./workloads/hpc/hb-series-overview.md)ve HB Serisi VM 'nin beklenen [performansı](./workloads/hpc/hb-series-performance.md) hakkında daha fazla bilgi edinin.

[!INCLUDE [hpc-include.md](./workloads/hpc/includes/hpc-include.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Işlem Tech Community bloglarında](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute)en son Duyurular, HPC iş yükü örnekleri ve performans sonuçları hakkında bilgi edinin.
- Çalıştırılan HPC iş yüklerinin daha yüksek düzey mimari görünümü için bkz. [Azure 'Da yüksek performanslı bilgi işlem (HPC)](/azure/architecture/topics/high-performance-computing/).
- Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
