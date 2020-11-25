---
ms.openlocfilehash: 82571d1a0e651f638dec29184f0ecdc88562b3ad
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96021018"
---


| Özellik |[Tüketim planı](../articles/azure-functions/functions-scale.md#consumption-plan)|[Premium planı](../articles/azure-functions/functions-scale.md#premium-plan)|[Adanmış plan](../articles/azure-functions/functions-scale.md#app-service-plan)|[ASE](../articles/app-service/environment/intro.md)| [Kubernetes](../articles/azure-functions/functions-kubernetes-keda.md) |
|----------------|-----------|----------------|---------|-----------------------| ---|
|[Gelen IP kısıtlamaları ve özel site erişimi](../articles/azure-functions/functions-networking-options.md#inbound-access-restrictions)|✅Yes|✅Yes|✅Yes|✅Yes|✅Yes|
|[Sanal Ağ tümleştirmesi](../articles/azure-functions/functions-networking-options.md#virtual-network-integration)|❌No|✅Evet (bölgesel)|✅Evet (bölgesel ve ağ geçidi)|✅Yes| ✅Yes|
|[Sanal ağ Tetikleyicileri (HTTP olmayan)](../articles/azure-functions/functions-networking-options.md#virtual-network-triggers-non-http)|❌No| ✅Yes |✅Yes|✅Yes|✅Yes|
|[Karma bağlantılar](../articles/azure-functions/functions-networking-options.md#hybrid-connections) (yalnızca Windows)|❌No|✅Yes|✅Yes|✅Yes|✅Yes|
|[Giden IP kısıtlamaları](../articles/azure-functions/functions-networking-options.md#outbound-ip-restrictions)|❌No| ✅Yes|✅Yes|✅Yes|✅Yes|