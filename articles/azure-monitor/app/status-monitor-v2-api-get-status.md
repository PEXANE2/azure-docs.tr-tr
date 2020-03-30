---
title: Azure Uygulama Öngörüleri AracıAP başvurusu
description: Uygulama Öngörüleri Aracısı API başvurusu. Get-ApplicationInsightsMonitoringStatus. Web sitesini yeniden dağıtmadan web sitesinin performansını izleyin. Şirket içinde, sanal makinelerde veya Azure üzerinde ASP.NET web uygulamaları ile çalışır.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: 159dab4a228c822ef62c45c9ccceff638a9bea45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671265"
---
# <a name="application-insights-agent-api-get-applicationinsightsmonitoringstatus"></a>Uygulama Öngörüleri Aracısı API: Get-ApplicationInsightsMonitoringStatus

Bu [makalede, Az.ApplicationMonitor PowerShell modülüüyesi](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)bir cmdlet açıklanır.

## <a name="description"></a>Açıklama

Bu cmdlet Durum İzleyicisi hakkında sorun giderme bilgileri sağlar.
PowerShell Modülü'nün izleme durumunu, sürümünü incelemek ve çalıştırma işlemini incelemek için bu cmdlet'i kullanın.
Bu cmdlet, izleme için gerekli olan önemli dosyalar la ilgili sürüm bilgilerini ve bilgilerini raporedecektir.

> [!IMPORTANT] 
> Bu cmdlet, Yönetici izinleri içeren bir PowerShell oturumu gerektirir.

## <a name="examples"></a>Örnekler

### <a name="example-application-status"></a>Örnek: Uygulama durumu

Web sitelerinin izleme durumunu görüntülemek için komutu `Get-ApplicationInsightsMonitoringStatus` çalıştırın.

```
PS C:\Windows\system32> Get-ApplicationInsightsMonitoringStatus
Machine Identifier:
811D43F7EC807E389FEA2E732381288ACCD70AFFF9F569559AC3A75F023FA639

IIS Websites:

SiteName               : Default Web Site
ApplicationPoolName    : DefaultAppPool
SiteId                 : 1
SiteState              : Stopped

SiteName               : DemoWebApp111
ApplicationPoolName    : DemoWebApp111
SiteId                 : 2
SiteState              : Started
ProcessId              : not found

SiteName               : DemoWebApp222
ApplicationPoolName    : DemoWebApp222
SiteId                 : 3
SiteState              : Started
ProcessId              : 2024
Instrumented           : true
InstrumentationKey     : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx123

SiteName               : DemoWebApp333
ApplicationPoolName    : DemoWebApp333
SiteId                 : 4
SiteState              : Started
ProcessId              : 5184
AppAlreadyInstrumented : true
```

Bu örnekte;
- **Makine Tanımlayıcısı,** sunucunuzu benzersiz olarak tanımlamak için kullanılan anonim bir kimliktir. Bir destek isteği oluşturursanız, sunucunuz için günlükleri bulmak için bu kimliğin gerekir.
- **Varsayılan Web Sitesi** IIS'de Durduruldu
- **DemoWebApp111** IIS'de başlatıldı, ancak herhangi bir istek almadı. Bu rapor, çalışma işlemi olmadığını gösterir (ProcessId: bulunamadı).
- **DemoWebApp222** çalışıyor ve izleniyor (Instrumented: true). Kullanıcı yapılandırmadayanarak, Instrumentation Key xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx123 bu site için eşleşti.
- **DemoWebApp333,** Application Insights SDK kullanılarak el ile işletilmiştir. Durum İzleyicisi SDK'yı algıladı ve bu siteyi izlemez.


### <a name="example-powershell-module-information"></a>Örnek: PowerShell modül bilgileri

Geçerli modül `Get-ApplicationInsightsMonitoringStatus -PowerShellModule` hakkında bilgi görüntülemek için komutu çalıştırın:

```
PS C:\> Get-ApplicationInsightsMonitoringStatus -PowerShellModule

PowerShell Module version:
0.4.0-alpha

Application Insights SDK version:
2.9.0.3872

Executing PowerShell Module Assembly:
Microsoft.ApplicationInsights.Redfield.Configurator.PowerShell, Version=2.8.14.11432, Culture=neutral, PublicKeyToken=31bf3856ad364e35

PowerShell Module Directory:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.2\content\PowerShell

Runtime Paths:
ParentDirectory (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content

ConfigurationPath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config

ManagedHttpModuleHelperPath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll

RedfieldIISModulePath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll

InstrumentationEngine86Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation32\MicrosoftInstrumentationEngine_x86.dll

InstrumentationEngine64Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll

InstrumentationEngineExtensionHost86Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation32\Microsoft.ApplicationInsights.ExtensionsHost_x86.dll

InstrumentationEngineExtensionHost64Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll

InstrumentationEngineExtensionConfig86Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation32\Microsoft.InstrumentationEngine.Extensions.config

InstrumentationEngineExtensionConfig64Path (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.InstrumentationEngine.Extensions.config

ApplicationInsightsSdkPath (Exists: True)
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.dll
```

