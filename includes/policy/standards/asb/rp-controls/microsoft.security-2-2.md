---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/14/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 56b2ae4aff99bed46fa5ad7f0947193b2f465431
ms.sourcegitcommit: 3b5cb7fb84a427aee5b15fb96b89ec213a6536c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2021
ms.locfileid: "107512012"
---
|Name<br /><sub>(Azure portal)</sub> |Açıklama |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Log Analytics aracısının otomatik olarak sağlanması aboneliğinizde etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F475aae12-b88a-4572-8b36-9b712b2b3a17) |Azure Güvenlik Merkezi, güvenlik açıklarını ve tehditleri izlemek için Azure sanal makinelerinizden veri toplar. Veriler, makineden güvenlikle ilgili çeşitli yapılandırma ve olay günlüklerini okuyan ve verileri analiz için Log Analytics çalışma alanınıza kopyalayan, daha önce Microsoft Monitoring Agent (MMA) olarak bilinen Log Analytics Aracısı tarafından toplanır. Otomatik sağlamanın, aracıyı desteklenen tüm Azure VM 'lerine ve oluşturulan tüm yeni makinelere otomatik olarak dağıtması için etkinleştirmenizi öneririz. |Auditınotexists, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_Automatic_provisioning_log_analytics_monitoring_agent.json) |
