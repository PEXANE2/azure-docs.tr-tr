---
title: Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme
description: Tehdit istihbaratı tabanlı filtreleme, güvenlik duvarınızın bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen trafiği uyarması ve reddetmesi için etkinleştirilebilir.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: c291dbe9c1eb37e68174a2353e296a376c7d0896
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74168665"
---
# <a name="azure-firewall-threat-intelligence-based-filtering"></a>Azure Güvenlik Duvarı tehdit zekası tabanlı filtreleme

Tehdit istihbaratı tabanlı filtreleme, güvenlik duvarınızın bilinen kötü amaçlı IP adreslerinden ve etki alanlarından gelen trafiği uyarması ve reddetmesi için etkinleştirilebilir. IP adresleri ve etki alanları Microsoft Threat Intelligence akışından kaynaklanmaktadır. [Akıllı Güvenlik Grafiği](https://www.microsoft.com/en-us/security/operations/intelligence) Microsoft tehdit zekasına güç sağlar ve Azure Güvenlik Merkezi de dahil olmak üzere birden çok hizmet tarafından kullanılır.

![Güvenlik duvarı tehdit istihbaratı](media/threat-intel/firewall-threat.png)

Tehdit zekası tabanlı filtrele'yi etkinleştirdiyseniz, ilişkili kurallar NAT kurallarından, ağ kurallarından veya uygulama kurallarından önce işlenir.

Bir kural tetiklendiğinde bir uyarıyı günlüğe kaydetmeyi seçebilir veya uyarı ve reddetme modunu seçebilirsiniz.

Varsayılan olarak, tehdit zekası tabanlı filtreleme uyarı modunda etkinleştirilir. Portal arabirimi bölgenizde kullanılabilir hale gelene kadar bu özelliği kapatamaz veya modu değiştiremezsiniz.

![Tehdit zekası tabanlı filtreleme portalı arabirimi](media/threat-intel/threat-intel-ui.png)

## <a name="logs"></a>Günlükler

Aşağıdaki günlük alıntısı tetiklenen bir kuralı gösterir:

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

## <a name="testing"></a>Sınama

- **Giden test** - Giden trafik uyarıları, ortamınızın tehlikeye atıldığı anlamına gelen nadir bir olay olmalıdır. Giden uyarılarıtest yardımcı olmak için, bir uyarı tetikleyen bir test FQDN oluşturuldu. Giden testler için **testmaliciousdomain.eastus.cloudapp.azure.com** kullanın.

- **Gelen testi** - DNAT kuralları güvenlik duvarında yapılandırılırsa gelen trafikte uyarılar görmeyi bekleyebilirsiniz. Bu, DNAT kuralında yalnızca belirli kaynaklara izin verilse ve trafik başka türlü reddedilmiş olsa bile geçerlidir. Azure Güvenlik Duvarı bilinen tüm bağlantı noktası tarayıcılarında uyarı yapmaz; yalnızca kötü amaçlı faaliyetlerde bulunduğu bilinen tarayıcılarda.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Güvenlik Duvarı Günlüğü Analizi örnekleri](log-analytics-samples.md)
- [Azure Güvenlik Duvarı'nı nasıl dağıtıp yapılandırılamayı](tutorial-firewall-deploy-portal.md) öğrenin
- Microsoft [Güvenlik istihbarat raporunu](https://www.microsoft.com/en-us/security/operations/security-intelligence-report) gözden geçirme