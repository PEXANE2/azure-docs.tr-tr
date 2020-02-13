---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: b3647f02274bdf0efa4cd8079c55be330f28abc9
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172880"
---
|Ad |Açıklama |Efekt (ler) |Sürüm |
|---|---|---|---|
|[RootManageSharedAccessKey hariç tüm yetkilendirme kuralları Service Bus ad alanından kaldırılmalıdır](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Service%20Bus/ServiceBus_AuditNamespaceAccessRules_Audit.json) |Hizmet veri yolu istemcileri tüm kuyrukları ve konuları ad alanında erişim sağlayan bir ad alanı düzeyinde erişim ilkesi kullanmamanız gerekir. En az ayrıcalık güvenlik modeliyle uyum sağlamak için, yalnızca belirli varlığa erişim sağlamak üzere kuyruklar ve konular için varlık düzeyinde erişim ilkeleri oluşturmanız gerekir |Denetim, reddetme, devre dışı |1.0.1 |
|[Service Bus tanılama günlükleri etkinleştirilmelidir](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Service%20Bus/ServiceBus_AuditDiagnosticLog_Audit.json) |Tanılama günlüklerini etkinleştirme denetimi. Bu, araştırma amacıyla kullanılacak etkinlik izlerini yeniden oluşturmayı sağlar; bir güvenlik olayı gerçekleştiğinde veya ağınız tehlikede olduğunda |Auditınotexists, devre dışı |2.0.0 |
