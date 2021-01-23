---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/21/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 722c02dc47dfea1376f216e7f5335051aea85af6
ms.sourcegitcommit: 75041f1bce98b1d20cd93945a7b3bd875e6999d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2021
ms.locfileid: "98701336"
---
|Name<br /><sub>(Azure portal)</sub> |Açıklama |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Azure SYNAPSE çalışma alanları, bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff7d52b2d-e161-4dfa-a82b-55e564167385) |Azure SYNAPSE çalışma alanlarında depolanan verilerin geri kalanında şifrelemeyi denetlemek için müşteri tarafından yönetilen anahtarları kullanın. Müşteri tarafından yönetilen anahtarlar, hizmet tarafından yönetilen anahtarlarla varsayılan şifrelemenin üzerine ikinci bir şifreleme katmanı ekleyerek Çift şifreleme sağlar. |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Synapse/SynapseWorkspaceCMK_Audit.json) |
|[Azure SYNAPSE çalışma alanlarında IP güvenlik duvarı kuralları kaldırılmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F56fd377d-098c-4f02-8406-81eb055902b8) |Tüm IP Güvenlik Duvarı kurallarının kaldırılması, Azure SYNAPSE çalışma alanınızın yalnızca özel bir uç noktadan erişilebilir olmasını sağlayarak güvenliği geliştirir. Bu yapılandırma, çalışma alanında genel ağ erişimine izin veren Güvenlik Duvarı kurallarının oluşturulmasını denetler. |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Synapse/SynapseWorkspaceFirewallRules_Audit.json) |
|[Azure SYNAPSE çalışma alanlarında yönetilen çalışma alanı sanal ağı etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F2d9dbfa3-927b-4cf0-9d0f-08747f971650) |Yönetilen bir çalışma alanı sanal ağını etkinleştirmek, çalışma alanınızın diğer çalışma alanlarından yalıtılmış olmasını sağlar. Bu sanal ağda dağıtılan veri tümleştirme ve Spark kaynakları, Spark etkinlikleri için Kullanıcı düzeyi yalıtımı da sağlar. |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Synapse/SynapseWorkspaceManagedVnet_Audit.json) |
|[Azure SYNAPSE çalışma alanlarında özel uç nokta bağlantıları etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F72d11df1-dd8a-41f7-8925-b05b960ebafc) |Özel uç noktalar, bir Azure SYNAPSE çalışma alanına özel olarak bağlanacak şekilde yapılandırılabilir. Bu, Azure SYNAPSE çalışma alanına güvenli bir iletişim kanalı zorlamak için kullanılır. |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Synapse/SynapseWorkspaceUsePrivateLinks_Audit.json) |
|[SYNAPSE yönetilen özel uç noktalar yalnızca onaylanan Azure Active Directory kiracılardaki kaynaklara bağlanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3a003702-13d2-4679-941b-937e58c443f0) |SYNAPSE çalışma alanınızı yalnızca onaylanan Azure Active Directory (Azure AD) kiracılarındaki kaynaklarla bağlantı kurmasına izin vererek koruyun. Onaylanan Azure AD kiracılar, ilke ataması sırasında tanımlanabilir. |Denetim, devre dışı, reddetme |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Synapse/Workspace_DataExfiltrationPrevention_Deny.json) |
