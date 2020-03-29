---
title: NCv2 serisi - Azure Sanal Makineler
description: NCv2 serisi VM'ler için teknik özellikler.
services: virtual-machines
author: vikancha
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 3643fbabef08d890ce121d41a9bc1eb40c88459d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273921"
---
# <a name="ncv2-series"></a>NCv2 serisi

NCv2 serisi VM'ler [NVIDIA Tesla P100](https://www.nvidia.com/data-center/tesla-p100/) GPU'ları tarafından desteklenmektedir. Bu GPU'lar NC serisinin hesaplama performansını 2 kat daha fazla sağlayabilir. Müşteriler rezervuar modelleme, DNA dizilemesi, protein analizi, Monte Carlo simülasyonları ve diğerleri gibi geleneksel HPC iş yükleri için bu güncelleştirilmiş GPU'lardan yararlanabilirler. GPU'lara ek olarak, NCv2 serisi VM'ler intel xeon E5-2690 v4 (Broadwell) CPU'ları ile de desteklenmektedir.

NC24rs v2 yapılandırması, sıkı bir şekilde birleştirilmiş paralel bilgi işlem iş yükleri için optimize edilmiş düşük gecikmeli, yüksek iş çıkışlı ağ arabirimi sağlar.

Premium Depolama: Desteklenen

Premium Depolama önbelleğe alma: Desteklenen

Canlı Geçiş: Desteklenmiyor

Bellek Koruma Güncelleştirmeleri: Desteklenmiyor

> [!IMPORTANT]
> Bu VM serisi için, aboneliğinizdeki vCPU (çekirdek) kotası başlangıçta her bölgede 0 olarak ayarlanır. Kullanılabilir bir [bölgede](https://azure.microsoft.com/regions/services/)bu seri için [vCPU kotası artışı isteyin.](../azure-supportability/resource-manager-core-quotas-request.md)
>
| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | Maksimum cached disk işçıktısı: IOPS/MBps | En fazla NIC |
|---|---|---|---|---|---|---|---|---|
| Standard_NC6s_v2    | 6  | 112 | 736  | 1 | 16 | 12 | 20000/200 | 4 |
| Standard_NC12s_v2   | 12 | 224 | 1474 | 2 | 32 | 24 | 40000/400 | 8 |
| Standard_NC24s_v2   | 24 | 448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |
| Standard_NC24rs_v2* | 24 | 448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |

1 GPU = bir P100 kartı.

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
