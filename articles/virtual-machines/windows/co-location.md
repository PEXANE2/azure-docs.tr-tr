---
title: İyileştirilmiş gecikme süresi için VM 'Leri birlikte bulun
description: Azure VM kaynakları ile gecikme süresini nasıl iyileştirebileceğinizi öğrenin.
author: cynthn
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: b5a3c0a582b1e9dfbcf81968ebc9d0c7a0a4f75e
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87288314"
---
# <a name="co-locate-resource-for-improved-latency"></a>Gelişmiş gecikme için kaynağı birlikte bulun

Uygulamanızı Azure 'da dağıttığınızda, örnekleri bölgeler veya kullanılabilirlik alanları arasında yaymak, uygulamanızın genel performansını etkileyebilecek ağ gecikmesi oluşturur. 


## <a name="proximity-placement-groups"></a>Yakınlık yerleştirme grupları 

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell kullanarak bir sanal makineyi bir [yakınlık yerleşimi grubuna](proximity-placement-groups.md) dağıtın.

[Ağ gecikmesini test](https://aka.ms/TestNetworkLatency?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)etme hakkında bilgi edinin.

[Ağ aktarım hızını en uygun hale getirmeyi](../../virtual-network/virtual-network-optimize-network-bandwidth.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)öğrenin.  

[SAP uygulamalarıyla yakınlık yerleştirme gruplarını nasıl kullanacağınızı](../workloads/sap/sap-proximity-placement-scenarios.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)öğrenin.
