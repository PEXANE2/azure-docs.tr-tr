---
ms.openlocfilehash: 8d5c3fff2646536ffe5c593e13ccdab23397d44b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "84906743"
---


| Özellik |[Tüketim planı](../articles/azure-functions/functions-scale.md#consumption-plan)|[Premium planı](../articles/azure-functions/functions-scale.md#premium-plan)|[Adanmış plan](../articles/azure-functions/functions-scale.md#app-service-plan)|[ASE](../articles/app-service/environment/intro.md)| [Kubernetes](../articles/azure-functions/functions-kubernetes-keda.md) |
|----------------|-----------|----------------|---------|-----------------------| ---|
|[Gelen IP kısıtlamaları ve özel site erişimi](../articles/azure-functions/functions-networking-options.md#inbound-ip-restrictions)|✅Evet|✅Evet|✅Evet|✅Evet|✅Evet|
|[Sanal ağ tümleştirmesi](../articles/azure-functions/functions-networking-options.md#virtual-network-integration)|❌Hayır|✅Evet (bölgesel)|✅Evet (bölgesel ve ağ geçidi)|✅Evet| ✅Evet|
|[Sanal ağ Tetikleyicileri (HTTP olmayan)](../articles/azure-functions/functions-networking-options.md#virtual-network-triggers-non-http)|❌Hayır| ✅Evet |✅Evet|✅Evet|✅Evet|
|[Karma bağlantılar](../articles/azure-functions/functions-networking-options.md#hybrid-connections) (yalnızca Windows)|❌Hayır|✅Evet|✅Evet|✅Evet|✅Evet|
|[Giden IP kısıtlamaları](../articles/azure-functions/functions-networking-options.md#outbound-ip-restrictions)|❌Hayır| ✅Evet|✅Evet|✅Evet|✅Evet|