### <a name="example-runtime-status"></a>Örnek: Çalışma zamanı durumu

Tüm DL'lerin yüklenip yüklenmediğinizi görmek için işlem aracılı bilgisayarda inceleyebilirsiniz. İzleme çalışıyorsa, en az 12 DL yüklenmelidir.

Komutu `Get-ApplicationInsightsMonitoringStatus -InspectProcess`çalıştırın:


```
PS C:\> Get-ApplicationInsightsMonitoringStatus -InspectProcess

iisreset.exe /status
Status for IIS Admin Service ( IISADMIN ) : Running
Status for Windows Process Activation Service ( WAS ) : Running
Status for Net.Msmq Listener Adapter ( NetMsmqActivator ) : Running
Status for Net.Pipe Listener Adapter ( NetPipeActivator ) : Running
Status for Net.Tcp Listener Adapter ( NetTcpActivator ) : Running
Status for World Wide Web Publishing Service ( W3SVC ) : Running

handle64.exe -accepteula -p w3wp
BF0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.ServerTelemetryChannel.dll
C58: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.AzureAppServices.dll
C68: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.DependencyCollector.dll
C78: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.WindowsServer.dll
C98: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.Web.dll
CBC: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.PerfCounterCollector.dll
DB0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.AI.Agent.Intercept.dll
B98: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll
BB4: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.Contracts.dll
BCC: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.Redfield.Lightup.dll
BE0: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.dll

listdlls64.exe -accepteula w3wp
0x0000000019ac0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
0x00000000198b0000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
0x000000000c460000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
0x000000000ad60000  0x108000  C:\Windows\TEMP\2.4.0.0.Microsoft.ApplicationInsights.Extensions.Intercept_x64.dll
```

## <a name="parameters"></a>Parametreler

### <a name="no-parameters"></a>(Parametre yok)

Varsayılan olarak, bu cmdlet web uygulamalarının izleme durumunu bildirir.
Uygulamanızın başarılı bir şekilde algılanmış olup olmadığını gözden geçirmek için bu seçeneği kullanın.
Ayrıca, hangi Enstrümantasyon Anahtarının sitenizle eşleşebileceğini de inceleyebilirsiniz.


### <a name="-powershellmodule"></a>-PowerShellModülü
**İsteğe bağlı**. İzleme için gereken DL'lerin sürüm numaralarını ve yollarını bildirmek için bu anahtarı kullanın.
Application Insights SDK de dahil olmak üzere herhangi bir DLL sürümünü tanımlamanız gerekiyorsa bu seçeneği kullanın.

### <a name="-inspectprocess"></a>-İnceleme Süreci

**İsteğe bağlı**. IIS'nin çalışıp çalışmadığını bildirmek için bu anahtarı kullanın.
Ayrıca, gerekli DL'lerin IIS çalışma süresine yüklenip yüklenmeyeceğini belirlemek için harici araçlar indirir.


Bu işlem herhangi bir nedenle başarısız olursa, bu komutları el ile çalıştırabilirsiniz:
- iisreset.exe /durum
- [handle64.exe](https://docs.microsoft.com/sysinternals/downloads/handle) -p w3wp | findstr /I "InstrumentationEngine AI. UygulamaInsights"
- [listdlls64.exe](https://docs.microsoft.com/sysinternals/downloads/listdlls) w3wp | findstr /I "InstrumentationEngine AI ApplicationInsights"


### <a name="-force"></a>-Force

**İsteğe bağlı**. Yalnızca InspectProcess ile kullanılır. Ek araçlar indirilmeden önce görünen kullanıcı istemini atlamak için bu anahtarı kullanın.


## <a name="next-steps"></a>Sonraki adımlar

 Application Insights Agent ile daha fazlasını yapın:
 - Uygulama Öngörüleri [Aracısı'nın sorun giderme](status-monitor-v2-troubleshoot.md) kılavuzunu kullanın.
