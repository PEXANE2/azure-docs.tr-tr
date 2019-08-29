---
title: Azure Windows VM boyutları-GPU | Microsoft Docs
description: Azure 'da Windows sanal makineleri için kullanılabilen farklı GPU iyileştirilmiş boyutları listeler. Bu serideki boyutlarda vCPU sayısı, veri diskleri ve NIC 'lerin yanı sıra depolama aktarım hızı ve ağ bant genişliği hakkındaki bilgileri listeler.
services: virtual-machines-windows
documentationcenter: ''
author: jonbeck7
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/11/2019
ms.author: jonbeck
ms.openlocfilehash: 0e809690f0453806402c27773ad0029fc5f64be2
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70102355"
---
# <a name="gpu-optimized-virtual-machine-sizes"></a>GPU iyileştirilmiş sanal makine boyutları

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

Windows çalıştıran Azure N serisi VM 'lerin GPU Özellikleri avantajlarından yararlanmak için NVıDıA GPU sürücüleri yüklenmelidir. [NVıDıA GPU sürücü uzantısı](../extensions/hpccompute-gpu-windows.md) , bir N serisi VM 'ye uygun NVIDIA CUDA veya kılavuz sürücülerini yükleme. Azure portal veya Azure PowerShell veya Azure Resource Manager şablonları gibi araçları kullanarak uzantıyı yükler veya yönetir. Desteklenen işletim sistemleri ve dağıtım adımları için [NVıDıA GPU sürücü uzantısı belgelerine](../extensions/hpccompute-gpu-windows.md) bakın. VM uzantıları hakkında genel bilgi için bkz. [Azure sanal makine uzantıları ve özellikleri](../extensions/overview.md).

NVıDıA GPU sürücülerini el ile yüklemeyi tercih ederseniz desteklenen işletim sistemleri, sürücüler ve yükleme ve doğrulama adımları için bkz. [Windows Için N SERISI GPU sürücü kurulumu](n-series-driver-setup.md) .

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

## <a name="other-sizes"></a>Diğer boyutlar
- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar
Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.

