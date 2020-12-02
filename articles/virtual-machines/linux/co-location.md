---
title: Linux VM 'lerini birlikte bulun
description: Linux için Azure VM kaynaklarının nasıl iyileştireolabileceğini öğrenin.
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: 304623ca50fd030ab6e016b940f8be52819c161a
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96500640"
---
# <a name="co-locate-resources-for-improved-latency"></a>Gecikme süresini geliştirmek için kaynakları birlikte konumlandırma

Uygulamanızı Azure 'da dağıttığınızda, örnekleri bölgeler veya kullanılabilirlik alanları arasında yaymak, uygulamanızın genel performansını etkileyebilecek ağ gecikmesi oluşturur. 

## <a name="proximity-placement-groups"></a>Yakınlık yerleştirme grupları

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure CLı kullanarak bir [yakınlık yerleşimi grubuna](proximity-placement-groups.md) VM dağıtın.

[Ağ gecikmesini test](../../virtual-network/virtual-network-test-latency.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)etme hakkında bilgi edinin.

[Ağ aktarım hızını en uygun hale getirmeyi](../../virtual-network/virtual-network-optimize-network-bandwidth.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)öğrenin.  

[SAP uygulamalarıyla yakınlık yerleştirme gruplarını nasıl kullanacağınızı](../workloads/sap/sap-proximity-placement-scenarios.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)öğrenin.