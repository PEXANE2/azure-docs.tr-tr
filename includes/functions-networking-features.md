---
ms.openlocfilehash: 8d5c3fff2646536ffe5c593e13ccdab23397d44b
ms.sourcegitcommit: 34eb5e4d303800d3b31b00b361523ccd9eeff0ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84906743"
---


| Özellik |[Tüketim planı](../articles/azure-functions/functions-scale.md#consumption-plan)|[Premium planı](../articles/azure-functions/functions-scale.md#premium-plan)|[Adanmış plan](../articles/azure-functions/functions-scale.md#app-service-plan)|[ASE](../articles/app-service/environment/intro.md)| [Kubernetes](../articles/azure-functions/functions-kubernetes-keda.md) |
|----------------|-----------|----------------|---------|-----------------------| ---|
|[Gelen IP kısıtlamaları ve özel site erişimi](../articles/azure-functions/functions-networking-options.md#inbound-ip-restrictions)|✅Yes|✅Yes|✅Yes|✅Yes|✅Yes|
|[Sanal Ağ tümleştirmesi](../articles/azure-functions/functions-networking-options.md#virtual-network-integration)|❌No|✅Evet (bölgesel)|✅Evet (bölgesel ve ağ geçidi)|✅Yes| ✅Yes|
|[Sanal ağ Tetikleyicileri (HTTP olmayan)](../articles/azure-functions/functions-networking-options.md#virtual-network-triggers-non-http)|❌No| ✅Yes |✅Yes|✅Yes|✅Yes|
|[Karma bağlantılar](../articles/azure-functions/functions-networking-options.md#hybrid-connections) (yalnızca Windows)|❌No|✅Yes|✅Yes|✅Yes|✅Yes|
|[Giden IP kısıtlamaları](../articles/azure-functions/functions-networking-options.md#outbound-ip-restrictions)|❌No| ✅Yes|✅Yes|✅Yes|✅Yes|