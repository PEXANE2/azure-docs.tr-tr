---
title: Azure Uygulama Öngörüleri'nden daha fazla bilgi edinin | Microsoft Dokümanlar
description: Application Insights ile başladıktan sonra, keşfedebileceğiniz özelliklerin bir özetini aşağıda bulabilirsiniz.
ms.topic: conceptual
ms.date: 02/03/2017
ms.openlocfilehash: 3c4a9a736f34d4f7dbfeb004d0837f1f2efa55dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77666033"
---
# <a name="more-telemetry-from-application-insights"></a>Uygulama Öngörüleri'nden daha fazla telemetri
[ASP.NET kodunuza Uygulama Öngörüleri ekledikten](../../azure-monitor/app/asp-net.md)sonra, daha fazla telemetri almak için yapabileceğiniz birkaç şey vardır. 

| Eylem | Ne alacaksınız|
|---|---|
|(IIS sunucuları) [Durum Monitörünü](https://go.microsoft.com/fwlink/?LinkId=506648) her sunucu makinesine yükleyin.<br/>(Azure web uygulamaları) Web uygulaması için Azure denetim panelinde Application Insights bıçağını açın.| [**Performans sayaçları**](../../azure-monitor/app/performance-counters.md)<br/>[**Özel durumlar**](asp-net-exceptions.md) - ayrıntılı yığın izleri<br/>[**Bağımlılıklar**](../../azure-monitor/app/asp-net-dependencies.md)|
|[JavaScript snippet'ini web sayfalarınıza ekleyin](../../azure-monitor/app/javascript.md)|[Sayfa performansı,](../../azure-monitor/app/usage-overview.md)tarayıcı özel durumları, AJAX performansı. Özel istemci tarafı telemetri.|
|[Kullanılabilirlik web testleri oluşturma](../../azure-monitor/app/monitor-web-app-availability.md)|Siteniz kullanılamıyorsa uyarı alın|
|[buildinfo.config](https://msdn.microsoft.com/library/dn449058.aspx) MSBuild tarafından oluşturulan emin olun|[Metrik grafiklerde ek açıklamalar oluşturma](https://docs.microsoft.com/azure/azure-monitor/app/annotations)
|[Özel olaylar ve ölçümler yazma](../../azure-monitor/app/api-custom-events-metrics.md)|İş olaylarını ve ölçümleri say, ayrıntılı kullanımı ve daha fazlasını izleyin.|
|[Canlı sitenizin profilini](https://aka.ms/AIProfilerPreview)|Canlı web uygulamanızdan ayrıntılı fonksiyon zamanlamaları|






