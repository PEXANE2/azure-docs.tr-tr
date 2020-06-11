---
title: NDv2 serisi
description: NDv2 serisi VM 'Ler için Özellikler.
author: vikancha
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: article
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 739e8763cf0c42fac68cce6603d087aaa4e0d9f9
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84672203"
---
# <a name="updated-ndv2-series"></a>NDv2-Series güncelleştirildi

NDv2 serisi sanal makine, en zorlu GPU hızlandırmalı AI, makine öğrenimi, simülasyon ve HPC iş yüklerinin ihtiyaçları için tasarlanan GPU ailesine yeni bir ektir.

NDv2, her biri 32 GB GPU belleği bulunan 8 NVıDıA Tesla V100 NVLINK bağlantılı GPU 'Lar tarafından desteklenir. Her NDv2 VM 'nin ayrıca 40 hiper olmayan Intel Xeon Platinum 8168 (ufuk Gölü) çekirdeği ve 672 GiB sistem belleği vardır.

NDv2 örnekleri, CUDA GPU ile iyileştirilmiş hesaplama kernels ' lerini ve Masorflow, Pytorch, Caffe, RAPıDS ve diğer çerçeveler gibi GPU hızlandırmasını destekleyen birçok AI, ML ve analiz aracını kullanan HPC ve AI iş yükleri için mükemmel bir performans sağlar.

Kritik öneme sahip olan NDv2, her iki hesaplama için de (VM başına 8 GPU) ve genişleme (birden fazla VM birlikte çalışan çok sayıda sanal makine) iş yükleri için oluşturulmuştur. NDv2 serisi artık, AI ve ML için dağıtılmış eğitim dahil olmak üzere paralel senaryolar için yüksek performanslı Kümelemeye izin veren 100-Gigabit InfiniBand EDR arka uç ağını destekler. Bu arka uç ağı, NVıDıA 'nin NCCL2 kitaplıkları tarafından çalıştırılan ve GPU 'Ların sorunsuz kümelemesine izin veren tüm önemli InfiniBand protokollerini destekler.

> ND40rs_v2 VM 'de [InfiniBand etkinleştirilirken](https://docs.microsoft.com/azure/virtual-machines/workloads/hpc/enable-infiniband) lütfen 4.7-1.0.0.1 Mellanox ofed sürücüsünü kullanın.
>
> Daha fazla GPU belleği nedeniyle, yeni ND40rs_v2 VM [2. nesil sanal makinelerin](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) ve Market görüntülerinin kullanılmasını gerektirir. 
>
> Lütfen unutmayın: GPU başına 16 GB bellek içeren ND40s_v2 artık önizleme için kullanılamaz ve güncelleştirilmiş ND40rs_v2 yerini almıştır.

<br>

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

Dinamik Geçiş: desteklenmiyor

Güncelleştirmeleri koruyan bellek: desteklenmiyor

InfiniBand: destekleniyor

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD): GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | En yüksek ağ bant genişliği | En fazla NIC |
|---|---|---|---|---|---|---|---|---|---|
| Standard_ND40rs_v2 | 40 | 672 | 2948 | 8 V100 32 GB (NVLink) | 32 | 32 | 80000/800 | 24000 Mbps | 8 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

Azure N serisi VM 'lerin GPU yeteneklerini avantajlarından yararlanmak için NVıDıA GPU sürücüleri yüklenmelidir.

[NVıDıA GPU sürücü uzantısı](./extensions/hpccompute-gpu-linux.md) , bir N serisi VM 'ye uygun NVIDIA CUDA veya kılavuz sürücülerini yükleme. Azure portal veya Azure PowerShell veya Azure Resource Manager şablonları gibi araçları kullanarak uzantıyı yükler veya yönetir. VM uzantıları hakkında genel bilgi için bkz. [Azure sanal makine uzantıları ve özellikleri](./extensions/overview.md).

NVıDıA GPU sürücülerini el ile yüklemeyi tercih ederseniz bkz. [Linux Için N SERISI GPU sürücü kurulumu](./linux/n-series-driver-setup.md).

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
