---
title: NDv2-Series-Azure sanal makineleri
description: NDv2 serisi VM 'Ler için Özellikler.
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 2d38b69e468f766d21a38bda1c2da3daae50d900
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77663717"
---
# <a name="updated-ndv2-series-preview"></a>NDv2-Series güncelleştirildi (Önizleme)

NDv2 serisi sanal makine, en zorlu GPU hızlandırmalı AI, makine öğrenimi, simülasyon ve HPC iş yüklerinin ihtiyaçları için tasarlanan GPU ailesine yeni bir ektir.

NDv2, her biri 32 GB GPU belleği bulunan 8 NVıDıA Tesla V100 NVLINK bağlantılı GPU 'Lar tarafından desteklenir. Her NDv2 VM 'nin ayrıca 40 hiper olmayan Intel Xeon Platinum 8168 (ufuk Gölü) çekirdeği ve 672 GiB sistem belleği vardır.

NDv2 örnekleri, CUDA GPU ile iyileştirilmiş hesaplama kernels ' lerini kullanan HPC ve AI, ML ve analiz araçlarının yanı sıra TensorFlow, Pytorch, Caffe, RAPıDS ve diğer gibi GPU hızlandırmasını destekleyen birçok AI, ML ve analiz aracını sağlar çerçeveleri.

Kritik öneme sahip olan NDv2, her iki hesaplama için de (VM başına 8 GPU) ve genişleme (birden fazla VM birlikte çalışan çok sayıda sanal makine) iş yükleri için oluşturulmuştur. NDv2 serisi artık, AI ve ML için dağıtılmış eğitim dahil olmak üzere paralel senaryolar için yüksek performanslı Kümelemeye izin veren 100-Gigabit InfiniBand EDR arka uç ağını destekler. Bu arka uç ağı, NVıDıA 'nin NCCL2 kitaplıkları tarafından çalıştırılan ve GPU 'Ların sorunsuz kümelemesine izin veren tüm önemli InfiniBand protokollerini destekler.


> [!NOTE]
> ND40rs_v2 VM 'de [InfiniBand etkinleştirilirken](https://docs.microsoft.com/azure/virtual-machines/workloads/hpc/enable-infiniband) lütfen 4.7-1.0.0.1 Mellanox ofed sürücüsünü kullanın.
>
> Daha fazla GPU belleği nedeniyle, yeni ND40rs_v2 VM [2. nesil sanal makinelerin](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) ve Market görüntülerinin kullanılmasını gerektirir. 
>
> [NDv2 sanal makine önizlemesine erken erişim istemek için kaydolun.](https://aka.ms/AzureNDrv2Preview)
>
> Lütfen unutmayın: GPU başına 16 GB bellek içeren ND40s_v2 artık önizleme için kullanılamaz ve güncelleştirilmiş ND40rs_v2 yerini almıştır.

<br>

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

InfiniBand: destekleniyor

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD): GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | En yüksek ağ bant genişliği | En fazla NIC |
|---|---|---|---|---|---|---|---|---|---|
| Standard_ND40rs_v2 | 40 | 672 | 2948 | 8 V100 32 GB (NVLink) | 16 | 32 | 80000/800 | 24000 Mbps | 8 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

Azure N serisi VM 'lerin GPU yeteneklerini avantajlarından yararlanmak için NVıDıA GPU sürücüleri yüklenmelidir.

[NVıDıA GPU sürücü uzantısı](./extensions/hpccompute-gpu-windows.md) , bir N serisi VM 'ye uygun NVIDIA CUDA veya kılavuz sürücülerini yükleme. Azure portal veya Azure PowerShell veya Azure Resource Manager şablonları gibi araçları kullanarak uzantıyı yükler veya yönetir. Desteklenen işletim sistemleri ve dağıtım adımları için [NVıDıA GPU sürücü uzantısı belgelerine](./extensions/hpccompute-gpu-windows.md) bakın. VM uzantıları hakkında genel bilgi için bkz. [Azure sanal makine uzantıları ve özellikleri](/.extensions/overview.md).

NVıDıA GPU sürücülerini el ile yüklemeyi tercih ederseniz desteklenen işletim sistemleri, sürücüler, yükleme ve doğrulama adımları için bkz. [Windows Için n SERISI GPU sürücü kurulumu](./windows/n-series-driver-setup.md) veya [Linux IÇIN n serisi GPU sürücü kurulumu](./linux/n-series-driver-setup.md) .

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
