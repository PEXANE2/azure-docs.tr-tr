---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 07/10/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: e91278de915971f8b23edbe0e10ef04639ceba35
ms.sourcegitcommit: 0b2367b4a9171cac4a706ae9f516e108e25db30c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86274398"
---
|Ad<br /><sub>(Azure portal)</sub> |Açıklama |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Özel RBAC kurallarının kullanımını denetleme](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa451c1ef-c6ca-483d-87ed-f49761e3ffb5) |Hataya açık olan özel RBAC rolleri yerine ' Owner, katılımcısı, Reader ' gibi yerleşik rolleri denetleyin. Özel rolleri kullanmak özel durum olarak değerlendirilir ve kapsamlı bir inceleme ve tehdit modelleme gerektirir |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/Subscription_AuditCustomRBACRoles_Audit.json) |
|[Özel abonelik sahibi rolleri mevcut olmamalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F10ee2ea2-fb4d-45b8-a7e9-a2e770044cd9) |Bu ilke, özel abonelik sahibi rolü olmamasını sağlar. |Denetim, devre dışı |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/CustomSubscription_OwnerRole_Audit.json) |
