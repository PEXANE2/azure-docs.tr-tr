---
title: Azure 'dan daha fazla yararlanın Application Insights | Microsoft Docs
description: Application Insights ile çalışmaya başladıktan sonra, keşfedebileceğiniz özelliklerin bir özeti aşağıda verilmiştir.
ms.topic: conceptual
ms.date: 02/03/2017
ms.openlocfilehash: 3c4a9a736f34d4f7dbfeb004d0837f1f2efa55dc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77666033"
---
# <a name="more-telemetry-from-application-insights"></a>Application Insights 'ten daha fazla telemetri
[ASP.net kodunuza Application Insights](../../azure-monitor/app/asp-net.md)ekledikten sonra daha fazla telemetri almak için yapabileceğiniz birkaç şey vardır. 

| Eylem | Ne alacaksınız|
|---|---|
|(IIS sunucuları) Her bir sunucu makinesine [durum İzleyicisi yükler](https://go.microsoft.com/fwlink/?LinkId=506648) .<br/>(Azure Web Apps) Web uygulamasının Azure denetim masasında Application Insights dikey penceresini açın.| [**Performans sayaçları**](../../azure-monitor/app/performance-counters.md)<br/>[**Özel durumlar**](asp-net-exceptions.md) -ayrıntılı yığın izlemeleri<br/>[**Bağımlılıklar**](../../azure-monitor/app/asp-net-dependencies.md)|
|[JavaScript kod parçacığını Web sayfalarınıza ekleme](../../azure-monitor/app/javascript.md)|[Sayfa performansı](../../azure-monitor/app/usage-overview.md), tarayıcı özel DURUMLARı, Ajax performansı. Özel istemci tarafı telemetrisi.|
|[Kullanılabilirlik Web testleri oluşturma](../../azure-monitor/app/monitor-web-app-availability.md)|Siteniz kullanılamaz hale gelirse uyarı alın|
|buildinfo.configMSBuild tarafından oluşturulduğundan [emin olun](https://msdn.microsoft.com/library/dn449058.aspx)|[Ölçüm grafiklerinde derleme ek açıklamaları](https://docs.microsoft.com/azure/azure-monitor/app/annotations)
|[Özel olayları ve ölçümleri yazma](../../azure-monitor/app/api-custom-events-metrics.md)|İş olayları ve ölçümleri Sayın, ayrıntılı kullanımı izleyin ve daha fazlasını yapın.|
|[Canlı sitenizin profilini yapın](https://aka.ms/AIProfilerPreview)|Canlı Web uygulamanızdan ayrıntılı işlev zamanlamaları|






