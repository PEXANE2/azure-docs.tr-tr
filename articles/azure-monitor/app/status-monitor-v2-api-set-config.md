---
title: Azure Uygulama Öngörüleri AracıAP başvurusu
description: Uygulama Öngörüleri Aracısı API başvurusu. Set-ApplicationInsightsMonitoringConfig. Web sitesini yeniden dağıtmadan web sitesinin performansını izleyin. Şirket içinde, VM'lerde veya Azure'da barındırılan ASP.NET web uygulamalarıyla çalışır.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: c47b9b5f297fa62c474e6c29737d6d11b887130d
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537483"
---
# <a name="application-insights-agent-api-set-applicationinsightsmonitoringconfig"></a>Uygulama Öngörüleri Aracısı API: Set-ApplicationInsightsMonitoringConfig

Bu belge, [Az.ApplicationMonitor PowerShell modülünün](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)bir üyesi olan bir cmdlet açıklar.

## <a name="description"></a>Açıklama

Config dosyasını tam yeniden yükleme yapmadan ayarlar.
Değişikliklerinizin etkili olması için IIS'yi yeniden başlatın.

> [!IMPORTANT] 
> Bu cmdlet, Yönetici izinleri içeren bir PowerShell oturumu gerektirir.


## <a name="examples"></a>Örnekler

### <a name="example-with-a-single-instrumentation-key"></a>Tek bir enstrümantasyon tuşu ile örnek
Bu örnekte, geçerli bilgisayardaki tüm uygulamalara tek bir enstrümantasyon anahtarı atanır.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### <a name="example-with-an-instrumentation-key-map"></a>Enstrümantasyon anahtar haritası ile örnek
Bu örnekte:
- `MachineFilter`joker kartı kullanarak geçerli `'.*'` bilgisayarla eşleşir.
- `AppFilter='WebAppExclude'`bir `null` enstrümantasyon anahtarı sağlar. Belirtilen uygulama enstrümanted olmayacaktır.
- `AppFilter='WebAppOne'`belirtilen uygulamaya benzersiz bir enstrümantasyon anahtarı atar.
- `AppFilter='WebAppTwo'`belirtilen uygulamaya benzersiz bir enstrümantasyon anahtarı atar.
- Son `AppFilter` olarak, `'.*'` joker kartı, önceki kurallarla eşleşmeyan tüm web uygulamalarını eşleştirmek ve varsayılan bir enstrümantasyon anahtarı atamak için de kullanır.
- Okunabilirlik için boşluklar eklenir.

```powershell
Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap `
       @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
          @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'}},
          @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'}},
          @{MachineFilter='.*';AppFilter='.*';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'}})
```

## <a name="parameters"></a>Parametreler

### <a name="-instrumentationkey"></a>-EnstrümantasyonAnahtar
**Gereklidir.** Hedef bilgisayardaki tüm uygulamalar tarafından kullanılmak üzere tek bir enstrümantasyon anahtarı sağlamak için bu parametreyi kullanın.

### <a name="-instrumentationkeymap"></a>-EnstrümantasyonKeyMap
**Gereklidir.** Birden çok enstrümantasyon anahtarı ve her uygulama tarafından kullanılan enstrümantasyon anahtarlarının eşlemi sağlamak için bu parametreyi kullanın.
Birkaç bilgisayar için tek bir yükleme `MachineFilter`komut dosyası ayarlayarak oluşturabilirsiniz.

> [!IMPORTANT]
> Uygulamalar, kuralların sağlandığı sırada kurallara göre eşleşir. Bu nedenle, önce en özel kuralları ve en genel kuralları en son belirtmelisiniz.

#### <a name="schema"></a>Şema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'})`

- **MachineFilter,** bilgisayarın veya VM adının gerekli bir C# regex'idir.
    - '.*' tüm maç olacak
    - 'ComputerName' yalnızca belirtilen ada sahip bilgisayarlarla eşleşir.
- **AppFilter,** bilgisayarın veya VM adının gerekli bir C# regex'idir.
    - '.*' tüm maç olacak
    - 'ApplicationName' belirtilen ada sahip yalnızca IIS uygulamalarıyla eşleşir.
- **InstrumentationKey,** önceki iki filtreyle eşleşen uygulamaların izlenmesini etkinleştirmek için gereklidir.
    - İzlemeyi hariç tutmak için kurallar tanımlamak istiyorsanız bu değeri boş bırakın.


### <a name="-verbose"></a>-Verbose
**Ortak parametre.** Ayrıntılı günlükleri görüntülemek için bu anahtarı kullanın.


## <a name="output"></a>Çıktı

Varsayılan olarak, çıktı yok.

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkey"></a>-InstrumentationKey üzerinden config dosyasını ayarlamaktan örnek verbose çıktı

```
VERBOSE: Operation: InstallWithIkey
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkeymap"></a>-InstrumentationKeyMap üzerinden config dosyasını ayarlamaktan örnek verbose çıktı

```
VERBOSE: Operation: InstallWithIkeyMap
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
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
- İzleme ve günlük aramaları ekleyebilmeniz [için Uygulama Öngörüleri SDK'yı kodunuza ekleyin](../../azure-monitor/app/asp-net.md)
 
 Application Insights Agent ile daha fazlasını yapın:
 - Uygulama Öngörüleri [Aracısı'nın sorun giderme](status-monitor-v2-troubleshoot.md) kılavuzunu kullanın.
 - Ayarlarınızın doğru kaydedildiğini doğrulamak için [config'i alın.](status-monitor-v2-api-get-config.md)
 - İzlemeyi incelemek için [durumu alın.](status-monitor-v2-api-get-status.md)
