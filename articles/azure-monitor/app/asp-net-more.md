---
title: Azure 'dan daha fazla yararlanın Application Insights | Microsoft Docs
description: Application Insights ile çalışmaya başladıktan sonra, keşfedebileceğiniz özelliklerin bir özeti aşağıda verilmiştir.
ms.topic: conceptual
ms.date: 02/03/2017
ms.openlocfilehash: c0be377636159dbb2bdb628af6a0bc9077942397
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87321352"
---
# <a name="more-telemetry-from-application-insights"></a>Application Insights 'ten daha fazla telemetri
[ASP.net kodunuza Application Insights](./asp-net.md)ekledikten sonra daha fazla telemetri almak için yapabileceğiniz birkaç şey vardır. 

| Eylem | Ne alacaksınız|
|---|---|
|(IIS sunucuları) Her bir sunucu makinesine [durum İzleyicisi yükler](https://go.microsoft.com/fwlink/?LinkId=506648) .<br/>(Azure Web Apps) Web uygulamasının Azure denetim masasında Application Insights dikey penceresini açın.| [**Performans sayaçları**](./performance-counters.md)<br/>[**Özel durumlar**](asp-net-exceptions.md) -ayrıntılı yığın izlemeleri<br/>[**Bağımlılıklar**](./asp-net-dependencies.md)|
|[JavaScript kod parçacığını Web sayfalarınıza ekleme](./javascript.md)|[Sayfa performansı](./usage-overview.md), tarayıcı özel DURUMLARı, Ajax performansı. Özel istemci tarafı telemetrisi.|
|[Kullanılabilirlik Web testleri oluşturma](./monitor-web-app-availability.md)|Siteniz kullanılamaz hale gelirse uyarı alın|
|buildinfo.configMSBuild tarafından oluşturulduğundan [emin olun](/visualstudio/debugger/diagnose-problems-after-deployment?view=vs-2015)|[Ölçüm grafiklerinde derleme ek açıklamaları](./annotations.md)
|[Özel olayları ve ölçümleri yazma](./api-custom-events-metrics.md)|İş olayları ve ölçümleri Sayın, ayrıntılı kullanımı izleyin ve daha fazlasını yapın.|
|[Canlı sitenizin profilini yapın](https://aka.ms/AIProfilerPreview)|Canlı Web uygulamanızdan ayrıntılı işlev zamanlamaları|

