---
title: İyileştirilmiş gecikme süresi için VM 'Leri birlikte bulun
description: Azure VM kaynakları ile gecikme süresini nasıl iyileştirebileceğinizi öğrenin.
author: cynthn
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: eb7b5d3b477a511432dbd3ee8a6b382002aaab44
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96488481"
---
# <a name="co-locate-resource-for-improved-latency"></a>Gelişmiş gecikme için kaynağı birlikte bulun

Uygulamanızı Azure 'da dağıttığınızda, örnekleri bölgeler veya kullanılabilirlik alanları arasında yaymak, uygulamanızın genel performansını etkileyebilecek ağ gecikmesi oluşturur. 


## <a name="proximity-placement-groups"></a>Yakınlık yerleştirme grupları 

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell kullanarak bir sanal makineyi bir [yakınlık yerleşimi grubuna](proximity-placement-groups.md) dağıtın.

[Ağ gecikmesini test](../../virtual-network/virtual-network-test-latency.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)etme hakkında bilgi edinin.

[Ağ aktarım hızını en uygun hale getirmeyi](../../virtual-network/virtual-network-optimize-network-bandwidth.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)öğrenin.  

[SAP uygulamalarıyla yakınlık yerleştirme gruplarını nasıl kullanacağınızı](../workloads/sap/sap-proximity-placement-scenarios.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)öğrenin.