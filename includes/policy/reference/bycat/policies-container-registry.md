---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 01/29/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: b631f447a5c43fb1695b6e07ec387c83747a8613
ms.sourcegitcommit: 54e1d4cdff28c2fd88eca949c2190da1b09dca91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2021
ms.locfileid: "99220551"
---
|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Kapsayıcı kayıt defterleri, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5b9159ae-1701-4a6f-9a7a-aa9c8ddd0580) |Kayıt defterlerinden geri kalan içeriklerinde şifrelemeyi yönetmek için müşteri tarafından yönetilen anahtarları kullanın. Varsayılan olarak, veriler hizmet tarafından yönetilen anahtarlarla birlikte şifrelenir, ancak yasal uyumluluk standartlarını karşılamak için genellikle müşteri tarafından yönetilen anahtarlar (CMK) gereklidir. CMKs, verilerin oluşturulmuş ve sizin tarafınızdan sahip olduğu bir Azure Key Vault anahtarla şifrelenmesini sağlar. Anahtar yaşam döngüsü için, döndürme ve yönetim de dahil olmak üzere tam denetim ve sorumluluğa sahipsiniz. Üzerinde CMK şifrelemesi hakkında daha fazla bilgi edinin [https://aka.ms/acr/CMK](https://aka.ms/acr/CMK) . |Denetim, reddetme, devre dışı |[1.1.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_CMKEncryptionEnabled_Audit.json) |
|[Kapsayıcı kayıt defterleri Kısıtlanmamış ağ erişimine izin vermiyor](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd0793b48-0edc-4296-a390-4c75d1bdfd71) |Azure Container Registry, varsayılan olarak herhangi bir ağdaki konaklardan internet üzerinden bağlantıları kabul eder. Kayıt defterlerinden olası tehditlere karşı korumak için yalnızca belirli ortak IP adreslerinden veya adres aralıklarından erişime izin verin. Kayıt defterinizde bir IP/güvenlik duvarı kuralı veya yapılandırılmış bir sanal ağ yoksa, sistem sağlıksız kaynaklarda görünür. Container Registry ağ kuralları hakkında daha fazla bilgi edinin: [https://aka.ms/acr/portal/public-network](https://aka.ms/acr/portal/public-network) ve burada [https://aka.ms/acr/vnet](https://aka.ms/acr/vnet) . |Denetim, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_NetworkRulesExist_Audit.json) |
|[Kapsayıcı kayıt defterleri özel bağlantı kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe8eef0a8-67cf-4eb4-9386-14b0e78733d4) |Azure özel bağlantısı, Sanal ağınızı kaynak veya hedefte genel bir IP adresi olmadan Azure hizmetlerine bağlamanıza olanak tanır. Özel bağlantı platformu, Azure omurga ağı üzerinden tüketici ve hizmetler arasındaki bağlantıyı işler. Özel uç noktaları tüm hizmet yerine kapsayıcı kayıt defterlerine eşleyerek, veri sızıntısı risklerine karşı de korunacaktır. Daha fazla bilgi: [https://aka.ms/acr/private-link](https://aka.ms/acr/private-link) . |Denetim, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_PrivateEndpointEnabled_Audit.json) |
