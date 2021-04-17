---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: c2cec02d309e18b26c4078ad888ad736da5c9e24
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2021
ms.locfileid: "107499283"
---
|Name<br /><sub>(Azure portal)</sub> |Açıklama |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[VM görüntü Oluşturucu şablonlarının özel bağlantı kullanması gerekir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F2154edb9-244f-4741-9970-660785bccdaa) |Azure özel bağlantısı, Sanal ağınızı kaynak veya hedefte genel bir IP adresi olmadan Azure hizmetlerine bağlamanıza olanak tanır. Özel bağlantı platformu, Azure omurga ağı üzerinden tüketici ve hizmetler arasındaki bağlantıyı işler. Özel uç noktaları VM görüntü Oluşturucu kaynak oluşturmaya eşleyerek, veri sızıntısı riskleri azalır. Şu adreste özel bağlantılar hakkında daha fazla bilgi edinin: [https://docs.microsoft.com/azure/virtual-machines/linux/image-builder-networking#deploy-using-an-existing-vnet](https://docs.microsoft.com/azure/virtual-machines/linux/image-builder-networking#deploy-using-an-existing-vnet) . |Denetim, devre dışı, reddetme |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/VM%20Image%20Builder/PrivateLinkEnabled_Audit.json) |
