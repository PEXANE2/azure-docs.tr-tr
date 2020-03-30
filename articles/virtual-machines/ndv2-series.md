---
title: NDv2 serisi - Azure Sanal Makineler
description: NDv2 serisi VM'ler için teknik özellikler.
services: virtual-machines
author: vikancha
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 8a2d275501816dd504130b255b73a752c5615f0d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247291"
---
# <a name="updated-ndv2-series"></a>Güncelleştirilmiş NDv2 serisi

NDv2 serisi sanal makine, gpu ailesine en zorlu GPU hızlandırılmış yapay gaz, makine öğrenimi, simülasyon ve HPC iş yüklerinin ihtiyaçları için tasarlanmış yeni bir ektir.

NDv2 8 NVIDIA Tesla V100 NVLINK bağlı GPU, her biri 32 GB GPU bellek ile desteklenmektedir. Her NDv2 VM ayrıca 40 non-HyperThreaded Intel Xeon Platinum 8168 (Skylake) çekirdek ve 672 GiB sistem belleği vardır.

NDv2 örnekleri, CUDA GPU tarafından optimize edilmiş hesaplama çekirdeklerini kullanan HPC ve AI iş yükleri ve TensorFlow, Pytorch, Caffe, RAPIDS ve diğer Gibi GPU ivmesini 'kullanıma hazır' olarak destekleyen birçok AI, ML ve analiz araçları için mükemmel performans sağlar Çerçeve.

Kritik olarak, NDv2 hem hesaplama açısından yoğun ölçeklendirme (VM başına 8 GPU'dan yararlanma) hem de ölçeklendirme (birlikte çalışan birden fazla VM'den yararlanma) iş yükleri için tasarlanmıştır. NDv2 serisi şimdi 100-Gigabit InfiniBand EDR arka uç ağ destekler, HPC VM HB serisi mevcut benzer, AI ve ML için dağıtılmış eğitim de dahil olmak üzere paralel senaryolar için yüksek performanslı kümeleme izin vermek. Bu arka uç ağı, NVIDIA'nın NCCL2 kitaplıkları tarafından kullanılanlar da dahil olmak üzere tüm büyük InfiniBand protokollerini destekler ve GPU'ların sorunsuz bir şekilde kümelanmasına olanak tanır.

> ND40rs_v2 VM'de [InfiniBand'ı etkinleştirirken](https://docs.microsoft.com/azure/virtual-machines/workloads/hpc/enable-infiniband) lütfen 4.7-1.0.0.1 Mellanox OFED sürücüsünü kullanın.
>
> Artan GPU belleği nedeniyle, yeni ND40rs_v2 [VM, Generation 2 VM'lerin](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) ve pazar yeri görüntülerinin kullanılmasını gerektirir. 
>
> Lütfen dikkat: GPU başına 16 GB belleğe sahip ND40s_v2 artık önizleme için kullanılamaz ve güncelleştirilmiş ND40rs_v2 tarafından eklenmiştir.

<br>

Premium Depolama: Desteklenen

Premium Depolama önbelleğe alma: Desteklenen

Canlı Geçiş: Desteklenmiyor

Bellek Koruma Güncelleştirmeleri: Desteklenmiyor

InfiniBand: Desteklenen

| Boyut | Sanal işlemci | Bellek: GiB | Geçici Depolama (SSD): GiB | GPU | GPU Bellek: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Maksimum ağ bant genişliği | En fazla NIC |
|---|---|---|---|---|---|---|---|---|---|
| Standard_ND40rs_v2 | 40 | 672 | 2948 | 8 V100 32 GB (NVLink) | 32 | 32 | 80000 / 800 | 24000 Mbps | 8 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

Azure N serisi VM'lerin GPU özelliklerinden yararlanmak için NVIDIA GPU sürücülerinin yüklenmesi gerekir.

[NVIDIA GPU Sürücü Uzantısı,](./extensions/hpccompute-gpu-linux.md) n serisi VM'ye uygun NVIDIA CUDA veya GRID sürücülerini yükler. Azure portalını veya Azure PowerShell veya Azure Kaynak Yöneticisi şablonları gibi araçları kullanarak uzantıyı yükleyin veya yönetin. VM uzantıları hakkında genel bilgi için [Azure sanal makine uzantıları ve özelliklerine](./extensions/overview.md)bakın.

NVIDIA GPU sürücülerini el ile yüklemeyi seçerseniz, [Linux için N serisi GPU sürücü kurulumuna](./linux/n-series-driver-setup.md)bakın.

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaç](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure bilgi [işlem birimlerinin (ACU)](acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.
