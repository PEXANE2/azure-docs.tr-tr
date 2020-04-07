---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/03/2020
ms.author: dacoulte
ms.openlocfilehash: 50b7b71883d924682a8af60ea365745cf6df863d
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80758355"
---
|Adı |Açıklama |Etki(ler) |Sürüm |GitHub |
|---|---|---|---|---|
|[RootManageSharedAccessKey dışındaki tüm yetkilendirme kuralları Event Hub ad alanından kaldırılmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb278e460-7cfc-4451-8294-cccc40a940d7) |Olay Hub istemcileri, ad alanındaki tüm kuyruklara ve konulara erişim sağlayan bir ad alanı düzeyi erişim ilkesi kullanmamalıdır. En az ayrıcalık güvenlik modeliyle hizalamak için, yalnızca belirli bir varlığa erişim sağlamak için kuyruklar ve konular için varlık düzeyinde erişim ilkeleri oluşturmanız gerekir |Denetim, Reddet, Devre Dışı Bırakma |1.0.1 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Event%20Hub/EventHub_AuditNamespaceAccessRules_Audit.json)
|[Olay Hub örneğindeki yetkilendirme kuralları tanımlanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff4826e5f-6a27-407c-ae3e-9582eb39891d) |En az ayrıcalıklı erişim sağlamak için Event Hub varlıklarına ilişkin yetkilendirme kurallarının denetim varlığı |Auditifnotexists, Devre Dışı |1.0.0 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Event%20Hub/EventHub_AuditEventHubAccessRules_Audit.json)
|[Event Hub'daki tanılama günlükleri etkinleştirilmeli](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F83a214f7-d01a-484b-91a9-ed54470c9a6a) |Tanılama günlüklerinin denetim için. Bu, araştırma amacıyla kullanmak üzere etkinlik izlerini yeniden oluşturmanıza olanak tanır; bir güvenlik olayı meydana geldiğinde veya ağınızın gizliliği ihlal edildiğinde |Auditifnotexists, Devre Dışı |2.0.0 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Event%20Hub/EventHub_AuditDiagnosticLog_Audit.json)
