---
title: Azure 'da WAF Application Gateway bot korumasına genel bakış
titleSuffix: Azure Web Application Firewall
description: Bu makalede Application Gateway Bot koruması üzerinde Web uygulaması güvenlik duvarı 'na (WAF) genel bir bakış sunulmaktadır
services: web-application-firewall
author: winthrop28
ms.service: web-application-firewall
ms.date: 05/20/2020
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: e01f9ac8966223e11ad218af7bf6fbb2462f28f6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83714908"
---
# <a name="azure-web-application-firewall-on-azure-application-gateway-bot-protection-overview"></a>Azure 'da Azure Web uygulaması güvenlik duvarı Application Gateway bot korumasına genel bakış

Tüm Internet trafiğinin yaklaşık %20 ' si hatalı botlardan geliyor. Bunlar, Web uygulamanızda güvenlik açıklarını bulmak, taramak ve bunlara bakmak gibi şeyler yapıyor. Bu botlar web uygulaması güvenlik duvarında (WAF) durdurulduğunda sizi saldırılara yönelik değildir. Ayrıca, arka uçlarınız ve diğer temel altyapı gibi kaynak ve hizmetlerinizi de kullanamaz.

WAF 'niz için yönetilen bir bot koruma kuralı kümesini, bilinen kötü amaçlı IP adreslerinden gelen istekleri engellemek veya günlüğe kaydetmek için etkinleştirebilirsiniz. IP adresleri Microsoft Threat Intelligence akışından kaynaklıdır. Intelligent Security Graph, Microsoft Threat Intelligence 'ı güçlendirir ve Azure Güvenlik Merkezi dahil birden çok hizmet tarafından kullanılır.

> [!IMPORTANT]
> Bot koruma kuralı kümesi şu anda genel önizleme aşamasındadır ve bir önizleme hizmet düzeyi sözleşmesi ile sunulmaktadır. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bölümüne bakın   .

## <a name="use-with-owasp-rulesets"></a>OWASP RuleSets ile kullanma

Bot Protection kural kümesini OWASP RuleSets (2.2.9, 3,0 ve 3,1) ile birlikte kullanabilirsiniz. Belirli bir zamanda yalnızca bir OWASP RuleSet kullanılabilir. Bot koruması RuleSet, kendi RuleSet içinde görüntülenen ek bir kural içerir. Bu, **1. Microsoft_BotManagerRuleSet_0**başlığına sahiptir ve DIĞER OWASP kuralları gibi etkinleştirebilir veya devre dışı bırakabilirsiniz.

![Bot RuleSet](../media/bot-protection-overview/bot-ruleset.png)

## <a name="ruleset-update"></a>RuleSet güncelleştirmesi

Bilinen hatalı IP adreslerinin, bilinen bozuk IP adreslerinin, Microsoft Threat Intelligence akışından günde birden çok kez güncelleştirdiği ve botlarla eşitlenmiş halde kalması için Web uygulamalarınız, bot saldırı vektörleri değiştikçe bile sürekli olarak korunur.

## <a name="log-example"></a>Günlük örneği

Bot koruması için bir örnek günlük girdisi aşağıda verilmiştir:

```
{
        "timeStamp": "0000-00-00T00:00:00+00:00",
            "resourceId": "appgw",
            "operationName": "ApplicationGatewayFirewall",
            "category": "ApplicationGatewayFirewallLog",
            "properties": {
            "instanceId": "vm1",
                "clientIp": "1.2.3.4",
                "requestUri": "/hello.php?arg1=aaaaaaabccc",
                "ruleSetType": "MicrosoftBotProtection",
                "message": "IPReputationTriggered",
                "action": "Blocked",
                "hostname": "example.com",
                "transactionId": "abc",
                "policyId": "waf policy 1",
                "policyScope": "Global",
                "policyScopeName": "Default Policy",
                "engine": "Azwaf"
        }
    }
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Application Gateway Web uygulaması güvenlik duvarı için bot korumasını yapılandırma (Önizleme)](bot-protection.md)
