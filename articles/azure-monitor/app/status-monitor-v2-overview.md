---
title: Azure Durum İzleyicisi v2 'ye Genel Bakış | Microsoft Docs
description: Durum İzleyicisi v2 'ye genel bakış. Web sitesini yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. Şirket içinde, VM 'lerde veya Azure 'da barındırılan ASP.NET Web Apps ile birlikte kullanılır.
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: 0264cf3a972c35edb3ad6dc600ca39bdaa076dfd
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68333692"
---
# <a name="status-monitor-v2"></a>Durum İzleyicisi v2

Durum İzleyicisi v2, [PowerShell Galerisi](https://www.powershellgallery.com/packages/Az.ApplicationMonitor)yayımlanan bir PowerShell modülüdür.
[Durum İzleyicisi](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now)yerini alır.
Modül, IIS ile barındırılan .NET Web Apps 'in kodsuz kullanacaksınız izleme sağlar.
Telemetri Azure portal, uygulamanızı izleyebileceğiniz bir şekilde gönderilir. [](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview)

## <a name="powershell-gallery"></a>PowerShell Galerisi

Durum İzleyicisi v2 şurada bulunur: https://www.powershellgallery.com/packages/Az.ApplicationMonitor.

![PowerShell Galerisi](https://img.shields.io/powershellgallery/v/Az.ApplicationMonitor.svg?color=Blue&label=Current%20Version&logo=PowerShell&style=for-the-badge)


## <a name="instructions"></a>Yönergeler
- Kısa kod örneklerini kullanmaya başlamak için Başlarken [yönergelerine](status-monitor-v2-get-started.md) bakın.
- Nasıl başlatacağınız hakkında ayrıntılı bilgi için bkz. [ayrıntılı yönergeler](status-monitor-v2-detailed-instructions.md) .

## <a name="powershell-api-reference"></a>PowerShell API başvurusu
- [Disable-ApplicationInsightsMonitoring](status-monitor-v2-api-disable-monitoring.md)
- [Disable-ınstrumentationengine](status-monitor-v2-api-disable-instrumentation-engine.md)
- [Enable-ApplicationInsightsMonitoring](status-monitor-v2-api-enable-monitoring.md)
- [Enable-ınstrumentationengine](status-monitor-v2-api-enable-instrumentation-engine.md)
- [Get-Applicationınsiısmonitoringconfig](status-monitor-v2-api-get-config.md)
- [Get-Applicationınsiısmonitoringstatus](status-monitor-v2-api-get-status.md)
- [Set-Applicationınsiısmonitoringconfig](status-monitor-v2-api-set-config.md)
- [Start-Applicationınsiizsmonitoringtrace](status-monitor-v2-api-start-trace.md)

## <a name="troubleshooting"></a>Sorun giderme
- [Sorun giderme](status-monitor-v2-troubleshoot.md)
- [Bilinen sorunlar](status-monitor-v2-troubleshoot.md#known-issues)


## <a name="faq"></a>SSS

- Durum İzleyicisi v2 proxy yüklemelerini destekliyor mu?

  *Evet*. Durum İzleyicisi v2 'yi indirmek için birden çok yol vardır. Bilgisayarınızda internet erişimi varsa, parametreleri kullanarak `-Proxy` PowerShell Galerisi ekleyebilirsiniz.
Ayrıca, modülü el ile indirebilir ve bilgisayarınıza yükleyebilir ya da doğrudan kullanabilirsiniz.
Bu seçeneklerin her biri [ayrıntılı yönergeler](status-monitor-v2-detailed-instructions.md)bölümünde açıklanmıştır.
  
- Nasıl yaparım?, etkinleştirme başarılı oldu mu?

  - [Get-Applicationınsisofsmonitoringstatus](status-monitor-v2-api-get-status.md) cmdlet 'i, etkinleştirme başarılı olduğunu doğrulamak için kullanılabilir.
  - Uygulamanızın telemetri gönderip göndermediğine hızlı bir şekilde anlamak için [canlı ölçümleri](https://docs.microsoft.com/azure/azure-monitor/app/live-stream) kullanmanızı öneririz.

  - Şu anda telemetri gönderen tüm bulut rollerini listelemek için [Log Analytics](../log-query/get-started-portal.md) de kullanabilirsiniz:
      ```Kusto
      union * | summarize count() by cloud_RoleName, cloud_RoleInstance
      ```

## <a name="next-steps"></a>Sonraki adımlar

Telemetrinizi görüntüleyin:

* Performansı ve kullanımı izlemek için [ölçümleri](../../azure-monitor/app/metrics-explorer.md) bulun.
* Sorunları tanılamak için [olayları ve günlükleri arayın](../../azure-monitor/app/diagnostic-search.md) .
* Daha gelişmiş sorgular için [analiz kullanın](../../azure-monitor/app/analytics.md) .
* [Panolar oluşturun](../../azure-monitor/app/overview-dashboard.md).

Daha fazla telemetri ekleyin:

* Sitenizin canlı kaldığından emin olmak için [Web testleri oluşturun](monitor-web-app-availability.md) .
* Web sayfası kodundan özel durumları görmek ve izleme çağrılarını etkinleştirmek için [Web istemcisi telemetrisini ekleyin](../../azure-monitor/app/javascript.md) .
* İzleme ve günlük çağrısı ekleyebilmeniz [için Application Insights SDK 'sını kodunuza ekleyin](../../azure-monitor/app/asp-net.md) .

