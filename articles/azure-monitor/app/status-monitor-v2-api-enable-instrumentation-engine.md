---
title: 'Azure Durum İzleyicisi v2 API başvurusu: İzleme altyapısını etkinleştir | Microsoft Docs'
description: Durum İzleyicisi v2 API başvurusu. Enable-ınstrumentationengine. Web sitesini yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. Şirket içinde, VM 'lerde veya Azure 'da barındırılan ASP.NET Web Apps ile birlikte kullanılır.
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
ms.openlocfilehash: d4683a1cad5172f7104e745433bd141bcf36d56f
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68326381"
---
# <a name="status-monitor-v2-api-enable-instrumentationengine"></a>Durum İzleyicisi v2 API 'SI: Enable-ınstrumentationengine

Bu makalede, [az. ApplicationMonitor PowerShell modülünün](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)üyesi olan bir cmdlet açıklanmaktadır.

## <a name="description"></a>Açıklama

Bazı kayıt defteri anahtarlarını ayarlayarak izleme altyapısını sunar.
Değişikliklerin etkili olması için IIS 'yi yeniden başlatın.

İzleme altyapısı, .NET SDK 'Ları tarafından toplanan verileri tamamlayabilir.
Yönetilen bir işlemin yürütülmesini tanımlayan olayları ve iletileri toplar. Bu olaylar ve mesajlar bağımlılık sonuç kodlarını, HTTP fiillerini ve [SQL komut metnini](asp-net-dependencies.md#advanced-sql-tracking-to-get-full-sql-query)içerir.

Şu durumlarda izleme altyapısını etkinleştirin:
- Enable cmdlet 'i ile izlemeyi zaten etkinleştirdiniz, ancak izleme altyapısını etkinleştirmediniz.
- Uygulamanızı .NET SDK 'Ları ile el ile gördünüz ve ek telemetri toplamak istiyorsunuz.

> [!IMPORTANT] 
> Bu cmdlet yönetici izinlerine sahip bir PowerShell oturumu gerektirir.

> [!NOTE] 
> - Bu cmdlet, lisans ve Gizlilik Bildirimimizi incelemenizi ve kabul etmenizi gerektirir.
> - İzleme altyapısı ek yük ekler ve varsayılan olarak kapalıdır.

## <a name="examples"></a>Örnekler

```powershell
PS C:\> Enable-InstrumentationEngine
```

## <a name="parameters"></a>Parametreler

### <a name="-acceptlicense"></a>-AcceptLicense
**İsteğe bağlı.** Gözetimsiz yüklemelerde lisans ve gizlilik bildirimini kabul etmek için bu anahtarı kullanın.

### <a name="-verbose"></a>-Ayrıntılı
**Ortak parametre.** Ayrıntılı günlükleri çıkarmak için bu anahtarı kullanın.

## <a name="output"></a>Output


#### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>İzleme altyapısını başarıyla etkinleştirmenin örnek çıkışı

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
```

## <a name="next-steps"></a>Sonraki adımlar

  Telemetrinizi görüntüleyin:
 - Performansı ve kullanımı izlemek için [ölçümleri](../../azure-monitor/app/metrics-explorer.md) bulun.
- Sorunları tanılamak için [olayları ve günlükleri arayın](../../azure-monitor/app/diagnostic-search.md) .
- Daha gelişmiş sorgular için [analiz](../../azure-monitor/app/analytics.md) kullanın.
- [Panolar oluşturun](../../azure-monitor/app/overview-dashboard.md).
 
 Daha fazla telemetri ekleyin:
 - Sitenizin canlı kaldığından emin olmak için [Web testleri oluşturun](monitor-web-app-availability.md) .
- Web sayfası kodundan özel durumları görmek ve izleme çağrılarını etkinleştirmek için [Web istemcisi telemetrisini ekleyin](../../azure-monitor/app/javascript.md) .
- İzleme ve günlük çağrısı ekleyebilmeniz [için Application Insights SDK 'sını kodunuza ekleyin](../../azure-monitor/app/asp-net.md) .
 
 Durum İzleyicisi v2 ile daha fazlasını yapın:
 - Durum İzleyicisi v2 [sorunlarını gidermek](status-monitor-v2-troubleshoot.md) için kılavuzumuzu kullanın.
 - Ayarlarınızın doğru kaydedildiğinden emin olmak için [yapılandırmayı alın](status-monitor-v2-api-get-config.md) .
 - İzlemeyi İnceleme [durumunu alın](status-monitor-v2-api-get-status.md) .
