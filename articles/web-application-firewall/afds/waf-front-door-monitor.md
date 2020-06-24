---
title: Azure Web uygulaması güvenlik duvarı izleme ve günlüğe kaydetme
description: Frontkapısı izleme ve günlüğe kaydetme ile Web uygulaması güvenlik duvarını (WAF) öğrenin
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 06/09/2020
ms.author: victorh
ms.openlocfilehash: 596374d4f3f188e08a10bd25b36b178cc79a6e57
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84808946"
---
# <a name="azure-web-application-firewall-monitoring-and-logging"></a>Azure Web uygulaması güvenlik duvarı izleme ve günlüğe kaydetme

Azure Web uygulaması güvenlik duvarı (WAF) izleme ve günlüğe kaydetme, Azure Izleyici ve Azure Izleyici günlükleri ile günlüğe kaydetme ve tümleştirme aracılığıyla sağlanır.

## <a name="azure-monitor"></a>Azure İzleyici

Frontkapısı günlüğü olan WAF, [Azure izleyici](../../azure-monitor/overview.md)ile tümleşiktir. Azure Izleyici, WAF uyarıları ve günlükleri dahil olmak üzere tanılama bilgilerini izlemenizi sağlar. WAF izlemesini, portaldaki ön kapı kaynağı içinde, **Tanılama** sekmesi altında veya doğrudan Azure İzleyici hizmeti aracılığıyla yapılandırabilirsiniz.

Azure portal, ön kapı kaynak türü ' ne gidin. Soldaki **izleme** / **ölçümleri** sekmesinden, WAF kurallarıyla eşleşen isteklerin sayısını izlemek için **webapplicationfirewallrequestcount** ekleyebilirsiniz. Özel filtreler, eylem türleri ve kural adlarına göre oluşturulabilir.

:::image type="content" source="../media/waf-frontdoor-monitor/waf-frontdoor-metrics.png" alt-text="Wafmetrik":::

## <a name="logs-and-diagnostics"></a>Günlükler ve Tanılamalar

Ön kapılı WAF algıladığı her tehdit için ayrıntılı raporlama sağlar. Günlük kaydı Azure Tanılama günlükleri ile tümleştirilir ve uyarılar json biçiminde kaydedilir. Bu Günlükler, [Azure izleyici günlükleri](../../azure-monitor/insights/azure-networking-analytics.md)ile tümleştirilebilir.

![WAFDiag](../media/waf-frontdoor-monitor/waf-frontdoor-diagnostics.png)

[FrontdoorAccessLog](../../frontdoor/front-door-diagnostics.md) tüm istekleri günlüğe kaydeder. FrontdoorWebApplicationFirewallLog, aşağıdaki şemaya sahip bir WAF kuralıyla eşleşen tüm istekleri günlüğe kaydeder:

| Özellik  | Açıklama |
| ------------- | ------------- |
|Eylem|İstek üzerinde gerçekleştirilen eylem|
| ClientIp | İsteği yapan istemcinin IP adresi. İstekte bir X-Iletilmiş-for üstbilgisi varsa, bu durumda Istemci IP 'si başlık alanından çekilir. |
| Istemci bağlantı noktası | İsteği yapan istemcinin IP bağlantı noktası. |
| Ayrıntılar|Eşleşen istek hakkında ek ayrıntılar |
|| matchVariableName: eşleşen isteğin HTTP parametre adı, örneğin üst bilgi adları|
|| matchVariableValue: eşleşmeyi tetikleyen değerler|
| Host | Eşleşen isteğin ana bilgisayar üst bilgisi |
| İlke | İsteğin eşleştiği WAF ilkesinin adı. |
| PolicyMode | WAF ilkesinin işlemler modu. Olası değerler şunlardır "önleme" ve "algılama" |
| RequestUri | Eşleşen isteğin tam URI 'SI. |
| RuleName | İsteğin eşleştiği WAF kuralının adı. |
| SocketIp | WAF tarafından görülen kaynak IP adresi. Bu IP adresi, herhangi bir istek üst bilgilerinden bağımsız olarak TCP oturumuna dayalıdır.|
| TrackingReference | Ön kapıya yönelik olarak sunulan ve istemciye X-Azure-ref üst bilgisi olarak gönderilen bir isteği tanımlayan benzersiz başvuru dizesi. Belirli bir istek için erişim günlüklerindeki ayrıntıları aramak için gereklidir. |

