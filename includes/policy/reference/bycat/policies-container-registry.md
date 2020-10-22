---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 10/20/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 5b1e2f52366e800a3f12e304ed0e626b300e948c
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92347335"
---
|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Kapsayıcı kayıt defterleri, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5b9159ae-1701-4a6f-9a7a-aa9c8ddd0580) |Müşteri tarafından yönetilen anahtarlarla (CMK) şifreleme özelliği etkinleştirilmemiş olan kapsayıcı kayıt defterlerini denetleyin. Azure, hizmet tarafından yönetilen anahtarlarla Rest 'te kayıt defteri içeriğini otomatik olarak şifreler. Azure Key Vault içinde oluşturduğunuz ve yönettiğiniz bir anahtarı kullanarak ek şifreleme katmanıyla varsayılan şifrelemeyi destekleyebilirsiniz. CMK şifrelemesi hakkında daha fazla bilgi için lütfen şu adresi ziyaret edin: [https://aka.ms/acr/CMK](https://aka.ms/acr/CMK) . |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_CMKEncryptionEnabled_Audit.json) |
|[Kapsayıcı kayıt defterleri Kısıtlanmamış ağ erişimine izin vermiyor](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd0793b48-0edc-4296-a390-4c75d1bdfd71) |Yapılandırılmış ağ veya güvenlik duvarı (IP) kuralına sahip olmayan ve varsayılan olarak tüm ağ erişimine izin veren kapsayıcı kayıt defterlerine erişin. Ağ erişimini kısıtlamak, kapsayıcı kayıt defterlerinin olası tehditlere karşı korunmasını sağlar. En az bir IP/güvenlik duvarı kuralına veya yapılandırılmış sanal ağa sahip kapsayıcı kayıt defterleri uyumlu olarak kabul edilir. Container Registry ağ kuralları hakkında daha fazla bilgi için, bkz [https://aka.ms/acr/portal/public-network](https://aka.ms/acr/portal/public-network) [https://aka.ms/acr/vnet](https://aka.ms/acr/vnet) . ve. |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_NetworkRulesExist_Audit.json) |
|[Kapsayıcı kayıt defterleri özel bağlantıları kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe8eef0a8-67cf-4eb4-9386-14b0e78733d4) |En az bir onaylanan özel uç nokta bağlantısı bulunmayan kapsayıcı kayıt defterlerine denetim. Bir sanal ağdaki istemciler özel bağlantı noktası bağlantılarına sahip kaynaklara güvenli şekilde erişebilir. Genel erişim daha sonra yalnızca özel bağlantıların kayıt defterine bağlanmak için kullanılabilir olduğundan emin olmak için devre dışı bırakılabilir. Daha fazla bilgi için şu adresi ziyaret edin: [https://aka.ms/acr/private-link](https://aka.ms/acr/private-link) . |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_PrivateEndpointEnabled_Audit.json) |
