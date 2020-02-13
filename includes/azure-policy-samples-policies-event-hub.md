---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: aa9320ab4a2ff28c185ecef0f525376ceab4d875
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172824"
---
|Ad |Açıklama |Efekt (ler) |Sürüm |
|---|---|---|---|
|[RootManageSharedAccessKey hariç tüm yetkilendirme kuralları, Olay Hub 'ı ad alanından kaldırılmalıdır](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Event%20Hub/EventHub_AuditNamespaceAccessRules_Audit.json) |Olay hub'ı istemcilerin tüm kuyrukları ve konuları ad alanında erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamanız gerekir. En az ayrıcalık güvenlik modeliyle uyum sağlamak için, yalnızca belirli varlığa erişim sağlamak üzere kuyruklar ve konular için varlık düzeyinde erişim ilkeleri oluşturmanız gerekir |Denetim, reddetme, devre dışı |1.0.1 |
|[Olay Hub 'ı örneğindeki yetkilendirme kuralları tanımlanmalıdır](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Event%20Hub/EventHub_AuditEventHubAccessRules_Audit.json) |En az ayrıcalıklı erişim sağlamak için Event hub Entities 'de yetkilendirme kurallarının varlığını denetleme |Auditınotexists, devre dışı |1.0.0 |
|[Olay Hub 'ındaki tanılama günlükleri etkinleştirilmelidir](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Event%20Hub/EventHub_AuditDiagnosticLog_Audit.json) |Tanılama günlüklerini etkinleştirme denetimi. Bu, araştırma amacıyla kullanılacak etkinlik izlerini yeniden oluşturmayı sağlar; bir güvenlik olayı gerçekleştiğinde veya ağınız tehlikede olduğunda |Auditınotexists, devre dışı |2.0.0 |
