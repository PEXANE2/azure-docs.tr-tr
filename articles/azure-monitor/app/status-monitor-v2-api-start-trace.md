---
title: Azure Uygulama Öngörüleri AracıAP başvurusu
description: Uygulama Öngörüleri Aracısı API başvurusu. Başlangıç-İzleme. Durum İzleme ve Uygulama Öngörüleri SDK'dan ETW günlüklerini toplayın.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: b9680101f1a22dd6d9c1617c8afc13a10ad1c594
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671231"
---
# <a name="application-insights-agent-api-start-applicationinsightsmonitoringtrace"></a>Uygulama Insights Ajan API: Başlat-UygulamaInsightsMonitoringTrace

Bu [makalede, Az.ApplicationMonitor PowerShell modülüüyesi](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)bir cmdlet açıklanır.

## <a name="description"></a>Açıklama

Kodsuz ekleme çalışma saatinden [ETW Olayları](https://docs.microsoft.com/windows/desktop/etw/event-tracing-portal) toplar. Bu cmdlet [PerfView](https://github.com/microsoft/perfview)çalışan bir alternatiftir.

Toplanan olaylar gerçek zamanlı olarak konsola yazdırılır ve bir ETL dosyasına kaydedilir. Çıktı ETL dosyası daha fazla araştırma için [PerfView](https://github.com/microsoft/perfview) tarafından açılabilir.

Bu cmdlet zaman adedine (varsayılan 5 dakika) ulaşıncaya`Ctrl + C`veya el ile durdurulana kadar çalışır ( ).

> [!IMPORTANT] 
> Bu cmdlet, Yönetici izinleri içeren bir PowerShell oturumu gerektirir.

## <a name="examples"></a>Örnekler

### <a name="how-to-collect-events"></a>Etkinlikleri nasıl toplarlar?

Normalde, başvurunuzun neden çalındığını araştırmak için olayları toplamanızı isteriz.

Kodsuz ekleme çalışma süresi, IIS başlatıldığında ve uygulamanız başlatıldığında ETW olayları yayacaktır.

Bu olayları toplamak için:
1. Yönetici ayrıcalıklarına sahip cmd konsolunda, IIS'yi ve tüm web uygulamalarını kapatmak için çalıştırın. `iisreset /stop`
2. Bu cmdlet çalıştırın
3. Yönetici ayrıcalıklarına sahip cmd konsolunda, IIS'yi başlatmak için çalıştırın. `iisreset /start`
4. Uygulamanıza göz atmaya çalışın.
5. Uygulamanız yüklemeyi bitirdikten sonra, uygulamayı`Ctrl + C`el ile durdurabilir veya zaman anına kadar bekleyebilirsiniz.

### <a name="what-events-to-collect"></a>Hangi etkinliklerin toplanması

Olayları toplarken üç seçeneğiniz var:
1. Application Insights SDK'dan yayılan olayları toplamak için anahtarı `-CollectSdkEvents` kullanın.
2. Status Monitor `-CollectRedfieldEvents` ve Redfield Runtime tarafından yayılan olayları toplamak için anahtarı kullanın. Bu günlükler, IIS ve uygulama başlatma tanılama yardımcı olur.
3. Her iki olay türünü de toplamak için her iki anahtarı da kullanın.
4. Varsayılan olarak, anahtar belirtilmemişse, her iki olay türü de toplanır.


## <a name="parameters"></a>Parametreler

### <a name="-maxdurationinminutes"></a>-MaxDurationInMinutes
**Isteğe bağlı.** Bu komut dosyasının olayları ne kadar süreyle toplaması gerektiğini ayarlamak için bu parametreyi kullanın. Varsayılan değer 5 dakikadır.

### <a name="-logdirectory"></a>-LogDirectory
**Isteğe bağlı.** ETL dosyasının çıktı dizini ayarlamak için bu anahtarı kullanın. Varsayılan olarak, bu dosya PowerShell Modülleri dizininde oluşturulur. Tam yol komut dosyası yürütme sırasında görüntülenir.


### <a name="-collectsdkevents"></a>-CollectSdkEtkinlikler
**Isteğe bağlı.** Application Insights SDK olaylarını toplamak için bu anahtarı kullanın.

### <a name="-collectredfieldevents"></a>-CollectRedfield Etkinlikler
**Isteğe bağlı.** Durum İzleyicisi ve Redfield çalışma saatinden olayları toplamak için bu anahtarı kullanın.

### <a name="-verbose"></a>-Verbose
**Ortak parametre.** Ayrıntılı günlükleri çıktıetmek için bu anahtarı kullanın.



## <a name="output"></a>Çıktı


### <a name="example-of-application-startup-logs"></a>Uygulama başlangıç günlükleri örneği
```
PS C:\Windows\system32> Start-ApplicationInsightsMonitoringTrace -ColectRedfieldEvents
Starting...
Log File: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\logs\20190627_144217_ApplicationInsights_ETW_Trace.etl
Tracing enabled, waiting for events.
Tracing will timeout in 5 minutes. Press CTRL+C to cancel.

2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 70 ms
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Current assembly 'Microsoft.ApplicationInsights.RedfieldIISModule, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3' location 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Matched filter '.*'~'STATUSMONITORTE', '.*'~'DemoWithSql'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Lightup assembly calculated path: 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.Redfield.Lightup.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Loaded applicationInsights.config from assembly's resource Microsoft.ApplicationInsights.Redfield.Lightup, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3/Microsoft.ApplicationInsights.Redfield.Lightup.ApplicationInsights-recommended.config
2:42:34 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Successfully attached ApplicationInsights SDK
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.LoadLightupAssemblyAndGetLightupHttpModuleClass, success, 2687 ms
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:34 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 3288 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 0 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 0 ms
Timeout Reached. Stopping...
```


## <a name="next-steps"></a>Sonraki adımlar

Ek Sorun Giderme:

- Ek sorun giderme adımlarını gözden geçirin:https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-troubleshoot
- Kaçırmış olabileceğiniz parametreler hakkında bilgi edinmek için [API Başvuru'yu](status-monitor-v2-overview.md#powershell-api-reference) gözden geçirin.
- Ek yardıma ihtiyacınız [varsa, GitHub'dan](https://github.com/Microsoft/ApplicationInsights-Home/issues)bizimle iletişime geçebilirsiniz.



 Application Insights Agent ile daha fazlasını yapın:
 - Uygulama Öngörüleri [Aracısı'nın sorun giderme](status-monitor-v2-troubleshoot.md) kılavuzunu kullanın.
 - Ayarlarınızın doğru kaydedildiğini doğrulamak için [config'i alın.](status-monitor-v2-api-get-config.md)
 - İzlemeyi incelemek için [durumu alın.](status-monitor-v2-api-get-status.md)