Aşağıdaki sorgu örneği, engellenen isteklerde WAF günlüklerini döndürür:

``` WAFlogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"

```

WAF günlüğünde günlüğe kaydedilen bir istek örneği aşağıda verilmiştir:

``` WAFlogQuerySample
{
    "time":  "2020-06-09T22:32:17.8376810Z",
    "category": "FrontdoorWebApplicationFirewallLog",
    "operationName": "Microsoft.Network/FrontDoorWebApplicationFirewallLog/Write",
    "properties":
    {
        "clientIP":"xxx.xxx.xxx.xxx",
        "clientPort":"52097",
        "socketIP":"xxx.xxx.xxx.xxx",
        "requestUri":"https://wafdemofrontdoorwebapp.azurefd.net:443/?q=%27%20or%201=1",
        "ruleName":"Microsoft_DefaultRuleSet-1.1-SQLI-942100",
        "policy":"WafDemoCustomPolicy",
        "action":"Block",
        "host":"wafdemofrontdoorwebapp.azurefd.net",
        "trackingReference":"08Q3gXgAAAAAe0s71BET/QYwmqtpHO7uAU0pDRURHRTA1MDgANjMxNTAwZDAtOTRiNS00YzIwLTljY2YtNjFhNzMyOWQyYTgy",
        "policyMode":"prevention",
        "details":
            {
            "matches":
                [{
                "matchVariableName":"QueryParamValue:q",
                "matchVariableValue":"' or 1=1"
                }]
            }
     }
}
```

Aşağıdaki örnek sorgu AccessLogs girdilerini döndürür:

``` AccessLogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorAccessLog"

```

Erişim günlüğünde günlüğe kaydedilen bir istek örneği aşağıda verilmiştir:

``` AccessLogSample
{
"time": "2020-06-09T22:32:17.8383427Z",
"category": "FrontdoorAccessLog",
"operationName": "Microsoft.Network/FrontDoor/AccessLog/Write",
 "properties":
    {
    "trackingReference":"08Q3gXgAAAAAe0s71BET/QYwmqtpHO7uAU0pDRURHRTA1MDgANjMxNTAwZDAtOTRiNS00YzIwLTljY2YtNjFhNzMyOWQyYTgy",
    "httpMethod":"GET",
    "httpVersion":"2.0",
    "requestUri":"https://wafdemofrontdoorwebapp.azurefd.net:443/?q=%27%20or%201=1",
    "requestBytes":"715",
    "responseBytes":"380",
    "userAgent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4157.0 Safari/537.36 Edg/85.0.531.1",
    "clientIp":"xxx.xxx.xxx.xxx",
    "socketIp":"xxx.xxx.xxx.xxx",
    "clientPort":"52097",
    "timeTaken":"0.003",
    "securityProtocol":"TLS 1.2",
    "routingRuleName":"WAFdemoWebAppRouting",
    "rulesEngineMatchNames":[],
    "backendHostname":"wafdemowebappuscentral.azurewebsites.net:443",
    "sentToOriginShield":false,
    "httpStatusCode":"403",
    "httpStatusDetails":"403",
    "pop":"SJC",
    "cacheStatus":"CONFIG_NOCACHE"
    }
}

```

## <a name="next-steps"></a>Sonraki adımlar

- [Ön kapı](../../frontdoor/front-door-overview.md)hakkında daha fazla bilgi edinin.
