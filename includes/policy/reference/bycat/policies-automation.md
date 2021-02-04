---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/04/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: e98092cfb0ae96b3d981b52094825810ab1dbfd0
ms.sourcegitcommit: f82e290076298b25a85e979a101753f9f16b720c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99561333"
---
|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Otomasyon hesabı değişkenleri şifrelenmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3657f5a0-770e-44a3-b44e-9431ba1e9735) |Hassas verileri depolarken Otomasyon hesabı değişken varlıklarının şifrelenmesini etkinleştirmek önemlidir |Denetim, reddetme, devre dışı |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Automation/Automation_AuditUnencryptedVars_Audit.json) |
|[Azure Otomasyonu hesapları, bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F56a5ee18-2ae6-4810-86f7-18e39ce5629b) |Azure Otomasyonu hesaplarınızın geri kalanında şifrelemeyi yönetmek için müşteri tarafından yönetilen anahtarları kullanın. Varsayılan olarak, müşteri verileri hizmet tarafından yönetilen anahtarlarla şifrelenir, ancak yasal uyumluluk standartlarını karşılamak için müşteri tarafından yönetilen anahtarlar yaygın olarak gereklidir. Müşteri tarafından yönetilen anahtarlar, verilerin oluşturulmuş ve sizin tarafınızdan sahip olduğu bir Azure Key Vault anahtarla şifrelenmesini sağlar. Anahtar yaşam döngüsü için, döndürme ve yönetim de dahil olmak üzere tam denetim ve sorumluluğa sahipsiniz. Daha fazla bilgi edinin [https://aka.ms/automation-cmk](https://aka.ms/automation-cmk) . |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Automation/AutomationAccount_CMK_Audit.json) |
