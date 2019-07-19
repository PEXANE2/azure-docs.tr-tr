---
title: 'Azure Durum İzleyicisi v2 API başvurusu: İzleme altyapısını devre dışı bırak | Microsoft Docs'
description: Durum İzleyicisi v2 API başvurusu. Disable-ınstrumentationengine. Web sitesini yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. Şirket içinde, VM 'lerde veya Azure 'da barındırılan ASP.NET Web Apps ile birlikte kullanılır.
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
ms.openlocfilehash: 827f8555718a22979617c9b0794dce3bd92207f4
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326401"
---
# <a name="status-monitor-v2-api-disable-instrumentationengine"></a>Durum İzleyicisi v2 API 'SI: Disable-ınstrumentationengine

Bu makalede, [az. ApplicationMonitor PowerShell modülünün](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)üyesi olan bir cmdlet açıklanmaktadır.

## <a name="description"></a>Açıklama
Bazı kayıt defteri anahtarlarını kaldırarak izleme altyapısını devre dışı bırakır.
Değişikliklerin etkili olması için IIS 'yi yeniden başlatın.

> [!IMPORTANT] 
> Bu cmdlet yönetici izinlerine sahip bir PowerShell oturumu gerektirir.

## <a name="examples"></a>Örnekler

```powershell
PS C:\> Disable-InstrumentationEngine
```

## <a name="parameters"></a>Parametreler 

### <a name="-verbose"></a>-Ayrıntılı
**Ortak parametre.** Ayrıntılı günlükleri çıkarmak için bu anahtarı kullanın.

## <a name="output"></a>Output


#### <a name="example-output-from-successfully-disabling-the-instrumentation-engine"></a>İzleme altyapısını başarıyla devre dışı bırakma örnek çıkışı

```
Configuring IIS Environment for instrumentation engine...
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]'
Configuring registry for instrumentation engine...
```


## <a name="next-steps"></a>Sonraki adımlar

 Durum İzleyicisi v2 ile daha fazlasını yapın:
 - Durum İzleyicisi v2 [sorunlarını gidermek](status-monitor-v2-troubleshoot.md) için kılavuzumuzu kullanın.
