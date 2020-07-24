---
title: Azure Kubernetes Service (AKS) üzerinde Application Insights-Azure Izleyici ile uygulama izleme | Microsoft Docs
description: Azure Izleyici, Kubernetes üzerinde çalışan uygulamanızla sorunsuz bir şekilde tümleşir ve uygulamalarınızın sorunlarını hiç zaman fark etmenize olanak tanır.
ms.topic: conceptual
author: MS-jgol
ms.author: jgol
ms.date: 05/13/2020
ms.openlocfilehash: 52bd6d2a98e5126ff2463de1ef99da03ca555567
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87075310"
---
# <a name="zero-instrumentation-application-monitoring-for-kubernetes---azure-monitor-application-insights"></a>Kubernetes için sıfır izleme uygulaması izlemesi-Azure Izleyici Application Insights

> [!IMPORTANT]
>  Şu anda, kodunuzu kullanıma açmadan Kubernetes üzerinde çalışan Java uygulamalarınız için izlemeyi etkinleştirebilirsiniz- [Java tek başına aracısını](./java-in-process-agent.md)kullanın. Uygulama izlemeyi sorunsuz bir şekilde etkinleştirmeye yönelik çözüm diğer diller için çalışır durumda olsa da, AKS 'de çalışan uygulamalarınızı izlemek için SDK 'Ları kullanın: [ASP.NET Core](./asp-net-core.md), [ASP.net](./asp-net.md), [Node.js](./nodejs.md), [JavaScript](./javascript.md)ve [Python](./opencensus-python.md).

## <a name="application-monitoring-without-instrumenting-the-code"></a>Kodu kaldırmadan uygulama izleme
Şu anda yalnızca Java, kodu kaldırmadan uygulama izlemeyi etkinleştirmenizi sağlar. Diğer dillerdeki uygulamaları izlemek için SDK 'Ları kullanın. 

## <a name="java"></a>Java
Java Aracısı etkinleştirildikten sonra, en yaygın kullanılan kitaplıklar ve çerçevelerden çok sayıda isteği, bağımlılığı, günlüğü ve ölçümü otomatik olarak toplar.

Kubernetes uygulamalarında çalışan Java uygulamalarınızı ve diğer ortamları izlemek için [ayrıntılı yönergeleri](./java-in-process-agent.md) izleyin. 

## <a name="other-languages"></a>Diğer diller

Diğer dillerdeki uygulamalar için şu anda SDK 'ların kullanılmasını öneririz:
* [ASP.NET Core](./asp-net-core.md)
* [ASP.NET](./asp-net.md)
* [Node.js](./nodejs.md) 
* [JavaScript](./javascript.md)
* [Python](./opencensus-python.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure izleyici](../overview.md) ve [Application Insights](./app-insights-overview.md) hakkında daha fazla bilgi edinin
* [Dağıtılmış izlemeye](./distributed-tracing.md) genel bir bakış alın ve Işletmeniz Için [uygulama Haritalarınızın](./app-map.md?tabs=net) neler yapabileceğini görün
