---
title: Azure Uygulama Öngörüleri AracıAP başvurusu
description: Uygulama Öngörüleri Aracısı API başvurusu. Devre Dışı-UygulamaInsightsMonitoring. Web sitesini yeniden dağıtmadan web sitesinin performansını izleyin. Şirket içinde, VM'lerde veya Azure'da barındırılan ASP.NET web uygulamalarıyla çalışır.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: 0678186012678a68fa80a23685f8b346c8c5b859
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671299"
---
# <a name="application-insights-agent-api-disable-applicationinsightsmonitoring"></a>Uygulama Öngörüleri Aracısı API: Devre Dışı-UygulamaInsightsMonitoring

Bu [makalede, Az.ApplicationMonitor PowerShell modülüüyesi](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)bir cmdlet açıklanır.

## <a name="description"></a>Açıklama

Hedef bilgisayardaki izlemeyi devre dışı kılabilir.
Bu cmdlet IIS applicationHost.config ve kayıt defteri anahtarlarını kaldırmak için yapılan ları kaldırır.

> [!IMPORTANT] 
> Bu cmdlet, Yönetici izinleri içeren bir PowerShell oturumu gerektirir.

## <a name="examples"></a>Örnekler

```powershell
PS C:\> Disable-ApplicationInsightsMonitoring
```

## <a name="parameters"></a>Parametreler 

### <a name="-verbose"></a>-Verbose
**Ortak parametre.** Ayrıntılı günlükleri görüntülemek için bu anahtarı kullanın.

## <a name="output"></a>Çıktı


#### <a name="example-output-from-successfully-disabling-monitoring"></a>İzlemeyi başarıyla devre dışı bırakmaktan örnek çıktı

```
Initiating Disable Process
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-00z'
in :1,237
No element in the source document matches '/configuration/location[@path='']/system.webServer/modules/add[@name='ManagedHttpModuleHelper']'
Not executing RemoveAll (transform line 1, 546)
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'disable'
GAC Module will not be removed, since this operation might cause IIS instabilities
Configuring IIS Environment for codeless attach...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring IIS Environment for instrumentation engine...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring registry for instrumentation engine...
Successfully disabled Application Insights Status Monitor
```


## <a name="next-steps"></a>Sonraki adımlar

 Application Insights Agent ile daha fazlasını yapın:
 - Uygulama Öngörüleri [Aracısı'nın sorun giderme](status-monitor-v2-troubleshoot.md) kılavuzunu kullanın.
