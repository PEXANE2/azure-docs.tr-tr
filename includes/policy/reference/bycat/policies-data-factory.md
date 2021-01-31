---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/29/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 1a70bbf434084faca5c8648f8901ec77675ca5fe
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2021
ms.locfileid: "99220471"
---
|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Azure veri fabrikaları, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F4ec52d6d-beb7-40c4-9a9e-fe753254690e) |Azure Data Factory geri kalanında şifrelemeyi yönetmek için müşteri tarafından yönetilen anahtarları (CMKs) kullanın. Varsayılan olarak, müşteri verileri hizmet tarafından yönetilen anahtarlarla şifrelenir, ancak aynı şekilde, yasal uyumluluk standartlarını karşılamak için genellikle CMKs gereklidir. CMKs, verilerin oluşturulmuş ve sizin tarafınızdan sahip olduğu bir Azure Key Vault anahtarla şifrelenmesini sağlar. Anahtar yaşam döngüsü için, döndürme ve yönetim de dahil olmak üzere tam denetim ve sorumluluğa sahipsiniz. Üzerinde CMK şifrelemesi hakkında daha fazla bilgi edinin [https://aka.ms/adf-cmk](https://aka.ms/adf-cmk) . |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Data%20Factory/DataFactory_CustomerManagedKey_Audit.json) |
