---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/04/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: c3ca9334c0ac0a63af68baf64ba35d0580cc6f6f
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89487953"
---
|Name<br /><sub>(Azure portal)</sub> |Açıklama |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Azure Cosmos DB hesap, bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1f905d99-2ab7-462c-a6b0-f709acca6c8f) |Bu bir düzenleme veya uyum gereksinimidir Azure Cosmos DB depolanan verilerin geri kalanında şifrelemeyi denetlemek için müşteri tarafından yönetilen anahtarları kullanın. Müşteri tarafından yönetilen anahtarlar Ayrıca, hizmet tarafından yönetilen anahtarlarla varsayılan olarak en üstüne bir şifreleme katmanı ekleyerek Çift şifreleme sağlar. Bakýn [https://aka.ms/cosmosdb-cmk](https://aka.ms/cosmosdb-cmk) |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/Cosmos_CMK_Deny.json) |
|[Azure Cosmos DB hesapların güvenlik duvarı kuralları olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F862e97cf-49fc-4a5c-9de4-40d4e2e7c8eb) |Yapılandırılmış IP kuralları olmayan kaynakları denetleyin veya reddedin ve varsayılan olarak tüm ağlara izin verin. Sanal ağ filtresi etkin olarak tanımlanmış en az bir IP kuralına sahip hesaplara uyumlu olduğu kabul edilir. Ortak erişimi devre dışı bırakan hesaplar da uyumlu olarak kabul edilir. |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/Cosmos_NetworkRulesExist_Audit.json) |
|[Azure Cosmos DB izin verilen konumlar](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0473574d-2d43-4217-aefe-941fcdf7e684) |Bu ilke, kuruluşunuzun Azure Cosmos DB kaynaklarını dağıttığı sırada belirtebileceğiniz konumları kısıtlamanıza olanak sağlar. Coğrafi uyumluluk gereksinimlerinizi zorunlu kılmak için kullanabilirsiniz. |[parametreler (' policyEffect ')] |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/Cosmos_Locations_Deny.json) |
|[Azure Cosmos DB anahtar tabanlı meta veri yazma erişimi devre dışı bırakılmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F4750c32b-89c0-46af-bfcb-2e4541a818d5) |Bu ilke, tüm Azure Cosmos DB hesaplarının anahtar tabanlı meta veri yazma erişimini devre dışı bırakmasına olanak sağlar. |ýna |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/Cosmos_DisableMetadata_Append.json) |
|[Azure Cosmos DB verimlilik sınırlı olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0b7ef78e-a035-4f23-b9bd-aff122a1b1cf) |Bu ilke, kuruluşunuzun kaynak sağlayıcısı aracılığıyla Azure Cosmos DB veritabanları ve kapsayıcılar oluştururken belirtebileceğiniz maksimum üretilen işi kısıtlamanıza olanak sağlar. Otomatik ölçeklendirme kaynakları oluşturulmasını engeller. |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/Cosmos_MaxThroughput_Deny.json) |
|[Cosmos DB hesapları için Gelişmiş tehdit koruması dağıtma](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb5f04e03-92a3-4b09-9410-2cc5e5047656) |Bu ilke, Cosmos DB hesapları genelinde gelişmiş tehdit koruması sunar. |DeployIfNotExists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Cosmos%20DB/CosmosDbAdvancedThreatProtection_Deploy.json) |
