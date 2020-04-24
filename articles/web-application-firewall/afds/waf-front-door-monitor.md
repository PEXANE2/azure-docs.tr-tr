---
title: Azure Web uygulaması güvenlik duvarı izleme ve günlüğe kaydetme
description: Frontkapısı izleme ve günlüğe kaydetme ile Web uygulaması güvenlik duvarını (WAF) öğrenin
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 08/21/2019
ms.author: victorh
ms.openlocfilehash: b4f666415a96307b89022c6caf6af90581f294f3
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82115372"
---
# <a name="azure-web-application-firewall-monitoring-and-logging"></a>Azure Web uygulaması güvenlik duvarı izleme ve günlüğe kaydetme 

Azure Web uygulaması güvenlik duvarı (WAF) izleme ve günlüğe kaydetme, Azure Izleyici ve Azure Izleyici günlükleri ile günlüğe kaydetme ve tümleştirme aracılığıyla sağlanır.

## <a name="azure-monitor"></a>Azure İzleyici

Frontkapısı günlüğü olan WAF, [Azure izleyici](../../azure-monitor/overview.md)ile tümleşiktir. Azure Izleyici, WAF uyarıları ve günlükleri dahil olmak üzere tanılama bilgilerini izlemenizi sağlar. WAF izlemesini, portaldaki ön kapı kaynağı içinde, **Tanılama** sekmesi altında veya doğrudan Azure İzleyici hizmeti aracılığıyla yapılandırabilirsiniz.

Azure portal, ön kapı kaynak türü ' ne gidin. Soldaki **izleme**/**ölçümleri** sekmesinden, WAF kurallarıyla eşleşen isteklerin sayısını izlemek için **webapplicationfirewallrequestcount** ekleyebilirsiniz. Özel filtreler, eylem türleri ve kural adlarına göre oluşturulabilir.

![Wafmetrik](../media/waf-frontdoor-monitor/waf-frontdoor-metrics.png)

## <a name="logs-and-diagnostics"></a>Günlükler ve Tanılamalar

Ön kapılı WAF algıladığı her tehdit için ayrıntılı raporlama sağlar. Günlük kaydı Azure Tanılama günlükleri ile tümleştirilir ve uyarılar json biçiminde kaydedilir. Bu Günlükler, [Azure izleyici günlükleri](../../azure-monitor/insights/azure-networking-analytics.md)ile tümleştirilebilir.

![WAFDiag](../media/waf-frontdoor-monitor/waf-frontdoor-diagnostics.png)

FrontdoorAccessLog, müşteri arka uçlarına iletilen tüm istekleri günlüğe kaydeder. FrontdoorWebApplicationFirewallLog, bir WAF kuralıyla eşleşen tüm istekleri günlüğe kaydeder.

Aşağıdaki örnek sorgu, engellenen isteklerde WAF günlüklerini edinir:

``` WAFlogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"

```

WAF günlüğünde günlüğe kaydedilen bir istek örneği aşağıda verilmiştir:

``` WAFlogQuerySample
{
    "PreciseTimeStamp": "2020-01-25T00:11:19.3866091Z",
    "time": "2020-01-25T00:11:19.3866091Z",
    "category": "FrontdoorWebApplicationFirewallLog",
    "operationName": "Microsoft.Network/FrontDoor/WebApplicationFirewallLog/Write",
    "properties": {
        "clientIP": "xx.xx.xxx.xxx",
        "socketIP": "xx.xx.xxx.xxx",
        "requestUri": "https://wafdemofrontdoorwebapp.azurefd.net:443/?q=../../x",
        "ruleName": "Microsoft_DefaultRuleSet-1.1-LFI-930100",
        "policy": "WafDemoCustomPolicy",
        "action": "Block",
        "host": "wafdemofrontdoorwebapp.azurefd.net",
        "refString": "0p4crXgAAAABgMq5aIpu0T6AUfCYOroltV1NURURHRTA2MTMANjMxNTAwZDAtOTRiNS00YzIwLTljY2YtNjFhNzMyOWQyYTgy",
        "policyMode": "prevention"
    }
}

``` 

Aşağıdaki örnek sorgu AccessLogs girdilerini edinir:

``` AccessLogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorAccessLog"

```

Erişim günlüğünde günlüğe kaydedilen bir istek örneği aşağıda verilmiştir:

``` AccessLogSample
{
    "PreciseTimeStamp": "2020-01-25T00:11:12.0160150Z",
    "time": "2020-01-25T00:11:12.0160150Z",
    "category": "FrontdoorAccessLog",
    "operationName": "Microsoft.Network/FrontDoor/AccessLog/Write",
    "properties": {
        "trackingReference": "0n4crXgAAAACnRKbdALbyToAqNfSHssDvV1NURURHRTA2MTMANjMxNTAwZDAtOTRiNS00YzIwLTljY2YtNjFhNzMyOWQyYTgy",
        "httpMethod": "GET",
        "httpVersion": "2.0",
        "requestUri": "https://wafdemofrontdoorwebapp.azurefd.net:443/",
        "requestBytes": "710",
        "responseBytes": "3116",
        "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4017.0 Safari/537.36 Edg/81.0.389.2",
        "clientIp": "xx.xx.xxx.xxx",
        "timeTaken": "0.598",
        "securityProtocol": "TLS 1.2",
        "routingRuleName": "WAFdemoWebAppRouting",
        "backendHostname": "wafdemouksouth.azurewebsites.net:443",
        "sentToOriginShield": false,
        "httpStatusCode": "200",
        "httpStatusDetails": "200"
    }
}

```

## <a name="next-steps"></a>Sonraki adımlar

- [Ön kapı](../../frontdoor/front-door-overview.md)hakkında daha fazla bilgi edinin.
