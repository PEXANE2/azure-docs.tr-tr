---
title: Windows Azure VM'leri birlikte bulun
description: Azure VM kaynaklarının birlikte bulunmasının gecikme gecikmesi nasıl artırabileceği hakkında bilgi edinin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: b61a84f2b284177b7443be00efeea65e7eb8aaef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266799"
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