---
title: Azure Uygulama Öngörüleri Aracısorun giderme ve bilinen sorunlar | Microsoft Dokümanlar
description: Application Insights Agent ve sorun giderme örneklerinin bilinen sorunları. Web sitesini yeniden dağıtmadan web sitesinin performansını izleyin. Şirket içinde, VM'lerde veya Azure'da barındırılan ASP.NET web uygulamalarıyla çalışır.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: 217629ba5c386557455cc2d2b8bd47f85fa8f84e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671163"
---
# <a name="troubleshooting-application-insights-agent-formerly-named-status-monitor-v2"></a>Sorun Giderme Uygulama Öngörüleri Aracısı (eski adıyla Status Monitor v2)

İzlemeyi etkinleştirdiğinizde, veri toplamayı engelleyen sorunlarla karşılaşabilirsiniz.
Bu makalede, bilinen tüm sorunları listeler ve sorun giderme örnekleri sağlar.
Burada listelenmemiş bir sorunla karşılaşırsanız, [Bizimle GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues)üzerinden iletişime geçebilirsiniz.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="conflicting-dlls-in-an-apps-bin-directory"></a>Uygulamanın depo gözü dizininde çakışan DL'ler

Bu DL'lerden herhangi biri depo gözü dizinde mevcutsa, izleme başarısız olabilir:

- Microsoft.ApplicationInsights.dll
- Microsoft.AspNet.TelemetryCorrelation.dll
- System.Diagnostics.DiagnosticSource.dll

Bu DL'lerden bazıları, uygulamanız bunları kullanmasa bile Visual Studio varsayılan uygulama şablonlarına dahildir.
Semptomatik davranışları görmek için sorun giderme araçlarını kullanabilirsiniz:

- PerfView:
    ```
    ThreadID="7,500" 
    ProcessorNumber="0" 
    msg="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ExtVer="2.8.13.5972" 
    SubscriptionId="" 
    AppName="" 
    FormattedMessage="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ```

- IISReset ve uygulama yükleme (telemetri olmadan). Sysinternals (Handle.exe ve ListDLLs.exe) ile araştırın:
    ```
    .\handle64.exe -p w3wp | findstr /I "InstrumentationEngine AI. ApplicationInsights"
    E54: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll

    .\Listdlls64.exe w3wp | findstr /I "InstrumentationEngine AI ApplicationInsights"
    0x0000000009be0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
    0x0000000009b90000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
    0x0000000004d20000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
    ```

### <a name="conflict-with-iis-shared-configuration"></a>IIS paylaşılan yapılandırma ile çakışma

