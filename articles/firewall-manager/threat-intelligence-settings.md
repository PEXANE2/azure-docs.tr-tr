---
title: Azure Güvenlik Duvarı tehdit zekası yapılandırması
description: Güvenlik duvarınızın, bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen/giden trafiği uyarmasını ve reddetmesi için tehdit zekası tabanlı filtreleme etkinleştirilebilir.
services: firewall-manager
author: vhorne
ms.service: firewall-manager
ms.topic: article
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 136ceeb271bec29bdbfc4572626936ee67f05556
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85569061"
---
# <a name="azure-firewall-threat-intelligence-configuration"></a>Azure Güvenlik Duvarı tehdit zekası yapılandırması

Tehdit zekası tabanlı filtreleme, Azure Güvenlik Duvarı ilkenizin ve bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen trafiği uyarmak ve reddetmek için yapılandırılabilir. IP adresleri ve etki alanları Microsoft Threat Intelligence akışından kaynaklıdır. [Intelligent Security Graph](https://www.microsoft.com/security/operations/intelligence) , Microsoft Threat Intelligence 'ı güçlendirir ve Azure Güvenlik Merkezi dahil birden çok hizmet tarafından kullanılır.<br>

Tehdit zekası tabanlı filtreleme 'yi yapılandırdıysanız, ilişkili kurallar NAT kurallarından, ağ kurallarından veya uygulama kurallarından önce işlenir.

:::image type="content" source="media/threat-intelligence-settings/threat-intelligence-policy.png" alt-text="Tehdit bilgileri ilkesi":::

## <a name="threat-intelligence-mode"></a>Tehdit bilgileri modu

Bir kural tetiklendiğinde yalnızca bir uyarıyı günlüğe kaydedebilir veya uyarı ve reddetme modunu seçebilirsiniz.

Varsayılan olarak, tehdit zekası tabanlı filtreleme, uyarı modunda etkindir.

## <a name="allowed-list-addresses"></a>İzin verilen liste adresleri

Tehdit bilgileri 'nin belirttiğiniz adreslerin, aralıkların veya alt ağların hiçbirini filtrelemesine olanak sağlamak için izin verilen IP adreslerinin bir listesini yapılandırabilirsiniz.



## <a name="logs"></a>Günlükler

Aşağıdaki günlük alıntısı tetiklenen bir kural gösterir:

```
{
    "category": "AzureFirewallNetworkRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallThreatIntelLog",
    "properties": {
         "msg": "HTTP request from 10.0.0.5:54074 to somemaliciousdomain.com:80. Action: Alert. ThreatIntel: Bot Networks"
    }
}
```

## <a name="testing"></a>Test Etme

- **Giden sınama** -giden trafik uyarıları nadir bir oluşum olmalıdır, çünkü ortamınız tehlikede olduğu anlamına gelir. Giden uyarıların test sağlanmasına yardımcı olmak için bir uyarıyı tetikleyen bir test FQDN 'SI oluşturulmuştur. Giden testleriniz için **testmaliciousdomain.eastus.cloudapp.Azure.com** kullanın.

- **Gelen sınama** -güvenlik duvarında DNAT kuralları yapılandırılırsa, gelen trafikte uyarıları görmeyi bekleyebilir. Bu, DNAT kuralında yalnızca belirli kaynaklara izin verildiğinden ve trafik başka bir şekilde reddedilse bile geçerlidir. Azure Güvenlik Duvarı tüm bilinen bağlantı noktası tarayıcıları üzerinde uyarı vermez; yalnızca kötü amaçlı etkinlikler için de bilinen tarayıcılarda.

## <a name="next-steps"></a>Sonraki adımlar

- [Microsoft Güvenlik Zekası raporunu](https://www.microsoft.com/en-us/security/operations/security-intelligence-report) gözden geçirin