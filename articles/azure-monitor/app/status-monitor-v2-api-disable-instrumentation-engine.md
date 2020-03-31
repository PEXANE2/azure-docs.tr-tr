---
title: Azure Uygulama Öngörüleri AracıAP başvurusu
description: Uygulama Öngörüleri Aracısı API başvurusu. Devre Dışı Bırakma-InstrumentationEngine. Web sitesini yeniden dağıtmadan web sitesinin performansını izleyin. Şirket içinde, VM'lerde veya Azure'da barındırılan ASP.NET web uygulamalarıyla çalışır.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: dafa12db95a10df467bf0d042cfd9395720648b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671384"
---
# <a name="application-insights-agent-api-disable-instrumentationengine"></a>Uygulama Öngörüleri Aracısı API: Devre Dışı-InstrumentationEngine

Bu [makalede, Az.ApplicationMonitor PowerShell modülüüyesi](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)bir cmdlet açıklanır.

## <a name="description"></a>Açıklama
Bazı kayıt defteri anahtarlarını kaldırarak enstrümantasyon motorunu devre dışı kaldırır.
Değişikliklerin etkili olması için IIS'yi yeniden başlatın.

> [!IMPORTANT] 
> Bu cmdlet, Yönetici izinleri içeren bir PowerShell oturumu gerektirir.

## <a name="examples"></a>Örnekler

```powershell
PS C:\> Disable-InstrumentationEngine
```

## <a name="parameters"></a>Parametreler 

### <a name="-verbose"></a>-Verbose
**Ortak parametre.** Ayrıntılı günlükleri çıktıetmek için bu anahtarı kullanın.

## <a name="output"></a>Çıktı


#### <a name="example-output-from-successfully-disabling-the-instrumentation-engine"></a>Enstrümantasyon motorunun başarılı bir şekilde devre dışı bırakılmasından örnek çıktı

```
Configuring IIS Environment for instrumentation engine...
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]'
Configuring registry for instrumentation engine...
```


## <a name="next-steps"></a>Sonraki adımlar

 Application Insights Agent ile daha fazlasını yapın:
 - Uygulama Öngörüleri [Aracısı'nın sorun giderme](status-monitor-v2-troubleshoot.md) kılavuzunu kullanın.
