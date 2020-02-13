---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 8319ec2bf3965ee30db3e7d4ba1346bd7a3dd7d4
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77169891"
---
|Ad |Açıklama |Efekt (ler) |Sürüm |
|---|---|---|---|
|[İzin verilen konumlar](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/AllowedLocations_Deny.json) |Bu ilke, kuruluşunuzun kaynakları dağıtırken belirleyebileceği konumları kısıtlamanıza olanak verir. Coğrafi uyumluluk gereksinimlerinizi zorunlu kılmak için kullanabilirsiniz. Kaynak gruplarını, Microsoft. AzureActiveDirectory/b2cDirectories ve ' Global ' bölgesini kullanan kaynakları dışlar. |reddedebilir |1.0.0 |
|[Kaynak grupları için izin verilen konumlar](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/ResourceGroupAllowedLocations_Deny.json) |Bu ilke, kuruluşunuzun içinde kaynak grupları oluşturabileceğiniz konumları kısıtlamanıza olanak sağlar. Coğrafi uyumluluk gereksinimlerinizi zorunlu kılmak için kullanabilirsiniz. |reddedebilir |1.0.0 |
|[İzin verilen kaynak türleri](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/AllowedResourceTypes_Deny.json) |Bu ilke, kuruluşunuzun dağıtabileceğiniz kaynak türlerini belirtmenizi sağlar. Bu ilkeden yalnızca ' Tags ' ve ' Location ' desteği olan kaynak türleri etkilenir. Tüm kaynakları kısıtlamak için lütfen bu ilkeyi çoğaltın ve ' Mode ' öğesini ' All ' olarak değiştirin. |reddedebilir |1.0.0 |
|[Kaynak konumu denetim kaynak grubu konumuyla eşleşiyor](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/ResourcesInResourceGroupLocation_Audit.json) |Kaynak konumunun kaynak grubu konumuyla eşleşip eşleşmediğini denetle |denetlenmesini |1.0.0 |
|[Özel RBAC kurallarının kullanımını denetleme](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/Subscription_AuditCustomRBACRoles_Audit.json) |Hataya açık olan özel RBAC rolleri yerine ' Owner, katılımcısı, Reader ' gibi yerleşik rolleri denetleyin. Özel rolleri kullanmak özel durum olarak değerlendirilir ve kapsamlı bir inceleme ve tehdit modelleme gerektirir |Denetim, devre dışı |1.0.0 |
|[Özel abonelik sahibi rolleri mevcut olmamalıdır](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/CustomSubscription_OwnerRole_Audit.json) |Bu ilke, özel abonelik sahibi rolü olmamasını sağlar. |Denetim, devre dışı |1.0.0 |
|[İzin verilmeyen kaynak türleri](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/General/InvalidResourceTypes_Deny.json) |Bu ilke, kuruluşunuzun dağıtabolduğu kaynak türlerini belirtmenizi sağlar. |Reddet |1.0.0 |
