---
title: ND serisi-Azure sanal makineleri
description: ND serisi VM 'Ler için Özellikler.
author: vikancha-MSFT
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 8c229a72b706333a117645095bd96e44abbdf469
ms.sourcegitcommit: 9c262672c388440810464bb7f8bcc9a5c48fa326
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89418900"
---
# <a name="nd-series"></a>ND serisi

ND serisi sanal makineler, AI için tasarlanan GPU ailesine ve derin öğrenme iş yüklerine yeni bir ektir. Eğitim ve çıkarım için mükemmel performans sunar. ND örnekleri [NVIDIA Tesla P40](https://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPU 'Lar ve Intel Xeon E5-2690 v4 (çok Iyi) CPU 'lar tarafından desteklenir. Bu örnekler Microsoft Cognitive Toolkit, TensorFlow, Caffe ve diğer çerçeveleri kullanan AI iş yükleri için tek duyarlıklı kayan nokta işlemleri için mükemmel bir performans sağlar. ND serisi aynı zamanda çok daha büyük bir GPU bellek boyutu (24 GB) sunarak çok daha büyük sinir net modellerini mümkün hale getirecek şekilde etkinleştirir. NC serisi gibi, ND serisi, RDMA aracılığıyla ikincil düşük gecikmeli, yüksek performanslı bir ağ ve InfiniBand bağlantısı sunan bir yapılandırma sunarak çok sayıda GPU 'yu kapsayan büyük ölçekli eğitim işleri çalıştırabilirsiniz.

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

Dinamik Geçiş: desteklenmiyor

Güncelleştirmeleri koruyan bellek: desteklenmiyor

> [!IMPORTANT]
> Bu VM Serisi için, aboneliğinizdeki bölge başına vCPU (çekirdek) kotası başlangıçta 0 olarak ayarlanır. [Kullanılabilir bir bölgede](https://azure.microsoft.com/regions/services/)bu seri Için [bir vCPU kota artışı isteyin](../azure-portal/supportability/resource-manager-core-quotas-request.md) .
>
| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | Önbelleğe alınmamış maksimum disk aktarım hızı: ıOPS/MBps | En fazla NIC |
|---|---|---|---|---|---|---|---|---|
| Standard_ND6s    | 6  | 112 | 736  | 1 | 24 | 12 | 20000/200 | 4 |
| Standard_ND12s   | 12 | 224 | 1474 | 2 | 48 | 24 | 40000/400 | 8 |
| Standard_ND24s   | 24 | 448 | 2948 | 4 | 24 | 32 | 80000/800 | 8 |
| Standard_ND24rs * | 24 | 448 | 2948 | 4 | 96 | 32 | 80000/800 | 8 |

1 GPU = bir P40 kart.

*RDMA özellikli

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

Azure N serisi VM 'lerin GPU yeteneklerini avantajlarından yararlanmak için NVıDıA GPU sürücüleri yüklenmelidir.

[NVıDıA GPU sürücü uzantısı](./extensions/hpccompute-gpu-windows.md) , bir N serisi VM 'ye uygun NVIDIA CUDA veya kılavuz sürücülerini yükleme. Azure portal veya Azure PowerShell veya Azure Resource Manager şablonları gibi araçları kullanarak uzantıyı yükler veya yönetir. Desteklenen işletim sistemleri ve dağıtım adımları için [NVıDıA GPU sürücü uzantısı belgelerine](./extensions/hpccompute-gpu-windows.md) bakın. VM uzantıları hakkında genel bilgi için bkz. [Azure sanal makine uzantıları ve özellikleri](./extensions/overview.md).

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
