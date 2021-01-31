---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/29/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 2950129d50e17ce0d2cd928a897e2aaf7d5d2270
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2021
ms.locfileid: "99220456"
---
|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Azure Batch hesap, verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F99e9ccd8-3db9-4592-b0d1-14b1715a4d8a) |Batch hesabınızın verilerinin geri kalanında şifrelemeyi yönetmek için müşteri tarafından yönetilen anahtarları (CMKs) kullanın. Varsayılan olarak, müşteri verileri hizmet tarafından yönetilen anahtarlarla şifrelenir, ancak aynı şekilde, yasal uyumluluk standartlarını karşılamak için genellikle CMKs gereklidir. CMKs, verilerin oluşturulmuş ve sizin tarafınızdan sahip olduğu bir Azure Key Vault anahtarla şifrelenmesini sağlar. Anahtar yaşam döngüsü için, döndürme ve yönetim de dahil olmak üzere tam denetim ve sorumluluğa sahipsiniz. Üzerinde CMK şifrelemesi hakkında daha fazla bilgi edinin [https://aka.ms/Batch-CMK](https://aka.ms/Batch-CMK) . |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Batch/Batch_CustomerManagedKey_Audit.json) |
|[Batch hesaplarındaki tanılama günlükleri etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F428256e6-1fac-4f48-a757-df34c2b3336d) |Tanılama günlüklerini etkinleştirme denetimi. Bu, araştırma amacıyla kullanılacak etkinlik izlerini yeniden oluşturmayı sağlar; bir güvenlik olayı gerçekleştiğinde veya ağınız tehlikede olduğunda |Auditınotexists, devre dışı |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Batch/Batch_AuditDiagnosticLog_Audit.json) |
|[Toplu Iş hesaplarında ölçüm uyarısı kuralları yapılandırılmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F26ee67a2-f81a-4ba8-b9ce-8550bd5ee1a7) |Gerekli ölçümü etkinleştirmek için Batch hesabındaki ölçüm uyarı kurallarının yapılandırmasını denetleme |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Batch/Batch_AuditMetricAlerts_Audit.json) |
|[Batch hesapları için genel ağ erişimi devre dışı bırakılmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F74c5a0ae-5e48-4738-b093-65e23a060488) |Batch hesabında genel ağ erişiminin devre dışı bırakılması, Batch hesabınıza yalnızca özel bir uç noktadan erişilebilmesini sağlayarak güvenliği iyileştirir. Üzerinde genel ağ erişimini devre dışı bırakma hakkında daha fazla bilgi edinin [https://docs.microsoft.com/azure/batch/private-connectivity](https://docs.microsoft.com/azure/batch/private-connectivity) . |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Batch/Batch_DisablePublicNetworkAccess_Audit.json) |
