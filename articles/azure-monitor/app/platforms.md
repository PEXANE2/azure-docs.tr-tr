---
title: 'Application Insights: diller, platformlar ve tümleştirmeler | Microsoft Docs'
description: Application Insights için kullanılabilen diller, platformlar ve tümleştirmeler
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 974db106-54ff-4318-9f8b-f7b3a869e536
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/18/2019
ms.reviewer: olegan
ms.author: mbullwin
ms.openlocfilehash: dd2fb0c9ef157e3706ca3d902f7feebaf4c2f279
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68478903"
---
# <a name="supported-languages"></a>Desteklenen diller

* [C#|VB (.NET)](../../azure-monitor/app/asp-net.md)
* [Java](../../azure-monitor/app/java-get-started.md)
* [JavaScript](../../azure-monitor/app/javascript.md)
* [Node.JS](../../azure-monitor/app/nodejs.md)

## <a name="supported-platforms-and-frameworks"></a>Desteklenen platformlar ve çerçeveler

### <a name="instrumentation-for-already-deployed-applications-codeless-agent-based"></a>Zaten dağıtılan uygulamalar için izleme (codeless, aracı tabanlı)
* [Azure VM ve Azure sanal makine ölçek kümeleri](../../azure-monitor/app/azure-vm-vmss-apps.md)
* [Azure uygulama hizmeti](../../azure-monitor/app/azure-web-apps.md)
* [ASP.NET - zaten canlı olan uygulamalar için](../../azure-monitor/app/monitor-performance-live-website-now.md)
* Hem Web hem de çalışan rolleri dahil olmak üzere [Azure Cloud Services](../../azure-monitor/app/cloudservices.md)
* [Azure İşlevleri](https://docs.microsoft.com/azure/azure-functions/functions-monitoring)
### <a name="instrumentation-through-code-sdks"></a>Kod üzerinden izleme (SDK)
* [ASP.NET](../../azure-monitor/app/asp-net.md)
* [ASP.NET Core](../../azure-monitor/app/asp-net-core.md)
* [Android](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Android](https://github.com/Microsoft/ApplicationInsights-Android) (App Center)
* [iOS](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Java EE](../../azure-monitor/app/java-get-started.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
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
* [Akış Analizi](../../azure-monitor/app/export-power-bi.md)

## <a name="unsupported-community-sdks"></a>Desteklenmeyen topluluk SDK 'Ları

Birçok topluluk Application Insights SDK 'Sı vardır. Topluluk SDK 'Ları Microsoft tarafından resmi olarak desteklenmez/korunmaz. Resmi olarak desteklenen listede olmayan herhangi bir SDK için destek sağlayamıyoruz. 