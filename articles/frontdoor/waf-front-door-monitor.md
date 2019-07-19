---
title: Azure Web uygulaması güvenlik duvarı izleme ve günlüğe kaydetme
description: Frontkapısı izleme ve günlüğe kaydetme ile Web uygulaması güvenlik duvarını (WAF) öğrenin
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: 675d06f3d2071022da3867a4c45137efb818980d
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849134"
---
# <a name="azure-web-application-firewall-monitoring-and-logging"></a>Azure Web uygulaması güvenlik duvarı izleme ve günlüğe kaydetme 

Azure Web uygulaması güvenlik duvarı (WAF) izleme ve günlüğe kaydetme, Azure Izleyici ve Azure Izleyici günlükleri ile günlüğe kaydetme ve tümleştirme aracılığıyla sağlanır.

## <a name="azure-monitor"></a>Azure İzleyici

Frontkapısı günlüğü olan WAF, [Azure izleyici](../azure-monitor/overview.md)ile tümleşiktir. Azure Izleyici, WAF uyarıları ve günlükleri dahil olmak üzere tanılama bilgilerini izlemenizi sağlar. WAF izlemesini, portaldaki ön kapı kaynağı içinde, **Tanılama** sekmesi altında veya doğrudan Azure İzleyici hizmeti aracılığıyla yapılandırabilirsiniz.

Azure portal, ön kapı kaynak türü ' ne gidin. Soldaki **izleme**/**ölçümleri** sekmesinden, WAF kurallarıyla eşleşen isteklerin sayısını izlemek için **webapplicationfirewallrequestcount** ekleyebilirsiniz. Özel filtreler, eylem türleri ve kural adlarına göre oluşturulabilir.

![Wafmetrik](./media//waf-front-door-monitor/waf-frontdoor-metrics.png)

## <a name="logs-and-diagnostics"></a>Günlükler ve Tanılamalar

Ön kapılı WAF algıladığı her tehdit için ayrıntılı raporlama sağlar. Günlük kaydı Azure Tanılama günlükleri ile tümleştirilir ve uyarılar json biçiminde kaydedilir. Bu Günlükler, [Azure izleyici günlükleri](../azure-monitor/insights/azure-networking-analytics.md)ile tümleştirilebilir.

![WAFDiag](./media/waf-front-door-monitor/waf-frontdoor-diagnostics.png)

FrontdoorAccessLog, müşteri arka uçlarına iletilen tüm istekleri günlüğe kaydeder. FrontdoorWebApplicationFirewallLog, bir WAF kuralıyla eşleşen tüm istekleri günlüğe kaydeder.

Aşağıdaki örnek sorgu, engellenen isteklerde WAF günlüklerini edinir:

``` WAFlogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"

```

Aşağıdaki örnek sorgu AccessLogs girdilerini edinir:

``` AccessLogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorAccessLog"


```

## <a name="next-steps"></a>Sonraki adımlar

- [Ön kapı](front-door-overview.md)hakkında daha fazla bilgi edinin.

