---
title: Azure 'dan daha fazla yararlanın Application Insights | Microsoft Docs
description: Application Insights ile çalışmaya başladıktan sonra, keşfedebileceğiniz özelliklerin bir özeti aşağıda verilmiştir.
ms.topic: conceptual
ms.date: 02/03/2017
ms.openlocfilehash: 6d30833840b62412ae03c761baacf54c939c9d33
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86540035"
---
# <a name="more-telemetry-from-application-insights"></a>Application Insights 'ten daha fazla telemetri
[ASP.net kodunuza Application Insights](../../azure-monitor/app/asp-net.md)ekledikten sonra daha fazla telemetri almak için yapabileceğiniz birkaç şey vardır. 

| Eylem | Ne alacaksınız|
|---|---|
|(IIS sunucuları) Her bir sunucu makinesine [durum İzleyicisi yükler](https://go.microsoft.com/fwlink/?LinkId=506648) .<br/>(Azure Web Apps) Web uygulamasının Azure denetim masasında Application Insights dikey penceresini açın.| [**Performans sayaçları**](../../azure-monitor/app/performance-counters.md)<br/>[**Özel durumlar**](asp-net-exceptions.md) -ayrıntılı yığın izlemeleri<br/>[**Bağımlılıklar**](../../azure-monitor/app/asp-net-dependencies.md)|
|[JavaScript kod parçacığını Web sayfalarınıza ekleme](../../azure-monitor/app/javascript.md)|[Sayfa performansı](../../azure-monitor/app/usage-overview.md), tarayıcı özel DURUMLARı, Ajax performansı. Özel istemci tarafı telemetrisi.|
|[Kullanılabilirlik Web testleri oluşturma](../../azure-monitor/app/monitor-web-app-availability.md)|Siteniz kullanılamaz hale gelirse uyarı alın|
|buildinfo.configMSBuild tarafından oluşturulduğundan [emin olun](/visualstudio/debugger/diagnose-problems-after-deployment?view=vs-2015)|[Ölçüm grafiklerinde derleme ek açıklamaları](./annotations.md)
|[Özel olayları ve ölçümleri yazma](../../azure-monitor/app/api-custom-events-metrics.md)|İş olayları ve ölçümleri Sayın, ayrıntılı kullanımı izleyin ve daha fazlasını yapın.|
|[Canlı sitenizin profilini yapın](https://aka.ms/AIProfilerPreview)|Canlı Web uygulamanızdan ayrıntılı işlev zamanlamaları|
