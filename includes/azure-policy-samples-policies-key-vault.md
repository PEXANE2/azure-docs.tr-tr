---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/12/2020
ms.author: dacoulte
ms.openlocfilehash: 7b2179c0a924f7fc29aa70ff748d435e664980ae
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77193006"
---
|Ad |Açıklama |Efekt (ler) |Sürüm |
|---|---|---|---|
|[Olay Hub 'ına Key Vault için tanılama ayarlarını dağıtma](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Key%20Vault/KeyVault_DiagnosticLog_Deploy.json) |, Bu tanılama ayarlarının eksik olduğu Key Vault bir bölgesel Olay Hub 'ına akışa Key Vault yönelik tanılama ayarlarını dağıtır. |deployIfNotExists |1.0.0 |
|[Key Vault tanılama günlükleri etkinleştirilmelidir](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Key%20Vault/KeyVault_AuditDiagnosticLog_Audit.json) |Tanılama günlüklerini etkinleştirme denetimi. Bu, araştırma amacıyla kullanılacak etkinlik izlerini yeniden oluşturmayı sağlar; bir güvenlik olayı gerçekleştiğinde veya ağınız tehlikede olduğunda |Auditınotexists, devre dışı |1.0.0 |
|[Key Vault nesneler kurtarılabilir olmalıdır](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Key%20Vault/KeyVault_Recoverable_Audit.json) |Bu ilke, Anahtar Kasası nesnelerinin kurtarılabilir olup olmadığını denetler. Geçici silme özelliği, belirli bir bekletme dönemi (90 gün) için bir SILME işleminden sonra bile kaynakları etkin bir şekilde tutmaya yardımcı olur. ' Korumayı Temizle ' açık olduğunda, 90 günlük bekletme süresi geçene kadar bir kasa veya silinen durumundaki bir nesne temizlenemiyor. Bu kasalar ve nesneler yine de kurtarılabilir, bu da bekletme ilkesi takip edilecek. |Denetim, devre dışı |1.0.0 |
