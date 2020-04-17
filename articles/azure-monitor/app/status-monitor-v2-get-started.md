---
title: Azure Uygulama Öngörüleri Aracısı - başlıyor | Microsoft Dokümanlar
description: Application Insights Agent için hızlı başlangıç kılavuzu. Web sitesini yeniden dağıtmadan web sitesinin performansını izleyin. Şirket içinde, VM'lerde veya Azure'da barındırılan ASP.NET web uygulamalarıyla çalışır.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: 4cfa136585611e81a4060c5544d5dc464b32f12c
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537449"
---
# <a name="get-started-with-azure-monitor-application-insights-agent-for-on-premises-servers"></a>Şirket içi sunucular için Azure Monitör Uygulama Öngörüleri Aracısı ile başlayın

Bu makalede, çoğu ortam için çalışması beklenen hızlı başlangıç komutları içerir.
Talimatlar güncellemeleri dağıtmak için PowerShell Galerisi'ne bağlıdır.
Bu komutlar PowerShell `-Proxy` parametresini destekler.

Bu komutların, özelleştirme yönergelerinin ve sorun giderme yle ilgili bilgilerin açıklaması için [ayrıntılı yönergelere](status-monitor-v2-detailed-instructions.md)bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="download-and-install-via-powershell-gallery"></a>PowerShell Gallery üzerinden indirin ve yükleyin

### <a name="install-prerequisites"></a>Ön koşulları yükleme
PowerShell'i Yönetici olarak çalıştırın.
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
Install-Module -Name PowerShellGet -Force
``` 
PowerShell'i kapatın.

### <a name="install-application-insights-agent"></a>Uygulama Öngörüleri Aracısı Yükle
PowerShell'i Yönetici olarak çalıştırın.
```powershell   
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-Module -Name Az.ApplicationMonitor -AllowPrerelease -AcceptLicense
``` 

### <a name="enable-monitoring"></a>İzlemeyi etkinleştirme
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```
    
        
## <a name="download-and-install-manually-offline-option"></a>El ile indirin ve yükleyin (çevrimdışı seçenek)
### <a name="download-the-module"></a>Modülü indirin
Modülün en son sürümünü [PowerShell Gallery'den](https://www.powershellgallery.com/packages/Az.ApplicationMonitor)manuel olarak indirin.

### <a name="unzip-and-install-application-insights-agent"></a>Zip'i kaldırın ve Uygulama Öngörüleri Aracısı'nı yükleyin
```powershell
$pathToNupkg = "C:\Users\t\Desktop\Az.ApplicationMonitor.0.3.0-alpha.nupkg"
$pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
$pathToNupkg | rename-item -newname $pathToZip
$pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\Az.ApplicationMonitor"
Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
```
### <a name="enable-monitoring"></a>İzlemeyi etkinleştirme
```powershell
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```



## <a name="next-steps"></a>Sonraki adımlar

 Telemetrinizi görüntüleyin:

- Performansı ve kullanımı izlemek için [ölçümleri keşfedin.](../../azure-monitor/platform/metrics-charts.md)
- Sorunları tanılamak için [olayları ve günlükleri arayın.](../../azure-monitor/app/diagnostic-search.md)
- Daha gelişmiş sorgular için [Analytics'i kullanın.](../../azure-monitor/app/analytics.md)
- [Panolar oluşturun.](../../azure-monitor/app/overview-dashboard.md)

 Daha fazla telemetri ekleyin:

- Sitenizin canlı kalması için [web testleri oluşturun](monitor-web-app-availability.md).
- Web sayfası kodundan özel durumları görmek ve izleme çağrılarını etkinleştirmek için [web istemcisi telemetrisi ekleyin.](../../azure-monitor/app/javascript.md)
- İzleme ve günlük aramaları ekleyebilmeniz [için Uygulama Öngörüleri SDK'yı kodunuza ekleyin.](../../azure-monitor/app/asp-net.md)

Application Insights Agent ile daha fazlasını yapın:

- Burada bulunan komutların açıklaması için [ayrıntılı yönergeleri](status-monitor-v2-detailed-instructions.md) gözden geçirin.
- Uygulama Öngörüleri [Aracısı'nın sorun giderme](status-monitor-v2-troubleshoot.md) kılavuzunu kullanın.
