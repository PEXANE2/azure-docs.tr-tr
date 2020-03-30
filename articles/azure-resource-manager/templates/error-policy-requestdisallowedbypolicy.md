---
title: RequestDisallowedByPolicy hatası
description: Azure Kaynak Yöneticisi ile kaynak dağıtırken RequestDisallowedByPolicy hatasının nedenini açıklar.
author: genlin
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 41581ba48da2f2e717c5abf2a749f8fd2b86ac06
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75477674"
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Azure kaynak ilkesiyle RequestDisallowedByPolicy hatası

Bu makalede RequestDisallowedByPolicy hatasının nedeni açıklanır, bu hata için de çözüm sağlar.

## <a name="symptom"></a>Belirti

Dağıtım sırasında, kaynakları oluşturmanızı engelleyen bir **RequestDisallowedByPolicy** hatası alabilirsiniz. Aşağıdaki örnekte hata gösterilmektedir:

```json
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Sorun giderme

Dağıtımınızı engelleyen ilke yle ilgili ayrıntıları almak için aşağıdaki yöntemlerden birini kullanın:

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

PowerShell'de, dağıtımınızı engelleyen ilke `Id` yle ilgili ayrıntıları almak için bu ilke tanımlayıcısını parametre olarak sağlayın.

```powershell
(Get-AzPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="azure-cli"></a>Azure CLI

Azure CLI'de ilke tanımının adını sağlayın:

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Çözüm

Güvenlik veya uyumluluk için, abonelik yöneticileriniz kaynakların nasıl dağıtılanlarını sınırlayan ilkeler atayabilir. Örneğin, aboneliğinizde Genel IP adresleri, Ağ Güvenlik Grupları, Kullanıcı Tanımlı Rotalar veya rota tabloları oluşturulmasını engelleyen bir ilke olabilir. **Belirtiler** bölümündeki hata iletisi ilkenin adını gösterir.
Bu sorunu gidermek için kaynak ilkelerini gözden geçirin ve bu ilkelere uygun kaynakların nasıl dağıtılacauyulabileceğini belirleyin.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure İlkesi nedir?](../../governance/policy/overview.md)
- [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](../../governance/policy/tutorials/create-and-manage.md)
