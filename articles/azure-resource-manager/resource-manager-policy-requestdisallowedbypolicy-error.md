---
title: Azure Kaynak ilkesi ile RequestDisallowedByPolicy hatası | Microsoft Docs
description: Kaynakları Azure Resource Manager ile dağıttığınızda RequestDisallowedByPolicy hatasının nedenini açıklar.
services: azure-resource-manager
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: c791342bf68f84f6893e549d8528d1a861aa9040
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72390294"
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Azure Kaynak ilkesi ile RequestDisallowedByPolicy hatası

Bu makalede RequestDisallowedByPolicy hatasının nedeni açıklanır. bu hata için çözüm de sağlar.

## <a name="symptom"></a>Belirti

Dağıtım sırasında kaynakları oluşturmanızı önleyen bir **Requestdisallowedbypolicy** hatası alabilirsiniz. Aşağıdaki örnekte hata gösterilmektedir:

```json
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Sorun giderme

Dağıtımınızı engelleyen ilkeyle ilgili ayrıntıları almak için aşağıdaki yöntemlerden birini kullanın:

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell 'de, dağıtımınızı engelleyen ilkeyle ilgili ayrıntıları almak için bu ilke tanımlayıcısını `Id` parametresi olarak belirtin.

```powershell
(Get-AzPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="azure-cli"></a>Azure CLI

Azure CLı 'de, ilke tanımının adını belirtin:

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Çözüm

Güvenlik veya uyumluluk için abonelik yöneticileri, kaynakların nasıl dağıtıldığını sınırlayan ilkeler atayabilir. Örneğin, aboneliğiniz genel IP adresleri, ağ güvenlik grupları, Kullanıcı tanımlı yollar veya yol tabloları oluşturmayı önleyen bir ilkeye sahip olabilir. **Belirtiler** bölümündeki hata mesajı ilkenin adını gösterir.
Bu sorunu çözmek için, kaynak ilkelerini gözden geçirin ve bu ilkelerle uyumlu kaynakların nasıl dağıtılacağını saptayın.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Ilkesi nedir?](../governance/policy/overview.md)
- [Uyumluluğu zorunlu tutmak için ilkeleri oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md)
