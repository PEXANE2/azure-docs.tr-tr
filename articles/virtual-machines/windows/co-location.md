---
title: Geliştirilmiş gecikme süremi için VM'leri birlikte bulma
description: Azure VM kaynaklarının birlikte bulunmasının gecikme gecikmesi nasıl artırabileceği hakkında bilgi edinin.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: 066b6d10dbe07b902abfd15565c5ccf1a5e9c115
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82083181"
---
# <a name="co-locate-resource-for-improved-latency"></a>Geliştirilmiş gecikme sonu için kaynağı birlikte bulma

Uygulamanızı Azure'da dağıtırken, örnekleri bölgelere veya kullanılabilirlik bölgelerine yaymak ağ gecikmesi oluşturur ve bu da uygulamanızın genel performansını etkileyebilir. 


## <a name="proximity-placement-groups"></a>Yakınlık yerleştirme grupları 

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell'i kullanarak [bir VM'yi yakınlık yerleşim grubuna](proximity-placement-groups.md) dağıtın.

[Ağ gecikmesini](https://aka.ms/TestNetworkLatency?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)nasıl test edebilirsiniz öğrenin.

Ağ iş akışını nasıl [optimize edebilirsiniz](https://docs.microsoft.com/azure/virtual-network/virtual-network-optimize-network-bandwidth?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)öğrenin.  

[SAP uygulamalarıyla yakınlık yerleşim gruplarını](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)nasıl kullanacağınızı öğrenin.