---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/29/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: f10f0d5cbd76597dd08c0beb172a51195d83d266
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2020
ms.locfileid: "84235671"
---
|Adı |Açıklama |Efekt (ler) |Sürüm |GitHub |
|---|---|---|---|---|
|[Kapsayıcı kayıt defterleri, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5b9159ae-1701-4a6f-9a7a-aa9c8ddd0580) |Müşteri tarafından yönetilen anahtarlarla (CMK) şifreleme özelliği etkinleştirilmemiş olan kapsayıcı kayıt defterlerini denetleyin. CMK şifrelemesi hakkında daha fazla bilgi için lütfen şu adresi ziyaret edin: [https://aka.ms/acr/CMK](https://aka.ms/acr/CMK) . |Denetim, devre dışı |1.0.0-Önizleme |[Bağlantısının](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_CMKEncryptionEnabled_Audit.json) |
|[Kapsayıcı kayıt defterleri Kısıtlanmamış ağ erişimine izin vermiyor](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd0793b48-0edc-4296-a390-4c75d1bdfd71) |Yapılandırılmış ağ (IP veya VNET) kuralına sahip olmayan ve varsayılan olarak tüm ağ erişimine izin veren kapsayıcı kayıt defterleri denetim. En az bir IP/güvenlik duvarı kuralına veya yapılandırılmış sanal ağa sahip kapsayıcı kayıt defterleri uyumlu olarak kabul edilir. Container Registry ağ kuralları hakkında daha fazla bilgi için lütfen şu adresi ziyaret edin: [https://aka.ms/acr/vnet](https://aka.ms/acr/vnet) . |Denetim, devre dışı |1.0.0-Önizleme |[Bağlantısının](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_NetworkRulesExist_Audit.json) |
|[Kapsayıcı kayıt defterleri özel bağlantıları kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe8eef0a8-67cf-4eb4-9386-14b0e78733d4) |En az bir onaylanan özel uç nokta bağlantısı bulunmayan kapsayıcı kayıt defterlerine denetim. Bir sanal ağdaki istemciler özel bağlantı noktası bağlantılarına sahip kaynaklara güvenli şekilde erişebilir. Daha fazla bilgi için şu adresi ziyaret edin: [https://aka.ms/acr/private-link](https://aka.ms/acr/private-link) . |Denetim, devre dışı |1.0.0-Önizleme |[Bağlantısının](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_PrivateEndpointEnabled_Audit.json) |
|[Container Registry bir sanal ağ hizmeti uç noktası kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc4857be7-912a-4c75-87e6-e30292bcdf78) |Bu ilke, bir sanal ağ hizmeti uç noktası kullanmak üzere yapılandırılmamış Container Registry denetler. |Denetim, devre dışı |1.0.0-Önizleme |[Bağlantısının](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/VirtualNetworkServiceEndpoint_ContainerRegistry_Audit.json) |
