---
title: PowerShell'i kullanarak kuralları özelleştirin
titleSuffix: Azure Web Application Firewall
description: Bu makalede, PowerShell ile Uygulama Ağ Geçidi'nde Web Uygulaması Güvenlik Duvarı kurallarının nasıl özelleştirilene ilişkin bilgiler verilmektedir.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 11/14/2019
ms.author: victorh
ms.topic: article
ms.openlocfilehash: 55eea15da8c3a10b0421ff1576082d6b42fc7c56
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74048519"
---
# <a name="customize-web-application-firewall-rules-using-powershell"></a>PowerShell kullanarak Web Uygulaması Güvenlik Duvarı kurallarını özelleştirin

Azure Uygulama Ağ Geçidi Web Uygulaması Güvenlik Duvarı (WAF), web uygulamaları için koruma sağlar. Bu korumalar Açık Web Uygulama Güvenlik Projesi (OWASP) Temel Kural Kümesi (CRS) tarafından sağlanır. Bazı kurallar yanlış pozitif neden olabilir ve gerçek trafiği engellemek. Bu nedenle, Uygulama Ağ Geçidi kural gruplarını ve kuralları özelleştirme olanağı sağlar. Belirli kural grupları ve kuralları hakkında daha fazla bilgi için Bkz. [Web Uygulaması Güvenlik Duvarı CRS Kural grupları ve kuralları listesi.](application-gateway-crs-rulegroups-rules.md)

## <a name="view-rule-groups-and-rules"></a>Kural gruplarını ve kuralları görüntüleme

Aşağıdaki kod örnekleri, WAF özellikli bir uygulama ağ geçidinde yapılandırılabilen kuralları ve kural gruplarını nasıl görüntülenebildiğini gösterir.

### <a name="view-rule-groups"></a>Kural gruplarını görüntüleme

Aşağıdaki örnekte kural gruplarının nasıl görüntülenenegösterilmektedir:

```powershell
Get-AzApplicationGatewayAvailableWafRuleSets
```

Aşağıdaki çıktı, önceki örnekten kesilen bir yanıttır:

```
OWASP (Ver. 3.0):

    General:
        Description:

        Rules:
            RuleId     Description
            ------     -----------
            200004     Possible Multipart Unmatched Boundary.

    REQUEST-911-METHOD-ENFORCEMENT:
        Description:

        Rules:
            RuleId     Description
            ------     -----------
            911011     Rule 911011
            911012     Rule 911012
            911100     Method is not allowed by policy
            911013     Rule 911013
            911014     Rule 911014
            911015     Rule 911015
            911016     Rule 911016
            911017     Rule 911017
            911018     Rule 911018

    REQUEST-913-SCANNER-DETECTION:
        Description:

        Rules:
            RuleId     Description
            ------     -----------
            913011     Rule 913011
            913012     Rule 913012
            913100     Found User-Agent associated with security scanner
            913110     Found request header associated with security scanner
            913120     Found request filename/argument associated with security scanner
            913013     Rule 913013
            913014     Rule 913014
            913101     Found User-Agent associated with scripting/generic HTTP client
            913102     Found User-Agent associated with web crawler/bot
            913015     Rule 913015
            913016     Rule 913016
            913017     Rule 913017
            913018     Rule 913018

            ...        ...
```

## <a name="disable-rules"></a>Kuralları devre dışı

Aşağıdaki örnek kuralları `911011` devre dışı `911012` kılabilir ve bir uygulama ağ geçidinde:

```powershell
$disabledrules=New-AzApplicationGatewayFirewallDisabledRuleGroupConfig -RuleGroupName REQUEST-911-METHOD-ENFORCEMENT -Rules 911011,911012
Set-AzApplicationGatewayWebApplicationFirewallConfiguration -ApplicationGateway $gw -Enabled $true -FirewallMode Detection -RuleSetVersion 3.0 -RuleSetType OWASP -DisabledRuleGroups $disabledrules
Set-AzApplicationGateway -ApplicationGateway $gw
```

## <a name="mandatory-rules"></a>Zorunlu kurallar

Aşağıdaki liste, WAF'ın Önleme Modundayken isteği engellemesine neden olan koşulları içerir (Algılama Modunda özel durum olarak kaydedilirler). Bunlar yapılandırılamaz veya devre dışı tutulamaz:

* Vücut denetimi kapalı olmadığı sürece istek gövdesinin ayrıştırılmaması, isteğin engellenmesine neden olur (XML, JSON, form verileri)
* İstek gövdesi (dosya olmadan) veri uzunluğu yapılandırılan sınırdan daha büyüktür
* İstek gövdesi (dosyalar dahil) sınırdan daha büyüktür
* WAF motorunda bir iç hata oluştu

CRS 3.x özel:

* Gelen anomali puanı eşik aşıldı

## <a name="next-steps"></a>Sonraki adımlar

Devre dışı bırakılmış kurallarınızı yapılandırıldıktan sonra WAF günlüklerinizi nasıl görüntülediğinizi öğrenebilirsiniz. Daha fazla bilgi için [Uygulama Ağ Geçidi Tanılama'ya](../../application-gateway/application-gateway-diagnostics.md#diagnostic-logging)bakın.

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
