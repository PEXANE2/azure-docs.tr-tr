---
title: NCas T4 v3-serisi
description: NCas T4 v3 serisi VM 'Leri için belirtim.
services: virtual-machines
ms.subservice: sizes
author: vikancha-MSFT
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 08/10/2020
ms.author: vikancha
ms.openlocfilehash: d07da12ecef7dfc6cf1a6df67f6beae01c4573d9
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88168125"
---
# <a name="ncast4_v3-series"></a>NCasT4_v3 serisi 

NCasT4_v3 serisi sanal makineler, [NVIDIA Tesla T4](https://www.nvidia.com/en-us/data-center/tesla-t4/) GPU 'LARı ve AMD Epde 7V12 (Roma) CPU 'lar tarafından desteklenir. VM 'Ler, en fazla 64, çok iş parçacıklı AMD EPIC 7V12 (Roma) işlemci çekirdeği ve 440 GiB 'a kadar, her biri 16 GB bellek içeren 4 KB 'a kadar NVıDıA T4 GPU özelliğine sahiptir. Bu sanal makineler CUDA, TensorFlow, Pytorch, Caffe ve diğer çerçeveleri kullanan ML ve AI iş yüklerini ve NVıDıA GRID teknolojisini kullanan grafik iş yüklerini çalıştırmak için idealdir. NCasT4_v3 serisi, çıkarım iş yüklerini çalıştırmak için idealdir.

<br>

ACU: 230-260

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

Dinamik Geçiş: desteklenmiyor

Güncelleştirmeleri koruyan bellek: desteklenmiyor

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC4as_T4_v3 |4 |28 |180 | 1 | 16 | 8 | 2 |
| Standard_NV8as_T4_v3 |8 |56 |360 | 1 | 16 | 16 | 4  |
| Standard_NV16as_T4_v3 |16 |110 |360 | 1 | 16 | 32 | 8  |
| Standard_NV64as_T4_v3 |64 |440 |2880 | 4 | 64 | 32 | 8  |


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

Windows veya Linux çalıştıran Azure NCasT4_v3 serisi VM 'lerin GPU Özellikleri avantajlarından yararlanmak için NVIDIA GPU sürücüleri yüklenmelidir.

NVIDIA GPU sürücülerini el ile yüklemek için desteklenen işletim sistemleri, sürücüler, yükleme ve doğrulama adımları için bkz. [Windows Için N SERISI GPU sürücü kurulumu](./windows/n-series-driver-setup.md) .

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
