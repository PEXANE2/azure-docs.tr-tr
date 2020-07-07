---
title: İyileştirilmiş gecikme süresi için VM 'Leri birlikte bulun
description: Azure VM kaynakları ile gecikme süresini nasıl iyileştirebileceğinizi öğrenin.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: 066b6d10dbe07b902abfd15565c5ccf1a5e9c115
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82083181"
---
# <a name="co-locate-resource-for-improved-latency"></a>Gelişmiş gecikme için kaynağı birlikte bulun

Uygulamanızı Azure 'da dağıttığınızda, örnekleri bölgeler veya kullanılabilirlik alanları arasında yaymak, uygulamanızın genel performansını etkileyebilecek ağ gecikmesi oluşturur. 


## <a name="proximity-placement-groups"></a>Yakınlık yerleştirme grupları 

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell kullanarak bir sanal makineyi bir [yakınlık yerleşimi grubuna](proximity-placement-groups.md) dağıtın.

[Ağ gecikmesini test](https://aka.ms/TestNetworkLatency?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)etme hakkında bilgi edinin.

[Ağ aktarım hızını en uygun hale getirmeyi](https://docs.microsoft.com/azure/virtual-network/virtual-network-optimize-network-bandwidth?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)öğrenin.  

[SAP uygulamalarıyla yakınlık yerleştirme gruplarını nasıl kullanacağınızı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)öğrenin.