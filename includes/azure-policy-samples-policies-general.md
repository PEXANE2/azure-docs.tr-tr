---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 04/03/2020
ms.author: dacoulte
ms.openlocfilehash: 0c64756a4b02240916283dc03176a7dbab7ce795
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80758876"
---
|Adı |Açıklama |Etki(ler) |Sürüm |GitHub |
|---|---|---|---|---|
|[İzin verilen konumlar](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe56962a6-4747-49cd-b67b-bf8b01975c4c) |Bu ilke, kuruluşunuzun kaynakları dağıtırken belirleyebileceği konumları kısıtlamanıza olanak verir. Coğrafi uyumluluk gereksinimlerinizi zorunlu kılmak için kullanabilirsiniz. Kaynak gruplarını, Microsoft.AzureActiveDirectory/b2cDirectories'i ve 'genel' bölgeyi kullanan kaynakları hariç tutar. |Inkar |1.0.0 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/AllowedLocations_Deny.json)
|[Kaynak grupları için izin verilen konumlar](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe765b5de-1225-4ba3-bd56-1ac6695af988) |Bu ilke, kuruluşunuzun kaynak grupları oluşturabileceği konumları kısıtlamanızı sağlar. Coğrafi uyumluluk gereksinimlerinizi zorunlu kılmak için kullanabilirsiniz. |Inkar |1.0.0 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/ResourceGroupAllowedLocations_Deny.json)
|[İzin verilen kaynak türleri](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa08ec900-254a-4555-9bf5-e42af04b5c5c) |Bu ilke, kuruluşunuzun dağıtabileceği kaynak türlerini belirtmenize olanak tanır. Bu ilkeden yalnızca 'etiketleri' ve 'konum'u destekleyen kaynak türleri etkilenir. Tüm kaynakları kısıtlamak için lütfen bu ilkeyi çoğaltın ve 'mod'u 'Tümü' olarak değiştirin. |Inkar |1.0.0 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/AllowedResourceTypes_Deny.json)
|[Denetim kaynak konumu kaynak grubu konumuyla eşleşir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0a914e76-4921-4c19-b460-a2d36003525a) |Kaynak konumunun kaynak grubu konumuyla eşleştiğini denetleme |Denetim |1.0.0 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/ResourcesInResourceGroupLocation_Audit.json)
|[Özel RBAC kurallarının denetim kullanımı](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa451c1ef-c6ca-483d-87ed-f49761e3ffb5) |Hataya açık özel RBAC rolleri yerine 'Sahibi, Katkıda Bulunan, Okuyucu' gibi yerleşik rolleri denetle. Özel rolleri kullanmak bir istisna olarak kabul edilir ve titiz bir inceleme ve tehdit modellemesi gerektirir |Denetim, Engelli |1.0.0 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/Subscription_AuditCustomRBACRoles_Audit.json)
|[Özel abonelik sahibi rolleri olmamalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F10ee2ea2-fb4d-45b8-a7e9-a2e770044cd9) |Bu ilke, özel abonelik sahibi rollerinin olmamasını sağlar. |Denetim, Engelli |1.0.0 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/CustomSubscription_OwnerRole_Audit.json)
|[İzin verilmeyen kaynak türleri](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6c112d4e-5bc7-47ae-a041-ea2d9dccd749) |Bu ilke, kuruluşunuzun dağıtamadığı kaynak türlerini belirtmenize olanak tanır. |Reddet |1.0.0 |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/InvalidResourceTypes_Deny.json)
