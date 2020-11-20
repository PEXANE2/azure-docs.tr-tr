---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 11/20/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: d09e118681261add63e737cbd2b7e44f69100537
ms.sourcegitcommit: 9889a3983b88222c30275fd0cfe60807976fd65b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94989801"
---
|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Azure Stream Analytics işlerin verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanması gerekir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F87ba29ef-1ab3-4d82-b763-87fcd4f531f7) |Depolama hesabınızdaki Stream Analytics işlerinizin tüm meta verilerini ve özel veri varlıklarını güvenli bir şekilde depolamak istediğinizde, müşteri tarafından yönetilen anahtarları kullanın. Bu, Stream Analytics verilerinizin nasıl şifrelendiği hakkında toplam denetim sağlar. |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Stream%20Analytics/StreamAnalytics_CMK_Audit.json) |
|[Olay Hub 'ına Stream Analytics için tanılama ayarlarını dağıtma](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fedf3780c-3d70-40fe-b17e-ab72013dafca) |, Bu tanılama ayarlarının eksik olduğu Stream Analytics bir bölgesel Olay Hub 'ına akışa Stream Analytics yönelik tanılama ayarlarını dağıtır. |DeployIfNotExists, devre dışı |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/StreamAnalytics_DeployDiagnosticLog_Deploy_EventHub.json) |
|[Log Analytics çalışma alanına Stream Analytics için tanılama ayarlarını dağıt](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F237e0f7e-b0e8-4ec4-ad46-8c12cb66d673) |Bu tanılama ayarlarının eksik olduğu Stream Analytics, bir bölgesel Log Analytics çalışma alanına akışa Stream Analytics için tanılama ayarlarını dağıtır. |DeployIfNotExists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/StreamAnalytics_DeployDiagnosticLog_Deploy_LogAnalytics.json) |
|[Azure Stream Analytics tanılama günlükleri etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff9be5368-9bf5-4b84-9e0a-7850da98bb46) |Tanılama günlüklerini etkinleştirme denetimi. Bu, araştırma amacıyla kullanılacak etkinlik izlerini yeniden oluşturmayı sağlar; bir güvenlik olayı gerçekleştiğinde veya ağınız tehlikede olduğunda |Auditınotexists, devre dışı |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Stream%20Analytics/StreamAnalytics_AuditDiagnosticLog_Audit.json) |
