---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/12/2020
ms.author: dacoulte
ms.openlocfilehash: a1b12a72434034ecc6cbe527cc3de6454e4293a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79381633"
---
|Adı |Açıklama |Etki(ler) |Sürüm |GitHub |
|---|---|---|---|---|
|[Konteyner Kayıtları Müşteri Tarafından Yönetilen Anahtar (CMK) ile şifrelenmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5b9159ae-1701-4a6f-9a7a-aa9c8ddd0580) |Müşteri Tarafından Yönetilen Anahtarlar (CMK) ile şifreleme etkinleştirilmiş olmayan Denetim Kapsayıcı Kayıtları. CMK şifreleme hakkında daha fazla https://aka.ms/acr/CMKbilgi için lütfen şu adresi ziyaret edin: . |Denetim, Engelli |1.0.0-önizleme |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_CMKEncryptionEnabled_Audit.json)
|[Konteyner Kayıtları sınırsız ağ erişimine izin vermemelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd0793b48-0edc-4296-a390-4c75d1bdfd71) |Herhangi bir Ağ (IP veya VNET) Kuralı yapılandırılan ve varsayılan olarak tüm ağ erişimine izin veren Denetim Kapsayıcı Kayıtları. En az bir IP / Güvenlik Duvarı kuralına veya yapılandırılmış sanal ağa sahip Konteyner Kayıtları uyumlu kabul edilecektir. Konteyner Kayıt Ağı kuralları hakkında daha fazla https://aka.ms/acr/vnetbilgi için lütfen şu adresi ziyaret edin: . |Denetim, Engelli |1.0.0-önizleme |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_NetworkRulesExist_Audit.json)
