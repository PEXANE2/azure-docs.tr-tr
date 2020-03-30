---
title: Bir Web Uygulaması Güvenlik Duvarı ilkesini varolan bir Azure Uygulama Ağ Geçidi ile ilişkilendirme
description: Bir Web Uygulaması Güvenlik Duvarı ilkesini varolan bir Azure Uygulama Ağ Geçidi ile nasıl ilişkilendireceklerini öğrenin.
services: web-application-firewall
ms.topic: article
author: vhorne
ms.service: web-application-firewall
ms.date: 10/25/2019
ms.author: victorh
ms.openlocfilehash: 1ed2e0cf8cc8cd841d8779462d62ba4852774a3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74083900"
---
# <a name="associate-a-waf-policy-with-an-existing-application-gateway"></a>WAF ilkesini varolan bir Uygulama Ağ Geçidi ile ilişkilendirme

Bir [WAF İlkesi oluşturmak](create-waf-policy-ag.md)için Azure PowerShell'i kullanabilirsiniz, ancak zaten bir Uygulama Ağ Geçidiniz olabilir ve bir WAF İlkesi ile ilişkilendirmek isteyebilirsiniz. Bu makalede, sadece bunu; bir WAF İlkesi oluşturun ve zaten varolan bir Uygulama Ağ Geçidi ile ilişkilendirin. 

1. Uygulama Ağ Geçidi ve Güvenlik Duvarı İlkenizi alın. Varolan bir Güvenlik Duvarı İlkeniz yoksa, bkz. 

   ```azurepowershell-interactive
      Connect-AzAccount
      Select-AzSubscription -Subscription "<sub id>"`

      #Get Application Gateway and existing policy object or create new`
      $gw = Get-AzApplicationGateway -Name <Waf v2> -ResourceGroupName <RG name>`
      $policy = Get-AzApplicationGatewayFirewallPolicy -Name <policy name> -ResourceGroupName <RG name>`
   ```

2. (İsteğe bağlı) Güvenlik Duvarı İlkesi oluşturun.

   ```azurepowershell-interactive
      New-AzApplicationGatewayFirewallPolicy -Name <policy name> -ResourceGroupName <RG name>'
      $policy = Get-AzApplicationGatewayFirewallPolicy -Name <policy name> -ResourceGroupName <RG name>`
   ```
   > [!NOTE]
   > BIR WAF Config'den WAF Politikasına geçiş için bu WAF İlkesini oluşturuyorsanız, Politika'nın eski Config'inizin tam bir kopyası olması gerekir. Bu, her dışlama, özel kural, devre dışı kural grubu, vb WAF Config olduğu gibi tam olarak aynı olması gerektiği anlamına gelir.
3. (İsteğe bağlı) WAF ilkesini ihtiyaçlarınıza göre yapılandırabilirsiniz. Buna özel kurallar, kuralları/kural gruplarını devre dışı bırakma, dışlamalar, dosya yükleme limitleri ayarlama vb. dahildir. Bu adımı atlarsanız, tüm varsayılanlar seçilir. 
   
4. İlkeyi kaydedin ve Uygulama Ağ Geçidinize takın. 
   
   ```azurepowershell-interactive
      #Save the policy itself
      Set-AzApplicationGatewayFirewallPolicy -InputObject $policy`
   
      #Attach the policy to an Application Gateway
      $gw.FirewallPolicy = $policy`
   
      #Save the Application Gateway
      Set-AzApplicationGateway -ApplicationGateway $gw`
   ```

## <a name="next-steps"></a>Sonraki adımlar
[Özel Kurallar hakkında bilgi edinin.](configure-waf-custom-rules.md)
