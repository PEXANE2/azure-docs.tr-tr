---
title: ND serisi - Azure Sanal Makineler
description: ND serisi VM'ler için teknik özellikler.
services: virtual-machines
author: vikancha
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 163dc0d6ecd4f91273a82b04ce186f7a8df868af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79465065"
---
# <a name="nd-series"></a>ND serisi

ND serisi sanal makineler, Yapay A ivedilik ve Derin Öğrenme iş yükleri için tasarlanmış GPU ailesine yeni bir ektir. Onlar eğitim ve çıkarım için mükemmel bir performans sunuyoruz. ND örnekleri [NVIDIA Tesla P40](https://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPU'lar ve Intel Xeon E5-2690 v4 (Broadwell) CPU'lar tarafından desteklenmektedir. Bu örnekler, Microsoft Cognitive Toolkit, TensorFlow, Caffe ve diğer çerçeveleri kullanan AI iş yükleri için tek duyarlıklı kayan nokta işlemleri için mükemmel performans sağlar. ND serisi de çok daha büyük bir GPU bellek boyutu sunuyor (24 GB), çok daha büyük nöral ağ modelleri sığdırmak için sağlayan. NC serisi gibi ND serisi de, birçok GPU'ya yayılan büyük ölçekli eğitim işlerini çalıştırabilmeniz için ikincil düşük gecikmesüresi, RDMA üzerinden yüksek verim ağı ve InfiniBand bağlantısı içeren bir yapılandırma sunar.

Premium Depolama: Desteklenen

Premium Depolama önbelleğe alma: Desteklenen

Canlı Geçiş: Desteklenmiyor

Bellek Koruma Güncelleştirmeleri: Desteklenmiyor

> [!IMPORTANT]
> Bu VM serisi için, aboneliğinizdeki bölge başına vCPU (çekirdek) kotası başlangıçta 0 olarak ayarlanır. Kullanılabilir bir [bölgede](https://azure.microsoft.com/regions/services/)bu seri için [vCPU kotası artışı isteyin.](../azure-supportability/resource-manager-core-quotas-request.md)
>
| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | Maksimum cached disk işçıktısı: IOPS/MBps | En fazla NIC |
|---|---|---|---|---|---|---|---|---|
| Standard_ND6s    | 6  | 112 | 736  | 1 | 24 | 12 | 20000/200 | 4 |
| Standard_ND12s   | 12 | 224 | 1474 | 2 | 48 | 24 | 40000/400 | 8 |
| Standard_ND24s   | 24 | 448 | 2948 | 4 | 96 | 32 | 80000/800 | 8 |
| Standard_ND24rs* | 24 | 448 | 2948 | 4 | 96 | 32 | 80000/800 | 8 |

1 GPU = bir P40 kartı.

*RDMA özellikli

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

Azure N serisi VM'lerin GPU özelliklerinden yararlanmak için NVIDIA GPU sürücülerinin yüklenmesi gerekir.

[NVIDIA GPU Sürücü Uzantısı,](./extensions/hpccompute-gpu-windows.md) n serisi VM'ye uygun NVIDIA CUDA veya GRID sürücülerini yükler. Azure portalını veya Azure PowerShell veya Azure Kaynak Yöneticisi şablonları gibi araçları kullanarak uzantıyı yükleyin veya yönetin. Desteklenen işletim sistemleri ve dağıtım adımları için [NVIDIA GPU Sürücü Uzantısı belgelerine](./extensions/hpccompute-gpu-windows.md) bakın. VM uzantıları hakkında genel bilgi için [Azure sanal makine uzantıları ve özelliklerine](./extensions/overview.md)bakın.

NVIDIA GPU sürücülerini el ile yüklemeyi seçerseniz, desteklenen işletim sistemleri, sürücüler, kurulum ve doğrulama adımları için Linux için Windows veya N serisi GPU sürücü [kurulumu için N serisi GPU sürücü kurulumuna](./windows/n-series-driver-setup.md) bakın. [N-series GPU driver setup for Linux](./linux/n-series-driver-setup.md)

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaç](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure bilgi [işlem birimlerinin (ACU)](acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.
