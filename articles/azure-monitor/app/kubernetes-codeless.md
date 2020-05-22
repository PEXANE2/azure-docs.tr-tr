---
title: Azure Kubernetes Service (AKS) üzerinde Application Insights-Azure Izleyici ile uygulama izleme | Microsoft Docs
description: Azure Izleyici, Kubernetes üzerinde çalışan uygulamanızla sorunsuz bir şekilde tümleşir ve uygulamalarınızın sorunlarını hiç zaman fark etmenize olanak tanır.
ms.topic: conceptual
author: MS-jgol
ms.author: jgol
ms.date: 05/13/2020
ms.openlocfilehash: 76f9f922697ef6be6c959ea7f9bafd0872dba6ff
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83773767"
---
# <a name="zero-instrumentation-application-monitoring-for-kubernetes---azure-monitor-application-insights"></a>Kubernetes için sıfır izleme uygulaması izlemesi-Azure Izleyici Application Insights

> [!IMPORTANT]
>  Şu anda, kodunuzu kullanıma açmadan Kubernetes üzerinde çalışan Java uygulamalarınız için izlemeyi etkinleştirebilirsiniz- [Java tek başına aracısını](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent)kullanın. Uygulama izlemeyi sorunsuz bir şekilde etkinleştirmeye yönelik çözüm diğer diller için çalışır durumda olsa da, AKS 'de çalışan uygulamalarınızı izlemek için SDK 'Ları kullanın: [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core), [ASP.net](https://docs.microsoft.com/azure/azure-monitor/app/asp-net), [Node. js](https://docs.microsoft.com/azure/azure-monitor/app/nodejs), [JavaScript](https://docs.microsoft.com/azure/azure-monitor/app/javascript)ve [Python](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python).

## <a name="application-monitoring-without-instrumenting-the-code"></a>Kodu kaldırmadan uygulama izleme
Şu anda yalnızca Java, kodu kaldırmadan uygulama izlemeyi etkinleştirmenizi sağlar. Diğer dillerdeki uygulamaları izlemek için SDK 'Ları kullanın. 

## <a name="java"></a>Java
Java Aracısı etkinleştirildikten sonra, en yaygın kullanılan kitaplıklar ve çerçevelerden çok sayıda isteği, bağımlılığı, günlüğü ve ölçümü otomatik olarak toplar.

Kubernetes uygulamalarında çalışan Java uygulamalarınızı ve diğer ortamları izlemek için [ayrıntılı yönergeleri](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent) izleyin. 

## <a name="other-languages"></a>Diğer diller

Diğer dillerdeki uygulamalar için şu anda SDK 'ların kullanılmasını öneririz:
* [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)
* [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net)
* [Node.js](https://docs.microsoft.com/azure/azure-monitor/app/nodejs) 
* [JavaScript](https://docs.microsoft.com/azure/azure-monitor/app/javascript)
* [Python](https://docs.microsoft.com/azure/azure-monitor/app/opencensus-python)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview) ve [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) hakkında daha fazla bilgi edinin
* [Dağıtılmış izlemeye](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing) genel bir bakış alın ve Işletmeniz Için [uygulama Haritalarınızın](https://docs.microsoft.com/azure/azure-monitor/app/app-map?tabs=net) neler yapabileceğini görün