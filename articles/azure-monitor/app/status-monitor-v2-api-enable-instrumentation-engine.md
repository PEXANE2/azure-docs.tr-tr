---
title: Azure Uygulama Öngörüleri AracıAP başvurusu
description: Uygulama Öngörüleri Aracısı API başvurusu. Enable-InstrumentationEngine. Web sitesini yeniden dağıtmadan web sitesinin performansını izleyin. Şirket içinde, VM'lerde veya Azure'da barındırılan ASP.NET web uygulamalarıyla çalışır.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: b3f298ac31cc584cd16553186359c87f69f27aad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671367"
---
# <a name="application-insights-agent-api-enable-instrumentationengine"></a>Uygulama Öngörüleri Aracısı API: Etkinleştir-InstrumentationEngine

Bu [makalede, Az.ApplicationMonitor PowerShell modülüüyesi](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)bir cmdlet açıklanır.

## <a name="description"></a>Açıklama

Bazı kayıt defteri anahtarlarını ayarlayarak enstrümantasyon motorunu etkinleştirir.
Değişikliklerin etkili olması için IIS'yi yeniden başlatın.

Enstrümantasyon motoru .NET SDK'lar tarafından toplanan verileri tamamlayabilir.
Yönetilen bir işlemin yürütülmesini açıklayan olayları ve iletileri toplar. Bu olaylar ve iletiler bağımlılık sonuç kodları, HTTP fiiller ve [SQL komut metni](asp-net-dependencies.md#advanced-sql-tracking-to-get-full-sql-query)içerir.

Aşağıdakiler varsa enstrümantasyon motorunu etkinleştirin:
- Etkinleştir cmdlet ile izlemeyi zaten etkinleştirdin, ancak enstrümantasyon motorunu etkinleştirmedin.
- Uygulamanızı .NET SDK'larla el ile belgelediniz ve ek telemetri toplamak istiyorsunuz.

> [!IMPORTANT] 
> Bu cmdlet, Yönetici izinleri içeren bir PowerShell oturumu gerektirir.

> [!NOTE] 
> - Bu cmdlet, lisans ve gizlilik bildirimimizi gözden geçirmenizi ve kabul lenmenizi gerektirir.
> - Enstrümantasyon motoru ek ek ek yükü ekler ve varsayılan olarak kapalıdır.

## <a name="examples"></a>Örnekler

```powershell
PS C:\> Enable-InstrumentationEngine
```

## <a name="parameters"></a>Parametreler

### <a name="-acceptlicense"></a>-Kabul Lisansı
**Isteğe bağlı.** Başsız yüklemelerde lisans ve gizlilik bildirimini kabul etmek için bu anahtarı kullanın.

### <a name="-verbose"></a>-Verbose
**Ortak parametre.** Ayrıntılı günlükleri çıktıetmek için bu anahtarı kullanın.

## <a name="output"></a>Çıktı


#### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>Enstrümantasyon motorunun başarılı bir şekilde etkinleştirilmesinden örnek çıktı

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
```

## <a name="next-steps"></a>Sonraki adımlar

  Telemetrinizi görüntüleyin:
 - Performansı ve kullanımı izlemek için [ölçümleri keşfedin.](../../azure-monitor/app/metrics-explorer.md)
- Sorunları tanılamak için [olayları ve günlükleri arayın.](../../azure-monitor/app/diagnostic-search.md)
- Daha gelişmiş sorgular için [analitiği](../../azure-monitor/app/analytics.md) kullanın.
- [Panolar oluşturun.](../../azure-monitor/app/overview-dashboard.md)
 
 Daha fazla telemetri ekleyin:
 - Sitenizin canlı kalması için [web testleri oluşturun](monitor-web-app-availability.md).
- Web sayfası kodundan özel durumları görmek ve izleme çağrılarını etkinleştirmek için [web istemcisi telemetrisi ekleyin.](../../azure-monitor/app/javascript.md)
- İzleme ve günlük aramaları ekleyebilmeniz [için Uygulama Öngörüleri SDK'yı kodunuza ekleyin.](../../azure-monitor/app/asp-net.md)
 
 Application Insights Agent ile daha fazlasını yapın:
 - Uygulama Öngörüleri [Aracısı'nın sorun giderme](status-monitor-v2-troubleshoot.md) kılavuzunu kullanın.
 - Ayarlarınızın doğru kaydedildiğini doğrulamak için [config'i alın.](status-monitor-v2-api-get-config.md)
 - İzlemeyi incelemek için [durumu alın.](status-monitor-v2-api-get-status.md)
