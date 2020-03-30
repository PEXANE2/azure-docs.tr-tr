---
title: NV serisi - Azure Sanal Makineler
description: NV serisi VM'ler için teknik özellikler.
services: virtual-machines
author: vikancha
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 7fd9fff87564b790915acf01373c078c2012371e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273867"
---
# <a name="nv-series"></a>NV serisi

NV serisi sanal makineler, müşterilerin verilerini veya simülasyonlarını görselleştirebildiği masaüstü hızlandırılmış uygulamalar ve sanal masaüstü bilgisayarlar için [NVIDIA Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU'ları ve NVIDIA GRID teknolojisi ile desteklenmektedir. Kullanıcılar, üstün grafik yeteneği elde etmek ve kodlama ve işleme gibi tek duyarlıklı iş yüklerini çalıştırmak için NV örneklerinde grafik yoğun iş akışlarını görselleştirebilir. NV serisi VM'ler de Intel Xeon E5-2690 v3 (Haswell) CPU'lar tarafından desteklenmektedir.

NV örneklerindeki her GPU bir GRID lisansıyla birlikte gelir. Bu lisans, bir NV örneğini tek bir kullanıcı için sanal iş istasyonu olarak kullanma esnekliği sağlar veya 25 eşzamanlı kullanıcı sanal bir uygulama senaryosu için VM'ye bağlanabilir.

Premium Depolama: Desteklenmiyor

Premium Depolama önbelleğe alma: Desteklenmiyor

Canlı Geçiş: Desteklenmiyor

Bellek Koruma Güncelleştirmeleri: Desteklenmiyor

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU bellek: GiB | Maksimum veri diskleri | En fazla NIC | Sanal İş İstasyonları | Sanal Uygulamalar |
|---|---|---|---|---|---|---|---|---|---|
| Standard_NV6  | 6  | 56  | 340  | 1 | 8  | 24 | 1 | 1 | 25  |
| Standard_NV12 | 12 | 112 | 680  | 2 | 16 | 48 | 2 | 2 | 50  |
| Standard_NV24 | 24 | 224 | 1440 | 4 | 32 | 64 | 4 | 4 | 100 |

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
