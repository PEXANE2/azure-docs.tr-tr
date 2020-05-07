---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/05/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 59d8090d1b0557aadc8d7a5af61bb3166075b51b
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82838370"
---
|Name |Açıklama |Efekt (ler) |Sürüm |GitHub |
|---|---|---|---|---|
|[RootManageSharedAccessKey hariç tüm yetkilendirme kuralları Service Bus ad alanından kaldırılmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa1817ec0-a368-432a-8057-8371e17ac6ee) |Service Bus istemcileri, bir ad alanındaki tüm kuyruklara ve konulara erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamalıdır. En az ayrıcalık güvenlik modeliyle uyum sağlamak için, yalnızca belirli varlığa erişim sağlamak üzere kuyruklar ve konular için varlık düzeyinde erişim ilkeleri oluşturmanız gerekir |Denetim, reddetme, devre dışı |1.0.1 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Bus/ServiceBus_AuditNamespaceAccessRules_Audit.json) |
|[Service Bus tanılama günlükleri etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff8d36e2f-389b-4ee4-898d-21aeb69a0f45) |Tanılama günlüklerini etkinleştirme denetimi. Bu, araştırma amacıyla kullanılacak etkinlik izlerini yeniden oluşturmayı sağlar; bir güvenlik olayı gerçekleştiğinde veya ağınız tehlikede olduğunda |Auditınotexists, devre dışı |2.0.0 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Service%20Bus/ServiceBus_AuditDiagnosticLog_Audit.json) |
