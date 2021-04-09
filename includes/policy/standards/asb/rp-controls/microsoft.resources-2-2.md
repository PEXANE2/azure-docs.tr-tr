---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/24/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: ddef021b81a07c1c453a9a56ea00eddd7f71a43a
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105104538"
---
|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Log Analytics aracısının otomatik olarak sağlanması aboneliğinizde etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F475aae12-b88a-4572-8b36-9b712b2b3a17) |Azure Güvenlik Merkezi, güvenlik açıklarını ve tehditleri izlemek için Azure sanal makinelerinizden veri toplar. Veriler, makineden güvenlikle ilgili çeşitli yapılandırma ve olay günlüklerini okuyan ve verileri analiz için Log Analytics çalışma alanınıza kopyalayan, daha önce Microsoft Monitoring Agent (MMA) olarak bilinen Log Analytics Aracısı tarafından toplanır. Otomatik sağlamanın, aracıyı desteklenen tüm Azure VM 'lerine ve oluşturulan tüm yeni makinelere otomatik olarak dağıtması için etkinleştirmenizi öneririz. |Auditınotexists, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_Automatic_provisioning_log_analytics_monitoring_agent.json) |
|[Azure Izleyici günlük profili, ' Write, ' ' DELETE, ' ve ' Action ' kategorilerinin günlüklerini toplamalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1a4e592a-6a6e-44a5-9814-e36264ca96e7) |Bu ilke, bir günlük profilinin ' Write, ' ' DELETE, ' ve ' Action ' kategorilerinin günlüklerini toplamasına sağlar |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_CaptureAllCategories.json) |
|[Azure Izleyici, tüm bölgelerdeki etkinlik günlüklerini toplamalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F41388f1c-2db0-4c25-95b2-35d7f5ccbfa9) |Bu ilke, genel dahil olmak üzere tüm Azure desteklenen bölgelerden etkinlikleri dışarı aktarmayan Azure Izleyici günlük profilini denetler. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_CaptureAllRegions.json) |
