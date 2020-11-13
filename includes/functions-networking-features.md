---
ms.openlocfilehash: 82571d1a0e651f638dec29184f0ecdc88562b3ad
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578850"
---


| Öne çıkan özelliği |[Tüketim planı](../articles/azure-functions/functions-scale.md#consumption-plan)|[Premium planı](../articles/azure-functions/functions-scale.md#premium-plan)|[Adanmış plan](../articles/azure-functions/functions-scale.md#app-service-plan)|[ASE](../articles/app-service/environment/intro.md)| [Kubernetes](../articles/azure-functions/functions-kubernetes-keda.md) |
|----------------|-----------|----------------|---------|-----------------------| ---|
|[Gelen IP kısıtlamaları ve özel site erişimi](../articles/azure-functions/functions-networking-options.md#inbound-access-restrictions)|✅Yes|✅Yes|✅Yes|✅Yes|✅Yes|
|[Sanal Ağ tümleştirmesi](../articles/azure-functions/functions-networking-options.md#virtual-network-integration)|❌Hayır|✅Evet (bölgesel)|✅Evet (bölgesel ve ağ geçidi)|✅Yes| ✅Yes|
|[Sanal ağ Tetikleyicileri (HTTP olmayan)](../articles/azure-functions/functions-networking-options.md#virtual-network-triggers-non-http)|❌Hayır| ✅Yes |✅Yes|✅Yes|✅Yes|
|[Karma bağlantılar](../articles/azure-functions/functions-networking-options.md#hybrid-connections) (yalnızca Windows)|❌Hayır|✅Yes|✅Yes|✅Yes|✅Yes|
|[Giden IP kısıtlamaları](../articles/azure-functions/functions-networking-options.md#outbound-ip-restrictions)|❌Hayır| ✅Yes|✅Yes|✅Yes|✅Yes|