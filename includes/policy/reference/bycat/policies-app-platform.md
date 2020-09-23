---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/16/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: a530e303c0e06ae7addd4e3934b63d18c7f16223
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90982360"
---
|Ad<br /><sub>(Azure portal)</sub> |Açıklama |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Dağıtılmış izlemenin etkinleştirilmediği Azure yay bulut örneklerini denetleyin](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0f2d8593-4667-4932-acca-6a9f187af109) |Azure yay bulutu 'ndaki dağıtılmış izleme araçları, bir uygulamadaki mikro hizmetler arasındaki karmaşık bağlantıları hata ayıklamaya ve izlemeye izin verir. Dağıtılmış izleme araçları etkinleştirilmelidir ve sağlıklı bir durumda olmalıdır. |Denetim, devre dışı |[1.0.0-Önizleme](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Platform/Spring_DistributedTracing_Audit.json) |
|[Azure yay bulutu, ağ ekleme kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Faf35e2a4-ef96-44e7-a9ae-853dd97032c4) |Azure yay bulutu örnekleri, şu amaçlar için sanal ağ ekleme kullanmalıdır: 1. Azure Spring Cloud 'ı Internet 'ten yalıtın. 2. Azure yay bulutunu, diğer sanal ağlardaki şirket içi veri merkezlerinde veya Azure hizmetindeki sistemlerle etkileşime geçmek üzere etkinleştirin. 3. Müşterilerin Azure yay bulutu için gelen ve giden ağ iletişimlerini denetlemesine olanak sağlayın. |Denetim, devre dışı, reddetme |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Platform/Spring_VNETEnabled_Audit.json) |
