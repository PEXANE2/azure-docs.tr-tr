---
ms.openlocfilehash: 8d5c3fff2646536ffe5c593e13ccdab23397d44b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84906743"
---


| Özellik |[Tüketim planı](../articles/azure-functions/functions-scale.md#consumption-plan)|[Premium planı](../articles/azure-functions/functions-scale.md#premium-plan)|[Adanmış plan](../articles/azure-functions/functions-scale.md#app-service-plan)|[ASE](../articles/app-service/environment/intro.md)| [Kubernetes](../articles/azure-functions/functions-kubernetes-keda.md) |
|----------------|-----------|----------------|---------|-----------------------| ---|
|[Gelen IP kısıtlamaları ve özel site erişimi](../articles/azure-functions/functions-networking-options.md#inbound-ip-restrictions)|✅Evet|✅Evet|✅Evet|✅Evet|✅Evet|
|[Sanal Ağ tümleştirmesi](../articles/azure-functions/functions-networking-options.md#virtual-network-integration)|❌Eşleşen|✅Evet (bölgesel)|✅Evet (bölgesel ve ağ geçidi)|✅Evet| ✅Evet|
|[Sanal ağ Tetikleyicileri (HTTP olmayan)](../articles/azure-functions/functions-networking-options.md#virtual-network-triggers-non-http)|❌Eşleşen| ✅Evet |✅Evet|✅Evet|✅Evet|
|[Karma bağlantılar](../articles/azure-functions/functions-networking-options.md#hybrid-connections) (yalnızca Windows)|❌Eşleşen|✅Evet|✅Evet|✅Evet|✅Evet|
|[Giden IP kısıtlamaları](../articles/azure-functions/functions-networking-options.md#outbound-ip-restrictions)|❌Eşleşen| ✅Evet|✅Evet|✅Evet|✅Evet|