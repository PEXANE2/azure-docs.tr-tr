---
title: Azure Web Uygulaması Güvenlik Duvarı izleme ve günlüğe kaydetme
description: FrontDoor izleme ve günlük ile Web Uygulama Güvenlik Duvarı (WAF) öğrenin
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 08/21/2019
ms.author: victorh
ms.openlocfilehash: 4488fadf5db3b32049b5dce4bbee1fa76c320e96
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80284152"
---
# <a name="azure-web-application-firewall-monitoring-and-logging"></a>Azure Web Uygulaması Güvenlik Duvarı izleme ve günlüğe kaydetme 

Azure Web Uygulaması Güvenlik Duvarı (WAF) izleme ve günlüğe kaydetme, Azure Monitor ve Azure Monitor günlükleriyle günlüğe kaydetme ve tümleştirme yoluyla sağlanır.

## <a name="azure-monitor"></a>Azure İzleyici

FrontDoor günlüğü ile WAF [Azure Monitor](../../azure-monitor/overview.md)ile entegre edilmiştir. Azure Monitor, WAF uyarıları ve günlükleri de dahil olmak üzere tanılama bilgilerini izlemenize olanak tanır. WAF izlemeyi, Portaldaki Ön Kapı kaynağında **Tanılama** sekmesi altındaki portalda veya doğrudan Azure Monitor hizmeti aracılığıyla yapılandırabilirsiniz.

Azure portalından Ön Kapı kaynak türüne gidin. Soldaki **İzleme**/**Ölçümleri** sekmesinden, WAF kurallarıyla eşleşen istek sayısını izlemek için **WebApplicationFirewallRequestCount** ekleyebilirsiniz. Özel filtreler eylem türleri ve kural adlarına göre oluşturulabilir.

![WAFMetrics](../media/waf-frontdoor-monitor/waf-frontdoor-metrics.png)

## <a name="logs-and-diagnostics"></a>Günlükler ve tanılama

Ön Kapılı WAF, algılayadaki her tehdit hakkında ayrıntılı raporlama sağlar. Günlük kaydı Azure Tanılama günlükleri ile tümleştirilir ve uyarılar json biçiminde kaydedilir. Bu günlükler Azure [Monitor günlükleriyle](../../azure-monitor/insights/azure-networking-analytics.md)tümleştirilebilir.

![WAFDiag](../media/waf-frontdoor-monitor/waf-frontdoor-diagnostics.png)

FrontdoorAccessLog, müşteri arka uçlarına iletilen tüm istekleri kaydeder. FrontdoorWebApplicationFirewallLog bir WAF kuralı eşleşen herhangi bir istek günlükleri.

Aşağıdaki örnek sorgu, engellenen istekler üzerindeki WAF günlüklerini alır:

``` WAFlogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorWebApplicationFirewallLog"
| where action_name_s == "Block"

```

WAF günlüğünde günlüğe kaydedilmiş bir istek örneği aşağıda verilmiştir:

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

Aşağıdaki örnek sorgu AccessLogs girişlerini alır:

``` AccessLogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorAccessLog"

```

Access günlüğünde günlüğe kaydedilmiş bir isteğin bir örneği aşağıda verilmiştir:

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

- [Ön Kapı](../../frontdoor/front-door-overview.md)hakkında daha fazla bilgi edinin.
