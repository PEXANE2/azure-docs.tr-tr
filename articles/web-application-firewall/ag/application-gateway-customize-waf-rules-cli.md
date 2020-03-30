---
title: CLI kullanarak kuralları özelleştirme - Azure Web Uygulaması Güvenlik Duvarı
description: Bu makalede, Azure CLI ile Uygulama Ağ Geçidi'nde Web Uygulaması Güvenlik Duvarı kurallarının nasıl özelleştirilene ilişkin bilgiler verilmektedir.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 11/14/2019
ms.author: victorh
ms.topic: article
ms.openlocfilehash: 8e8aaa9458619bc937c5bb11c450f3197b92f451
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74048524"
---
# <a name="customize-web-application-firewall-rules-using-the-azure-cli"></a>Azure CLI'yi kullanarak Web Uygulaması Güvenlik Duvarı kurallarını özelleştirin

Azure Uygulama Ağ Geçidi Web Uygulaması Güvenlik Duvarı (WAF), web uygulamaları için koruma sağlar. Bu korumalar Açık Web Uygulama Güvenlik Projesi (OWASP) Temel Kural Kümesi (CRS) tarafından sağlanır. Bazı kurallar yanlış pozitif neden olabilir ve gerçek trafiği engellemek. Bu nedenle, Uygulama Ağ Geçidi kural gruplarını ve kuralları özelleştirme olanağı sağlar. Belirli kural grupları ve kuralları hakkında daha fazla bilgi için [bkz.](application-gateway-crs-rulegroups-rules.md)

## <a name="view-rule-groups-and-rules"></a>Kural gruplarını ve kuralları görüntüleme

Aşağıdaki kod örnekleri, yapılandırılabilir kurallar ve kural gruplarının nasıl görüntülenebildiğini gösterir.

### <a name="view-rule-groups"></a>Kural gruplarını görüntüleme

Aşağıdaki örnekte kural gruplarının nasıl görüntülenenegösterilmektedir:

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --type OWASP
```

Aşağıdaki çıktı, önceki örnekten kesilen bir yanıttır:

```json
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  },
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_2.2.9",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
   "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "crs_20_protocol_violations",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "2.2.9",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

### <a name="view-rules-in-a-rule-group"></a>Kural grubunda kuralları görüntüleme

Aşağıdaki örnek, belirli bir kural grubunda kuralların nasıl görüntülenebildiğini gösterir:

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --group "REQUEST-910-IP-REPUTATION"
```

Aşağıdaki çıktı, önceki örnekten kesilen bir yanıttır:

```json
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": [
          {
            "description": "Rule 910011",
            "ruleId": 910011
          },
          ...
        ]
      }
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

## <a name="disable-rules"></a>Kuralları devre dışı

Aşağıdaki örnek kuralları `910018` devre dışı `910017` kılabilir ve bir uygulama ağ geçidinde:

```azurecli-interactive
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
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

Devre dışı bırakılmış kurallarınızı yapılandırıldıktan sonra WAF günlüklerinizi nasıl görüntülediğinizi öğrenebilirsiniz. Daha fazla bilgi için [Uygulama Ağ Geçidi tanılama](../../application-gateway/application-gateway-diagnostics.md#diagnostic-logging)bakın.

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
