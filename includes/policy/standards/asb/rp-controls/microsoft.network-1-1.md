---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/09/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: f2b7b8b9d1d6eceaa771a43cd23073d90183fc25
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/10/2021
ms.locfileid: "101718354"
---
|Name<br /><sub>(Azure portal)</sub> |Açıklama |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Tüm Internet trafiği, dağıtılan Azure Güvenlik duvarınız aracılığıyla yönlendirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffc5e4038-4584-4632-8c85-c0448d374b2c) |Azure Güvenlik Merkezi, bazı alt ağlarınızın bir sonraki nesil güvenlik duvarı ile korunmuyor olduğunu belirledi. Azure Güvenlik Duvarı veya desteklenen bir yeni nesil güvenlik duvarı ile erişimi kısıtlayarak alt ağlarınızı olası tehditlere karşı koruyun |Auditınotexists, devre dışı |[3.0.0-Önizleme](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/ASC_All_Internet_traffic_should_be_routed_via_Azure_Firewall.json) |
|[Alt ağlar bir ağ güvenlik grubuyla ilişkilendirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe71308d3-144b-4262-b144-efdc3cc90517) |Ağ güvenlik grubu (NSG) ile erişimi kısıtlayarak alt ağınızı olası tehditlere karşı koruyun. NSG 'ler, alt ağınıza ağ trafiğine izin veren veya reddeden Access Control listesi (ACL) kurallarının bir listesini içerir. |Auditınotexists, devre dışı |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_NetworkSecurityGroupsOnSubnets_Audit.json) |
|[Sanal makinelerin onaylanan bir sanal ağa bağlanması gerekir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd416745a-506c-48b6-8ab1-83cb814bcaa3) |Bu ilke, onaylanmamış bir sanal ağa bağlı tüm sanal makineleri denetler. |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/ApprovedVirtualNetwork_Audit.json) |
|[Sanal ağlar belirtilen sanal ağ geçidini kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff1776c76-f58c-4245-a8d0-2b207198dc8b) |Varsayılan yol belirtilen sanal ağ geçidini işaret etmez, bu ilke tüm sanal ağı denetler. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/VirtualNetwork_ApprovedVirtualNetworkGateway_AuditIfNotExists.json) |
