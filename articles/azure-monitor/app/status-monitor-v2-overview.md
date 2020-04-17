---
title: Azure Uygulama Öngörüleri Aracısı'na genel bakış | Microsoft Dokümanlar
description: Uygulama Öngörüleri Aracısı'na genel bakış. Web sitesini yeniden dağıtmadan web sitesinin performansını izleyin. Şirket içinde, VM'lerde veya Azure'da barındırılan ASP.NET web uygulamalarıyla çalışır.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 09/16/2019
ms.openlocfilehash: 4042fc05f278915fe72bf8fc4e6afd69a5bbb4b9
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537432"
---
# <a name="deploy-azure-monitor-application-insights-agent-for-on-premises-servers"></a>Şirket içi sunucular için Azure Monitör uygulama istatistikleri aracısı dağıtma

> [!IMPORTANT]
> Bu kılavuz, Uygulama Öngörüleri Aracısı'nın Şirket içi ve Azure'a uygun olmayan bulut dağıtımları için önerilir. Azure [sanal makine ve sanal makine ölçeği kümesi dağıtımları](https://docs.microsoft.com/azure/azure-monitor/app/azure-vm-vmss-apps)için önerilen yaklaşım aşağıda veda edinebilirsiniz.

Application Insights Agent (eski adıyla Status Monitor V2), [PowerShell Gallery'de](https://www.powershellgallery.com/packages/Az.ApplicationMonitor)yayınlanan bir PowerShell modülüdür.
[Durum Monitörü'nin](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now)yerini alır.
Telemetri, uygulamanızı [izleyebileceğiniz](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) Azure portalına gönderilir.

> [!NOTE]
> Modül şu anda yalnızca IIS ile barındırılan .NET web uygulamalarının kodsuz enstrümantasyonunu destekler. Core, Java ve Node.js uygulamalarını ASP.NET bir SDK'yı kullanın.

## <a name="powershell-gallery"></a>PowerShell Galerisi

Uygulama Insights Aracısı burada https://www.powershellgallery.com/packages/Az.ApplicationMonitoryer almaktadır: .

![PowerShell Galerisi](https://img.shields.io/powershellgallery/v/Az.ApplicationMonitor.svg?color=Blue&label=Current%20Version&logo=PowerShell&style=for-the-badge)


## <a name="instructions"></a>Yönergeler
- Kısa kod örnekleriyle başlamak için [başlangıç yönergelerine](status-monitor-v2-get-started.md) bakın.
- Nasıl başlayacağınız hakkında derin bir dalış için [ayrıntılı talimatlara](status-monitor-v2-detailed-instructions.md) bakın.

## <a name="powershell-api-reference"></a>PowerShell API başvurusu
- [Devre Dışı-UygulamaInsightsİzleme](status-monitor-v2-api-disable-monitoring.md)
- [Devre Dışı Bırakma-InstrumentationEngine](status-monitor-v2-api-disable-instrumentation-engine.md)
- [Enable-ApplicationInsightsMonitoring](status-monitor-v2-api-enable-monitoring.md)
- [Etkinleştirme-EnstrümantasyonMotoru](status-monitor-v2-api-enable-instrumentation-engine.md)
- [Get-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-get-config.md)
- [Get-ApplicationInsightsİzlemeDurumu](status-monitor-v2-api-get-status.md)
- [Set-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-set-config.md)
- [Başlat-UygulamaInsightsMonitoringTrace](status-monitor-v2-api-start-trace.md)

## <a name="troubleshooting"></a>Sorun giderme
- [Sorun giderme](status-monitor-v2-troubleshoot.md)
- [Bilinen sorunlar](status-monitor-v2-troubleshoot.md#known-issues)


## <a name="faq"></a>SSS

- Application Insights Agent proxy yüklemelerini destekliyor mu?

  *Evet, evet.* Application Insights Agent'ı indirmenin birden çok yolu vardır. Bilgisayarınızda internet erişimi varsa, parametreleri kullanarak `-Proxy` PowerShell Galerisi'ne binebilirsiniz.
Ayrıca modülü el ile indirebilir ve bilgisayarınıza yükleyebilir veya doğrudan kullanabilirsiniz.
Bu seçeneklerin her biri [ayrıntılı yönergelerde](status-monitor-v2-detailed-instructions.md)açıklanmıştır.

- Status Monitor v2, Core uygulamalarını ASP.NET destekliyor mu?

  *Hayır, hayır.* ASP.NET Çekirdek uygulamalarının izlenmesini sağlamak için ASP.NET [Temel uygulamalar için Uygulama Öngörüleri](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core)bölümüne bakın. ASP.NET Core uygulaması için StatusMonitor'u yüklemenize gerek yoktur. ASP.NET Core uygulaması IIS'de barındırılan olsa bile bu durum geçerlidir.

- Etkinleştirmenin başarılı olduğunu nasıl doğrulayabilirim?

  - [Get-ApplicationInsightsMonitoringStatus](status-monitor-v2-api-get-status.md) cmdlet, etkinleştirmenin başarılı olduğunu doğrulamak için kullanılabilir.
  - Uygulamanızın telemetri gönderip göndermedığını hızlı bir şekilde belirlemek için [Canlı Ölçümler'i](https://docs.microsoft.com/azure/azure-monitor/app/live-stream) kullanmanızı öneririz.

  - Telemetri gönderen tüm bulut rollerini listelemek için [Log Analytics'i](../log-query/get-started-portal.md) de kullanabilirsiniz:
      ```Kusto
      union * | summarize count() by cloud_RoleName, cloud_RoleInstance
      ```

## <a name="next-steps"></a>Sonraki adımlar

Telemetrinizi görüntüleyin:

* Performansı ve kullanımı izlemek için [ölçümleri keşfedin.](../../azure-monitor/platform/metrics-charts.md)
* Sorunları tanılamak için [olayları ve günlükleri arayın.](../../azure-monitor/app/diagnostic-search.md)
* Daha gelişmiş sorgular için [Analytics'i kullanın.](../../azure-monitor/app/analytics.md)
* [Panolar oluşturun.](../../azure-monitor/app/overview-dashboard.md)

Daha fazla telemetri ekleyin:

* Sitenizin canlı kalması için [web testleri oluşturun](monitor-web-app-availability.md).
* Web sayfası kodundan özel durumları görmek ve izleme çağrılarını etkinleştirmek için [web istemcisi telemetrisi ekleyin.](../../azure-monitor/app/javascript.md)
* İzleme ve günlük aramaları ekleyebilmeniz [için Uygulama Öngörüleri SDK'yı kodunuza ekleyin.](../../azure-monitor/app/asp-net.md)

