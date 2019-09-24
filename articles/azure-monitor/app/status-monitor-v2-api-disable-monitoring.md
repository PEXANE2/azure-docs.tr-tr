---
title: 'Azure Durum İzleyicisi v2 API başvurusu: İzlemeyi devre dışı bırak | Microsoft Docs'
description: Durum İzleyicisi v2 API başvurusu. Disable-ApplicationInsightsMonitoring. Web sitesini yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. Şirket içinde, VM 'lerde veya Azure 'da barındırılan ASP.NET Web Apps ile birlikte kullanılır.
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
ms.openlocfilehash: af105a6a819ced475f490d3c99fc1e63d8eee5c8
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71203442"
---
# <a name="status-monitor-v2-api-disable-applicationinsightsmonitoring"></a>Durum İzleyicisi v2 API 'SI: Disable-ApplicationInsightsMonitoring

Bu makalede, [az. ApplicationMonitor PowerShell modülünün](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)üyesi olan bir cmdlet açıklanmaktadır.

## <a name="description"></a>Açıklama

Hedef bilgisayarda izlemeyi devre dışı bırakır.
Bu cmdlet, IIS applicationHost. config ' deki düzenlemeleri kaldırır ve kayıt defteri anahtarlarını kaldırır.

> [!IMPORTANT] 
> Bu cmdlet yönetici izinlerine sahip bir PowerShell oturumu gerektirir.

## <a name="examples"></a>Örnekler

```powershell
PS C:\> Disable-ApplicationInsightsMonitoring
```

## <a name="parameters"></a>Parametreler 

### <a name="-verbose"></a>-Ayrıntılı
**Ortak parametre.** Ayrıntılı günlükleri göstermek için bu anahtarı kullanın.

## <a name="output"></a>Output


#### <a name="example-output-from-successfully-disabling-monitoring"></a>İzlemeyi başarıyla devre dışı bırakarak oluşan örnek çıkış

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

 Durum İzleyicisi v2 ile daha fazlasını yapın:
 - Durum İzleyicisi v2 [sorunlarını gidermek](status-monitor-v2-troubleshoot.md) için kılavuzumuzu kullanın.
