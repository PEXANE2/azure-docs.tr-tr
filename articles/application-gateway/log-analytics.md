---
title: Azure Günlük Analitiği'ni kullanarak WAF günlüklerini inceleyin
titleSuffix: Azure Application Gateway
description: Bu makalede, Uygulama Ağ Geçidi Web Uygulaması Güvenlik Duvarı günlüklerini incelemek için Azure Log Analytics'i nasıl kullanabileceğinizi gösterir
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: victorh
ms.openlocfilehash: 9fe4462a71852e5f66268f798f6f0418f2dd39c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74048118"
---
# <a name="use-log-analytics-to-examine-application-gateway-web-application-firewall-logs"></a>Log Analytics kullanarak Application Gateway Web Uygulaması Güvenlik Duvarı Günlüklerini inceleme

Uygulama Ağ Geçidi WAF'ınız çalışmaya başladıktan sonra, günlüklerin her istekte neler olup bittiğini denetlemesini sağlayabilirsiniz. Güvenlik duvarı günlükleri, WAF'ın neyi değerlendirdiği, eşleştirdiği ve engellediği hakkında bilgi verir. Log Analytics ile, daha fazla bilgi vermek için güvenlik duvarı günlüklerinin içindeki verileri inceleyebilirsiniz. Günlük Analizi çalışma alanı oluşturma hakkında daha fazla bilgi için azure [portalında Günlük Analizi çalışma alanı oluşturma bölümüne](../azure-monitor/learn/quick-create-workspace.md)bakın. Günlük sorguları hakkında daha fazla bilgi için [Azure Monitor'daki günlük sorgularına genel bakış](../azure-monitor/log-query/log-query-overview.md)bölümüne bakın.

## <a name="import-waf-logs"></a>WAF günlüklerini içe aktarma

Güvenlik duvarı günlüklerinizi Log Analytics'e almak [için, Uygulama Ağ Geçidi için Arka uç durumu, tanılama günlükleri ve ölçümlere](application-gateway-diagnostics.md#diagnostic-logging)bakın. Log Analytics çalışma alanınızda güvenlik duvarı günlükleri olduğunda, verileri görüntüleyebilir, sorgular yazabilir, görselleştirmeler oluşturabilir ve bunları portal panonuza ekleyebilirsiniz.

## <a name="explore-data-with-examples"></a>Örneklerle verileri keşfedin

Güvenlik duvarı günlüğündeki ham verileri görüntülemek için aşağıdaki sorguyu çalıştırabilirsiniz:

```
AzureDiagnostics 
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
```

Bu, aşağıdaki sorguya benzer:

![Günlük Analizi sorgusu](media/log-analytics/log-query.png)

Verileri ayrıntılı olarak araştırabilir, grafikler çizebilir veya buradan görselleştirmeler oluşturabilirsiniz. Aşağıdaki sorguları başlangıç noktası olarak görün:

### <a name="matchedblocked-requests-by-ip"></a>IP ile eşleşen/engellenen istekler

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by clientIp_s, bin(TimeGenerated, 1m)
| render timechart
```

### <a name="matchedblocked-requests-by-uri"></a>URI tarafından eşleşen/engellenen istekler

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by requestUri_s, bin(TimeGenerated, 1m)
| render timechart
```

### <a name="top-matched-rules"></a>En iyi eşleşen kurallar

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize count() by ruleId_s, bin(TimeGenerated, 1m)
| where count_ > 10
| render timechart
```

### <a name="top-five-matched-rule-groups"></a>En iyi beş eşleşen kural grupları

```
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.NETWORK" and Category == "ApplicationGatewayFirewallLog"
| summarize Count=count() by details_file_s, action_s
| top 5 by Count desc
| render piechart
```

## <a name="add-to-your-dashboard"></a>Panonuza ekleme

Bir sorgu oluşturduktan sonra, bunu panonuza ekleyebilirsiniz.  Günlük analizi çalışma alanının sağ üst kısmında **ki panoya Pin'i** seçin. Önceki dört sorgu bir örnek panoya sabitlenmişken, bir bakışta görebileceğiniz veriler aşağıdakileri elde edebilirsiniz:

![Pano](media/log-analytics/dashboard.png)

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama Ağ Geçidi için arka uç durumu, tanılama günlükleri ve ölçümler](application-gateway-diagnostics.md)
