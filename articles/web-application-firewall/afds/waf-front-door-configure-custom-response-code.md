---
title: Azure Ön Kapı ile WAF için özel bir yanıt yapılandırma
description: Web Uygulama Güvenlik Duvarı (WAF) bir isteği engellediğinde özel yanıt kodunu ve iletiyi nasıl yapılandırıştırmayı öğrenin.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: article
ms.date: 08/21/2019
ms.author: victorh
ms.reviewer: tyao
ms.openlocfilehash: 215d4058937ad5fded6bef7a36e873b52a1b5ae9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74185338"
---
# <a name="configure-a-custom-response-for-azure-web-application-firewall"></a>Azure Web Uygulaması Güvenlik Duvarı için özel bir yanıt yapılandırma

Varsayılan olarak, Azure Ön Kapı'ya sahip Azure Web Uygulaması Güvenlik Duvarı (WAF) eşleşen bir kural nedeniyle bir isteği engellediğinde, **istek engellenen** iletiyle birlikte 403 durum kodunu döndürür. Bu makalede, bir istek WAF tarafından engellendiğinde özel yanıt durum kodu ve yanıt iletisi nasıl yapılandırılabilen açıklanmaktadır.

## <a name="set-up-your-powershell-environment"></a>PowerShell ortamınızı hazırlama
Azure PowerShell, Azure kaynaklarınızı yönetmek için [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) modelini kullanan bir dizi cmdlet sunar. 

[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)'i yerel makinenize yükleyebilir ve herhangi bir PowerShell oturumunda kullanabilirsiniz. Azure kimlik bilgilerinizle oturum açmanız ve Az PowerShell modüllerini yüklemek için sayfadaki yönergeleri izleyin.

### <a name="connect-to-azure-with-an-interactive-dialog-for-sign-in"></a>Oturum açmak için etkileşimli bir iletişim kutusuyla Azure'a bağlanma
```
Connect-AzAccount
Install-Module -Name Az
```
PowerShellGet'in geçerli sürümünün yüklü olduğundan emin olun. Aşağıdaki komutu çalıştırın ve PowerShell'i yeniden açın.

```
Install-Module PowerShellGet -Force -AllowClobber
``` 
### <a name="install-azfrontdoor-module"></a>Az.FrontDoor modüllerini yükleyin 

```
Install-Module -Name Az.FrontDoor
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure'da, ilgili kaynakları bir kaynak grubuna ayırırsınız. Bu örnekte, [Yeni-AzResourceGroup](/powershell/module/Az.resources/new-Azresourcegroup)kullanarak bir kaynak grubu oluşturun.

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupWAF
```

## <a name="create-a-new-waf-policy-with-custom-response"></a>Özel yanıtla yeni bir WAF ilkesi oluşturma 

Aşağıda, 405 olarak ayarlanmış özel yanıt durum kodu ve Size iletisi engellenmiş olan yeni bir WAF ilkesi oluşturma örneği **verilmiştir.** [New-AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy)kullanarak.

```azurepowershell
# WAF policy setting
New-AzFrontDoorWafPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-EnabledState enabled `
-Mode Detection `
-CustomBlockResponseStatusCode 405 `
-CustomBlockResponseBody "<html><head><title>You are blocked.</title></head><body></body></html>"
```

[Update-AzFrontDoorFireWallPolicy'yi](/powershell/module/az.frontdoor/Update-AzFrontDoorWafPolicy)kullanarak varolan bir WAF ilkesinin özel yanıt kodu veya yanıt gövdesi ayarlarını değiştirin.

```azurepowershell
# modify WAF response code
Update-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-EnabledState enabled `
-Mode Detection `
-CustomBlockResponseStatusCode 403
```

```azurepowershell
# modify WAF response body
Update-AzFrontDoorFireWallPolicy `
-Name myWAFPolicy `
-ResourceGroupName myResourceGroupWAF `
-CustomBlockResponseBody "<html><head><title> Forbidden</title></head><body></body></html>"
```

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Ön Kapı ile Web Uygulaması Güvenlik Duvarı](../afds/afds-overview.md) hakkında daha fazla bilgi edinin