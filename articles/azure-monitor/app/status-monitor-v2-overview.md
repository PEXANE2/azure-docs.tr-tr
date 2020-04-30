---
title: Azure Application Insights aracısına genel bakış | Microsoft Docs
description: Application Insights aracısına genel bakış. Web sitesini yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. Şirket içinde, VM 'lerde veya Azure 'da barındırılan ASP.NET Web Apps ile birlikte kullanılır.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 09/16/2019
ms.openlocfilehash: 626a8c96fdc247db174e2cbcd2e5c99cec43d2ea
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81770956"
---
# <a name="deploy-azure-monitor-application-insights-agent-for-on-premises-servers"></a>Şirket içi sunucular için Azure Izleyici Application Insights aracısını dağıtma

> [!IMPORTANT]
> Bu kılavuz, Application Insights aracısına ait şirket Içi ve Azure olmayan bulut dağıtımları için önerilir. [Azure sanal makinesi ve sanal makine ölçek kümesi dağıtımları](https://docs.microsoft.com/azure/azure-monitor/app/azure-vm-vmss-apps)için önerilen yaklaşım aşağıda verilmiştir.

Application Insights Aracısı (eski adıyla Durum İzleyicisi v2) [PowerShell Galerisi](https://www.powershellgallery.com/packages/Az.ApplicationMonitor)yayımlanmış bir PowerShell modülüdür.
[Durum İzleyicisi](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now)yerini alır.
Telemetri Azure portal, uygulamanızı [izleyebileceğiniz](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) bir şekilde gönderilir.

> [!NOTE]
> Modül Şu anda IIS ile barındırılan .NET Web Apps 'in kodsuz kullanacaksınız araçlarını desteklemektedir. ASP.NET Core, Java ve Node. js uygulamalarını işaretlemek için bir SDK kullanın.

## <a name="powershell-gallery"></a>PowerShell Galerisi

Application Insights Aracısı şurada bulunur: https://www.powershellgallery.com/packages/Az.ApplicationMonitor.

![PowerShell Galerisi](https://img.shields.io/powershellgallery/v/Az.ApplicationMonitor.svg?color=Blue&label=Current%20Version&logo=PowerShell&style=for-the-badge)


## <a name="instructions"></a>Yönergeler
- Kısa kod örneklerini kullanmaya başlamak için Başlarken [yönergelerine](status-monitor-v2-get-started.md) bakın.
- Nasıl başlatacağınız hakkında ayrıntılı bilgi için bkz. [ayrıntılı yönergeler](status-monitor-v2-detailed-instructions.md) .

## <a name="powershell-api-reference"></a>PowerShell API başvurusu
- [Disable-ApplicationInsightsMonitoring](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-api-reference#disable-applicationinsightsmonitoring)
- [Disable-ınstrumentationengine](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-api-reference#disable-instrumentationengine)
- [Enable-ApplicationInsightsMonitoring](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-api-reference#enable-applicationinsightsmonitoring)
- [Enable-ınstrumentationengine](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-api-reference#enable-instrumentationengine)
- [Get-Applicationınsiısmonitoringconfig](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-api-reference#get-applicationinsightsmonitoringconfig)
- [Get-Applicationınsiısmonitoringstatus](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-api-reference#get-applicationinsightsmonitoringstatus)
- [Set-Applicationınsiısmonitoringconfig](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-api-reference#set-applicationinsightsmonitoringconfig)
- [Start-Applicationınsiizsmonitoringtrace](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-api-reference#start-applicationinsightsmonitoringtrace)

## <a name="troubleshooting"></a>Sorun giderme
- [Sorun giderme](status-monitor-v2-troubleshoot.md)
- [Bilinen sorunlar](status-monitor-v2-troubleshoot.md#known-issues)


## <a name="faq"></a>SSS

- Application Insights Aracısı proxy yüklemelerini destekliyor mu?

  *Evet*. Application Insights aracısını indirmek için birden çok yol vardır. Bilgisayarınızda internet erişimi varsa, parametreleri kullanarak `-Proxy` PowerShell Galerisi ekleyebilirsiniz.
Ayrıca, modülü el ile indirebilir ve bilgisayarınıza yükleyebilir ya da doğrudan kullanabilirsiniz.
Bu seçeneklerin her biri [ayrıntılı yönergeler](status-monitor-v2-detailed-instructions.md)bölümünde açıklanmıştır.

- Durum İzleyicisi v2 ASP.NET Core uygulamaları destekliyor mu?

  *Hayır*. ASP.NET Core uygulamalarının izlenmesini etkinleştirme yönergeleri için, bkz. [ASP.NET Core uygulamalar için Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core). ASP.NET Core bir uygulama için StatusMonitor yüklemeniz gerekmez. Bu, ASP.NET Core uygulama IIS 'de barındırıldığında bile geçerlidir.

- Nasıl yaparım?, etkinleştirme başarılı oldu mu?

  - [Get-Applicationınsisofsmonitoringstatus](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-api-reference#get-applicationinsightsmonitoringstatus) cmdlet 'i, etkinleştirme başarılı olduğunu doğrulamak için kullanılabilir.
  - Uygulamanızın telemetri gönderip göndermediğine hızlı bir şekilde anlamak için [canlı ölçümleri](https://docs.microsoft.com/azure/azure-monitor/app/live-stream) kullanmanızı öneririz.

  - Şu anda telemetri gönderen tüm bulut rollerini listelemek için [Log Analytics](../log-query/get-started-portal.md) de kullanabilirsiniz:
      ```Kusto
      union * | summarize count() by cloud_RoleName, cloud_RoleInstance
      ```

## <a name="next-steps"></a>Sonraki adımlar

Telemetrinizi görüntüleyin:

* Performansı ve kullanımı izlemek için [ölçümleri](../../azure-monitor/platform/metrics-charts.md) bulun.
* Sorunları tanılamak için [olayları ve günlükleri arayın](../../azure-monitor/app/diagnostic-search.md) .
* Daha gelişmiş sorgular için [analiz kullanın](../../azure-monitor/app/analytics.md) .
* [Panolar oluşturun](../../azure-monitor/app/overview-dashboard.md).

Daha fazla telemetri ekleyin:

* Sitenizin canlı kalması için [web testleri oluşturun](monitor-web-app-availability.md).
* Web sayfası kodundan özel durumları görmek ve izleme çağrılarını etkinleştirmek için [Web istemcisi telemetrisini ekleyin](../../azure-monitor/app/javascript.md) .
* İzleme ve günlük çağrısı ekleyebilmeniz [için Application Insights SDK 'sını kodunuza ekleyin](../../azure-monitor/app/asp-net.md) .

