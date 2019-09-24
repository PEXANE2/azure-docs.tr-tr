---
title: 'Azure Durum İzleyicisi v2 API başvurusu: İzlemeyi Başlat | Microsoft Docs'
description: Durum İzleyicisi v2 API başvurusu. Başlat-Izle. Durum İzleyicisi ve Application Insights SDK 'dan ETW günlüklerini toplayın.
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
ms.openlocfilehash: f4c43e6bdb70687606041c2f0859ab072db2b587
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71200380"
---
# <a name="status-monitor-v2-api-start-applicationinsightsmonitoringtrace"></a>Durum İzleyicisi v2 API 'SI: Start-Applicationınsiizsmonitoringtrace

Bu makalede, [az. ApplicationMonitor PowerShell modülünün](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)üyesi olan bir cmdlet açıklanmaktadır.

## <a name="description"></a>Açıklama

Kodsuz kullanacaksınız Attach çalışma zamanından [ETW olaylarını](https://docs.microsoft.com/windows/desktop/etw/event-tracing-portal) toplar. Bu cmdlet [PerfView](https://github.com/microsoft/perfview)çalıştırmaya alternatiftir.

Toplanan olaylar, gerçek zamanlı olarak konsola yazdırılır ve bir ETL dosyasına kaydedilir. Çıktı ETL dosyası, daha fazla araştırma için [PerfView](https://github.com/microsoft/perfview) tarafından açılabilir.

Bu cmdlet, zaman aşımı süresine (varsayılan 5 dakika) ulaşıncaya kadar veya el ile (`Ctrl + C`) durduruluncaya kadar çalışır.

> [!IMPORTANT] 
> Bu cmdlet yönetici izinlerine sahip bir PowerShell oturumu gerektirir.

## <a name="examples"></a>Örnekler

### <a name="how-to-collect-events"></a>Olayları toplama

Normalde, uygulamanızın neden görüntülenmediğini araştırmak için olay toplamanızı isteyeceğiz.

Kodsuz kullanacaksınız Attach çalışma zamanı, IIS başlatıldığında ve uygulamanız başlatıldığında ETW olaylarını yayacaktır.

Bu olayları toplamak için:
1. Yönetici ayrıcalıklarına sahip bir cmd konsolunda, IIS 'yi `iisreset /stop` ve tüm Web uygulamalarını kapatmak için yürütün.
2. Bu cmdlet 'i Yürüt
3. Yönetici ayrıcalıklarına sahip bir cmd konsolunda, IIS 'yi `iisreset /start` başlatmak için yürütün.
4. Uygulamanıza gözatmayı deneyin.
5. Uygulamanızın yüklenmesi tamamlandıktan sonra, el ile durdurabilirsiniz (`Ctrl + C`) veya zaman aşımı süresini bekleyebilirsiniz.

### <a name="what-events-to-collect"></a>Toplanacak olaylar

Olayları toplarken üç seçeneğiniz vardır:
1. Application Insights SDK 'dan `-CollectSdkEvents` yayılan olayları toplamak için anahtarını kullanın.
2. Durum İzleyicisi ve Redfield çalışma zamanı tarafından yayılan olayları toplamak için anahtarını `-CollectRedfieldEvents` kullanın. Bu Günlükler IIS ve uygulama başlangıcını tanılarken yararlı olur.
3. Her iki olay türünü de toplamak için her iki anahtarı kullanın.
4. Varsayılan olarak, hiçbir anahtar belirtilmemişse olay türleri toplanacaktır.


## <a name="parameters"></a>Parametreler

### <a name="-maxdurationinminutes"></a>-MaxDurationInMinutes
**İsteğe bağlı.** Bu betiğin olay toplama süresini ayarlamak için bu parametreyi kullanın. Varsayılan değer 5 dakikadır.

### <a name="-logdirectory"></a>-LogDirectory
**İsteğe bağlı.** ETL dosyasının çıkış dizinini ayarlamak için bu anahtarı kullanın. Varsayılan olarak, bu dosya PowerShell modülleri dizininde oluşturulur. Tam yol, betik yürütme sırasında görüntülenecektir.


### <a name="-collectsdkevents"></a>-CollectSdkEvents
**İsteğe bağlı.** Application Insights SDK olaylarını toplamak için bu anahtarı kullanın.

### <a name="-collectredfieldevents"></a>-CollectRedfieldEvents
**İsteğe bağlı.** Durum İzleyicisi ve Redfield çalışma zamanının olaylarını toplamak için bu anahtarı kullanın.

### <a name="-verbose"></a>-Ayrıntılı
**Ortak parametre.** Ayrıntılı günlükleri çıkarmak için bu anahtarı kullanın.



## <a name="output"></a>Output


### <a name="example-of-application-startup-logs"></a>Uygulama başlangıç günlüklerine örnek
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

Ek sorun giderme:

- Aşağıdaki ek sorun giderme adımlarını gözden geçirin: https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-troubleshoot
- Kaçırmış olabileceğiniz parametreler hakkında bilgi edinmek için [API başvurusunu](status-monitor-v2-overview.md#powershell-api-reference) gözden geçirin.
- Ek yardıma ihtiyacınız varsa [GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues)' da bizimle iletişim kurmanız yeterlidir.



 Durum İzleyicisi v2 ile daha fazlasını yapın:
 - Durum İzleyicisi v2 [sorunlarını gidermek](status-monitor-v2-troubleshoot.md) için kılavuzumuzu kullanın.
 - Ayarlarınızın doğru kaydedildiğinden emin olmak için [yapılandırmayı alın](status-monitor-v2-api-get-config.md) .
 - İzlemeyi İnceleme [durumunu alın](status-monitor-v2-api-get-status.md) .
