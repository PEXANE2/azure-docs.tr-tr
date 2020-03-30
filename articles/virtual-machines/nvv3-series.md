---
title: NVv3 serisi - Azure Sanal Makineler
description: NVv3 serisi VM'ler için teknik özellikler.
services: virtual-machines
author: vikancha
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: d74b00a4bade956d3a511a47b0a6b0011b9fb212
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78267425"
---
# <a name="nvv3-series"></a>NVv3 serisi

NVv3 serisi sanal makineler Intel E5-2690 v4 (Broadwell) CPU'lar ve Intel Hyper-Threading Teknolojisi ile [NVIDIA Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU ve NVIDIA GRID teknolojisi tarafından desteklenmektedir. Bu sanal makineler, müşterilerin verilerini görselleştirmek, sonuçları görüntülemek, CAD üzerinde çalışmak veya içeriği işlemek ve aktarmak için simüle etmek istedikleri GPU hızlandırılmış grafik uygulamaları ve sanal masaüstü bilgisayarlar için hedeflenir. Ayrıca, bu sanal makineler kodlama ve işleme gibi tek duyarlıklı iş yüklerini çalıştırabilir. NVv3 sanal makineleri Premium Depolama desteği ve selefi NV serisi ile karşılaştırıldığında iki kat sistem belleği (RAM) ile birlikte gelir.  

NVv3 örneklerindeki her GPU bir GRID lisansıyla birlikte gelir. Bu lisans, bir NV örneğini tek bir kullanıcı için sanal iş istasyonu olarak kullanma esnekliği sağlar veya 25 eşzamanlı kullanıcı sanal bir uygulama senaryosu için VM'ye bağlanabilir.

Premium Depolama önbelleğe alma: Desteklenen

Canlı Geçiş: Desteklenmiyor

Bellek Koruma Güncelleştirmeleri: Desteklenmiyor

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | Maksimum cached disk işçıktısı: IOPS/MBps | En fazla NIC | Sanal İş İstasyonları | Sanal Uygulamalar |
|---|---|---|---|---|---|---|---|---|---|---|
| Standard_NV12s_v3 |12 | 112 | 320  | 1 | 8  | 12 | 20000/200 | 4 | 1 | 25  |
| Standard_NV24s_v3 |24 | 224 | 640  | 2 | 16 | 24 | 40000/400 | 8 | 2 | 50  |
| Standard_NV48s_v3 |48 | 448 | 1280 | 4 | 32 | 32 | 80000/800 | 8 | 4 | 100 |

1 GPU = M60 kartın yarısı.

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
