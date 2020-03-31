---
title: 'Uygulama Öngörüleri: diller, platformlar ve entegrasyonlar | Microsoft Dokümanlar'
description: Uygulamalar Öngörüleri için kullanılabilen diller, platformlar ve tümleştirmeler
ms.topic: conceptual
ms.date: 07/18/2019
ms.reviewer: olegan
ms.openlocfilehash: cbda252fece638393ee7757dc165c4b009a0195e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79136732"
---
# <a name="supported-languages"></a>Desteklenen diller

* [C#|VB (.NET)](../../azure-monitor/app/asp-net.md)
* [Java](../../azure-monitor/app/java-get-started.md)
* [Javascript](../../azure-monitor/app/javascript.md)
* [Node.js](../../azure-monitor/app/nodejs.md)
* [Python](../../azure-monitor/app/opencensus-python.md)

## <a name="supported-platforms-and-frameworks"></a>Desteklenen platformlar ve çerçeveler

### <a name="instrumentation-for-already-deployed-applications-codeless-agent-based"></a>Zaten dağıtılan uygulamalar için enstrümantasyon (kodsuz, aracı tabanlı)
* [Azure VM ve Azure sanal makine ölçek setleri](../../azure-monitor/app/azure-vm-vmss-apps.md)
* [Azure Uygulama Hizmeti](../../azure-monitor/app/azure-web-apps.md)
* [ASP.NET - zaten canlı olan uygulamalar için](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [Hem](../../azure-monitor/app/cloudservices.md)web hem de çalışan rolleri de dahil olmak üzere Azure Bulut Hizmetleri
* [Azure İşlevleri](https://docs.microsoft.com/azure/azure-functions/functions-monitoring)
### <a name="instrumentation-through-code-sdks"></a>Kod (SDK) ile enstrümantasyon
* [ASP.NET](../../azure-monitor/app/asp-net.md)
* [ASP.NET Core](../../azure-monitor/app/asp-net-core.md)
* [Android](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [iOS](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Java EE](../../azure-monitor/app/java-get-started.md)
* [Node.js](https://www.npmjs.com/package/applicationinsights)
* [Python](../../azure-monitor/app/opencensus-python.md)
* [Evrensel Windows uygulaması](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Windows masaüstü uygulamaları, hizmetleri ve çalışan rolleri](../../azure-monitor/app/windows-desktop.md)

## <a name="logging-frameworks"></a>Günlük altyapıları
* [ILogger](https://docs.microsoft.com/azure/azure-monitor/app/ilogger)
* [Log4Net, NLog veya System.Diagnostics.Trace](../../azure-monitor/app/asp-net-trace-logs.md)
* [Java, Log4J veya Logback](../../azure-monitor/app/java-trace-logs.md)
* [LogStash eklentisi](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Azure İzleyici](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)

## <a name="export-and-data-analysis"></a>İhracat ve veri analizi
* [Güç BI](https://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx)
* [Akış Analizi](../../azure-monitor/app/export-power-bi.md)

## <a name="unsupported-sdks"></a>Desteklenmeyen SDK’lar
Topluluk destekli başka sDK'ların da var olduğunun farkındayız. Ancak Azure Monitor yalnızca bu sayfada listelenen desteklenen SDK'ları kullanırken destek sağlar. Diğer dillere verdiğimiz desteği genişletme fırsatlarını sürekli olarak değerlendiriyoruz, bu nedenle en son SDK haberlerini almak için [GitHub Duyurular](https://github.com/microsoft/ApplicationInsights-Announcements/issues) sayfamızı takip edin. 
