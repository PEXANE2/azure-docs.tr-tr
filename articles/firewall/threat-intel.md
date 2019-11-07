---
title: Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme
description: Azure Güvenlik Duvarı tehdit bilgileri filtrelemesi hakkında bilgi edinin
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/05/2019
ms.author: victorh
ms.openlocfilehash: f6a60d7c29fc7e482e32233aa86d65a801e3f55c
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582243"
---
# <a name="azure-firewall-threat-intelligence-based-filtering"></a>Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme

Güvenlik duvarınız için tehdit bilgileri tabanlı filtrelemeyi etkinleştirerek, kötü amaçlı olduğu bilinen IP adresleri ve etki alanları ile trafik yaşanması durumunda uyarı alabilir ve trafiği reddedebilirsiniz. IP adresleri ve etki alanları, Microsoft Tehdit Bilgileri akışından alınır. [Intelligent Security Graph](https://www.microsoft.com/en-us/security/operations/intelligence) , Microsoft Threat Intelligence 'ı güçlendirir ve Azure Güvenlik Merkezi dahil birden çok hizmet tarafından kullanılır.

![Güvenlik Duvarı tehdit bilgileri](media/threat-intel/firewall-threat.png)

Tehdit zekası tabanlı filtrelemeyi etkinleştirdiyseniz, ilişkili kurallar NAT kurallarından, ağ kurallarından veya uygulama kurallarından önce işlenir.

Bir kural tetiklendiğinde yalnızca bir uyarıyı günlüğe kaydedebilir veya uyarı ve reddetme modunu seçebilirsiniz.

Varsayılan olarak, tehdit zekası tabanlı filtreleme, uyarı modunda etkindir. Bu özelliği kapatamaz veya Portal arabirimi bölgenizde kullanılabilir hale gelene kadar modu değiştiremezsiniz.

![Tehdit zekası tabanlı filtreleme portalı arabirimi](media/threat-intel/threat-intel-ui.png)

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

- Bkz. [Azure Güvenlik duvarı Log Analytics örnekleri](log-analytics-samples.md)
- [Azure Güvenlik duvarını dağıtmayı ve yapılandırmayı](tutorial-firewall-deploy-portal.md) öğrenin
- [Microsoft Güvenlik Zekası raporunu](https://www.microsoft.com/en-us/security/operations/security-intelligence-report) gözden geçirin