---
title: Azure Durum İzleyicisi v2-Başlarken | Microsoft Docs
description: Durum İzleyicisi v2 için hızlı başlangıç kılavuzu. Web sitesini yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. Şirket içinde, VM 'lerde veya Azure 'da barındırılan ASP.NET Web Apps ile birlikte kullanılır.
services: application-insights
documentationcenter: .net
author: TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: d9c354edac3cbd3faccaa261654e56f858befdf6
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71058244"
---
# <a name="get-started-with-status-monitor-v2"></a>Durum İzleyicisi v2 ile çalışmaya başlayın

Bu makale, çoğu ortam için çalışması beklenen hızlı başlangıç komutlarını içerir.
Yönergeler, güncelleştirmeleri dağıtmak için PowerShell Galerisi bağımlıdır.
Bu komutlar PowerShell `-Proxy` parametresini destekler.

Bu komutların açıklaması, özelleştirme yönergeleri ve sorun giderme hakkında bilgi için bkz. [ayrıntılı yönergeler](status-monitor-v2-detailed-instructions.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="download-and-install-via-powershell-gallery"></a>PowerShell Galerisi aracılığıyla indirme ve yükleme

### <a name="install-prerequisites"></a>Ön koşulları yükle
PowerShell 'i yönetici olarak çalıştırın.
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
Install-Module -Name PowerShellGet -Force
``` 
PowerShell 'i kapatın.

### <a name="install-status-monitor-v2"></a>Durum İzleyicisi v2 'yi yükler
PowerShell 'i yönetici olarak çalıştırın.
```powershell   
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-Module -Name Az.ApplicationMonitor -AllowPrerelease -AcceptLicense
``` 

### <a name="enable-monitoring"></a>İzlemeyi etkinleştir
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```
    
        
## <a name="download-and-install-manually-offline-option"></a>El ile indir ve yükle (çevrimdışı seçenek)
### <a name="download-the-module"></a>Modülü indir
[PowerShell Galerisi](https://www.powershellgallery.com/packages/Az.ApplicationMonitor)' dan modülün en son sürümünü el ile indirin.

### <a name="unzip-and-install-status-monitor-v2"></a>Durum İzleyicisi v2 'yi unzip ve yükler
```powershell
$pathToNupkg = "C:\Users\t\Desktop\Az.ApplicationMonitor.0.3.0-alpha.nupkg"
$pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
$pathToNupkg | rename-item -newname $pathToZip
$pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\Az.ApplicationMonitor"
Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
```
### <a name="enable-monitoring"></a>İzlemeyi etkinleştir
```powershell
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```



## <a name="next-steps"></a>Sonraki adımlar

 Telemetrinizi görüntüleyin:

- Performansı ve kullanımı izlemek için [ölçümleri](../../azure-monitor/app/metrics-explorer.md) bulun.
- Sorunları tanılamak için [olayları ve günlükleri arayın](../../azure-monitor/app/diagnostic-search.md) .
- Daha gelişmiş sorgular için [analiz kullanın](../../azure-monitor/app/analytics.md) .
- [Panolar oluşturun](../../azure-monitor/app/overview-dashboard.md).

 Daha fazla telemetri ekleyin:

- Sitenizin canlı kaldığından emin olmak için [Web testleri oluşturun](monitor-web-app-availability.md) .
- Web sayfası kodundan özel durumları görmek ve izleme çağrılarını etkinleştirmek için [Web istemcisi telemetrisini ekleyin](../../azure-monitor/app/javascript.md) .
- İzleme ve günlük çağrısı ekleyebilmeniz [için Application Insights SDK 'sını kodunuza ekleyin](../../azure-monitor/app/asp-net.md) .

Durum İzleyicisi v2 ile daha fazlasını yapın:

- Burada bulunan komutların açıklaması için [ayrıntılı yönergeleri](status-monitor-v2-detailed-instructions.md) gözden geçirin.
- Durum İzleyicisi v2 [sorunlarını gidermek](status-monitor-v2-troubleshoot.md) için kılavuzumuzu kullanın.