Bir web sunucuları kümeniz varsa, paylaşılan bir [yapılandırma](https://docs.microsoft.com/iis/web-hosting/configuring-servers-in-the-windows-web-platform/shared-configuration_211)kullanıyor olabilirsiniz.
HttpModülü bu paylaşılan yapılandırmaya enjekte edilemez.
DLL'yi her sunucunun GAC'sine yüklemek için her web sunucusundaki Etkinleştir komutunu çalıştırın.

Etkinleştir komutunu çalıştırdıktan sonra aşağıdaki adımları tamamlayın:
1. Paylaşılan yapılandırma dizinine gidin ve applicationHost.config dosyasını bulun.
2. Yapılandırmanızın modüller bölümüne bu satırı ekleyin:
    ```
    <modules>
        <!-- Registered global managed http module handler. The 'Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll' must be installed in the GAC before this config is applied. -->
        <add name="ManagedHttpModuleHelper" type="Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" preCondition="managedHandler,runtimeVersionv4.0" />
    </modules>
    ```

### <a name="iis-nested-applications"></a>IIS İç Içe Uygulamalar

Sürüm 1.0'da IIS'deki iç içe uygulamaları çalgılatam etmiyoruz.
Bu sorunu [burada](https://github.com/microsoft/ApplicationInsights-Home/issues/369)takip ediyoruz.

### <a name="advanced-sdk-configuration-isnt-available"></a>Gelişmiş SDK Yapılandırması kullanılamıyor.

SDK yapılandırması sürüm 1.0'da son kullanıcıya açık değildir.
Bu sorunu [burada](https://github.com/microsoft/ApplicationInsights-Home/issues/375)takip ediyoruz.

    
    
## <a name="troubleshooting"></a>Sorun giderme
    
### <a name="troubleshooting-powershell"></a>Sorun Giderme PowerShell

#### <a name="determine-which-modules-are-available"></a>Hangi modüllerin mevcut olduğunu belirleme
Hangi modüllerin `Get-Module -ListAvailable` yüklü olduğunu belirlemek için komutu kullanabilirsiniz.

#### <a name="import-a-module-into-the-current-session"></a>Geçerli oturuma bir modül alma
Bir modül PowerShell oturumuna yüklenmediyse, komutu `Import-Module <path to psd1>` kullanarak modülü el ile yükleyebilirsiniz.


### <a name="troubleshooting-the-application-insights-agent-module"></a>Uygulama Öngörüleri Aracısı modülünde sorun giderme

#### <a name="list-the-commands-available-in-the-application-insights-agent-module"></a>Application Insights Agent modülünde bulunan komutları listele
Kullanılabilir komutları almak için komutu `Get-Command -Module Az.ApplicationMonitor` çalıştırın:

```
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Disable-ApplicationInsightsMonitoring              0.4.0      Az.ApplicationMonitor
Cmdlet          Disable-InstrumentationEngine                      0.4.0      Az.ApplicationMonitor
Cmdlet          Enable-ApplicationInsightsMonitoring               0.4.0      Az.ApplicationMonitor
Cmdlet          Enable-InstrumentationEngine                       0.4.0      Az.ApplicationMonitor
Cmdlet          Get-ApplicationInsightsMonitoringConfig            0.4.0      Az.ApplicationMonitor
Cmdlet          Get-ApplicationInsightsMonitoringStatus            0.4.0      Az.ApplicationMonitor
Cmdlet          Set-ApplicationInsightsMonitoringConfig            0.4.0      Az.ApplicationMonitor
Cmdlet          Start-ApplicationInsightsMonitoringTrace           0.4.0      Az.ApplicationMonitor
```

#### <a name="determine-the-current-version-of-the-application-insights-agent-module"></a>Application Insights Agent modülünün geçerli sürümünü belirleme
Modül `Get-ApplicationInsightsMonitoringStatus -PowerShellModule` hakkında aşağıdaki bilgileri görüntülemek için komutu çalıştırın:
   - PowerShell modül versiyonu
   - Uygulama Öngörüleri SDK sürümü
   - PowerShell modülünün dosya yolları
    
Bu cmdlet'in nasıl kullanılacağına ilgili ayrıntılı bir açıklama için [API başvurularını](status-monitor-v2-api-get-status.md) gözden geçirin.


### <a name="troubleshooting-running-processes"></a>Çalışan işlemleri sorun giderme

Tüm DL'lerin yüklenip yüklenmediğinizi belirlemek için enstrümanting edilen bilgisayardaki işlemleri inceleyebilirsiniz.
İzleme çalışıyorsa, en az 12 DL yüklenmelidir.

DL'leri kontrol etmek için `Get-ApplicationInsightsMonitoringStatus -InspectProcess` komutu kullanın.

Bu cmdlet'in nasıl kullanılacağına ilgili ayrıntılı bir açıklama için [API başvurularını](status-monitor-v2-api-get-status.md) gözden geçirin.


### <a name="collect-etw-logs-by-using-perfview"></a>PerfView'i kullanarak ETW günlüklerini topla

#### <a name="setup"></a>Kurulum

1. İndir PerfView.exe ve PerfView64.exe [GitHub](https://github.com/Microsoft/perfview/releases).
2. PerfView64.exe'yi başlatın.
3. **Gelişmiş Seçenekleri**Genişletin.
4. Bu onay kutularını temizleyin:
    - **Zip**
    - **Birleştir**
    - **.NET Sembol Koleksiyonu**
5. Bu **Ek Sağlayıcıları**Ayarlayın:`61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,925fa42b-9ef6-5fa7-10b8-56449d7a2040,f7d60e07-e910-5aca-bdd2-9de45b46c560,7c739bb9-7861-412e-ba50-bf30d95eae36,61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,252e28f4-43f9-5771-197a-e8c7e750a984`


#### <a name="collecting-logs"></a>Günlükleri toplama

1. Yönetici ayrıcalıklarına sahip bir komut `iisreset /stop` konsolunda, IIS'yi ve tüm web uygulamalarını kapatmak için komutu çalıştırın.
2. PerfView'de **Koleksiyonu Başlat'ı**seçin.
3. Yönetici ayrıcalıklarına sahip bir komut `iisreset /start` konsolunda, IIS'yi başlatmak için komutu çalıştırın.
4. Uygulamanıza göz atmaya çalışın.
5. Uygulamanız yüklendikten sonra PerfView'e dönün ve **Koleksiyonu Durdur'u**seçin.



## <a name="next-steps"></a>Sonraki adımlar

- Kaçırmış olabileceğiniz parametreler hakkında bilgi edinmek için [API başvurularını](status-monitor-v2-overview.md#powershell-api-reference) gözden geçirin.
- Burada listelenmemiş bir sorunla karşılaşırsanız, [Bizimle GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues)üzerinden iletişime geçebilirsiniz.
