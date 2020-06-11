---
title: NVv4 serisi
description: NVv4 serisi VM 'Ler için Özellikler.
services: virtual-machines
ms.subservice: sizes
author: vikancha
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 856ad9cede58b7a8dd0856cac9568832702d4b23
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84678070"
---
# <a name="nvv4-series"></a>NVv4 serisi 

NVv4 serisi sanal makineler, [AMD Radeon INSTINCT MI25](https://www.amd.com/en/products/professional-graphics/instinct-mi25) GPU 'LARı ve AMD epi dili 7V12 (Roma) CPU 'ları tarafından desteklenir. NVv4-Series ile Azure, kısmi GPU 'lara sahip sanal makineler ile tanışın. 16 GiB çerçeve arabelleğiyle tam bir GPU 'ya 2 GiB çerçeve arabelleğinden 1 GB 'den başlayan GPU hızlandırılmış grafik uygulamaları ve sanal masaüstleri için doğru boyutlu sanal makineyi seçin. NVv4 sanal makineler Şu anda yalnızca Windows Konuk işletim sistemini destekliyor.

<br>

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

Dinamik Geçiş: desteklenmiyor

Güncelleştirmeleri koruyan bellek: desteklenmiyor

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV4as_v4 |4 |14 |88 | 1/8 | 2 | 4 | 2 |
| Standard_NV8as_v4 |8 |28 |176 | 1/4 | 4 | 8 | 4 |
| Standard_NV16as_v4 |16 |56 |352 | 1/2 | 8 | 16 | 8 |
| Standard_NV32as_v4 |32 |112 |704 | 1 | 16 | 32 | 8 |

<sup>1</sup> NVv4 serisi VM 'ler özelliği AMD eşzamanlı çoklu Iş parçacığı teknolojisi

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

Windows çalıştıran Azure NVv4 serisi VM 'lerin GPU Özellikleri avantajlarından yararlanmak için, AMD GPU sürücüleri yüklenmelidir.

AMD GPU sürücülerini el ile yüklemek için desteklenen işletim sistemleri, sürücüler, yükleme ve doğrulama adımları için bkz. [Windows Için N SERISI AMD GPU sürücü kurulumu](./windows/n-series-amd-driver-setup.md) .

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
