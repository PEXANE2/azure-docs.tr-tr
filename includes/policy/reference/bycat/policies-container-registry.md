---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/04/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: b4b02ffddb8f691e395b9b5baf6780cc9769e69d
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89487892"
---
|Name<br /><sub>(Azure portal)</sub> |Açıklama |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Kapsayıcı kayıt defterleri, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5b9159ae-1701-4a6f-9a7a-aa9c8ddd0580) |Müşteri tarafından yönetilen anahtarlarla (CMK) şifreleme özelliği etkinleştirilmemiş olan kapsayıcı kayıt defterlerini denetleyin. Azure, hizmet tarafından yönetilen anahtarlarla Rest 'te kayıt defteri içeriğini otomatik olarak şifreler. Azure Key Vault içinde oluşturduğunuz ve yönettiğiniz bir anahtarı kullanarak ek şifreleme katmanıyla varsayılan şifrelemeyi destekleyebilirsiniz. CMK şifrelemesi hakkında daha fazla bilgi için lütfen şu adresi ziyaret edin: [https://aka.ms/acr/CMK](https://aka.ms/acr/CMK) . |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_CMKEncryptionEnabled_Audit.json) |
|[Kapsayıcı kayıt defterleri Kısıtlanmamış ağ erişimine izin vermiyor](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd0793b48-0edc-4296-a390-4c75d1bdfd71) |Yapılandırılmış ağ veya güvenlik duvarı (IP) kuralına sahip olmayan ve varsayılan olarak tüm ağ erişimine izin veren kapsayıcı kayıt defterlerine erişin. Ağ erişimini kısıtlamak, kapsayıcı kayıt defterlerinin olası tehditlere karşı korunmasını sağlar. En az bir IP/güvenlik duvarı kuralına veya yapılandırılmış sanal ağa sahip kapsayıcı kayıt defterleri uyumlu olarak kabul edilir. Container Registry ağ kuralları hakkında daha fazla bilgi için, bkz [https://aka.ms/acr/portal/public-network](https://aka.ms/acr/portal/public-network) [https://aka.ms/acr/vnet](https://aka.ms/acr/vnet) . ve. |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_NetworkRulesExist_Audit.json) |
|[Kapsayıcı kayıt defterleri özel bağlantıları kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe8eef0a8-67cf-4eb4-9386-14b0e78733d4) |En az bir onaylanan özel uç nokta bağlantısı bulunmayan kapsayıcı kayıt defterlerine denetim. Bir sanal ağdaki istemciler özel bağlantı noktası bağlantılarına sahip kaynaklara güvenli şekilde erişebilir. Genel erişim daha sonra yalnızca özel bağlantıların kayıt defterine bağlanmak için kullanılabilir olduğundan emin olmak için devre dışı bırakılabilir. Daha fazla bilgi için şu adresi ziyaret edin: [https://aka.ms/acr/private-link](https://aka.ms/acr/private-link) . |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_PrivateEndpointEnabled_Audit.json) |
