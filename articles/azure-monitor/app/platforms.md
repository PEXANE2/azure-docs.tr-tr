---
title: 'Application Insights: diller, platformlar ve tümleştirmeler | Microsoft Docs'
description: Application Insights için kullanılabilen diller, platformlar ve tümleştirmeler
ms.topic: conceptual
ms.date: 07/18/2019
ms.reviewer: olegan
ms.openlocfilehash: cbda252fece638393ee7757dc165c4b009a0195e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79136732"
---
# <a name="supported-languages"></a>Desteklenen diller

* [C#|VB (.NET)](../../azure-monitor/app/asp-net.md)
* [Java](../../azure-monitor/app/java-get-started.md)
* [JavaScript](../../azure-monitor/app/javascript.md)
* [Node. JS](../../azure-monitor/app/nodejs.md)
* [Python](../../azure-monitor/app/opencensus-python.md)

## <a name="supported-platforms-and-frameworks"></a>Desteklenen platformlar ve çerçeveler

### <a name="instrumentation-for-already-deployed-applications-codeless-agent-based"></a>Zaten dağıtılan uygulamalar için izleme (codeless, aracı tabanlı)
* [Azure VM ve Azure sanal makine ölçek kümeleri](../../azure-monitor/app/azure-vm-vmss-apps.md)
* [Azure App Service](../../azure-monitor/app/azure-web-apps.md)
* [ASP.NET - zaten canlı olan uygulamalar için](../../azure-monitor/app/monitor-performance-live-website-now.md)
* Hem Web hem de çalışan rolleri dahil olmak üzere [Azure Cloud Services](../../azure-monitor/app/cloudservices.md)
* [Azure İşlevleri](https://docs.microsoft.com/azure/azure-functions/functions-monitoring)
### <a name="instrumentation-through-code-sdks"></a>Kod üzerinden izleme (SDK)
* [ASP.NET](../../azure-monitor/app/asp-net.md)
* [ASP.NET Core](../../azure-monitor/app/asp-net-core.md)
* [Android](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [iOS](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Java EE](../../azure-monitor/app/java-get-started.md)
* [Node. JS](https://www.npmjs.com/package/applicationinsights)
* [Python](../../azure-monitor/app/opencensus-python.md)
* [Evrensel Windows uygulaması](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Windows masaüstü uygulamaları, hizmetleri ve çalışan rolleri](../../azure-monitor/app/windows-desktop.md)

## <a name="logging-frameworks"></a>Günlük altyapıları
* [ILogger](https://docs.microsoft.com/azure/azure-monitor/app/ilogger)
* [Log4Net, NLog veya System.Diagnostics.Trace](../../azure-monitor/app/asp-net-trace-logs.md)
* [Java, Log4J veya Logback](../../azure-monitor/app/java-trace-logs.md)
* [LogStash eklentisi](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Azure İzleyici](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)

## <a name="export-and-data-analysis"></a>Dışarı aktarma ve veri analizi
* [Power BI](https://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx)
* [Stream Analytics](../../azure-monitor/app/export-power-bi.md)

## <a name="unsupported-sdks"></a>Desteklenmeyen SDK’lar
Diğer topluluk tarafından desteklenen birkaç SDK 'nın mevcut olduğunu biliyoruz. Ancak, Azure Izleyici yalnızca bu sayfada listelenen desteklenen SDK 'Ları kullanırken destek sağlar. Diğer dillere yönelik desteğimizi genişletmeyi sürekli olarak değerlendirdik, bu nedenle en son SDK haberlerini almak için [GitHub duyuruları](https://github.com/microsoft/ApplicationInsights-Announcements/issues) sayfamızı izleyin. 
