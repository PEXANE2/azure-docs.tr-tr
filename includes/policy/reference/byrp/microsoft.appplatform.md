---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 19c5d2ef3845d4939aa28d07bee4506422916262
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2021
ms.locfileid: "107499330"
---
|Name<br /><sub>(Azure portal)</sub> |Açıklama |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Dağıtılmış izlemenin etkinleştirilmediği Azure yay bulut örneklerini denetleyin](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0f2d8593-4667-4932-acca-6a9f187af109) |Azure yay bulutu 'ndaki dağıtılmış izleme araçları, bir uygulamadaki mikro hizmetler arasındaki karmaşık bağlantıları hata ayıklamaya ve izlemeye izin verir. Dağıtılmış izleme araçları etkinleştirilmelidir ve sağlıklı bir durumda olmalıdır. |Denetim, devre dışı |[1.0.0-Önizleme](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Platform/Spring_DistributedTracing_Audit.json) |
|[Azure yay bulutu, ağ ekleme kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Faf35e2a4-ef96-44e7-a9ae-853dd97032c4) |Azure yay bulutu örnekleri, şu amaçlar için sanal ağ ekleme kullanmalıdır: 1. Azure Spring Cloud 'ı Internet 'ten yalıtın. 2. Azure yay bulutunu, diğer sanal ağlardaki şirket içi veri merkezlerinde veya Azure hizmetindeki sistemlerle etkileşime geçmek üzere etkinleştirin. 3. Müşterilerin Azure yay bulutu için gelen ve giden ağ iletişimlerini denetlemesine olanak sağlayın. |Denetim, devre dışı, reddetme |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Platform/Spring_VNETEnabled_Audit.json) |
