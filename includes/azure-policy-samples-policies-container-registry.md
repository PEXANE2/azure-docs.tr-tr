---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/05/2020
ms.author: dacoulte
ms.openlocfilehash: 45cb4e589a00a2ab17fee280bb6066eaee4f24c9
ms.sourcegitcommit: 05b36f7e0e4ba1a821bacce53a1e3df7e510c53a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78669055"
---
|Adı |Açıklama |Efekt (ler) |Sürüm |GitHub |
|---|---|---|---|---|
|[Kapsayıcı kayıt defterleri, müşteri tarafından yönetilen bir anahtarla şifrelenmelidir (CMK)](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5b9159ae-1701-4a6f-9a7a-aa9c8ddd0580) |Müşteri tarafından yönetilen anahtarlarla (CMK) şifreleme özelliği etkinleştirilmemiş olan kapsayıcı kayıt defterlerini denetleyin. CMK şifrelemesi hakkında daha fazla bilgi için lütfen şu adresi ziyaret edin: https://aka.ms/acr/CMK. |Denetim, devre dışı |1.0.0-Önizleme |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_CMKEncryptionEnabled_Audit.json)
|[Kapsayıcı kayıt defterleri Kısıtlanmamış ağ erişimine izin vermiyor](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd0793b48-0edc-4296-a390-4c75d1bdfd71) |Yapılandırılmış ağ (IP veya VNET) kuralına sahip olmayan ve varsayılan olarak tüm ağ erişimine izin veren kapsayıcı kayıt defterleri denetim. En az bir IP/güvenlik duvarı kuralına veya yapılandırılmış sanal ağa sahip kapsayıcı kayıt defterleri uyumlu olarak kabul edilir. Container Registry ağ kuralları hakkında daha fazla bilgi için lütfen şu adresi ziyaret edin: https://aka.ms/acr/vnet. |Denetim, devre dışı |1.0.0-Önizleme |[Bağlantı](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Container%20Registry/ACR_NetworkRulesExist_Audit.json)
